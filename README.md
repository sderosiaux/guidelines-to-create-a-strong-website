# Guidelines to create a strong website

Here you'll find out all the things I could think of and find out, to create a "good" website.

From security, to performance, social sharing, analytics etc. I'm trying to not forget anything.
This is not about which framework to use, but about everything that makes a "good" website in general: secured, performant, social compliant, SEO compliant, offline ready, and more.

This list is growing over time.

## Know more ?

Don't hesitate to PR! Let's try to be concise: other resources on the web go further in details for each topic, let's keep them one-liner here with a sample code when necessary.

![Only hundreds of things to do](73873677.jpg)

# Summary

- [Care about security ?](#care-about-security-)
- [Care about social ?](#care-about-social-)
- [Care about SEO ?](#care-about-seo-)
- [Care about communication ?](#care-about-communication-)
- [Care about Apple ?](#care-about-apple-)
- [Care about accessibility (a11y) ?](#care-about-accessibility-a11y-)
- [Care about privacy ?](#care-about-privacy-)
- [Care about style ?](#care-about-style-)
- [Care about legacy ?](#care-about-legacy-)
- [Care about performance ?](#care-about-performance-)
- [Care about mobile ?](#care-about-mobile-)
- [Care about offline ?](#care-about-offline-)
- [Care about analytics ?](#care-about-analytics-)
- [Care about bugs ?](#care-about-bugs-)
- [Care about misc ?](#care-about-misc-)
- [More tips](#more-tips)

## Care about security ?

  - Use HTTPS (add a letsencrypt certificate, renew every 3 months automatically)
    - get A+ on https://www.ssllabs.com/ssltest/index.html
    - check more with https://certlogik.com/ssl-checker/
  - Add all security headers
    - get A+ on https://securityheaders.io/
    - **Content-Security-Policy**: define which hosts are allowed for the browser to download/send from/to (scripts, styles, images, iframes, forms..). `Content-Security-Policy: script-src 'self' https://apis.google.com; img-src 'self'`. All details here https://www.html5rocks.com/en/tutorials/security/content-security-policy/. A very interesting piece to read: [https://hackernoon.com/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5](https://hackernoon.com/im-harvesting-credit-card-numbers-and-passwords-from-your-site-here-s-how-9a8cb347c5b5) (look for "Content Security Policy") about CSP tricks and escapes.
    - **Content-Security-Policy-Report-Only**: when migrating an existing website to CSP, use this first just to get reports on CSP violations (the browser will still acts normal)
    - ~~X-Webkit-CSP (old Chrome)~~
    - ~~X-Content-Security-Policy: IE10, FF&lt;24)~~
    - **Public-Key-Pins**: ensure the webclient has the right public keys, to avoid MITM attacks `public-key-pins-report-only:max-age=500; pin-sha256="WoiWRyIOVNa9ihaBciRSC7XHjliYS9VwUGOIud4PB18="; report-uri="http://example.com/hpkp/"`
    - **Public-Key-Pins-Report-Only**: Same as CSP-RO. At first, add this one to see if you get any error. Facebook is using this one for instance.
    - **Strict-Transport-Security**: specify to the browser to use only HTTPS for a period of time. The browser will automatically use https if it got the header before. `Strict-Transport-Security: max-age=63072000; includeSubDomains; preload` (2 years), and add it to the preload list of Chrome https://hstspreload.appspot.com and figure inside Chromium's source: https://cs.chromium.org/chromium/src/net/http/transport_security_state_static.json
    - **X-Content-Type-Options**: do not rely on files MIME types. (a .txt containing some .js, and there are even some exploits that insert JS into MIME-types!) `X-Content-Type-Options: nosniff`
    - **X-Frame-Options**: avoid your website to be embedded into an iframe, to just allow for same domain. `X-Frame-Options: deny`. For a finer control, CSP exist.
    - **X-XSS-Protection**: enable by default, the browser does not execute a js if it finds the same in the querystring. As CSP, a report url option exists: `X-XSS-Protection: 1; report=http://www.company.com/report`
    - ~~X-Download-Options: IE8~~
    - **X-Permitted-Cross-Domain-Policies**: Restrict Adobe Flash Player's and Reader's access to data. `X-Permitted-Cross-Domain-Policies: none`
    - **Access-Control-Allow-Origin**: Known as CORS. Control how a HTTP request is handled (accepted or rejected) if it's coming from another domain. Generally, an API adds the host(s) serving the UI in this header (only if on different domain ofc). Never use `Access-Control-Allow-Origin: *` except if you are reckless or don't care. Lots of details: https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS
    - **Timing-Allow-Origin**: prevent browsers to access timing information through PerformanceResourceTiming (window.performance) for privacy reasons: `Timing-Allow-Origin: ` (no value)
    - Add CRI (Subresource Integrity) to your resources https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity
```html
<script src="framework.js"
        integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
        crossorigin="anonymous"></script>
```
  - Protect again exploits
    - CSRF/XSRF https://en.wikipedia.org/wiki/Cross-site_request_forgery
    - JSON Hijacking http://stackoverflow.com/questions/2669690/why-does-google-prepend-while1-to-their-json-responses GET requests that respond with a JSON array can be leaked.
    - XSS https://en.wikipedia.org/wiki/Cross-site_scripting
    - XSSI http://stackoverflow.com/questions/8028511/what-is-cross-site-script-inclusion-xssi
    - SQL Injection https://en.wikipedia.org/wiki/SQL_injection
  - Protect the servers against brutefores attacks (add some kind of ban politics)
  - Remove the server signature from the response headers (`Server: Apache`, nginx etc.) and same for `X-Powered-By`.
  - Optimize your responses `Set-Cookie` by setting them `httpOnly` if  you don't need them in Javascript (only in the server), `secure` if you deal only with HTTPS.
  - Use JSON Web Tokens https://jwt.io/ to talk to the server
  - Protect against DDOS if you need to, for instance using [Cloudflare](https://www.cloudflare.com/)
  - Use `rel="noopener noreferrer"` for external links: `<a href="http://example.com" target="_blank" rel="noopener noreferrer">` (`noreferrer` is for firefox) to avoid a vulnerability.
  - Identify frauds before they happen with Smyte (a js tag to add) https://www.smyte.com/
  - For registration pages, put a captcha. https://www.google.com/recaptcha/intro/index.html
  - Note that if your website is https, and links to http websites, they won't get the referrer (bad for analytics). The meta "referrer" lets you control when to send it bypassing this restriction: `<meta name="referrer" content="origin-when-cross-origin">`. [Referrer Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy).
  - Define a security policy and email adress in `/.well-known/security.txt`. See https://securitytxt.org/ for details.
  - Handle standard addresses: security@, admin@, webmaster@, support@, postmaster@, hostmaster@
  - If using `npm`, use `npm audit` to ensure you don't depend on unsecured packages
  
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
  - If your website uses CSP, don't forget to add rules to allow social buttons' integration

## Care about SEO ?

  - Use HTTPS
  - Isomorphic/Universal Javascript (prerendering page)
  - Define the crawlers politic. Define the file `robots.txt`, or you can add a meta:
```html
<meta name="robots" content="index,follow" />
```
  - Create a `humans.txt` because we are humans, not machines http://humanstxt.org/. You can add a meta to refer to it (or not). Try https://www.medium.com/humans.txt or https://www.google.com/humans.txt ;)
```html
<link type="text/plain" rel="author" href="humans.txt" />
```
  - If you have a multilanguage website, use `hreflang` tag to indicate alternative versions. Check https://support.google.com/webmasters/answer/189077
```html
<link rel="alternate" hreflang="es" href="http://es.example.com/" />
```
  - If your website publishes news:
    - you can use `<meta name="news_keywords" content="football, world cup" />` to help Google News to reference you https://support.google.com/news/publisher/answer/68297?hl=en
    - you can use `<meta name="syndication-source" content="http://example.com/my_news" />` if you are the original creator of the news, to indicate you are the original source.
    - it exists also a meta `original-source` to reference the sources you reference in your article
   
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
  - Define a canonical URL for every page (to avoid to reference twice the same page, for instance the mobile version and the desktop's)
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
  - Favicons/Tiles (+ Apple/Windows variations) (can use http://realfavicongenerator.net/ to generate them)
```html
<!-- The classic one; ico or png -->
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
  - Tasks in Windows jump lists (https://msdn.microsoft.com/en-us/library/gg491725(v=vs.85).aspx)
```html
<meta name="msapplication-task" content="name=Example: home;action-uri=http://www.example.com/home;icon-uri=http://www.example.com/favicon.ico">
```
  - OpenSearch
```html
<link rel="search" type="application/opensearchdescription+xml" href="/opensearch.xml" title="...">
```
  - JSON-LD aka Structured Data. It's a mandatory thing to add to your pages. The search engine will format its result according to that. Check https://developers.google.com/search/docs/guides/search-gallery to know what is possible and how to write them. 
```html
<script type="application/ld+json">{"@context":"http:\/\/schema.org","@type":"WebSite","url":"https:\/\/...","name":"...","alternateName":"..."}</script>
```
  - XFN (human relationships) http://microformats.org/wiki/rel-profile
```html
<link rel="profile" href="http://gmpg.org/xfn/11" />
```
  - Dublin Core Metadata (not sure if used?) https://en.wikipedia.org/wiki/Dublin_Core
```html
<meta name="DC.Format" content="text/html">
<meta name="DC.Language" content="en">
<meta name="DC.Type" content="Text">
<meta name="DC.Title" content="My revolution">
```
  - Keep URLs short and meaningful
  - Handle page errors (404, etc.) with some links inside, instead of dumping the classic raw default page

## Care about Apple ?

  - Apple has some options to customize the web app appearance https://developer.apple.com/library/content/documentation/AppleApplications/Reference/SafariWebContent/ConfiguringWebApplications/ConfiguringWebApplications.html
```html
<link rel="apple-touch-startup-image" href="/startup.png">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black">
<meta name="format-detection" content="telephone=no">
```

## Care about accessibility (a11y) ?

  - Read http://www.bbc.co.uk/gel/guidelines/how-to-design-for-accessibility to get a global why we should care
  - Check the posters in https://github.com/UKHomeOffice/posters/tree/master/accessibility to get more clear headlines
  - Audit your website: https://github.com/addyosmani/a11y [![GitHub stars](https://img.shields.io/github/stars/addyosmani/a11y.svg?style=social&label=Star)](https://github.com/addyosmani/a11y)
  - Be ARIA (Accessible Rich Internet Applications)
    - Check http://a11yproject.com/posts/getting-started-aria/ to understand what it is
    - For more details https://classroom.udacity.com/courses/ud891
    - Use its roles: `role="banner"` `role="navigation"` etc. see http://a11yproject.com/checklist.html for a useful list
    - Use `alt` on images, `label` on forms controls labels    
  - Read the "Web Content Accessibility Guidelines" ([WCAG](https://www.w3.org/TR/WCAG/))
  - Ensure the color contrast you are using is fine: http://leaverou.github.io/contrast-ratio/    
  - If using jsx, add this ESLint plugin to enforce them: https://github.com/evcohen/eslint-plugin-jsx-a11y
  
## Care about privacy ?

  - Add Do Not Track (DNT) header: opt-out of third-party tracking for purposes including behavioral advertising https://en.wikipedia.org/wiki/Do_Not_Track `DNT: 1`
  - Respect DNT and expose the file .well-known/dnt-policy.txt to say it (check https://www.eff.org/dnt-policy)

## Care about style ?

  - Consider using a "reset" css such as https://necolas.github.io/normalize.css
  - Add viewport meta mobile compliant
```html
<meta name="viewport" content="width=device-width, minimum-scale=1, initial-scale=1, user-scalable=yes, minimal-ui">
```
  - Avoid FOUC (Flash Of Unstyled Content) and FOIC (Flash Of Invisible Content) https://css-tricks.com/fout-foit-foft/
  - Make it responsive using media queries and other css techniques
  - Talk to a UI and UX designer
  - Avoid to use custom scrollbars plugins. People tends to not like them. There are often used to cancel the style of the ugly scrollbars in Windows unfortunately.
  - Fix the size of elements in the page (images, videos..) to avoid shifting layouts
  - Ensure the color contrast you are using is fine: http://leaverou.github.io/contrast-ratio/

## Care about legacy ?

  - Add X-UA-Compatible
```html
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
```
  - If you uses specific features, you can use `@supports` if css only, or https://modernizr.com/ to detect if a feature is available and can fallback.
  - Add shims
```html
<!--[if lt IE 9]>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html5shiv/3.7.3/html5shiv.min.js"></script>
<![endif]-->
```
  - Add polyfills (use cloudflare cdn to grab them)
    - `fetch`: https://github.com/github/fetch [![GitHub stars](https://img.shields.io/github/stars/github/fetch.svg?style=social&label=Star)](https://github.com/github/fetch)
    - `Promise`: https://github.com/stefanpenner/es6-promise [![GitHub stars](https://img.shields.io/github/stars/stefanpenner/es6-promise.svg?style=social&label=Star)](https://github.com/stefanpenner/es6-promise)
    - media queries: https://github.com/scottjehl/Respond [![GitHub stars](https://img.shields.io/github/stars/scottjehl/Respond.svg?style=social&label=Star)](https://github.com/scottjehl/Respond)
    - all es6 and es7 features: https://github.com/zloirock/core-js [![GitHub stars](https://img.shields.io/github/stars/zloirock/core-js.svg?style=social&label=Star)](https://github.com/zloirock/core-js)
    - Web Animations API (`document.querySelector('.pulse').animate({...})`): https://github.com/web-animations/web-animations-js [![GitHub stars](https://img.shields.io/github/stars/web-animations/web-animations-js.svg?style=social&label=Star)](https://github.com/web-animations/web-animations-js)
    - You can also not bundle them and let the browser download what's needed with https://qa.polyfill.io/v2/docs/examples
  - Handle modules and fallback to non modules
```html
<script type="module" src="main.mjs"></script>
<script nomodule type="javascript" src="fallback.js"></script>
```  

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
  - Resource preload (high priority) Download a resource right now (into cache) if we know we'll need it. It supports media queries (for instance, if you want to preload an image that is available in 3 formats, according to the screen max-width)
```html
<link rel="preload" href="image.png" as="image" media="(min-width: 1024px)">
```  
  - Module preload (mjs)
```html
<link rel="modulepreload" href="lib.mjs">
```  
  - Resource Prefetch (low priority). Download a resource (into the cache) when the browser will have time, if we know it's going to be used later.
```html
<link rel="prefetch" href="image.png">
```
  - ~~Subresource (deprecated, not supported anymore): Download directly (high priority, whereas prefetch is low priority) a resource that will be discovered later in the page (such as `<script>` at the end)~~ Use preload.
```html
<link rel="subresource" href="app.js">
```
  - Prerender. Fetch the whole content of another page (css, process js etc.). Useful when you know that the user will click on it for sure. It will take only a instant (everything will be already loaded!)
```html
<link rel="prerender" href="http://example.com/about">
```
  - `defer` (if the script rely on DOM) or `async` (totally independant) your `<script>`s if possible
  - Think about the critical css path. Inject it in the `<head>` directly
  - Move non critical stylesheets outside of the `<head>` (it blocks the first paint otherwise)
  - Shrink your js/css bundles
    - Split the libs bundle(s) (rarely changed) from the app bundle(s)
    - Analyze your js bundle, to be sure you are not bundling crap (using [source-map-explorer](https://github.com/danvk/source-map-explorer for instance))
  - Batch layout trashing using [fastdom](https://github.com/wilsonpage/fastdom)
  - Load unnecessary modules after the initial rendering
  - Delay if not in the viewport at first sight
    - Load images lazily
  - Use `requestAnimationFrame` to handle any animation if using JS (avoid `setTimeout`/`setInterval`) https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution
  - Use `requestIdleCallback` when you want to process something not time critical https://developers.google.com/web/updates/2015/08/using-requestidlecallback
  - Avoir layout trashing (write/read DOM continuously). Batch. And simply let the frameworks (if available) update the DOM for you (with the Virtual DOM nowadays)
  - Gzip your resources
  - Optimize your images and your SVGs https://jakearchibald.github.io/svgomg/ http://getoptimage.com/ or https://imageoptim.com (or using js pluggable things https://github.com/imagemin/imagemin). More recently: Guetzli seems the best for jpgs: https://github.com/google/guetzli
  - Use `async` and `defer` on your scripts when it's possible
  - To do something before unloading the page, use the navigator beacon to not block the closing https://developer.mozilla.org/en-US/docs/Web/API/Navigator/sendBeacon
  - Only import the necessary font families and weights
```html
<link rel="stylesheet" href="//fonts.googleapis.com/css?family=Open+Sans:300">
```
  - Better, don't use `<link rel="stylesheet" ..>`, it's a blocking resource download. Try to inline into `<style>`.
  - For classic js third party libraries, use a cdn (unpkg, cdnjs, jsdelivr, maxcdn..)
  - Use a generic CDN for your resources like [Cloudflare](https://www.cloudflare.com/)
  - If you don't want an external CDN, install a "HTTP Web Accelerator" like Varnish to cache static resources server side and serve them faster
  - Use `rel="noopener"` for external links: `<a href="http://example.com" target="_blank" rel="noopener">`
  - Check your performances with https://testmysite.io/ 
  - Check your performances and best practices with Lighthouse: https://github.com/GoogleChrome/lighthouse [![GitHub stars](https://img.shields.io/github/stars/GoogleChrome/lighthouse.svg?style=social&label=Star)](https://github.com/GoogleChrome/lighthouse) (test if you app can be considered as "progressive")
  - Analyze what changes in the DOM with Chrome extensions such as DOMListener and http://google.github.io/tracing-framework/ if you want the best!
  - Evaluate your bloat score http://www.webbloatscore.com/
  - Async image processing to get better responsiveness `<img decoding=async src="...">` ([proposal](https://groups.google.com/a/chromium.org/forum/#!msg/blink-dev/MbXp16hQclY/bQjegyrbAgAJ))
  - Load lazily your resources
```js
// js modules here (or dynamic imports, à la webpack)
const module = await import('more.mjs')
module.something()
```
	
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
  - Use AMP https://www.ampproject.org/. It adds constraints and tons of tricks to get über-fast pages.
  - Check if your website is mobile friendly: https://search.google.com/search-console/mobile-friendly?hl=en-US

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
  - Alternatives: https://amplitude.com/ https://mixpanel.com/ http://gaug.es/ https://piwik.org/
  - You have a bunch of tags ? Use Google Tag Manager.
  - You can also (or) add a Facebook Pixel to get stats, events.. and even target Facebook users afterwards. (if they visited your pixel) https://www.facebook.com/ads/manager/pixel/facebook_pixel/ 
 
## Care about bugs ?

  - Monitor browser's JS errors, for instance with https://trackjs.com, https://sentry.io/, or https://bugsnag.com/, or https://logrocket.com/
  - Check your Google Webmaster for crawl/sitemap/robots errors

## Care about misc ?

  - If the website sets cookies to an EU visitor, you must display a notice https://www.cookielaw.org/the-cookie-law/. You can use https://silktide.com/tools/cookie-consent/ to make it nice!
  - Add a legal mentions page
  - Add Google notranslate to avoid Google to display the translation bar when you know it's not needed
```html
<meta name="google" value="notranslate">  
```
  - Try to make your website working without JS (use server-side rendering)
  - Validate your pages syntax using https://validator.w3.org/nu/?doc=http%3A%2F%2Fwww.twitter.com to quickly grab some bits of warnings and things not properly HTML standard or things you forgot. You probably have errors but it's because it's a bit too old and strict. ;-)
  - Use a clear and proper wording in your website. Check this out: https://uxplanet.org/effective-writing-for-your-ui-things-to-avoid-f6084e94e009  
  - Consider having a way for users to send you some feedbacks. I would recommend something like https://doorbell.io/. A feedback widget easy to integrate.
  - Consider having a status page status.mywebsite.com to expose your services health using https://www.statuspage.io/


# More tips

https://developers.google.com/speed/docs/insights/rules
https://www.awesomeweb.com/blog/make-website-awesome


