page_description: Al about scrlapping google with SERPS - Generate URLs and analyse the google pages.
page_title: Google Client Overview

Google Client
=============

<img class="frameless-image" alt="Google logo" src="../../images/logo-google.png"/>

<center>Everything about the google client</center>

---


- [**Installation and overview**](#installation)
- [**Configure the google client**](google/client-configuration.md)
- [**Create urls**](google/google-url.md)
- [**Parse a google page**](google/parse-page.md)
- [**Handle errors**](google/handle-errors.md)

---

Installation
------------

The google client is available with the package 
[serps/search-engine-google](https://packagist.org/packages/serps/search-engine-google): 

``$ composer require 'serps/search-engine-google'``

In addition you will require to install either zend-diactoros or guzzle/psr7 to provide a psr7 implementation

``$ composer require 'zendframework/zend-diactoros'``

or 

``$ composer require 'guzzlehttp/psr7'``

!!! Note "Important note agout google updates"
    We don't know when google dom updates, but when it does, we will place as many efforts as possible
    to get the client updated.
    
    To make sure you are always up to date we advice you to 
    [watch the repository on github](https://github.com/serp-spider/search-engine-google/subscription).
    Not only you will be notified when a release is published, but you will also know when a bug is detected and 
    reported on the issue tracker.

Overview
--------

The google client needs a browser to be constructed and an url to be parsed.

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;
    use Serps\Core\Browser\Browser;
    
    $userAgent = "Mozilla/5.0 (Windows NT 10.0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.93 Safari/537.36";
    $browserLanguage = "fr-FR";

    $browser = new Browser(new CurlClient(), $userAgent, $browserLanguage);

    // Create a google client using the browser we configured
    $googleClient = new GoogleClient($browser);

    // Create the url that will be parsed
    $googleUrl = new GoogleUrl();
    $googleUrl->setSearchTerm('simpsons');
    
    $response = $googleClient->query($googleUrl);
    
    $results = $response->getNaturalResults();
    
    foreach($results as $result){
        // Analyse the results
    }
```

<br/>

## Disclaimer

> <cite>Using our Services does not give you ownership of any intellectual property rights in 
> our Services or the content you access. 
> You may not use content from our Services unless you obtain permission from its owner or 
> are otherwise permitted by law.</cite>
>
> <small>Extract from [Google terms of services](https://www.google.com/policies/terms/)</small>

When using this software must respect terms of services of third parties like Google.
Serps authors and contributors cannot be hold as liable for the use you make of this software. 


---------------

Next steps: 

- [**Configure the google client**](google/client-configuration.md)
- [**Create urls**](google/google-url.md)
- [**Parse a google page**](google/parse-page.md)
- [**Handle errors**](google/handle-errors.md)