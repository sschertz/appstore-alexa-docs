---
title: Dolby Integration Guidelines for Fire TV
permalink: amazon-fire-tv-dolby-integration-guidelines.html
sidebar: firetv
product: Fire TV
toc: false
github: true
---

This documentation provides integration guidelines for app developers who need to support Dolby Digital or Dolby Digital Plus audio content in their apps. 

{% include important.html content="All apps that support Dolby audio content must also work on devices that do NOT support a Dolby decoder." %}

You can see the Dolby settings in Fire TV by going to **Settings > Display & Sounds > Audio > Dolby Digital Output**. 

* TOC
{:toc}

## Terms

Note the following abbreviations used in this documentation:

*  **EAC3**: Dolby Digital Plus (DDP) 
*  **AC3**: Dolby Digital (DD) 


The following terms are used to refer to Fire TV:

*  **Fire TV (1st Generation)**: Refers to the first version of the Fire TV box (released in 2014).
*  **Fire TV (2nd Generation)**: Refers to the second version of the Fire TV box (released in 2015).
*  **Fire TV Stick**: Refers to the first (and so far only) version of the Fire TV stick (released in 2014).

{% include note.html content="Usually the latest version of the Fire TV box is just referred to as \"Fire TV,\" but since this documentation contrasts two versions of Fire TV, the terms \"Fire TV &ndash; 1st Generation\" and \"Fire TV &ndash; 2nd Generation\" are used. These terms always refer to the box. There is just one version of Fire TV Stick, so there's no need to distinguish versions through a 1st Generation or 2nd Generation descriptor." %}

## Four Ways to Play Dolby Audio

You can play Dolby (DD or DDP) audio content in your app in four ways: 

