---
title: Uninstalling Angular's service worker
date: 2020-09-05T11:30:58.697Z
author: John Appleseed
summary: How to uninstall Angular's service worker from client machines
tags:
  - angular
  - service worker
  - javascript
---
The default service worker that Angular provides can be difficult to uninstall once it is registered on client browsers

### Turning on the service worker

Steps.

### Removing the service worker

If you have turned on the service worker via Angular configuration, the easiest way to deregister the service worker from client browsers is to update this configuration. 

\[Instructions]

The service worker files (ngsw.json, service-worker.js, safety-worker.js) will not be generated in the build.

The next time the client machine opens the page, the service worker will attempt to hit the ngsw.json file. Since that file is no longer there, it should return a 404, and the service worker should de-register itself. 

#### Issue when servers don't return a 404 for missing assets

Sometimes the server is configured to catch 404s and return something else, eg the index.html page (200). One case where this can happen is if the server is hosted on an S3 bucket, using the hash location strategy and requiring CloudFront to trap all 404s and redirect to the index.html, returning a 200.  In this case, the service worker will not de-register itself.  This can especially bad because the service worker will remain on the client machine, and keep the old version of the site indefinitely and display it each time the site is initially loaded. The page will update itself on manual refresh, but each time the user visits the site after they close the browser or in a new tab, they will see the old version