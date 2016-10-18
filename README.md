# making-a-website

- Care about security ?
  - Use HTTPS (add a letsencrypt certificate, renew every 3 months automatically)
    - get A+ on https://www.ssllabs.com/ssltest/index.html
    - check more with https://certlogik.com/ssl-checker/
  - Add all security headers
    - get A+ on [securityheaders.io](securityheaders.io)
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
- Care about social ?
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
- Care about SEO ?
  - Use HTTPS
  - Isomorphic/Universal Javascript (prerendering page)
- Care about communication ?
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
  - UTF8 content
  - Language
  - Sitemap
  - Favicons/Tiles (+ Apple/Windows variations)
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
  - Subresource (high priority)
  - XFN (human relationships)
- Care about accessibility (a11y) ?
  - Audit your website: https://github.com/addyosmani/a11y
  - Use role="xxx"
- Care about style ?
  - Add viewport meta mobile compliant
- Care about legacy ?
  - Add X-UA-Compatible 
  - Add shims
  ```html
<!--[if lt IE 9]>
<script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
  ```
- Care about performance ?
  - DNS Prefetch <link>
  - DNS+Handshake+TLS Preconnect <link>
  - Resource Prefetch (low priority)
  - Prerender <link>
  - Think about the critical css path
  - Shrink your js/css bundles
  - Load unnecessary modules after the initial rendering
  - Delay if not in the viewport at first sight
  - GZip your resources
  - Optimize your images and your SVGs https://jakearchibald.github.io/svgomg/
- Care about analytics ?
  - Google Webmaster
  - Google analytics
- Care about bugs ?
  - Monitor browser's JS errors, for instance with https://trackjs.com
- Care about misc ?
  - Google notranslate
