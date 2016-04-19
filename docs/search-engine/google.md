page_description: Al about scrlapping google with SERPS - Generate URLs and analyse the google pages.
page_title: Google Client Overview

Google Client
=============

<img class="frameless-image" alt="Google logo" src="../../images/logo-google.png"/>

<center>Everything about the google client</center>

---


- [**Installation**](#installation)
- [**Overview**](#overview)
- [**Create urls**](google/google-url.md)
- [**Configure the google client**](google/client-configuration.md)
- [**Parse a google page**](google/parse-page.md)
- [**Handle errors**](google/handle-errors.md)

---

Installation
------------

The google client is available with the package 
[serps/search-engine-google](https://packagist.org/packages/serps/search-engine-google): 

``$ composer require 'serps/search-engine-google'``


!!! Note "Important note agout google updates"
    We don't know when google dom updates, but when it does, we will place as many efforts as possible
    to get the client updated.
    
    To make sure you are always up to date we advice you to 
    [watch the repository on github](https://github.com/serp-spider/search-engine-google/subscription).
    Not only you will be notified when a release is published, but you will also know when a bug is detected and 
    reported on the issue tracker.

Overview
--------

The google client needs a http client interface to be constructed and an url to be parsed.

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;

    // Create a google client using the curl http client
    $googleClient = new GoogleClient(new CurlClient());
    
    // Tell the client to use a user agent
    $userAgent = "Mozilla/5.0 (Windows NT 10.0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.93 Safari/537.36";
    $googleClient->request->setUserAgent($userAgent);

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

Next step: [**configure the client**](google/client-configuration.md)