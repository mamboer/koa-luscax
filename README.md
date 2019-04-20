# koa-xlusca

[![NPM version][npm-image]][npm-url]
[![build status][travis-image]][travis-url]
[![Test coverage][coveralls-image]][coveralls-url]
[![David deps][david-image]][david-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/koa-xlusca.svg?style=flat-square
[npm-url]: https://npmjs.org/package/koa-xlusca
[travis-image]: https://img.shields.io/travis/mamboer/koa-xlusca.svg?style=flat-square
[travis-url]: https://travis-ci.org/mamboer/koa-xlusca
[coveralls-image]: https://img.shields.io/coveralls/mamboer/koa-xlusca.svg?style=flat-square
[coveralls-url]: https://coveralls.io/r/mamboer/koa-xlusca?branch=master
[david-image]: https://img.shields.io/david/mamboer/koa-xlusca.svg?style=flat-square
[david-url]: https://david-dm.org/mamboer/koa-xlusca
[download-image]: https://img.shields.io/npm/dm/koa-xlusca.svg?style=flat-square
[download-url]: https://npmjs.org/package/koa-xlusca

Web application security middleware for the latest koa 2.x.

Fork from [koa-lusca](https://github.com/chrisveness/koa-lusca),

> It's a pity that [koa-lusca](https://github.com/koajs/koa-lusca) is out of maintenances for over 3 years, so i made this fork and re-released it as koa-xlusca, and let's keep it fresh.



## Usage

```js
const Koa = require('koa');
const lusca = require('koa-xlusca');
const app = new Koa();

app.use(lusca({
  csrf: true,
  csp: { /* ... */},
  xframe: 'SAMEORIGIN',
  p3p: 'ABCDEF',
  hsts: { maxAge: 31536000, includeSubDomains: true },
  xssProtection: true,
  referrerPolicy: 'same-origin'
}));
```

Setting any value to `false` will disable it. Alternately, you can opt into methods one by one:

```js
app.use(lusca.csrf());
app.use(lusca.csp({/* ... */}));
app.use(lusca.xframe({ value: 'SAMEORIGIN' }));
app.use(lusca.p3p({ value: 'ABCDEF' }));
app.use(lusca.hsts({ maxAge: 31536000 });
app.use(lusca.xssProtection();
app.use(lusca.referrerPolicy('same-origin'));
```

## API

### lusca.csrf(options)

* `key` String - Optional. The name of the CSRF token added to the model. Defaults to `_csrf`.
* `secret` String - Optional. The key to place on the session object which maps to the server side token. Defaults to `_csrfSecret`.
* `impl` Function - Optional. Custom implementation to generate a token.

Enables [Cross Site Request Forgery](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)) (CSRF) headers.

If enabled, the CSRF token must be in the payload when modifying data or you will receive a *403 Forbidden*. To send the token you'll need to echo back the `_csrf` value you received from the previous request.

### lusca.csp(options)

* `options.policy` Object - Object definition of policy.
* `options.policy` String, Object, or an Array - Object definition of policy. Valid policies examples include:
    * `{"default-src": "*"}`
    * `"referrer no-referrer"`
    * `[{ "img-src": "'self' http:" }, "block-all-mixed-content"]`
* `options.reportOnly` Boolean - Enable report only mode.
* `options.reportUri` String - URI where to send the report data

Enables [Content Security Policy](https://www.owasp.org/index.php/Content_Security_Policy) (CSP) headers.

#### Example Options

```js
// Everything but images can only come from own domain (excluding subdomains)
{
  policy: {
    'default-src': '\'self\'',
    'img-src': '*'
  }
}
```

See the [MDN CSP usage](https://developer.mozilla.org/en-US/docs/Web/Security/CSP/Using_Content_Security_Policy) page for more information on available policy options.

### lusca.xframe(value)

* `value` String - Required. The value for the header, e.g. DENY, SAMEORIGIN or ALLOW-FROM uri.

Enables X-FRAME-OPTIONS headers to help prevent [Clickjacking](https://www.owasp.org/index.php/Clickjacking).

### lusca.p3p(value)

* `value` String - Required. The compact privacy policy.

Enables [Platform for Privacy Preferences Project](http://support.microsoft.com/kb/290333) (P3P) headers.

### lusca.hsts(options)

* `options.maxAge` Number - Required. Number of seconds HSTS is in effect.
* `options.includeSubDomains` Boolean - Optional. Applies HSTS to all subdomains of the host

Enables [HTTP Strict Transport Security](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security) for the host domain. The preload flag is required for HSTS domain submissions to [Chrome's HSTS preload list](https://hstspreload.appspot.com)

### lusca.xssProtection(options)

* `options.enabled` Boolean - Optional. If the header is enabled or not (see header docs). Defaults to `1`.
* `options.mode` String - Optional. Mode to set on the header (see header docs). Defaults to `block`.

Enables [X-XSS-Protection](http://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-iv-the-xss-filter.aspx) headers to help prevent cross site scripting (XSS) attacks in older IE browsers (IE8)

### lusca.cto()

Enables [X-Content-Type-Options](https://blogs.msdn.microsoft.com/ie/2008/09/02/ie8-security-part-vi-beta-2-update/) header to prevent MIME-sniffing a response away from the declared content-type.

### lusca.referrerPolicy(value)

* `value` String - Optional. The value for the header, e.g. `origin`, `same-origin`, `no-referrer`. Defaults to `` (empty string).

Enables [Referrer-Policy](https://www.w3.org/TR/referrer-policy/#intro) header to control the Referer header.

## License

- Original License: Apache License, Version 2.0, Copyright (C) 2014 eBay Software Foundation
- Now: [MIT](LICENSE.txt)

## Origin Contributors Of koa-lusca

- Jeff Harrell <jeff@juxtadesign.com> (https://github.com/jeffharrell)
- Jeff Harrell <jeharrell@paypal.com>
- Erik Toth <ertoth@paypal.com>
- rragan <rragan@ebay.com>
- skoranga <skoranga@paypal.com>
- Lenny Markus <lmarkus@paypal.com> (https://github.com/lmarkus)
- totherik <totherik@gmail.com>
- Lenny Markus <lensam69@yahoo.com>
- Trevor <tlivings@gmail.com>
- Steve Stedman <sstedman@paypal.com>
- AlexSantos <santosam72@gmail.com>
- mstuart <stuartmark@gmail.com>
- swesthafer <swesthafer@paypal.com>
- Dmitry Shirokov <deadrunk@gmail.com> (https://github.com/runk)
- Sahat Yalkabov <sakhat@gmail.com>
- fengmk2 <fengmk2@gmail.com> (https://github.com/fengmk2)
- Anant Singh <anantsingh@paypal.com>
- Aria Stewart <aredridel@dinhe.net> (https://github.com/aredridel)
- Jean-Charles Sisk <jasisk@gmail.com>
- Matt Edelman <medelman@paypal.com>
- Ilya Radchenko <ilya@burstcreations.com>
- Poornima Venkat <pvenkatakrishnan@paypal.com>
- ali-sdk <m@fengmk2.com> (https://github.com/ali-sdk)
- fundon <cfddream@gmail.com>
- Christoffer Hallas <hallas@users.noreply.github.com>
- Geller <geller@Gellers-MacBook-Pro.local>
- Marek Fajkus <marek.faj@gmail.com> (https://github.com/turboMaCk)
- Shawn <shaoshuai0102@gmail.com> (https://github.com/shaoshuai0102)
- Chris Veness <chrisv@movable-type.co.uk>

