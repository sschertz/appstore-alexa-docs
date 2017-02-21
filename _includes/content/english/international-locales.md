Fire devices have a default locale with a region and language that you can query programmatically. You can increase the reach of your app and improve the experience for users in different locales by providing resources that are not only designed for a specific device, but are also responsive to locale. For example, you may already provide different bitmaps for your app based on the pixel density of the device running the app.

You can extend this model and provide different bitmaps for each locale. When you put your resources for each locale in the correct folders, the system finds the appropriate resources at runtime. Creating resource files in this way is a preferred method of localizing your app.

* TOC
{:toc}

## Strings Resources

The `strings.xml` file holds the user-viewable strings for your app. The following is a sample definition in `strings.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
	<resources>
  		<string name="hello">Hello!</string>
	</resources>
```

In your source code, you can use the following to reference the string resource named "hello":

```java
String helloText = getString(R.id.hello);
```

In your XML files such as layout.xml or AndroidManifest.xml, you can reference the resource by doing the following:

```java
<application android:label="@string/hello" >
```

To vary the strings by locale, create a `strings.xml` file that contains the non-default resources for the locale. Put the file in a directory named `values-xx-rYY`, where `xx` is the ISO-639 language code and `YY` is the ISO-3166-1 region code. The following shows several example directories.

```java
	/res
	/values         (default directory, make sure all references are present)
	/values-fr      (contains French language strings, region not used)
	/values-de      (contains German language strings, region not used)
	/values-en-rGB  (contains English language strings for Great Britain)
```

When the system looks for a string reference, it looks first for a resource that is specific to a region and language. It then tries to match by language. The system falls back to the default `strings.xml` for the resources that you do not specify in the locale-specific file. For example, if you do not put a "hello" string in `values-en-rGB`, the system falls back to using the default "hello" string.

{% include important.html content="Make sure that the default directory includes all string references. If the system fails to find a reference after searching the default directory, the system forces your app to close." %}

## Drawable Resources

Many apps have menus, price lists, or instructions written as bitmaps or other graphical data. The following example shows a folder structure for providing dynamic resource handling for drawable resources.

```java
/res
  /drawable
  /drawable-fr      (contains French language strings, region not used)
  /drawable-de      (contains German language strings, region not used)
```

If you have several drawable directories with resources based on pixel density, you can extend the directory structure to account for language. The following example shows how you can chain together language and pixel density. When you specify multiple qualifiers in a directory name, you must do so in the order listed in the Android documentation in [this table](http://developer.android.com/guide/topics/resources/providing-resources.html#table2).

```java
  /drawable-fr-ldpi
  /drawable-fr-mdpi
```

To reuse a drawable asset, for example to put copies of the same bitmap in several folders, you can create an XML file to link to the asset. Suppose you want the resource named "background" in the Great Britain locale to point to a resource in the default `drawable` directory. You can create a file `/drawable-en-rGB/background.xml` with the following contents. Any reference to "background" that resolves to the `drawable-en-GB` directory automatically uses the resource `/drawable/background_common`.

```xml
<?xml version="1.0" encoding="utf-8"?>
  <bitmap xmlns:android="http://schemas.android.com/apk/res/android"
        android:src="@drawable/background_common" />
```

## Currency Resources

You can format currency data by locale. Important elements are the currency symbol and the decimal divider, as shown in the following example:

```java
  €19,95   // in some European locales
  $19.95   // in North America
```

The following example shows how to display the price of [In-App Purchasing](https://developer.amazon.com/in-app-purchasing) items in local format. To fetch a price for an item from the regional store in your app, do the following. This applies formatting only, and does not exchange currency.

```java
NumberFormat nf = NumberFormat.getCurrencyInstance(Locale.getDefault());
  String formattedPrice = nf.format(19.99f);
```

If you decide to use a locale other than the default, make sure you define it using both language and region, for example `en_US`, or `fr_FR`. Otherwise, you may not get the correct formatting. For example, not all countries that use French as primary language have the same currency.
