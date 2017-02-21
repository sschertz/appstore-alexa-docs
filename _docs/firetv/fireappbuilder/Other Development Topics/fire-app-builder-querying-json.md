---
title: Querying JSON
permalink: fire-app-builder-querying-json.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

When you [set up your recipes][fire-app-builder-set-up-recipes-overview], you used Jayway JsonPath syntax for the `query` parameter. You can learn more about the JSON query syntax here: [Jayway JsonPath](https://github.com/jayway/JsonPath).

To get a better sense of how the query syntax words, you can use the [Jayway JsonPath Evaluator](http://jsonpath.herokuapp.com/). Specifically, see the [Operators](https://github.com/jayway/JsonPath#operators) section to understand how the various syntax (`$, @, *, .., .<name>, [?(<expression>)]`) targets specific elements in your JSON.

The following sections provide examples about how to construct Jayway JsonPath queries.

* TOC
{:toc}

### JSON Path Query Example 1

In this example, we'll step through some Jayway JsonPath queries in more detail. 

1.  Open the **urlFile.json** (in app > assets).
2.  Copy the first URL:
    
    ```html
    http://www.lightcast.com/api/firetv/channels.php?app_id=257&app_key=0ojbgtfcsq12&action=channels_videos
    ```
    
3.  Paste this URL into a browser and press **Return** to see the contents.
4.  Copy the contents and prettify the JSON using a tool such as [jsonprettyprint.com](http://jsonprettyprint.com/).
5.  Copy the prettified JSON and paste it into the [Jayway JsonPath Evaluator](http://jsonpath.herokuapp.com/).
6.  In the **Go!** box, enter the query to get all categories:

    ```
    $..categories[*]
    ```
    
7.  Click **Go!**

    The result shows all the categories from the feed. 
    
    ```json
    [
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Underwater",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Attractions",
       "Bahamas Islands",
       "Bahamas Attractions",
       "Bahamas Underwater",
       "Bahamas Islands",
       "Bahamas Attractions",
       "Bahamas Islands",
       "Bahamas Islands",
       "Bahamas Attractions",
       "Bahamas Weddings",
       "Bahamas Weddings",
       "Bahamas Attractions",
       "Bahamas Underwater",
       "Bahamas Islands",
       "Bahamas Attractions",
       "Bahamas Islands",
       "Bahamas Weddings",
       "Bahamas Islands",
       "Bahamas Underwater"
    ]
    ```
    
    With this particular feed, each media object lists its category, so the query shows numerous "Bahamas Islands" results (one for each media object containing this category).

    {% include image.html file="firetv/fireappbuilder/images/fireappbuilder_jsonpath_evaluator" type="png" alt="JsonPath Evaluator example" caption="Extracting all categories from a feed using Jayway JsonPath query syntax." %}

    The syntax `$..categories[*]` means to start in the root directory (`$`) and look in all subdirectories (`..`) for the key `categories`. This key must contain an array (`[]`), but the array can containing anything (`*`).

    If you were to switch the query to `$..title[*]`, nothing would match because title isn't a key containing an array in the feed. To retrieve all titles, you would use `$..title`.
    
    The DynamicParser class in the Fire App Builder code will remove duplicates when putting these categories into a map.

    {% include tip.html content="See the [Path Examples](https://github.com/jayway/JsonPath#path-examples) in the JsonPath documentation to get a better sense of the variety of queries you can use to target specific elements in your feed." %}
    
### JSON Path Query Example 2

Here's another potential feed structure for media:


```json
{  
   "total":"13",
   "results":[  
      {  
         "id":"45454333",
         "title":"'Sample video title1",
         "link":"http:\/\/example.com\/assets\/some\/media\/path\/myvideo1.mp4",
         "pubDate":"Tue, 01 Oct 2016 18:29:52 +0530",
         "media:thumbnail":"http:\/\/example.com\/assets\/some\/someimage1.jpg",
         "media:duration":"1913",
         "description":"This is a sample description1",
         "media:filepath":"http:\/\/example.com\/videos\/1234\/myvideo1.mp4",
         "media:fullimage":"http:\/\/example.com\/videos\/1234\/thumbnail1.jpg",
         "media:keywords":"some, sample, keywords,random"
      },
      {  
         "id":"44545333",
         "title":"'Sample video title 2",
         "link":"http:\/\/example.com\/assets\/some\/media\/path\/myvideo2.mp4",
         "pubDate":"Tue, 01 Sep 2016 18:29:52 +0530",
         "media:thumbnail":"http:\/\/example.com\/assets\/some\/someimage2.jpg",
         "media:duration":"1945",
         "description":"This is a sample description 2",
         "media:filepath":"http:\/\/example.com\/videos\/1234\/myvideo2.mp4",
         "media:fullimage":"http:\/\/example.com\/videos\/1234\/thumbnail2.jpg",
         "media:keywords":"some, sample, keywords,random"
      },
           {  
         "id":"4543434333",
         "title":"'Sample video title3",
         "link":"http:\/\/example.com\/assets\/some\/media\/path\/myvideo3.mp4",
         "pubDate":"Tue, 01 Aug 2016 18:29:52 +0530",
         "media:thumbnail":"http:\/\/example.com\/assets\/some\/someimage3.jpg",
         "media:duration":"1955",
         "description":"This is a sample description",
         "media:filepath":"http:\/\/example.com\/videos\/1234\/myvideo3.mp4",
         "media:fullimage":"http:\/\/example.com\/videos\/1234\/thumbnail3.jpg",
         "media:keywords":"some, sample, keywords,random"
      }
      
   ]
}
```

Let's say you wanted to select the media:thumbnail values for all the items. You could use this syntax:


```
$..results[*].media:thumbnail
```

This syntax says to start at the root, look recursively in all the items, match everything within the results array, and select the `media:thumbnail` element. The following gets returned: 

```
[
   "http://example.com/assets/some/someimage1.jpg",
   "http://example.com/assets/some/someimage2.jpg",
   "http://example.com/assets/some/someimage3.jpg"
]
```

### JSON Path Query Example 3

Instead of using the feed from Fire App Builder, in this example you will capture content from a more complicated feed structure.

1.  Copy the following feed and insert it into the [Jayway JsonPath Evaluator](http://jsonpath.herokuapp.com/).

    ```json
    {
    "brand": "myChannel",
      "version": "2.0",
      "creationDate": 1462590933,

        "containers": [
            {
                "type": "movie.Container",
                "subtype": "movies",
                "common": {
                    "title": "Movies"
                },
                "movies": [
                    {
                        "subtype": "movie",
                        "cat": "romance",
                        "availableDate": 1351631300,
                        "expirationDate": 1364764900,
                        "common": {
                            "authId": "1234",
                            "title": "Sample Movie Title",
                            "subtitle": "My sample subtitle for the movie",
                            "description": "This is a description for the movie. It describes the basic plot and story.",
                            "tags": [
                                "Some",
                                "Tags",
                                "Here"
                            ],
                            "promotionalText": "",
                            "theshareURL": "http://yourcompany.com/mymovieurl1",
                            "imageUrls": {
                                "1000x500_portal": "http://www.yourcompany.com/moviethumbl.jpg"
                            }
                        }
                    }
                ]
            },

            {
                "type": "movie.Container",
                "subtype": "movies",
                "common": {
                    "title": "Movies"
                },
                "movies": [
                    {
                        "subtype": "movie",
                        "cat": "adventure",
                        "availableDate": 1351631400,
                        "expirationDate": 1364765900,
                        "common": {
                            "authId": "5678",
                            "title": "Sample Movie Title 2",
                            "subtitle": "Another sample subtitle for the movie",
                            "description": "This is another sample movie title.",
                            "tags": [
                                "Some",
                                "Tags",
                                "Here"
                            ],
                            "promotionalText": "",
                            "theshareURL": "http://yourcompany.com/mymovieurl2",
                            "imageUrls": {
                                "1000x500_portal": "http://www.yourcompany.com/moviethumb2.jpg"
                            }
                        }
                    }
                ]
            }
        ]
    }
    ```

2.  In the **Go!** box, enter the following:

    ```
    $.containers[?(@.type == 'movie.Container')].movies[*].cat
    ```

    Here's what this query says:
    
    | Query syntax | What it means|
    |----|----|
    | `$`| Start at the root level|
    |`.containers[`| Match all instances of the key "containers" that contains an array (with specific contents) at the root level|
    |`?(@.type == 'movie.Container')]`: Create a filter that matches on the key `type` where type is equal to `movie.Container`.|
    |`.movies[*]`| Match all content in the `movies` array.|
    |`.cat`| Match the `cat` within the `movies` array.|

3. Click **Go!**

    The result retrieves the cats:
    
    ```
    [
       "romance",
       "adventure"
    ]
    ```
    
    
## Example 4


Suppose your feed structure looks like this:

```
[
  {
    "ChannelLang": "English",
    "VideoId": "239870",
    "VidTitle": "Top Stories",
    "videoKeywords": "news, stories, crime,",
    "videoDescription": "This is my video description.",
    "slug": "top-news-stories",
    "duration": 4318,
    "image": "http:\/\/example.com\/2016\/11\/43434.jpg",
    "thumb": "http:\/\/example.com\/2016\/11\/43434_thumb.jpg",
    "vidUrl": "http:\/\/example.com\/2016\/10\/myvideo.mp4",
    "created": "2016-11-02 23:08:42",
    "vidcategory": "Adventure"
  },
  {
    "ChannelLang": "English",
    "VideoId": "239870",
    "VidTitle": "Top Stories",
    "videoKeywords": "news, stories, crime,",
    "videoDescription": "This is my video description.",
    "slug": "top-news-stories",
    "duration": 4318,
    "image": "http:\/\/example.com\/2016\/11\/43434.jpg",
    "thumb": "http:\/\/example.com\/2016\/11\/43434_thumb.jpg",
    "vidUrl": "http:\/\/example.com\/2016\/10\/myvideo.mp4",
    "created": "2016-11-02 23:08:42",
    "vidcategory": "Adventure"
  },
   {
     "ChannelLang": "English",
     "VideoId": "239870",
     "VidTitle": "Top Stories",
     "videoKeywords": "news, stories, crime,",
     "videoDescription": "This is my video description.",
     "slug": "top-news-stories",
     "duration": 4318,
     "image": "http:\/\/example.com\/2016\/11\/43434.jpg",
     "thumb": "http:\/\/example.com\/2016\/11\/43434_thumb.jpg",
     "vidUrl": "http:\/\/example.com\/2016\/10\/myvideo.mp4",
     "created": "2016-11-02 23:08:42",
     "vidcategory": "Adventure"
   }
]
```

The following query would target the category:

```
$..vidcategory
```
 

{% include links.html %}
