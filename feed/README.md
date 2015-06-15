# “Popular” JSON/P feed

This folder contains example code used to generate a [JSON/P](http://json-jsonp-tutorial.craic.com/index.html) feed, powered by [NEWSCYCLE Solutions’](http://www.newscyclesolutions.com/) `##class(dt.cms.trace.Popular).getPopular()` (a.k.a. `<dti:popular></dti:popular>`) code.

## JSON structure

The `popular.csp` CSP page generates JSON with this basic structure:

```json
{
	"stories": [
		{
			"byline": "Billy Johnson",
			"category": "Crime",
			"count": 1,
			"deck": "Lorem ipsum dolor sit amet",
			"headline": "The quick brown fox jumps over the lazy dog",
			"path": "\/foo\/permalink.html",
			"published": "2014-09-16 15:48:01",
			"section": "local",
			"section_link": "\/foo\/",
			"server": "http://site.com"
		}
	]
}
```

Check out a [live example](http://registerguard.com/csp/cms/sites/rg/feeds/popular.csp).

## Options

The code can be manipulated via these URL [query string](http://en.wikipedia.org/wiki/Query_string) ([`$get()`](http://docs.intersystems.com/cache20091/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_fget)) parameters:

Parameter | Description | Example
--- | --- | ---
`callback` | JSONP callback name. | `http://site.com/popular.csp?callback=foo`
`page` | Page number for pagination. | `http://site.com/popular.csp?page=3`
`per` | Items per page. | `http://site.com/popular.csp?per=15`
`publication` | Site publication (default is `rg`). | `http://site.com/popular.csp?publication=rg`
`section` | Site section (default is entire publication). | `http://site.com/popular.csp?section=sports`

For details, please read through the [source code](./popular.csp).

## Dependencies …

Some of the below methods are drag/drop while others will require modifications to work on your system.

### … on [GitHub](https://github.com/):

**[Pick](https://github.com/mhulse/pick)** / `##class(custom.rg.Pick).pics()`

> Get pictures attached to a story and order them by vieworder (+ a whole lot more!).

> For use with DTI's ContentPublisher which is powered by Intersystem's Caché.

If you can’t install the above code, you could remove it from [`popular.csp`](./popular.csp) and/or replace it with your own image-getting code.

### … as [Gists](https://gist.github.com/):

Method | Description
--- | ---
[`byline()`](https://gist.github.com/mhulse/c493894e109a210f6ef4) | Author byline.
[`catName()`](https://gist.github.com/mhulse/633f47c79ce671aae1b5) | Sub/category name from `subCategoryId`.
[`cmsPubTracking()`](https://gist.github.com/mhulse/33bb98b29f08bc1e2a10) | Earliest possible “published to web” date.
[`getimagesize()`](https://gist.github.com/mhulse/382b0f46c3ad5f8a8357) | Width and height of image stream.
[`headline()`](https://gist.github.com/mhulse/59ede19115960ee5a716) | Story headline.
[`meta()`](https://gist.github.com/mhulse/1aab2d8ee7e7559288a1) | Extracts caption from image stream.
[`subHeadline()`](https://gist.github.com/mhulse/9a2fe838f9f1fc3ae7c1) | Story deck.
[`uri()`](https://gist.github.com/mhulse/43f68c1773d6a0a04dce) |  Story URL.

## In the wild

Our [“Top 100 stories in the last 24 hours” page](http://pages.registerguard.com/popular/) utilizes the JSONP output:

[![Popular](https://raw.githubusercontent.com/mhulse/popular/gh-pages/popular.gif)](http://pages.registerguard.com/popular/)

Visit [this project on GitHub](https://github.com/mhulse/popular/).

## Notes

* The code defaults to JSON; JSONP is an option if you utilize the `callback` query string parameter.
* The maximum number of results is hard-coded to `100`; you can change this number by editing the `page("max")` delcaration/initialization.
* If/when the requested page is out of range, the code will return a response status of `404 Not Found` (this is needed if using [Infinite Scroll](https://github.com/paulirish/infinite-scroll)).
* Response is cached for 10 minutes and varies by `page` and `per` query string parameters.
* The `getPopular()` method will grab results from entire publication; use `section` query string parameter to target a specific section.
* The default `publication` is `rg`; use `publication` query string parameter to target a specific publication.
* Code tested on 7.7.3 Lighting, Cache for Windows (x86-64) 2009.1.5 (Build 901_0_11112U) Thu Dec 8 2011 18:33:08 GMTST.

