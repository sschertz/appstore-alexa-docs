The Advertising ID is a user-resettable, unique, anonymous identifier that can be used for advertising and user analytics. Users can reset the Advertising ID or opt out of tracking for interest-based ads altogether. The Advertising ID is currently available for tablet devices running Fire OS 5.1 and later and TV devices running Fire OS 5.2.1.1 and later.
{% if include.device == "firetablets" %}
On Fire Tablets, users can manage the Advertising ID by going to **Settings > Apps & Games > Advertising ID.**
{% endif %}

{% if include.device == "firetv" %}
On Fire TV, users can manage the Advertising ID by going to **Settings > System > Advertising ID**.
{% endif %}

* TOC
{:toc}

## When to Use the Advertising ID 

You should use the Advertising ID (when supported by the device) if your app does the following:

*  Displays ads
*  Collects data for user analytics
*  Collects data to build user profiles for advertising purposes or for targeting users with interest based advertising

When available, use the Advertising ID over any other ID. Ensure that if you use a third-party ad service in your app, the service follows the requirements around the user's choice.

## Developer Expectations 

When you use the Advertising ID, follow these principles:

*  Use the Advertising ID only for advertising and user analytics.
*  When the user opts out of interest-based ads, the Advertising ID is still available but the developer must honor the user's opt-out choice. Do not collect information about the user's behavior to build user profiles for advertising purposes or show them interest based ads. In your code, precede any call to retrieve the Advertising ID with a call that verifies the user's opt-out choice. Allowed activities include contextual advertising, frequency capping, conversion tracking, reporting, and security and fraud detection.
*  Do not associate any persistent device identifier or personally-identifiable information with the Advertising ID unless the user has explicitly given consent to do so.
*  When a user resets the Advertising ID, do not fold earlier data into the new Advertising ID or associate the new ID with the old one unless the user has explicitly given consent for you to do so.

For more information, see the [App Distribution and Services Agreement](https://developer.amazon.com/public/support/legal/da#advertising_Id_schedule).

## Verifying and Responding to the User's Advertising ID Choices

The Android [*Settings.Secure*](http://developer.android.com/reference/android/provider/Settings.Secure.html) class exposes the user's Advertising ID choices through the [*getInt*](http://developer.android.com/reference/android/provider/Settings.Secure.html#getInt%28android.content.ContentResolver,%20java.lang.String%29) and [*getString*](http://developer.android.com/reference/android/provider/Settings.Secure.html#getString%28android.content.ContentResolver,%20java.lang.String%29) methods. The following Java example shows the logic for verifying the user setting and retrieving an Advertising ID (if available).

```java
import android.content.ContentResolver;
import android.provider.Settings.Secure;
import android.provider.Settings.SettingNotFoundException;

String advertisingID = "";
boolean limitAdTracking = false;

try {
    ContentResolver cr = getContentResolver();

    // get user's tracking preference
    limitAdTracking = (Secure.getInt(cr, "limit\_ad\_tracking") == 0) ? false : true;

    // get advertising
    advertisingID = Secure.getString(cr, "advertising\_id");
} catch (SettingNotFoundException ex) {
    // not supported

}
```

The code first gets the user's ad tracking preference. Then depending on the ad tracking value, the following happens:

{% if include.device == "firetablets" %}

*  If the user has allowed ad tracking, the value for `limit_ad_tracking` will be `false`. 
*  If the user has disabled ad tracking, the value for `limit_ad_tracking` will be `true`. 
*  The Advertising ID gets stored in the `advertisingID` variable. A sample Advertising ID value might be `df07c7dc-cea7-4a89-b328-810ff5acb15d`. (For child profiles, the `advertisingID` will be `00000000-0000-0000-0000-00000000000`.)
*  If the system doesn't return any value for `limit_ad_tracking` (such as for non-Fire-OS devices or Fire Devices running older versions of FireOS), a `SettingNotFoundException` gets thrown. You can handle this exception as desired.

{% endif %}

{% if include.device == "firetv" %}

*  If the user has allowed ad tracking, the value for `limit_ad_tracking` will be `false`. 
*  If the user has disabled ad tracking, the value for `limit_ad_tracking` will be `00000000-0000-0000-0000-000000000000`. (Note: This is a bug. In the future, the value will be `true`, similar to the response for Fire tablets. ) 
*  The Advertising ID gets stored in the `advertisingID` variable. A sample Advertising ID value might be `df07c7dc-cea7-4a89-b328-810ff5acb15d`. (For child profiles, the `advertisingID` will be `00000000-0000-0000-0000-00000000000`.)
*  If the system doesn't return any value for `limit_ad_tracking` (such as for non-Fire-OS devices or Fire Devices running older versions of FireOS), a `SettingNotFoundException` gets thrown. You can handle this exception as desired.
*  When a user toggles the interest based opt-out value (from enabled to disabled or vice-versa), the Advertising ID is set to a new value. (Note: This is a bug. In the future, the same Advertising ID will be retained, similar to how it works on Fire tablets.)

{% endif %}