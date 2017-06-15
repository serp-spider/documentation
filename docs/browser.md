page_description: This guide helps you to configure the browsing experience with SERPS - The search engine Spider.


Browser
=======

Guide for browser instance management 

---


In SERPS the ``browser`` is an element that helps to mimic the behavior of a browser. 

Be aware that the name browser stand for the fact that this entity is capable of emitting http request as a real
browser would do, taking care of cookies, proxies, language, etc... but **in any case** this class is able
to render a html page or to evaluate css or javascript.

``Browser`` are available since version ``0.2`` of ``serps/core``. 

## Create a browser

A minimal browser only requires a http client to send request through:
    
```php
use Serps\Core\Browser\Browser;
use Serps\HttpClient\CurlClient;

$browser = new Browser(new CurlClient());
```

## Setting user agent

The next step in configuring the browser is to specify what user agent we want to use:

```php
use Serps\Core\Browser\Browser;
use Serps\HttpClient\CurlClient;

$userAgent = 'some user agent';

$browser = new Browser(new CurlClient(), $userAgent);

// Or set it latter:

$browser->setAcceptLanguage('fr-CA');

```

When setting a user agent you will prefer using a real user agent string.
Here are a few user agent lists:

- [from Chrome](http://www.useragentstring.com/pages/useragentstring.php?name=Chrome)
- [from Firefox](http://www.useragentstring.com/pages/useragentstring.php?name=Firefox)
- [from Opera](http://www.useragentstring.com/pages/useragentstring.php?name=Opera)
- [from IE](http://www.useragentstring.com/pages/useragentstring.php?name=Internet+Explorer)


## Setting the language

The browser is responsible for headers management. The accept-language header is one of the most important and thus
you should specify it when creating a new browser:

```php
use Serps\Core\Browser\Browser;
use Serps\HttpClient\CurlClient;

$language = 'fr-FR';

$browser = new Browser(new CurlClient(), $userAgent, $language);

// Or set it latter:

$browser->setUserAgent('other UA');

```

If you dont specify a language or if you set it to ``null`` the default value ``en-US,en;q=0.8`` will be used.



## Using a cookie jar


!!! Warning
    Cookies usage is still at prototype stage and all http engines do not support cookies yet.

As real browser the browser class has the ability to use a cookie jar:

```php
use Serps\Core\Browser\Browser;
use Serps\HttpClient\CurlClient;

$cookieJar = ....;

$browser = new Browser(new CurlClient(), $userAgent, $language, $cookieJar);

// Or set it latter:

$browser->setCookieJar($otherCookieJar);

// You can also remove it to disable cookies

$browser->setCookieJar(null);

```

By default no cookie jar will be used and requests will be free of cookies.

To learn more on how to create cookie jar, please check [the cookie documentation](cookies.md)




## Using a proxy

It's possible to give a proxy for the browser. The browser will use this proxy for every requests.

```php
use Serps\Core\Browser\Browser;
use Serps\HttpClient\CurlClient;

$proxy = ....;

$browser = new Browser(new CurlClient(), $userAgent, $language, $cookieJar, $proxy);

// Or set it latter:

$browser->setProxy($otherProxy);

// You can also remove it to disable proxy

$browser->setProxy(null);

```

By default no proxy is used.

To learn more on how to create proxies, please check [the proxy documentation](proxy.md)
