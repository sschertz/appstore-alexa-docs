---
title: Set Up the Category Recipe
permalink: fire-app-builder-set-up-recipes-categories.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

In this step, you will query for the categories from your feed. Categories group your media into different collections or channels. It's a way of organizing your media so that all your video isn't in one long list. For a general overview of recipe configuration, see [Recipe Configuration Overview][fire-app-builder-set-up-recipes-overview].

* TOC
{:toc}

## Configure the Category Recipe

1.  Open the **LightCastCategoriesRecipe.json** file (located in **app > assets > recipes**).
2.  Configure the file's values as explained in the following table. If parameters need more explanation, the sections below the table provide more detail.

    <table class="grid">
    <colgroup>
    <col width="20%" />
    <col width="80%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Key</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td markdown="1">`cooker`
    </td>
    <td markdown="1">{% include_relative recipe_cooker.md %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`format`
    </td>
    <td markdown="1">{% include_relative recipe_format.md %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`translator`
    </td>
    <td markdown="1">{% include_relative recipe_translator.md default_value="`ContentContainerTranslator`" %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`model`
    </td>
    <td markdown="1">Specifies the content model for the data. The content model provides the structure for your content and maps it into the Fire App Builder UI. Leave it at the default: `com.amazon.android.model.content.ContentContainer`. 
    </td>
    </tr>
    <tr>
    <td markdown="1">`modelType`
    </td>
    <td markdown="1">{% include_relative recipe_modeltype.md %}
    </td>
    </tr>
    
    <tr>
    <td markdown="1">`query`
    </td>
    <td markdown="1">{% include_relative recipe_query.md type="categories" %} 
    </td>
    </tr>
    
    <tr>
    <td markdown="1">`queryResultType`
    </td>
    <td markdown="1">{% include_relative recipe_queryresulttype.md %}
    </td>
    </tr>
    
    <tr>
    <td markdown="1">`matchList`
    </td>
    <td markdown="1">{% include_relative recipe_matchlist.md %}
    </td>
    </tr>
    <tr>
    <td markdown="1">`keyDataType` <br/>
    </td>
    <td markdown="1">The media objects that are related to the category. It's essential to identify which media should be grouped into which category. Similar to `matchList`, you target the items on the left followed by `@` and then `keyDataPath` to map and identify these media objects to the category. For example: `StringKey@keyDataPath`. See [keyDataType Parameter](#keydatatypeparameter) for more details.
    </td>
    </tr>
    </tbody>
    </table>

### query Parameter {#queryparameter}

The syntax you use differs between JSON and XML. See the section that is relevant to your feed:

* [JSON Feeds](#categoryjson)
* [XML Feeds](#categoryxml)

#### JSON Feeds {#categoryjson}

The sample app in Fire App Builder reads from a generic LightCast media feed that uses a JSON format. The Categories recipe uses the following value for the `query` parameter to return a list of categories from your feed: `$..categories[*]`. This is [Jayway JsonPath](https://github.com/jayway/JsonPath) syntax. Here's what this syntax means:

| Query Syntax | What It Matches |
|----|----|
| `$` | Specifies the root directory as the beginning of the search.| 
| `..` | Indicates a recursive search in every directory and subdirectory of the root for matches.| 
| `categories[]` | Says to look for the named array called “categories”.| 
| `*` | Matches any contents (wildcard).| 

Putting it all together: `$..categories[*]` Starts at the root (`$`), looks in every directory and subdirectory recursively for matches (`..`), and looks to match on a named array called `categories`, with any contents in the array (`*`).

You can test your queries using the [Jayway JsonPath Evaluator](http://jsonpath.herokuapp.com/).

With the sample app in Fire App Builder, if you run this query (`$..categories[*]`) against one of the [LightCast feed URLs](http://www.lightcast.com/api/firetv/channels.php?app_id=263&app_key=4rghy65dcsqa&action=channels_videos), the result returns the category names as strings, like this:

```
[
   "Jamaican Attractions",
   "The Country Jamaica",
   "Jamaican Attractions",
   "The Country Jamaica",
   "The Country Jamaica",
   "The Country Jamaica",
   "Jamaican Attractions",
   "The Country Jamaica",
   "Jamaican Attractions",
   "The Country Jamaica",
   "Jamaican Attractions",
   "Jamaican History",
   "The Country Jamaica",
   "Jamaican Attractions",
   "Jamaican History",
   "Jamaican History",
   "The Country Jamaica",
   "Jamaican Attractions",
   "Jamaican History",
   "The Country Jamaica",
   "Jamaican Attractions",
   "Jamaican History",
   "Jamaican History",
   "Jamaican History",
   "The Country Jamaica",
   "Jamaican Wildlife",
   "Jamaican Attractions",
   "The Country Jamaica",
   "Jamaican Wildlife",
   "The Country Jamaica",
   "Jamaican Attractions",
   "Jamaican Wildlife",
   "Jamaican Attractions",
   "Jamaican Wildlife"
]
```

(Fire App Builder will remove duplicates from the query result.)

Fire App Builder needs to take the array of objects and convert the array to a hashmap to process in a Java class. So the next parameter (`queryResultType`) is used to convert the array of strings into an array of objects. 

```
"queryResultType": "[]$",
```

See [Querying JSON][fire-app-builder-querying-json] for more details about constructing Jayway JsonPath queries. See also [Jayway JsonPath](https://github.com/jayway/JsonPath) for more details about Jayway JsonPath in general.

#### XML Feeds {#categoryxml}

If your feed is XML, instead of using Jayway JsonPath, you must use [XPath expressions](http://www.w3schools.com/xsl/xpath_syntax.asp) to target the specific elements in your feed. XPath reduces your XML document into various items called "nodes." The XPath syntax targets the location of specific nodes. 

Suppose your XML feed contains a structure like this:

```xml
<rss>
    <channel>
        <item>
        <title>Sample Title</title>
        <pubDate>Wed, 26 Oct 2016 20:34:22 PDT</pubDate>
        <link>https://example.com/myshow/episodes/110</link>
        <author>Sample Author name</author>
        <category>Technology</category>
        <category>Gadgets</category>
        </item>
    </channel>
</rss>
```

Here's a sample XPath expression to get the categories in this XML feed:

```
//category/text()
```

The `//` looks recursively in all nodes for matches, no matter where those nodes are located in the hierarchy. `category` selects the category node, and `text()` selects the text from that node.

You can test this out using the [XPath Tester/Evaluator](http://www.freeformatter.com/xpath-tester.html). The response from the expression is as follows:

```
Text='Technology'
Text='Gadgets'
```

(You can ignore the `Text=` part. This is just part of the XPath Tester/Evaluator's display, not what was matched in the query.)

Note that with iTunes feeds, there's a general category for the feed (such as `<itunes:category text="Technology">`) as well as categories for each item (`<category>Technology</category>`). When you target categories for your recipe, you want to target the categories for each item in the feed, not the general feed categories. 

Try copying your XML feed into the [XPath Tester/Evaluator](http://www.freeformatter.com/xpath-tester.html) and selecting the categories using a similar syntax.

See [Querying XML][fire-app-builder-querying-xml] for more examples showing XPath expressions. See also [XPath syntax](http://www.w3schools.com/xsl/xpath_syntax.asp) and the examples section on [XPath Examples](http://www.freeformatter.com/xpath-tester.html#xpath-examples) for more details about XPath expressions in general.

{% include note.html content="If individual items in your feed are associated with multiple categories, your app's display will show those items in each category they're associated with. For example, if your Cool Video appears in both Technology and Gadgets categories, this video will appear in two places in your app's display." %}

### matchList Parameter {#matchlistparameter}

The purpose of the `matchList` parameter is to select specific properties from the category query result and map them to the Fire App Builder content model.

The syntax used by `matchList` is not Jayway JsonPath or XPath expressions but rather custom Fire App Builder syntax that targets specific elements in the query result. (Hence the JSON and XML instructions are combined in the same sections.)

In the sample app in Fire App Builder, the value for the Categories recipe is `StringKey@name`. 

Here's how this syntax works. On the left of the ampersand (`@`) you put the property you want to target in the query result (`StringKey` selects the list of strings). On the right of the ampersand (`@`), you put the Fire App Builder element you want to map the property to (`name`). 

For the **Categories recipe**, your `matchList` parameter should map your feed's categories to `name`.
 
In the Fire App Builder sample app, since the query result is a list of strings, `StringKey` is used to match the strings. But suppose the result set from your query contained a JSON object such as the following:

```json
"list": { "title" : "My category title" }
```

To match on `My category title` and convert it to `name`, you would use the following:
 
```
list/title@name
```

Use the forward slash (`/`) to go deeper in object levels (just like with with XPath). In this case, `title` is one object below `list`, which is one object below the root. After moving past these two levels, the result is simply the category name.

To match XML elements, you follow a similar technique. Supposing the result is a list of strings:

```
Text='Technology'
Text='Gadgets'
```

To match the text contents and map them to the category element in the Fire App Builder UI, you would use the following syntax:

```xml
StringKey@name
```

If your query result looked like this:

```xml
<list>
<title>My category title</title>
</list>
```

Then you would map the category title like this:

```
/list/title@name
```

The forward slash (`/`) takes you one level deeper in the XML nodes.

### keyDataType Parameter {#keydatatypeparameter}

Fire App Builder needs to know which media items should be grouped with the selected categories. The `keyDataType` parameter identifies the media items that are related to a category. This parameter is used for the Category recipe and later passed into a variable in the Contents recipe.  

The Fire App Builder query result is a list of strings, so `StringKey@keyDataPath` is used to target the media items and associate them with the category. If your result is also a list of strings, then you would use the default:

```
"keyDataType": "StringKey@keyDataPath"
```

However, suppose your media objects were listed inside an `assets` node that in turn was nested inside a `container` node, like this:

```json
"container": {
    "assets": [
    "5825652561001",
    "5825652558001",
    "5825652569001",
    "5873045223001",
    ]
}
```

To get the media objects, you would write the query like this: 

```
container/assets@keyDataPath
```

Similar to the `matchList` parameter, the `keyDataType` query does not use Jayway JsonPath syntax either. Instead, you match each node by writing the node name followed by `/` to move into the next level. `container/assets` matches all the items at this level. 

On the right of the ampersand `@`, the `keyDataPath` key is how these media objects are stored and used by Fire App Builder. The `@keyDataPath` helps match up the items with the Fire App Builder content model.

## Feeds without Categories

If your feed lacks categories but you have separate feeds for each category, you can hard-code the category names when you configure Navigator.json. This will group all the videos in a particular feed with a particular category. More detail for this approach is provided in the ["Hardcoding Your Categories."][fire-app-builder-configure-navigator-open-feeds#hardcodingcategories]

## Next Steps

Now that you've configured the categories for your app's media feed, you need to configure the contents. See [Set Up your Content Recipe][fire-app-builder-set-up-recipes-content].

{% include links.html %}

