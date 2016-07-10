---
title: Pics, Pics, Pics - Incomplete Guide to Image Research
date: '2016-07-06 05:40:00'
layout: post
tags:
  - osint
  - research
  - guide
---
"Pics" (Photos, Logos, Icons, Maps) can be of great value in OSINT Investigations. This post is a roundup of resources and tricks. This guide will show you how to search, find, get, scrape, analyze digital images.

### Basic search

If you are searching for images there is more than [Google](https://images.google.com): All big search engines have an image-search feature: [Bing](http://www.bing.com/Images), [Yandex](https://yandex.com/images/), [Baidu](http://image.baidu.com/), [Yahoo](https://images.search.yahoo.com/). Other image search engines are [exalead](http://www.exalead.com/), [picsearch](http://www.picsearch.com/), [photobucket](http://photobucket.com/) or [dreamstime](https://www.dreamstime.com/). A good overview of Image If you are searching for press/news photos [gettyimages](http://www.gettyimages.com/Editorial/Editorial.aspx), [corbis](http://pro.corbis.com/default.aspx) and [reuters pictures](http://pictures.reuters.com/) are the place to go. For images shared through social media give [instagram](https://instagram.com), [flickr](https://www.flickr.com), [imgur](https://imgur.com/search?q=) or [Pinterest](https://www.pinterest.com) a try.

### Advanced search

![]({{ site.baseurl }}/forestryio/images/GoogleSearch.png)

When searching for images use some common sense and [advanced search tools](https://images.google.com/advanced_image_search?hl=uz) if available. Remember you can jump in time, search for faces or photos, search by color or limit your search to top-level/country domains or websites. Use a translator and try searching in other languages too.

[twipho](https://twipho.net/) and [twicsy](http://twicsy.com/) search media posted on twitter based on geolocation. Use the [Location Guard](https://github.com/chatziko/location-guard) Browser Extension to fake your location (set to fixed location) in order to get media from the region of interest.

![]({{ site.baseurl }}/forestryio/images/LocationGuard.png)

Research on image databases and forums based on your area of interest. To mention only a few examples:

* [Panoramio (Landscape/Urban Photography on Google Maps)](http://www.panoramio.com/)
* [Skyscrapercity (forum for skyscraper enthusiasts, high-res architecture photos, blueprints, large community)](http://www.skyscrapercity.com/)
* [Public Sources for Satellite and Aerial Photos and Maps](https://cryptome.org/gis-sources.htm)

As you might imagine high-resolution photos of control-rooms, desks, whiteboards, phones, screens, license plates, faces can reveal interesting information. Have a look at what you can find when you know what to search for:

![]({{ site.baseurl }}/forestryio/images/whattosearchfor.jpg)

### Reverse image search

You can use reverse search to double check images before you believe that they are current or even relevant to the story they are attached to.

Good tutorials on reverse image search are ["Manual reverse image search with Google and TinEye"](https://www.bellingcat.com/resources/how-tos/2015/05/08/manual-reverse-image-search-with-google-and-tineye/) and ["Reverse image search for anime and manga"](http://docwatson42.livejournal.com/1783.html).

A list of relevant sites and tools:

* [TinEye Reverse image search](https://tineye.com/)
* [Image Raider](https://www.imageraider.com/)
* [Karmadecay - Reverse image search of Reddit.com](http://karmadecay.com/)
* [RevEye Chrome Extension](https://chrome.google.com/webstore/detail/reveye-reverse-image-sear/keaaclcjhehbbapnphnmpiklalfhelgf)

### Downloading

Now that you've found the pictures you want to download them. This may sometimes be hindered by copy protection mechanisms (like a disabled context-menu or images loaded by javascript). Let's take [Arab Images Foundation](http://www.arabimages.com/) for example:

![]({{ site.baseurl }}/forestryio/images/2016-07-05 13_55_07-Arab Images Foundation _ The Arab World Photography Collection.png)

You can see a preview on hover, but the browser context-menu is disabled by javascript and downloadable images are small and heavily watermarked. It is still possible to download images though: Open your *Page Inspector/DevTools* panel and open the *Network* ([Firefox](https://support.mozilla.org/en-US/kb/keyboard-shortcuts-perform-firefox-tasks-quickly?redirectlocale=en-US&redirectslug=Keyboard+shortcuts)) or *Resources* ([Chrome](https://developers.google.com/web/tools/chrome-devtools/)) tab to browse loaded images:

![]({{ site.baseurl }}/forestryio/images/2016-07-05 13_56_28-Arab Images Foundation _ The Arab World Photography Collection.png)

Another method is to open the page information menu (small information-icon next to URL in Firefox), expand the panel, click on *more Information* and download through the *Media* tab. Both methods work on 'protected' flickr photos:

![]({{ site.baseurl }}/forestryio/images/Flickr.png)

Pay attention to filenames and directories, the URL often reveals where the high-resolution images are. A filename like `image-640x480.jpg` may indicate that there is an image with higher resolution like `image-1200x800.jpg` or `image-highres.jpg`.

![]({{ site.baseurl }}/forestryio/images/naming.png)

**Get 'em all**

If you need to download many images and you can't go one by one, you'll need to *scrape* or *crawl*. Web-Scraping is a topic too broad to be covered in detail here, but here are some basics to get you started:

> Web Scrapers are tools designed to extract / gather data in a website via crawling engine usually made in Java, Python, Ruby and other programming languages.Web Scrapers are also called as Web Data Extractor, Data Harvester , Crawler and so on which most of them are web-based or can be installed in local desktops.

>  [A Guide to Web Scraping Tools, Gareth James](http://www.garethjames.net/a-guide-to-web-scrapping-tools/)

I'd suggest to learn the basic syntaxt of `wget` so you can start mass-downloading images. Here is an example:

```bash
$ wget -nd -r -P /save/location -A jpeg,jpg,bmp,gif,png http://www.domain.com
```
A more advanced bash script that scrapes images from a list of urls with wget:
[wget images scraper](https://github.com/eduardschaeli/wget-image-scraper)

As sites grow larger and more dynamic you need more sophisticated tools. [HTTrack](http://www.httrack.com/) is pretty simple to use and comes with a lot of options (proxy, custom browser agents, timeouts, limits, file inclusion/exclusion, ...).

When it comes to community sites like tumblr or instagram you often need custom made scripts or developer account to connect to their API:

* [Facebook images scraper](https://github.com/irkarthikeyan/Facebook-images-scraper)
* [tumblr scraper](https://github.com/rranshous/tumblr_scraper)
* [instagram screen scrape](https://www.npmjs.com/package/instagram-screen-scrape)

## Analyze

> Image analysis is the **extraction of meaningful information from images**; mainly from digital images by means of digital image processing techniques. Image analysis tasks can be as simple as reading bar coded tags or as sophisticated as identifying a person from their face. 

> [Image Analysis, Wikipedia](https://en.wikipedia.org/wiki/Image_analysis)

Not going to cover how to analyze the visual content of images, since there is already a lot of information out there. Read this short piece on ["Analyzing Images as Text Instructions"](https://wikis.engrade.com/warimages/analyzingtext) focusing on war photography and the S-P-A-T-E-R (**S**ubject, **P**urpose, **A**udience, **T**one, **E**ffect, **R**hetorical **D**evices/**S**trategies) method for analyzing visual media.

**Metadata**

Without going too deep into forensic analysis, it should be known by now that images contain tons of information also known as metadata. The most basic tool to extract metadata is probably [Jeffrey's Image Metadata Viewer](http://regex.info/exif.cgi). Just upload your image to the website and view the results.

A more advanced tool with dozens of features (Metadata extraction, GPS Localization, Mime Information, Error Level Analysis, Hash Matching, ...) and a fancy interface is [Ghiro](http://www.getghiro.org/).

Recommended Read: [MetaUseful & MetaCreepy (Bellingcat)](https://www.bellingcat.com/uncategorized/2015/04/24/metadata-metauseful-metacreepy/)

**Verification**

Every eyewitness photo and video will contain visual clues that can help with verification. Learn more about what they are and how to find them:

[Piecing together visual clues for verification](https://firstdraftnews.com/resource/learning-to-piece-together-verification-clues/)

[Verification Handbook](https://firstdraftnews.com/resource/the-verification-handbook/)