1. [Custom Media Player Approach](../amazon-fire-tv-dolby-integration-guidelines.md#custom)
2. [ExoPlayer](../amazon-fire-tv-dolby-integration-guidelines.md#exoplayer)
3. [Other Supported Media Players](../amazon-fire-tv-dolby-integration-guidelines.md#supported)
4. [Android Media Player](../amazon-fire-tv-dolby-integration-guidelines.md#androidmp)

## Option 1: Custom Media Player Approach {#custom}

Implement a custom player that uses  [AudioManager][AudioManager],  [AudioTrack][AudioTrack], and (optionally)  [MediaCodec APIs][MediaCodec_APIs]. This section specifies the use of AudioManager, AudioTrack, and optionally MediaCodec APIs to play Dolby audio content as recommended by Android.

### AudioManager Overview

Android L enhanced the [ACTION_HDMI_AUDIO_PLUG][ACTION_HDMI_AUDIO_PLUG] intent in [AudioManager][AudioManager] to report the capabilities of sinks in Lollipop. This intent is broadcast whenever HDMI is plugged into or unplugged from to the device. 

This intent also has a provision for apps to detect the capabilities of the connected endpoint (AVR or TV) via [EXTRA_ENCODINGS][EXTRA_ENCODINGS] present in the intent. The possible values are one of the ENCODING_XXXX values. For example:
 
*  [ENCODING_PCM_16BIT][ENCODING_PCM_16BIT]
*  [ENCODING_AC3][ENCODING_AC3]
*  [ENCODING_E_AC3][ENCODING_E_AC3], and so on

### AudioTrack Overview

Android L added the support of [ENCODING_AC3][ENCODING_AC3] and [ENCODING_E_AC3][ENCODING_E_AC3] encoding formats to AudioFormat. If the AudioTrack is configured with Dolby encoding format, and if the connecting endpoint supports that encoding format, AudioTrack accepts the Dolby raw bitstream as input and passes the bitstream to the endpoint after IEC61937 packetization. AudioTrack supports only clear (not encrypted) input bitstream.

Additionally, the creation of AudioTrack fails if the encoding format is not supported by the connected endpoint.

Applications can use the [ACTION_HDMI_AUDIO_PLUG][ACTION_HDMI_AUDIO_PLUG] intent to find the list of supported Dolby encoding formats for the connected device, create AudioTrack with one of the supported Dolby encoding formats, and pass the corresponding Dolby raw bitstream (in clear) as input to AudioTrack to achieve Dolby pass-through playback. 

Optionally, AudioTrack can also support transcoding DDP bitstream to DD if the connected endpoint only supports DD. This feature is made available to the application by adding ENCODING_E_AC3 to the EXTRA_ENCODINGS extra of the ACTION_HDMI_AUDIO_PLUG intent if the endpoint only supports DD. 

Hence, from an application's perspective, an endpoint that supports DD also supports DDP and can continue to send DDP bitstream to AudioTrack, which internally transcodes the bitstream to DD before passing it through.

### MediaCodec (Optional) Overview

{% include tip.html content="Apps that use MediaCodec for Dolby must also be able to operate on devices without Dolby availability in MediaCodec. To do so, call [findDecoderForFormat][findDecoderForFormat]. If Dolby is not supported, apps must fall back to non-Dolby (such as AAC) content." %}

AudioTrack does not support Dolby audio content that is encrypted with DRM keys. Hence, for an encrypted Dolby audio bitstream, it is mandatory to use MediaCodec APIs to decrypt the bitstream and then pass the decrypted bitstream to AudioTrack. 

Android provides a generic raw audio decoder via MediaCodec interface named "OMX.google.raw.dec," which is essentially a no-op (no operation) copy decoder. If the bitstream is encrypted, it uses MediaCrypto APIs to decrypt the bitstream and outputs it. Apps must use this decoder to decrypt the encrypted Dolby bitstream and pass the clear Dolby bitstream to AudioTrack. 

Optionally, a platform can support Dolby decoder via MediaCodec interface. This decoder decodes the Dolby audio bitstream to PCM. Applications can use [findDecoderForFormat][findDecoderForFormat] to detect the presence of a decoder that supports Dolby MIME type.

The following table summarizes various Dolby playback scenarios in a typical Android TV that does not support DDP to DD transcode in AudioTrack.

<table class="grid">
<colgroup>
<col width="10%" />
<col width="22%" />
<col width="13%" />
<col width="20%" />
<col width="18%" />
<col width="10%" />
</colgroup>
   <thead>
      <tr class="header">
         <th>Connected End-Point capabilities (AVR or TV)</th>
         <th>EXTRA_ENCODINGS in ACTION_HDMI_<br/>AUDIO_PLUG intent</th>
         <th>Source Audio Stream</th>
         <th>MediaCodec (optional)</th>
         <th>AudioTrack encoding format</th>
         <th>Scenario</th>
      </tr>
   </thead>
     <tr>
        <td class="white" rowspan="2">EAC3, AC3 and PCM</td>
        <td class="white" rowspan="2">ENCODING_E_AC3, ENCODING_AC3, ENCODING_PCM_16BIT</td>
        <td class="white">Clear EAC3 or AC3</td>
        <td class="white">None</td>
        <td class="white" rowspan="2">ENCODING_E_AC3 or ENCODING_AC3</td>
        <td class="white" rowspan="2">DDP or DD pass-through</td>
     </tr> 
     <tr>
           <td class="white">Encrypted EAC3 or AC3</td>
           <td class="white">OMX.google.raw.dec</td>
        </tr>
        
    <tr>
          <td class="gray" rowspan="2">AC3 and PCM</td>
          <td class="gray" rowspan="2">ENCODING_AC3, ENCODING_PCM_16BIT</td>
          <td class="gray">Clear AC3</td>
          <td class="gray">None</td>
          <td class="gray" rowspan="2">ENCODING_AC3</td>
          <td class="gray" rowspan="2"> DD passthrough</td>
       </tr>
       <tr>
             <td class="gray">Encrypted AC3</td>
             <td class="gray">OMX.google.raw.dec</td>
          </tr>
        <tr>
           <td class="white" rowspan="2">PCM</td>
           <td class="white" rowspan="2">ENCODING_PCM_16BIT</td>
           <td class="white">AAC or any non-Dolby (recommended)</td>
           <td class="white">OMX.google.aac.decoder</td>
           <td class="white">ENCODING_PCM_16BIT</td>
           <td class="white">PCM playback</td>
        </tr>
      <tr>
         <td class="white">AC3 or EAC3 (only if non-dolby stream is unavailable &amp; Dolby decoder is available in the platform)</td>
         <td class="white">OMX.google.aac.decoder, OMX.dolby.eac3.decoder</td>
         <td class="white">ENCODING_PCM_16BIT</td>
         <td class="white">DD or DDP decode</td>
      </tr>
</table>

## Dolby Support in Fire TV &ndash; 2nd Generation

Fire TV (2nd Generation) 4K was launched with Android L based Fire OS 5 and fully complies with the Android L recommended Dolby architecture. This section describes the Dolby support in Fire TV (2nd Generation). 

### AudioTrack (Fire TV &ndash; 2nd Generation)

AudioTrack supports both DDP/DD pass-through and DDP-to-DD transcoder. The AudioTrack accepts Dolby raw bitstream and, depending on the Dolby capability of the endpoint, decides to either do a DD/DDP pass-through or DDP-to-DD transcode and pass-through.

### AudioManager (Fire TV &ndash; 2nd Generation)

Since AudioTrack also supports transcoding DDP bitstream to DD when the connected endpoint only supports DD, AudioManager indicates this by adding [ENCODING_E_AC3][ENCODING_E_AC3] to the [EXTRA_ENCODINGS][EXTRA_ENCODINGS] extra of the [ACTION_HDMI_AUDIO_PLUG][ACTION_HDMI_AUDIO_PLUG] intent if the endpoint only supports DD. 

Hence, from an application's perspective, an endpoint that supports DD also supports DDP and can continue to send DDP bitstream to AudioTrack, which internally transcodes it to DD before passing it through.

### MediaCodec (Fire TV &ndash; 2nd Generation)

The MediaCodec for Dolby Decoder (OMX.dolby.ac3.decoder & OMX.dolby.eac3.decoder) supports decoding to PCM only. Additionally, Dolby decoders also down-mixes the multi-channel output to stereo. So irrespective of the number of input channels of the content, the output is always stereo PCM. The table below summarizes various Dolby playback scenarios in Fire TV &ndash; 2nd Generation:

<table class="grid">
<colgroup>
<col width="10%" />
<col width="20%" />
<col width="8%" />
<col width="23%" />
<col width="12%" />
<col width="18%" />
<col width="10%" />
</colgroup>
   <thead>
      <tr class="header">
         <th>Connected End-Point capabilities (AVR or TV)</th>
         <th>EXTRA_ENCODINGS in ACTION_HDM_AUDIO_<br/>PLUG intent</th>
         <th>Source Audio Stream</th>
         <th>MediaCodec</th>
         <th>Output Mime Type of MediaCodec</th>
         <th>AudioTrack encoding format</th>
         <th>Scenario</th>
      </tr>
   </thead>
     <tr>
        <td class="white">EAC3, AC3 and PCM</td>
        <td class="white">ENCODING_E_AC3, ENCODING_AC3, ENCODING_PCM_16BIT</td>
        <td class="white">EAC3 or AC3</td>
        <td class="white">OMX.google.raw.dec<sup>**</sup></td>
        <td class="white">audio/eac3 or audio/ac3</td>
        <td class="white">ENCODING_E_AC3 or ENCODING_AC3</td>
        <td class="white">DDP / DD pass-through</td>
     </tr> 
     <tr>
           <td class="gray">AC3 and PCM</td>
           <td class="gray">ENCODING_E_AC3<sup>*</sup>, ENCODING_AC3, ENCODING_PCM_16BIT</td>
           <td class="gray">EAC3 or AC3</td>
           <td class="gray">OMX.google.raw.dec<sup>**</sup></td>
           <td class="gray">audio/eac3 or audio/ac3</td>
           <td class="gray">ENCODING_E_AC3 or ENCODING_AC3</td>
           <td class="gray">DDP-to-DD transcode or DD pass-through</td>
        </tr>
        
     <tr>
           <td class="white" rowspan="3">PCM only</td>
           <td class="white" rowspan="3">ENCODING_PCM_16BIT</td>
           <td class="white">AAC (rec-<br/>ommended)</td>
           <td class="white">OMX.google.raw.dec**</td>
           <td class="white">audio/raw</td>
           <td class="white">ENCODING_PCM_16BIT</td>
           <td class="white">AAC decode</td>
        </tr>
        
     <tr>
           <td class="white">EAC3</td>
           <td class="white">OMX.dolby.eac3.decoder</td>
           <td class="white">audio/raw</td>
           <td class="white">ENCODING_PCM_16BIT</td>
           <td class="white">DDP decode</td>
        </tr>
   
        <tr>
              <td class="white">AC3</td>
              <td class="white">OMX.dolby.ac3.decoder</td>
              <td class="white">audio/raw</td>
              <td class="white">ENCODING_PCM_16BIT</td>
              <td class="white">DDP decode</td>
           </tr>
</table>

\* Because transcoding from DDP to DD is supported.<br/>
\** Optional if the content is not encrypted.

## Dolby Support in FireTV Stick 

Fire TV Stick, which is based on JellyBean Android OS, was launched before Android L was released. Android L officially introduced support for Dolby pass-through audio in Android TV. 

Android L has a recommended architecture for Dolby integration in the platform, which unfortunately does not match with the Dolby integration architecture on Fire TV Stick. This section describes the Dolby support in Fire TV Stick. 

{% include note.html content="The information contained in this section is applicable even though Fire TV Stick is now upgraded to Android L based Fire OS 5." %}

### MediaCodec (Fire TV Stick)

The MediaCodec that handles Dolby bitstream supports all scenarios: Dolby pass-through, transcode, and decode. The MediaCodec depends on the Dolby capability of the connected endpoint and the system's Dolby settings to select the appropriate mode for handling the incoming Dolby bitstream. 

*  For Dolby pass-through or transcode mode, the decoder converts the raw input Dolby bitstream to the IEC61937 packetized format. 
*  For Dolby decode mode, Dolby decoders also down-mixes the multi-channel output to stereo PCM. So irrespective of the number of input channels of the content, the output is always stereo PCM. The decoders available are OMX.dolby.ec3.decoder to handle DDP bitstream and OMX.dolby.ac3.decoder to handle DD bitstream. 

The following table illustrates the functionality of MediaCodec.

<table class="grid">
<colgroup>
<col width="10%" />
<col width="10%" />
<col width="22%" />
<col width="20%" />
<col width="15%" />
<col width="15%" />
</colgroup>
   <thead>
      <tr class="header">
         <th>Endpoint capability</th>
         <th>Input stream type</th>
         <th>Codec</th>
         <th>Dolby Setting</th>
         <th>Output format MimeType</th>
         <th>Comments</th>
      </tr>
   </thead>
     <tr>
        <td class="white" rowspan="3">DDP, DD and PCM</td>
        <td class="white" rowspan="2">DDP</td>
        <td class="white" rowspan="2">OMX.dolby.ec3.decoder</td>
        <td class="white">DDP over HDMI or DDP Auto</td>
        <td class="white">audio/ec3<sup>*</sup></td>
        <td class="white">DDP Pass-through</td>
     </tr> 
     <tr>
           <td class="white">DDP OFF</td>
           <td class="white">audio/raw</td>
           <td class="white">DDP Decode</td>
        </tr>
     <tr>
            <td class="white">DD</td>
            <td class="white">OMX.dolby.ac3.decoder</td>
            <td class="white">DDP over HDMI or DD over HDMI or DDP Auto</td>
            <td class="white">audio/ac3</td>
            <td class="white">DD Pass-through</td>
      </tr>
 <tr>
         <td class="gray" rowspan="4">DD and PCM</td>
         <td class="gray" rowspan="2">DDP</td>
         <td class="gray" rowspan="2">OMX.dolby.ec3.decoder</td>
         <td class="gray">DDP over HDMI, DD over HDMI, DDP Auto</td>
         <td class="gray">audio/ec3<sup>*</sup></td>
         <td class="gray">DDP > DD Transcode</td>
      </tr> 
      <tr>
            <td class="gray">DDP OFF</td>
            <td class="gray">audio/raw</td>
            <td class="gray">DDP Decode</td>
         </tr>
      <tr>
             <td class="gray" rowspan="2">DD</td>
             <td class="gray" rowspan="2">OMX.dolby.ac3.decoder</td>
             <td class="gray">DDP over HDMI, DD over HDMI, or DDP Auto</td>
             <td class="gray">audio/ac3</td>
             <td class="gray">DD Pass-through</td>
       </tr>
       <tr>
       <td class="gray">DDP OFF</td>
       <td class="gray">audio/raw</td>
       <td class="gray">DD Decode</td>
       
</tr>
     <tr>
                <td class="white" rowspan="2">PCM</td>
                <td class="white">DDP</td>
                <td class="white">OMX.dolby.ec3.decoder</td>
                <td class="white">N/A</td>
                <td class="white">audio/raw</td>
                <td class="white">DDP decode</td>
             </tr>
           <tr>
              <td class="white">DD</td>
              <td class="white">OMX.dolby.ac3.decoder</td>
              <td class="white">N/A</td>
              <td class="white">audio/raw</td>
              <td class="white">DDP decode</td>
           </tr>
</table>

\* The missing "a" in eac3 is intentional. This is a subtle difference between Fire TV Stick and Fire TV.

### AudioTrack (Fire TV Stick)

AudioTrack in Fire TV Stick has certain limitations &mdash; it does not support Dolby Transcode or Dolby Decode, and only supports Dolby Pass-through. Additionally,the input Dolby bitstream must be in IEC61937 packetized format &mdash; which is same as the output of Dolby MediaCodec decoders. 

Due to these architectural differences between Fire TV Stick and Android L, app developers need to do major changes in their app to support Dolby audio content playback. The application must always use MediaCodec (even if the content is not encrypted) to process the Dolby bitstream and must configure the AudioTrack based on the output MIME type indicated by MediaCodec. The following steps provide more detail about this process.

#### A. Create the Decoder by MimeType 

Create the MediaCodec by MIME Type (audio/eac3, audio/ec3, or audio/ac3). This is a generic Android way of creating a codec &mdash; no custom changes are required.

#### B. Handle MediaCodec Output Format Change Notification 

When the app starts using MediaCodec to process the input bit stream, it detects an output format change and returns `INFO_OUTPUT_FORMAT_CHANGED` in the `dequeueOutputBuffer` API call. The application must handle this response and use the `getOutputFormat` API to find out the output format MIME type. It should use this MIME type to configure AudioTrack as per the following table:

| MimeType AudioFormat value |
|-----|-----|
| audio/eac3 | [ENCODING_E_AC3][ENCODING_E_AC3] |
| audio/ac3 | [ENCODING_AC3][ENCODING_AC3] |
| audio/raw | [ENCODING_PCM_16BIT] |

#### C. Limitations of AudioTrack for Dolby encoding formats 

AudioTrack for Dolby encoding formats ENCODING_E_AC3 and ENCODING_AC3 has following limitations and known issues:

*  The AudioTrack start/pause API is not effective. Your app might start writing data to the AudioTrack just after creation, and as a result, it will start playing. It does not wait for start API to be called. 
*  AudioTrack’s [getTimeStamp][getTimeStamp] API (which was introduced in KitKat) is back-ported to Jelly-Bean-based Fire TV &ndash; 1st Generation and Fire TV Stick and can be used for A/V Sync logic.
  
{% include note.html content="These limitations are applicable only for the Dolby audio formats mentioned in this section." %}

## Dolby Support in Fire TV &mdash; 1st Generation

Fire TV &ndash; 1st Generation was initially built on a Jelly-Bean-based OS and launched much before Android L -- which officially introduced support for Dolby audio pass-through in Android TV. 

Android L has a recommended architecture for Dolby integration in the platform, which unfortunately does not fully match with the Dolby integration architecture on Fire TV &ndash; 1st Generation). This section describes the Dolby architecture in Fire TV &ndash; 1st Generation.

{% include note.html content="The information contained in this section is applicable even though the Fire TV &ndash; 1st Generation is now upgraded with Android-L-based Fire OS 5." %}

### MediaCodec (Fire TV &mdash; 1st Generation)

The default MediaCodec for Dolby MIME type available in Fire TV &ndash; 1st Generation is a pass-through decoder only. It copies input data to output data without changing the MIME type. Hence, the output format has same MIME type as that of the input format. 

The decoders available are **OMX.qcom.audio.decoder.passthrougheac3** to handle DDP bitstream and **OMX.qcom.audio.decoder.passthroughac3** to handle DD bitstream. In addition, these decoders also decrypt the data if it is encrypted by a DRM scheme. Hence, if the content is encrypted by DRM, applications must use this decoder. Otherwise, the usage of this decoder is optional and the Dolby bitstream can be passed to AudioTrack directly.

### AudioTrack (Fire TV &mdash; 1st Generation)

AudioTrack handles Dolby pass-through, Dolby Decode, and Dolby transcode internally. This means that depending on the Dolby capability of the connected endpoint and the system Dolby setting, AudioTrack selects the appropriate mode of handling the incoming Dolby bitstream.

<table class="grid">
<thead>
<tr class="header">
<th>endpoint capability</th>
<th>Input stream type</th>
<th>Dolby Setting</th>
<th>HDMI Output</th>
<th>Comments</th>
</tr>
</thead>
  <tr>
    <td class="white" rowspan="3">DDP, DD, and PCM</td>
    <td class="white" rowspan="2">DDP</td>
    <td class="white">DDP over HDMI/Optical or DDP Auto</td>
    <td class="white">DDP</td>
    <td class="white">DDP Passthrough</td>
  </tr>
  <tr>
    <td class="white">DDP OFF</td>
    <td class="white">PCM</td>
    <td class="white">DDP Decode</td>
  </tr>
  <tr>
    <td class="white">DD</td>
    <td class="white">DDP over HDMI/Optical, DD over HDMI, or DDP Auto</td>
    <td class="white">DD</td>
    <td class="white">DD Passthrough</td>
  </tr>
  <tr>
    <td class="gray" rowspan="4">DD and PCM</td>
    <td class="gray" rowspan="2">DDP</td>
    <td class="gray">DDP over HDMI/Optical, DD over HDMI, or DDP Auto</td>
    <td class="gray">DD</td>
    <td class="gray">DDP &gt; DD Transcode</td>
  </tr>
  <tr>

    <td class="gray">DDP OFF</td>
    <td class="gray">PCM</td>
    <td class="gray">DDP Decode</td>
  </tr>
  <tr>

    <td class="gray" rowspan="2">DD</td>
    <td class="gray">DDP over HDMI/Optical, DD over HDMI, or DDP Auto</td>
    <td class="gray">DD</td>
    <td class="gray">DD Passthrough</td>
  </tr>
  <tr>
    <td class="gray">DDP OFF</td>
    <td class="gray">PCM</td>
    <td class="gray">DD Decode</td>
  </tr>
  <tr>
    <td class="white">PCM</td>
    <td class="white">DDP or DD</td>
    <td class="white">NA</td>
    <td class="white">PCM</td>
    <td class="white">DD or DDP decode</td>
  </tr>
</table>

### Limitations of AudioTrack for Dolby Encoding Formats (Fire TV &mdash; 1st Generation)

AudioTrack for Dolby encoding formats ENCODING_E_AC3 and ENCODING_AC3 has following limitations and known issues: 

*  The [getPlaybackHeadPosition][getPlaybackHeadPosition] API of AudioTrack is not implemented. Consequently, do not depend on it for A/V sync. Your app must not call this API. Calling this API may result into a crash. 
*  AudioTrack's start/pause API is not effective. You app might start writing data to the AudioTrack just after creation and it will start playing. It does not wait for start API to be called. 
*  AudioTrack’s [getTimeStamp][getTimeStamp] API (which was introduced in KitKat) is back-ported to Jelly-Bean-based Fire TV 1st Gen and Fire TV Stick and can be used for A/V Sync logic via reflection. 

{% include note.html content="These limitations are applicable only for the Dolby audio formats mentioned in this section." %}

## Option 2: Use ExoPlayer {#exoplayer}

[ExoPlayer][fire-tv-media-players#exoplayer] is a open source player backed by Google that supports playing Dolby audio stream. However, due to the architectural differences in Dolby support between a standard Android L device and Fire TV &ndash; 1st Generation family of devices (as described in preceding sections), ExoPlayer fails to play Dolby audio content. 

Amazon has developed patches for Exoplayer to enable playback of Dolby audio content in Fire TV Gen1 family of devices. These patches are open sourced here: [exoplayer-amazon-port](https://github.com/amzn/exoplayer-amazon-port). This port also includes other patches required to enable playback of PlayReady DRM protected content. (Note that each "amazon/rx.y.z" branch maps to ExoPlayer version x.y.z.)

## Option 3: Use Other Supported Media Players {#supported}

You can use other supported media players for Fire TV, such as VisualOn or NexPlayer, to play Dolby audio content and DRM content. Refer to [Media Players for Fire TV][fire-tv-media-players] for further details.

## Option 4: Use Android Media Player {#androidmp}

Your app can use [Android MediaPlayer](https://developer.android.com/reference/android/media/MediaPlayer.html) to play Dolby content.

## Handling non-Dolby End-Points and Private Listening Modes

This section describes recommendations and best practices for Dolby audio content playback on the Fire TV family of devices when connected to a non-Dolby endpoint and private listening. Following these guidelines will help you create the best user experience for Dolby audio content playback. 

As described previously, Android L introduced a new API in [AudioManager][AudioManager] called [ACTION_HDMI_AUDIO_PLUG][ACTION_HDMI_AUDIO_PLUG] intent. This intent allows apps to detect the capabilities of the connected endpoint (AVR or TV). This detection can be used to support Dolby audio content playback in various scenarios including private listening.

Q. How does an app detect the capabilities of the connected endpoint?
:   The app must register a broadcast receiver to receive the [ACTION_HDMI_AUDIO_PLUG][ACTION_HDMI_AUDIO_PLUG] intent and read the [EXTRA_ENCODINGS][EXTRA_ENCODINGS] extra values in the intent. This list of encoding values will indicate the endpoint capabilities. The possible values are one of the [ENCODING_XXXX][ENCODING_XXXX] values (for example, [ENCODING_PCM_16BIT][ENCODING_PCM_16BIT], [ENCODING_AC3][ENCODING_AC3], [ENCODING_E_AC3][ENCODING_E_AC3], and so on).

Q. Which audio stream should be selected for playback when the connected endpoint does not support DD or DDP?
:   In these cases, let your app select a non-Dolby audio content for playback (for example, AAC). This option has the advantage of reduced bandwidth usage as compared to Dolby audio content. 
    
    However, if the app cannot select an alternative non-Dolby audio content and chooses to continue to stream Dolby (DD or DDP) audio, it must do the following for each device.
    
    * **Fire TV &ndash; 1st Generation**: Send Dolby raw bitstream to AudioTrack, which internally decodes and down-mixes to stereo PCM. 
    * **Fire TV Stick and Fire TV &ndash; 2nd Generation**: Use the MediaCodec for Dolby (OMX.dolby.ac3.decoder or OMX.dolby.eac3.decoder). This MediaCodec decodes the Dolby bitstream, down-mixes to stereo PCM, and configures AudioTrack with the ENCODING_PCM_16BIT audio format to render the PCM data. Since the Dolby Decoder in all the platforms down-mixes multi-channel content to stereo, there is no real advantage in choosing Dolby audio content when the endpoint does not support DD or DDP. 

    {% include tip.html content="It is strongly recommended that you select a non-Dolby audio stream when the device is connected to an endpoint that does not support Dolby (DD or DDP)." %}

Q. Which audio stream should be selected for playback when the connected endpoint supports DD but does not support DDP?
:   The answer depends on the Fire TV device: 
    
    * **Fire TV &ndash; 1st Generation and Fire TV &ndash; 2nd Generation**: AudioTrack supports transcoding DDP bitstream to DD. Hence, even if the endpoint supports DD only, AudioManager appends DDP as the supported capability in the ACTION_HDMI_AUDIO_PLUG intent. As a result, from an app's perspective, the endpoint that supports DD also supports DDP (via transcoding in AudioTrack). 
    * **Fire TV Stick**: The app can select any audio content for playback because the MediaCodec decides whether to pass-through, transcode, or decode based on endpoint capabilities. In this use case, it would pass-through DD or transcode DDP to DD and pass-through DD.

Q. Which audio stream should be selected for playback when the connected endpoint supports DDP and DD?
:  **Fire TV &ndash; 1st Generation and Fire TV &ndash; 2nd Generation**: The app can select DDP or DD audio content for playback. If the audio is not encrypted, the app can then send this clear DD/DDP bitstream data as is to AudioTrack. Internally, AudioTrack will pass-through the DD/DDP bitstream to the connected endpoint.
    
    If the audio contend is DRM protected, the app must instantiate the following MediaCodec to decrypt the bitstream:
    
    *  In Fire TV &ndash; 1st Generation, OMX.qcom.audio.decoder.passthrougheac3 or OMX.qcom.audio.decoder.passthroughac3 . This special decoder is a no-op pass-through decoder that supports decryption (that is, it copies the input bitstream to output after decrypting if needed). 
    
    *  In Fire TV &ndash; 2nd Generation, OMX.google.raw.dec (by name). This special decoder is a no-op pass-through decoder that supports decryption (that is, it copies the input bitstream to output after decrypting if needed).
    
    {% include note.html content="OMX.google.raw.dec is also available in the Fire OS 5 version of Fire TV &ndash; 1st Generation." %}
    
    **Fire TV Stick**: The app can select any audio content for playback because the MediaCodec decides whether to pass-through, transcode, or decode based on the endpoint capabilities. In this use case it would pass-through the Dolby audio content.

Q. How does the app handle private listening use cases involving audio headsets or Bluetooth A2DP headsets during Dolby pass-through playback?
:   **Fire TV &ndash; 1st Generation**: The app does not need to do anything special to handle the private listening use cases. This is automatically taken care of internally in AudioTrack (it switches over from pass-through to decode the mode of operating).
    
    **Fire TV Stick and Fire TV &ndash; 2nd Generation**: When either an audio headset (via a game controller) or Bluetooth headset is connected to the device during Dolby pass-through playback, the operating system re-broadcasts the [ACTION_HDMI_AUDIO_PLUG][ACTION_HDMI_AUDIO_PLUG] intent with modified encoding extra values.
    
    Since the audio headset via a game controller or Bluetooth headset do not support Dolby pass-through, the encoding extra value would now be [ENCODING_PCM_16BIT][ENCODING_PCM_16BIT] only. The app must register to receive this intent during playback and re-configure the audio pipeline to do one of the following:
    
    *  Switch to an alternate non-Dolby audio content mid-stream (for example AAC)
    *  If an alternative audio content is not available, re-configure the audio pipeline to use the MediaCodec Dolby decoder to decode the Dolby content to Stereo PCM. In case of Fire TV Stick, re-creation of the MediaCodec is required as part of re-creating the audio pipeline.
    
    {% include note.html content="The first option (switching to non-Dolby audio) is highly recommended because the Dolby decoder in the device always down-mixes multi-channel output to stereo internally, regardless of the endpoint's multi-channel capabilities. As a result, there is no advantage of playing the 5.1 Dolby stream over stereo with a non-Dolby stream. Additionally, the non-Dolby audio stream (such as AAC) consumes less bandwidth than the multi-channel Dolby audio stream." %}

[AudioTrack]: http://developer.android.com/reference/android/media/AudioTrack.html
[AudioManager]: http://developer.android.com/reference/android/media/AudioManager.html
[ACTION_HDMI_AUDIO_PLUG]: https://developer.android.com/reference/android/media/AudioManager.html#ACTION_HDMI_AUDIO_PLUG
[EXTRA_ENCODINGS]: https://developer.android.com/reference/android/media/AudioManager.html#EXTRA_ENCODINGS
[ENCODING_PCM_16BIT]: https://developer.android.com/reference/android/media/AudioFormat.html#ENCODING_PCM_16BIT
[ENCODING_AC3]: https://developer.android.com/reference/android/media/AudioFormat.html#ENCODING_AC3
[ENCODING_E_AC3]: https://developer.android.com/reference/android/media/AudioFormat.html#ENCODING_E_AC3
[findDecoderForFormat]: https://developer.android.com/reference/android/media/MediaCodecList.html#findDecoderForFormat(android.media.MediaFormat)
[MediaCodec_APIs]: http://developer.android.com/reference/android/media/MediaCodec.html)
[getPlaybackHeadPosition]: https://developer.android.com/reference/android/media/AudioTrack.html#getPlaybackHeadPosition()
[getTimeStamp]: https://developer.android.com/reference/android/media/AudioTrack.html#getTimestamp(android.media.AudioTimestamp)
[ENCODING_XXXX]: https://developer.android.com/reference/android/media/AudioFormat.html

{% include links.html %}
