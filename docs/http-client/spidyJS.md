page_description: SpidyJS makes possible to query search engines with a javascript emulated browser built around jsdom.
page_title: SpidyJS Http client

SpidyJS HTTP Client
===================

[Spidy](github.com/serp-spider/spidyjs) is a browser built with javascript. 

---

This adapter allows you to query search engines with spidyJS, it is a javascript headless browser. 

!!! Warning 
    This adapter is still a prototype. Use it with care.

Installation
------------

The client is available with the package 
[serps/http-client-spidyjs](https://packagist.org/packages/serps/http-client-spidyjs): 

``$ composer require 'serps/http-client-spidyjs'``


### Additional requirement

You will also need nodejs and npm to install spidy.

```
$ npm install -g spidy@2
```

If you use a nodejs version that is before 4.0, you will need to install spidy version 1 instead:

```
$ npm install -g spidy@1
```

## Usage

```php
use Serps\HttpClient\SpidyJsClient;


// The constructor accepts 1 optional parameter that is the path to the spidyjs binaries (default to 'spidyjs')
$client = new SpidyJsClient();
```
