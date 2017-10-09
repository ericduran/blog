---
layout: post
comment: 1
title: Add JS Modules support to python's simpleHTTPServer
categories:
- javascript
- python
---

Recently I've been playing a lot with [JavaScript modules](https://caniuse.com/#feat=es6-module) but
every once in a while I start a web server using python SimpleHTTPServer from  the command line while forgetting
it doesn't know how to serve `.mjs` files. I keep running into the following error:

```
Failed to load module script: The server responded with a non-JavaScript MIME type of "application/octet-stream".
Strict MIME type checking is enforced for module scripts per HTML spec.
```

This error makes perfect sense since python doesn't know what the `.mjs` extension is and it uses the default mime type of `application/octet-stream`.

The only problem is that JavaScript modules MUST return a proper JavaScript MIME <sup id="ref-0" class="reference">[<a href="#ref-0">0</a>]</sup> type in order for
the user agent to execute it as JavaScript.

There are multiple ways you can fix this problem but I decided to make python aware of the new extension by
adding a new file called `mime.types` at `/usr/local/etc/mime.types` <sup id="ref-1" class="reference">[<a href="#ref-1">1</a>]</sup> with the following content:

```
# Add proper mime type for mjs files.
application/javascript                          js mjs
```

Now when running `python -m SimpleHTTPServer 8000` or `python3 -m http.server 8000` JavaScript `.mjs` files just work.

##### Reference:
 <a id="ref-0" href="https://html.spec.whatwg.org/multipage/scripting.html#scriptingLanguages">
  [0] https://html.spec.whatwg.org/multipage/scripting.html#scriptingLanguages
 </a>
 <a id="ref-1" href="https://svn.python.org/projects/python/trunk/Lib/mimetypes.py">
  [1] https://svn.python.org/projects/python/trunk/Lib/mimetypes.py
 </a>
