Google Client
=============

Installation
------------

The google client is available with the package 
[serps/search-engine-google](https://packagist.org/packages/serps/search-engine-google): 

``$ composer require 'serps/search-engine-google'``

Overview
--------

The google client needs an http client interface to be constructed and an url to be parsed

> <sub>Overview of querying google for the keyword 'simpsons' and getting the natural results</sub>

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleUrl = new GoogleUrl();
    $google->setSearchTerm('simpsons');
    
    $response = $googleClient->query($googleUrl);
    
    $results = $response->getNaturalResults();
    
    foreach($results as $result){
        $resultTitle = $result->getDataValue('title');
    }
```


**TODO**