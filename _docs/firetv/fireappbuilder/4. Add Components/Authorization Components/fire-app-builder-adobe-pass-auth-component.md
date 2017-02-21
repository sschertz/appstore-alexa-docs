---
title: Adobe Pass Authorization Component
permalink: fire-app-builder-adobe-pass-auth-component.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

Adobe Primetime (formerly Adobe Pass) provides an authentication mechanism that requires users to log in prior to viewing media. Users sign in to their ISP or content provider, and those credentials then authenticate them with the app. You can learn more at [Adobe Primetime here](http://www.adobe.com/marketing-cloud/primetime-tv-platform.html).

{% include note.html content="Previously, when the component was coded, \"Adobe Primetime\" was called \"Adobe Pass.\" The name Adobe Pass is still used in the code, and the component's name is AdobePassAuthComponent, so you will see the terms Adobe Pass and Adobe Primetime used somewhat interchangeably in the documentation." %}

* TOC
{:toc}

## The User Experience with Adobe Pass

This example shows a configuration of Adobe Pass/Primetime with a sample app.

When users click the Watch Now button on the Content Details screen, they're greeted by an Adobe Primetime Login prompt:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepassprompt" type="png" caption="Adobe Primetime login prompt" %}

Users open a browser on their computer and go to the indicated URL (in this example, www.example.com/amazon/firetv) to enter the registration code. The user also signs in to their cable provider.

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepassbrowserlogin" type="png" caption="Authenticating with Adobe Pass requires users to sign in to their cable provider." %}

After the user enters the registration code and cable provider credentials, the user is logged in and sees a success screen similar to this one. (You configure these URLs and screens through your Adobe Primetime account.)

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepassloginsuccess" type="png" caption="Login success." %}

Now the user turns back to the Fire TV and clicks the **Submit** button to log in. The app now lets the user watch media.

If the login is unsuccessful, the user sees an error message on the screen indicating what went wrong:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepasserror" type="png" caption="Possible error messages screen from an unsuccessful Adobe Primetime login attempt." %}

## Configuring the Adobe Pass Auth Component

To configure the Adobe Pass Component, follow these five steps:

*  [Step 1. Configure the Adobe Pass Auth Component in Your App](#configurecomponent)
*  [Step 2. Encrypt Your Adobe Pass Keys](#generatingyourkeys)
*  [Step 3. Configure the Strings for the Adobe Pass Login Prompt](#configurestrings)
*  [Step 4. Customize the Styles for the Adobe Pass Screens](#customizestyles)
*  [Step 5. Configure Which Screen to Prompt Users to Log in](#configurescreens)

## Step 1. Configure the Adobe Pass Auth Component in Your App {#configurecomponent}

The Adobe Pass Auth Component provides three separate groups of files you can customize that configure the Adobe Pass information and the Fire TV user interface that users see.

To configure the Adobe Pass Authorization Component:

1.  Load the Adobe Pass Authorization component into your app. See [Load a Component in Your App][fire-app-builder-load-a-component] for details about how to load a component into your app.
2.  Remove any other authentication components that are loaded in your app (such as FacebookAuthComponent or LoginWithAmazonComponent). See [Remove a Component][fire-app-builder-load-a-component#removeacomponent] for details.

    {% include note.html content="You can load only *one* component per interface. For example, you cannot load both the Adobe Pass Auth component *and* the Pass Through Login component, because both components use the same `IAuthentication` interface. For a list of the components by interface, see [Components Overview][fire-app-builder-interfaces-and-components]." %}

3.  Go to **AdobePassAuthComponent > res > values** and open the **custom.xml** file.
4.  Copy the following values and paste them into your app's **custom.xml** file:

    ```xml
    <!--Adobe pass clientless API Requestor ID -->
    <string name="adobe_pass_requestor_id">YOUR REQUESTOR ID</string>
    <!-- Encrypted Adobe pass public key for your application, encrypt it using
    KeyEncrypterStandaloneUtility -->
    <string name="encrypted_adobe_pass_public_key">YOUR ENCRYPTED PUBLIC KEY</string>
    <!-- Encrypted Adobe pass secret key for your application, encrypt it using
    KeyEncrypterStandaloneUtility -->
    <string name="encrypted_adobe_pass_private_key">YOUR ENCRYPTED PRIVATE KEY</string>
    <!--Adobe pass clientless API registration URL for second screen login -->
    <string name="adobe_pass_registration_url">YOUR REGISTRATION URL</string>
    <!--Adobe pass clientless API time to live value for the registration token -->
    <string name="adobe_pass_registration_code_ttl">YOUR TIME TO LIVE VALUE</string>
    <!-- URL used by user to authenticate -->
    <string name="adobepass_login_instruction_line_2">Visit YOUR_AUTHENTICATION_URL</string>
    <!-- PseudoRandom strings, used to generate random key for encrypting/decrypting resources.
    These keys should always remain in sync with the keys used by encrypting utility -->
    <string name="random_key_1">random_key_1</string>
    <string name="random_key_2">random_key_2</string>
    <string name="random_key_3">random_key_3</string>
    <string name="random_key_4">random_key_4</string>
    ```

5.  Customize the values for each property as explained in the following table:

    <table class="grid">
    <colgroup>
    <col width="40%" />
    <col width="60%" />
    </colgroup>
      <thead>
        <tr>
          <th>Value</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><code>adobe_pass_requestor_id</code></td>
          <td>Your Adobe Primetime requestor ID. This value is provided by Adobe.</td>
        </tr>
        <tr>
          <td><code>encrypted_adobe_pass_public_key</code></td>
          <td>Your encrypted Adobe Primetime public key (provided by Adobe). Although this key is provided to you by Adobe, you must encrypt it. See <a href="#generatingyourkeys">Encrypt Your Adobe Primetime Keys</a> for details on generating this key.</td>
        </tr>
        <tr>
          <td><code>encrypted_adobe_pass_private_key</code></td>
          <td>Your encrypted Adobe Primetime private key. Although this key is provided to you by Adobe, you must encrypt it. See <a href="#generatingyourkeys">Encrypt Your Adobe Primetime Keys</a> for details on generating this key.</td>
        </tr>
        <tr>
          <td><code>adobe_pass_registration_url</code></td>
          <td>The registration URL. This value is provided by Adobe.</td>
        </tr>
        <tr>
          <td><code>adobe_pass_registration_code_ttl</code></td>
          <td>How long until the registration code expires.</td>
        </tr>
        <tr>
          <td><code>adobepass_login_instruction_line_2</code></td>
          <td>Information about where users go to log in.</td>
        </tr>
        <tr>
          <td><code>random_key_1</code></td>
          <td>A random string used in encrypting the public and private keys. Type any alphanumeric string for the value.</td>
        </tr>
        <tr>
          <td><code>random_key_2</code></td>
          <td>A random string used in encrypting the public and private keys. Type any alphanumeric string for the value.</td>
        </tr>
        <tr>
          <td><code>random_key_3</code></td>
          <td>A random string used in encrypting the public and private keys. Type any alphanumeric string for the value.</td>
        </tr>
        <tr>
          <td><code>random_key_4</code></td>
          <td>A random string used in encrypting the public and private keys. Type any alphanumeric string for the value.</td>
        </tr>
      </tbody>
    </table>

## Step 2. Encrypt Your Adobe Primetime Keys {#generatingyourkeys}

When you set up an Adobe Primetime account, you're provided with a public and private key. To keep these values secure, the Adobe Pass Component in Fire App Builder encrypts the keys with a security algorithm. The algorithm is implemented through the `ResourceObfuscator` and `ResourceObfuscationStandaloneUtility` classes in your app's Utils folder.

To encrypt your Adobe Primetime public and private key:

1.  In the Android View, expand the **Utils > java > com > amazon > utils > security** folder and open the **ResourceObfuscationStandaloneUtility** class.
2.  In the `getRandomStringsForKey()` method, enter the values you used for `random_key_1`, `random_key_4`, and `random_key_3` (in the component's custom.xml file) respectively.

    For example, suppose you used the following random strings in your custom.xml file:

    ```xml
    <string name="random_key_1">calypso</string>
    <string name="random_key_2">dadadadadappppp</string>
    <string name="random_key_3">more_random_stuff</string>
    <string name="random_key_4">something_random</string>
    ```
    You would customize the strings in the `ResourceObfuscationStandaloneUtility` class as follows:

    ```java
    private static String[] getRandomStringsForKey() {

        return new String[]{
                "calypso",
                "something_random",
                "more_random_stuff"
        };
    }
    ```

    In this example, the values are as follows:

    *  `calypso` is the value used for `random_key_1`.
    *  `something_random` is the value used for `random_key_4`.
    *  `more_random_stuff` is the value used for `random_key_3`.

4.  In the `getRandomStringsForIv()` method, enter the values you used for `random_key_2` and `random_key_3` respectively:

    ```java
        private static String[] getRandomStringsForIv() {

            return new String[]{
                    "dadadadadappppp",
                    "more_random_stuff"
            };
        }
    }
    ```

    In this example, the values are as follows:

    *  `dadadadadappppp` is the value used for `random_key_2`.
    *  `more_random_stuff` is the value used for `random_key_3`. (Same as before.)

5.  In the `getPlainTextToEncrypt()` method, insert your Adobe Pass public key in place of `Encrypt_this_text`:

    ```java
     private static String getPlainTextToEncrypt() {
            return "Encrypt_this_text";
        }
    ```

6.  Right-click the **ResourceObfuscationStandaloneUtility.java** file and select **Run 'ResourceObfusc...main()**.


7.  Look for the encrypted result printed to the console. It will look something like this:

    ```
    Encrypted version of plain text 123456789 is gnobHJEIxnkBMobJk7mBaQ==
    ```

8.  Copy your encrypted key. Paste this key into the `encrypted_adobe_pass_public_key` value in your app's **custom.xml** file (as per the instructions in the previous section). For example:

    ```xml
    <string name="encrypted_adobe_pass_public_key">gnobHJEIxnkBMobJk7mBaQ==</string>
    <string name="encrypted_adobe_pass_private_key">YOUR ENCRYPTED PRIVATE KEY</string>
    ```

9.  Now insert your Adobe Pass *private* key into the `getPlainTextToEncrypt()` method, and run the script again (using the same random strings). Copy the encrypted key into the `encrypted_adobe_pass_private_key` string value in your app's **custom.xml** file. For example:

    ```xml
    <string name="encrypted_adobe_pass_public_key">gnobHJEIxnkBMobJk7mBaQ==</string>
    <string name="encrypted_adobe_pass_private_key">bhjKDUYhdlkNNbUEYyvbn==</string>
    ```

    {% include tip.html content="Save your random keys in a safe place, such as your company wiki, so that you always have an easy way to retrieve them." %}

### Encrypting Other Values

The encryption utility can be used for any keys you want to encrypt in your app, not just for Adobe Primetime keys. You can use the `ResourceObfuscatorStandaloneUitility` class to encrypt the keys and the `ResourceObfuscator` class to decrypt keys.

The Adobe Pass Component already leverages the `ResourceObfuscator` class to decrypt the keys. The random strings you entered (in the custom.xml file in the component) get passed into the `ResourceObfuscator` class, which does the encryption. The AdobepassRestClient.java class in the Adobe Pass Auth Component instantiates this `ResourceObfuscator` class and passes in your random strings:

```java
  ResourceObfuscator obfuscator = new ResourceObfuscator();

        String plainKey = obfuscator.unobfuscate(key, getRandomStringsForKey(appContext),
                                                 getRandomStringsForIv(appContext));
        return plainKey;
    }
```

Note that this encryption technique is not hack proof. There are stronger methods of encryption. However, the algorithm does help prevent malicious users from easily finding and using your keys.

## Step 3. Configure the Strings for the Adobe Primetime Login Prompt {#configurestrings}

You can configure the strings that appear in the Adobe Primetime Login prompt screen:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepassprompt" type="png" caption="Adobe Primetime login prompt" %}

You can also control the text in the error messages screen that appears if the login fails:

{% include image.html file="firetv/fireappbuilder/images/fireappbuilder_adobepasserror" type="png" caption="Adobe Pass error messages" %}

To customize the text on these screens:

1.  Go to **AdobepassAuthComponent > res > values** and open the **strings.xml** file.
2.  Customize the string values for each of the elements.

    ```xml
    <string name="app_name">AdobepassAuthComponent</string>

       <string name="title_activity_adobe_authentication">Authentication</string>
       <string name="adobepass_login_instruction_line_1">Go to your computer or mobile device</string>
       <string name="adobepass_login_instruction_line_3">Enter the following case-sensitive code:</string>
       <string name="adobepass_login_instruction_line_4">Loading...</string>
       <string name="btn_submit">SUBMIT</string>
       <string name="btn_get_new_code">GET NEW CODE</string>
       <string name="adobe_pass_error_authentication_message">There was an error authenticating the account. Please try again later.</string>
       <string name="adobe_pass_error_registration_message">There was an error authenticating the account. Please try again later.</string>
       <string name="adobe_pass_no_authorization_message">Your subscription package does not include this video.
    ```

## Step 4. Customize the Styles for the Adobe Pass Screens {#customizestyles}

You can customize the logo and colors of the Adobe Primetime login user interface in your app.

To customize the styles:

1.  Go to **AdobepassAuthComponent > res > values** and open the **styles.xml** file.
2.  Customize the string values for each of the elements. See the preceding screenshots to see how the element names and text affects the display.

    ```xml
        <style name="AppTheme" parent="android:Theme.Holo.Light.DarkActionBar">
        </style>

        <drawable name="company_logo">@drawable/logo</drawable>
        <drawable name="splash_background">@drawable/bg_generic_nopreview</drawable>
        <drawable name="action_button_focused">@drawable/btn_generic_focused</drawable>
        <color name="action_button_text_color">#E6FFFFFF</color>
        <color name="action_button_text_color_focused">#E6FFFFFF</color>
        <drawable name="action_button_normal">@drawable/btn_normal</drawable>
    ```

## Step 5. Configure Which Screen to Prompt Users to Log in {#configurescreens}

You need to configure which screens should implement the authentication. For example, you might want to require authentication only for the `PlaybackActivity` on the Content Renderer screens, so that unauthenticated users can be enticed by the media in your app and be motivated to sign in.

To configure which screens require authentication:

1.  Open the **Navigator.json** file (located in **app > assets**).
2.  In the `graph` object, locate the activity you want to restrict (such as `PlaybackActivity`), and change `verifyScreenAccess` to true. For example:

    ```json
    "com.amazon.android.uamp.ui.PlaybackActivity": {
      "verifyScreenAccess": true,
      "verifyNetworkConnection": true,
      "onAction": "CONTENT_RENDERER_SCREEN"
    }
    ```

{% include links.html %}
