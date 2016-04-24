page_description: Get started using PhantomJS to query search engines - The search engine Spider
page_title: Using PhantomJS

PhantomJS HTTP Client
=====================

<img class="frameless-image" alt="PhantomJS logo" src="../../images/phantomjs.png"/>

<center>[PhantomJS](http://phantomjs.org/) is a webkit implementation that helps to simulate the real browser.</center>

---

By using this client you will execute the inner javascript code and make the DOM as real as in the true browser,
that can be required for some search engines to work properly.

!!! warning "Notice about cookies"
    At the current state phantomJS adapter does not support internal cookieJar usage.

Installation
------------

The client is available with the package 
[serps/http-client-phantomjs](https://packagist.org/packages/serps/http-client-phantomjs): 

``$ composer require 'serps/http-client-phantomjs'``

### Additional requirement

**[PhantomJS](http://phantomjs.org/) binaries have to be installed** to use the client. The process for installing
it depends on your environment, you will find further guides on the internet. 

The following PhantomJs version were tested with the library:

- 1.9.7
- 1.9.8
- 2.0.0
- 2.1.0

!!! note
    The package [jakoch/phantomjs-installer](https://github.com/jakoch/phantomjs-installer) 
    can help you to manage phantomJS as a dependency of your project.

## Usage

```php
use Serps\HttpClient\PhantomJsClient;

// The constructor accepts 1 optional parameter that is the path to the phantomjs binaries (default to 'phantomjs')
$client = new PhantomJsClient();
```
