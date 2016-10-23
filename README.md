# Making a website (or a webapp)

I've started to put everything I can think of to create a good website. Don't hesitate to PR. 

Let's try to be concise.

- [Care about security ?](#care-about-security-)
- [Care about social ?](#care-about-social-)
- [Care about SEO ?](#care-about-seo-)
- [Care about communication ?](#care-about-communication-)
- [Care about Apple ?](#care-about-apple-)
- [Care about accessibility (a11y) ?](#care-about-accessibility-a11y-)
- [Care about style ?](#care-about-style-)
- [Care about legacy ?](#care-about-legacy-)
- [Care about performance ?](#care-about-performance-)
- [Care about mobile ?](#care-about-mobile-)
- [Care about offline ?](#care-about-offline-)
- [Care about analytics ?](#care-about-analytics-)
- [Care about bugs ?](#care-about-bugs-)
- [Care about misc ?](#care-about-misc-)

## Care about security ?

  - Use HTTPS (add a letsencrypt certificate, renew every 3 months automatically)
    - get A+ on https://www.ssllabs.com/ssltest/index.html
    - check more with https://certlogik.com/ssl-checker/
  - Add all security headers
    - get A+ on https://securityheaders.io/
    - Content-Security-Policy
    - Content-Security-Policy-Report-Only
    - X-Webkit-CSP
    - X-Content-Security-Policy
    - Public-Key-Pins
    - Public-Key-Pins-Report-Only
    - Strict-Transport-Security
    - X-Content-Type-Options
    - X-Frame-Options
    - X-Xss-Protection
    - X-Download-Options
    - X-Permitted-Cross-Domain-Policies
    - Access-Control-Allow-Origin
    - Add CRI (Subresource Integrity) to your resources https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity
```html
<script src="framework.js"
        integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
        crossorigin="anonymous"></script>
```
  - Protect again exploits
    - CSRF https://en.wikipedia.org/wiki/Cross-site_request_forgery
    - XSS https://en.wikipedia.org/wiki/Cross-site_scripting
    - Sql Injection https://en.wikipedia.org/wiki/SQL_injection
  - Protect the servers against brutefores attacks (add some kind of ban politics)
  - Use JSON Web Tokens https://jwt.io/ to talk to the server
  - Protect against DDOS if you need to, for instance using [Cloudflare](https://www.cloudflare.com/)
  - Use `rel="noopener"` for external links: `<a href="http://example.com" target="_blank" rel="noopener">`
  - Identify frauds before they happen with Smyte (a js tag to add) https://www.smyte.com/

## Care about social ?

 - Facebook https://developers.facebook.com/docs/reference/opengraph/ http://stackoverflow.com/questions/10836135/when-do-i-need-a-fbapp-id-or-fbadmins
```html
<meta property="fb:admins" content="USER_ID" />
<meta property="fb:app_id" content="123456789456489" />
```
  - Twitter Card
```html
<meta name="twitter:card" content="summary"/>
<meta name="twitter:title" content="{{ PAGE_NAME }}"/>
<meta name="twitter:description" content="{{ PAGE_DESCRIPTION }}"/>
<meta name="twitter:site" content="{{ TWITTER_WEBSITE_ACCOUNT }}"/>
<meta name="twitter:image" content="{{ PAGE_IMAGE_URL }}" />
<meta name="twitter:creator" content="{{ TWITTER_CREATOR_ACCOUNT }}"/>
<meta name="twitter:domain" content="mon.site.com"/>
```
  - Google Plus
  - Add the Sharing, Like, +1 buttons
    - Facebook https://developers.facebook.com/docs/plugins
    - Twitter https://dev.twitter.com/web/tweet-button
    - Google Plus https://developers.google.com/+/web/share/

## Care about SEO ?

  - Use HTTPS
  - Isomorphic/Universal Javascript (prerendering page)
  - Define the crawlers politic. Define the file robots.txt, or you can add a meta:
```
<meta name="robots" content="index,follow" />
```

## Care about communication ?

  - OpenGraph http://ogp.me/
```html
<meta property="og:locale" content="{{ LOCALE }}" />
<meta property="og:type" content="article" /> <!-- product... -->
<meta property="og:title" content="{{ PAGE_NAME }}" />
<meta property="og:description" content="{{ PAGE_DESCRIPTION }}" />
<meta property="og:image" content="{{ PAGE_IMAGE_URL }}" />
<meta property="og:url" content="{{ PAGE_CANONICAL_URL }}" />
<meta property="og:site_name" content="{{ APPLICATION_NAME }}" />	
<meta property="og:updated_time" content="2015-05-12T22:24:50+00:00" />
<meta property="article:publisher" content="{{ PUBLISHER }}" />
<meta property="article:author" content="{{ AUTHOR }}" />
<meta property="article:section" content="Technology" />
<meta property="article:published_time" content="2015-01-06T23:07:41+00:00" />
<meta property="article:modified_time" content="2015-05-12T22:24:50+00:00" />
```
  - Bing
```html
<meta name="geo.placename" content="United States" />
<meta name="geo.position" content="x;x" />
<meta name="geo.region" content="usa" />
<meta name="ICBM" content="x,x" />
```
  - Define a canonical URL for every page (to avoid to reference twice the same page)
```html
<link rel="canonical" href="article.html">
```
  - Define it's UTF8
```html
<meta charset="utf-8">
```  
   - Define the content type
```html
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
```
  - Language
```html
<html lang="en">
<meta http-equiv="Content-Language" content="en">
<meta name="language" content="{{ LANG }}" /><!-- Old -->
```
  - Provide some info the browsers/crawlers can use to describe your app (if web app)
```html
<meta name="application-name" content="{{ APPLICATION_NAME }}">
<meta name="description" content="{{ PAGE_DESCRIPTION }}">
<meta name="keywords" content="{{ PAGE_KEYWORD }}" />
```
  - Define a pingback route to know who is linking to you http://wordpress.stackexchange.com/questions/116079/what-is-rel-pingback-and-what-is-the-use-of-this-in-my-website
```html
<link rel="pingback" href="http://www.example.com/xmlrpc.php" />
```
  - You can put some RSS
```html
<link rel="alternate" type="application/rss+xml" href="http://www.example.com/rss.xml" />
 ```
  - Prev/Next pages if you are in a listing
```html
<link rel="prev" title="..." href=".../page/1" />
<link rel="next" title="..." href=".../page/3" />
```
  - Define the shortlink of the pages
```html
<link rel="shortlink" type="text/html" href="http://example.com/Ad1ca9">
```
  - Define the sitemap of the website
```html
<link rel="sitemap" type="application/xml" title="Sitemap" href="{{ SITEMAP_URL }}" />
```
  - Favicons/Tiles (+ Apple/Windows variations) 
```html
<!-- The classic one -->
<link rel="shortcut icon" type="image/x-icon" href="favicon.ico">

<!-- Used by http://fluidapp.com/ (website to native app on Mac) -->
<link rel="fluid-icon" href="fluidicon.png" title="...">

<!-- Apple formats https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html -->
<link rel="apple-touch-icon" sizes="57x57" href="/apple-icon-57x57.png">
<link rel="apple-touch-icon" sizes="60x60" href="/apple-icon-60x60.png">
<link rel="apple-touch-icon" sizes="72x72" href="/apple-icon-72x72.png">
<link rel="apple-touch-icon" sizes="76x76" href="/apple-icon-76x76.png">
<link rel="apple-touch-icon" sizes="114x114" href="/apple-icon-114x114.png">
<link rel="apple-touch-icon" sizes="120x120" href="/apple-icon-120x120.png">
<link rel="apple-touch-icon" sizes="144x144" href="/apple-icon-144x144.png">
<link rel="apple-touch-icon" sizes="152x152" href="/apple-icon-152x152.png">
<link rel="apple-touch-icon" sizes="180x180" href="/apple-icon-180x180.png">

<!-- For Safari pinned tabs -->
<link rel="mask-icon" href="logo.svg" color="orange">

<!-- Recommanded is 192x192 only -->
<link rel="icon" type="image/png" sizes="192x192"  href="/icon-192x192.png">
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="96x96" href="/favicon-96x96.png">
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">

<!-- Not sure if still used. App should used it if og:image is unspecified, to display an image when sharing -->
<link rel="image_src" href="{{ PAGE_IMAGE_URL }}">

<!-- Windows -->
<meta name="msapplication-TileColor" content="#ffffff">
<meta name="msapplication-TileImage" content="/ms-icon-144x144.png">
<meta name="theme-color" content="#ffffff">
```
  - Really Simple Discovery  http://en.wikipedia.org/wiki/Really_Simple_Discovery
```html
<link rel="EditURI" type="application/rsd+xml" title="RSD" href="http://www.example.com/xmlrpc.php?rsd" />
```
  - WindowsLiveWriter
```html
<link rel="wlwmanifest" type="application/wlwmanifest+xml" href="http://www.example.com/wlwmanifest.xml" />
```
  - OpenSearch
```html
<link rel="search" type="application/opensearchdescription+xml" href="/opensearch.xml" title="...">
```
  - JSON-LD
```html
<script type="application/ld+json">{"@context":"http:\/\/schema.org","@type":"WebSite","url":"https:\/\/...","name":"...","alternateName":"..."}</script>
```
  - XFN (human relationships) http://microformats.org/wiki/rel-profile
```html
<link rel="profile" href="http://gmpg.org/xfn/11" />
```

## Care about Apple ?

  - Apple has some options to customize the web app appearance https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html
```html
<link rel="apple-touch-startup-image" href="/startup.png">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<meta name="format-detection" content="telephone=no">
```

## Care about accessibility (a11y) ?

  - Audit your website: https://github.com/addyosmani/a11y
  - Use role="xxx"
  - Use aria-xxx

## Care about style ?

  - Add viewport meta mobile compliant
```html
<meta name="viewport" content="width=device-width, minimum-scale=1, initial-scale=1, user-scalable=yes, minimal-ui">
```
  - Avoid FOUC (Flash Of Unstyled Content) https://en.wikipedia.org/wiki/Flash_of_unstyled_content
  - Make it responsive using media queries and other css techniques
  - Talk to a UI and UX designer
  - Avoid to use custom scrollbars plugins. People tends to not like them. There are often used to cancel the style of the ugly scrollbars in Windows unfortunately.

## Care about legacy ?

  - Add X-UA-Compatible
````html
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
```
  - Add shims
```html
<!--[if lt IE 9]>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html5shiv/3.7.3/html5shiv.min.js"></script>
<![endif]-->
```
  - Add polyfills (use cloudflare cdn to grab them)
    - `fetch`: https://github.com/github/fetch
    - `Promise`: https://github.com/stefanpenner/es6-promise 
    - media queries: https://github.com/scottjehl/Respond

## Care about performance ?

  - Use HTTP/2
  - DNS Prefetch to resolve DNS asap (for future pages)
```html
<link rel="dns-prefetch" href="//fonts.googleapis.com">
<link rel="dns-prefetch" href="//themes.googleusercontent.com">
```
  - DNS+Handshake+TLS Preconnect. Better. DNS+TCP handshake + optional TLS negotiation. Use for the current page.
```html
<link rel="preconnect" href="//fonts.googleapis.com">  
```
  - Resource Prefetch (low priority). Download a resource right now (into cache) if we know it's going to be used later.
```html
<link rel="prefetch" href="image.png">
```
  - Subresource: Download directly (high priority, whereas prefetch is low priority) a resource that will be discovered later in the page (such as `<script>` at the end)
```html
<link rel="subresource" href="app.js">
```
  - Prerender. Fetch the whole content of another page (css, process js etc.). Useful when you know that the user will click on it for sure. It will take only a instant (everything will be already loaded!)
```html
<link rel="prerender" href="http://example.com/about">
```
  - Think about the critical css path. Inject it in the `<head>` directly
  - Shrink your js/css bundles
    - Split the libs bundle(s) (rarely changed) from the app bundle(s)
  - Load unnecessary modules after the initial rendering
  - Delay if not in the viewport at first sight
    - Load images lazily
  - Use requestAnimationFrame to handle any animation (avoid setTimeout/setInterval) https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution
  - Use requestIdleCallback when you want to process something not time critical https://developers.google.com/web/updates/2015/08/using-requestidlecallback
  - Avoir layout trashing (write/read DOM continuously). Batch. And simply let the frameworks (if available) update the DOM for you (with the Virtual DOM nowadays)
  - GZip your resources
  - Optimize your images and your SVGs https://jakearchibald.github.io/svgomg/
  - Use `async` and `defer` on your scripts when it's possible
  - To do something before unloading the page, use the navigator beacon to not block the closing https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon
  - Only import the necessary font families and weights
```html
<link rel="stylesheet" href="//fonts.googleapis.com/css?family=Open+Sans:300">
```
  - For classic js third party libraries, use a cdn (unpkg, cdnjs, jsdelivr, maxcdn..)
  - Use a generic CDN for your resources like [Cloudflare](https://www.cloudflare.com/)
  - If you don't want an external CDN, install a "HTTP Web Accelerator" like Varnish to cache static resources server side and serve them faster
  - Use `rel="noopener"` for external links: `<a href="http://example.com" target="_blank" rel="noopener">`
  - Check your performances with https://testmysite.io/ 
  - Check your performances and best practices with Lighthouse: https://github.com/GoogleChrome/lighthouse (test if you app can be considered as "progressive")
  
## Care about mobile ?

  - Add a manifest to know how to display it on the home screen https://developer.mozilla.org/en-US/docs/Web/Manifest
```html
<link rel="manifest" href="/manifest.json">
```
```json
{
	"name": "example.com",
	"short_name": "EXX",
	"start_url": "/",
	"display": "standalone",
	"background_color": "#fff",
	"theme_color": "#0379C4",
	"description": "The official website and documentation for ...",
	"icons": [ { } ]
}
```

## Care about offline ?

  - Add a service worker and cache resources and responses https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/
    - you can generate automatically some service-worker.js code using https://github.com/GoogleChrome/sw-precache
  - (Add a AppCache manifest. Deprecated)

## Care about analytics ?

  - Google Webmaster
```html
<meta name="google-site-verification" content="xyz">
```
  - Google analytics https://developers.google.com/analytics/devguides/collection/analyticsjs/
```html
<script>
window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)};ga.l=+new Date;
ga('create', 'UA-XXXXX-Y', 'auto');
ga('send', 'pageview');
</script>
<script async src='https://www.google-analytics.com/analytics.js'></script>  
```
  - You an try Amplitude too https://amplitude.com/
  - You have a bunch of tags ? Use Google Tag Manager.
 
## Care about bugs ?

  - Monitor browser's JS errors, for instance with https://trackjs.com or https://sentry.io/

## Care about misc ?

  - If the website sets cookies to an EU visitor, you must display a notice https://www.cookielaw.org/the-cookie-law/
  - Add Google notranslate to avoid Google to display the translation bar when you know it's not needed
```html
<meta name="google" value="notranslate">  
```
  - If you can, try to make your website working without JS.

