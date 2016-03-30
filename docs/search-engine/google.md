page_description: All about scrapping google with SERPS - Generate URLs and analyse the google pages.

Google Client
=============

<img class="frameless-image" alt="Google logo" src="../../images/logo-google.png"/>

<center>Everything about the google client</center>

---

Jump to:

- [**Parse a google page**](google/parse-page.md)
    - [**Parse natural results**](google/parse-page.md#natural-results)
    - [**Parse adwords results**](google/parse-page.md#adwords-results)
    

---

Installation
------------

The google client is available with the package 
[serps/search-engine-google](https://packagist.org/packages/serps/search-engine-google): 

``$ composer require 'serps/search-engine-google'``

Overview
--------

The google client needs a http client interface to be constructed and an url to be parsed.

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;

    // Create a google client using the curl http client
    $googleClient = new GoogleClient(new CurlClient());

    // Create the url that will be parsed
    $googleUrl = new GoogleUrl();
    $google->setSearchTerm('simpsons');
    
    $response = $googleClient->query($googleUrl);
    
    $results = $response->getNaturalResults();
    
    foreach($results as $result){
        // Do stuff
    }
```

Working with urls
-----------------

``GoogleUrl`` class offers many convenient tools to work with google urls.

### Create an url

The url builder has the required tools to build an url from scratch.

```php
    use Serps\SearchEngine\Google\GoogleUrl;
    
    $googleUrl = new GoogleUrl();
    $googleUrl->setSearchTerm('simpsons');
    $googleUrl->setLanguageRestriction('lang_en');
    echo $googleUrl->buildUrl();
    // https://google.com/search?q=simpsons&lr=lang_en
```

### Url from a string

It's also possible to parse an an existing google url string to an url object.

```php
    use Serps\SearchEngine\Google\GoogleUrl;
    
    $googleUrl = GoogleUrl::fromString('https://google.com/search?q=simpsons');
    echo $googleUrl->getSearchTerm();
    // simpsons
```

Additionally you can continue to manipulate this url

```php
    $googleUrl->setLanguageRestriction('lang_en');
    echo $googleUrl->buildUrl();
    // https://google.com/search?q=simpsons&lr=lang_en
```



### Google domain

By default an url is generated for ``google.com`` but you can choose any domain of your choice:

```php
    use Serps\SearchEngine\Google\GoogleUrl;
    
    $googleUrl = new GoogleUrl('google.fr');
    $googleUrl->setSearchTerm('simpsons');
    echo $googleUrl->buildUrl();
    // https://google.fr/search?q=simpsons
```

It's also possible to modify it latter

```php
    $googleUrl->setHost('google.de');
    echo $googleUrl->buildUrl();
    // https://google.de/search?q=simpsons
```

### Add and remove parameters

It's possible to add or remove parameters

```php
    use Serps\SearchEngine\Google\GoogleUrl;
    
    $googleUrl = new GoogleUrl();
    $googleUrl->setParam('q', 'simpsons');
    $googleUrl->setParam('start', 11);
    echo $googleUrl->buildUrl();
    // https://google.com/search?q=simpsons&start=11
    
    $googleUrl->removeParam('start');
    echo $googleUrl->buildUrl();
    // https://google.com/search?q=simpsons
```

### Raw parameters

By default parameters are encoded for urls. For instance ``"Homer Simpsons"`` will become ``"Homer+Simpsons"``
but ``"Homer+Simpsons"`` will become ``"Homer%2BSimpson"``

```php
    use Serps\SearchEngine\Google\GoogleUrl;
    
    $googleUrl = new GoogleUrl();
    $googleUrl->setParam('q', 'Homer+Simpson');
    echo $googleUrl->buildUrl();
    // https://google.com/search?q=Homer%2BSimpson
```
 
It's possible to deal with raw params, this way the param will be passed to the url with no additional encoding. 
That is achieved by passing true as the third argument of ``setParam``.

```php
    $googleUrl = new GoogleUrl();
    $googleUrl->setParam('q', 'Homer+Simpson', true);
    echo $googleUrl->buildUrl();
    // https://google.com/search?q=Homer+Simpson
```

### More parameters 

Some parameters are very common and for some of them we created convenient shortcuts.

TODO

Parsing results
---------------

The google client is also responsible for parsing the page and outputting a standard result set.

A page can return different kind of results. For convenience the guide for parsing a page has its own page: [**view the guide**](google/parse-page.md)


Proxy usage
-----------

You can use a proxy at the request time


```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;
    use Serps\Core\Http\Proxy;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleUrl = new GoogleUrl();
    $google->setSearchTerm('simpsons');
    
    $proxy = new Proxy('1.1.1.1', 8080);
    
    $response = $googleClient->query($googleUrl, $proxy);
```

Cookie usage
------------

The google client can persist cookies across several requests. 
By default it is disabled, to enable it, simply do:

```php
    $googleClient->enableCookies();
```

And to disable it again:

```php
    $googleClient->disableCookies();
```

You can also get the current state of the cookieJar:

```php
    $googleClient->getCookieJar();
```

Or set a custom CookieJar


```php
    $googleClient->setCookieJar($cookieJar);
```


View the dedicated [cookie documentation](/cookies.md)



Solve a Captcha
---------------

Solving captcha is not implemented at the moment