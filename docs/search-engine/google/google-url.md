page_description: With SERPS it's easy and semantic to create urls to browse google.

Work with google urls
=====================

SERPS gives you the tools you need to create urls for google.

---

Back to the [**general google documentation**](../google.md).

---


The class ``GoogleUrl`` offers many convenient tools to work with google urls. See how it works with examples.

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

Some parameters are very common and for some of them we created convenient shortcuts. See the list:

#### setSearchTerm

``$url->setSearchTerm($searchTerm)``

Sets the keywords to search for. Modifies the value for the ``q`` parameter.

#### setPage

``$url->setPage($pageNumber)``

Sets the page to parse (starts at 1). Modifies the value for the ``start`` parameter.

!!! Note
    If value is less than 1, the param ``start`` will be removed from the url.

#### setResultsPerPage

``$url->setResultsPerPage($resultsPerPages)``

Sets the number of results per pages (between 1 and 100). Modifies the value for the ``num`` parameter.


#### setLanguageRestriction

``$url->setLanguageRestriction($lang)``

Sets language of the results. Modifies the value for the ``lr`` parameter. e.g  ``"lang_en"``. 

!!! Note
    ``"lang_"`` will be automatically prepended if it is not present. 
    
    That means that ``$url->setLanguageRestriction('en')``
    and ``$url->setLanguageRestriction('lang_en')`` do the same thing.

#### setAutoCorrectionEnabled

``$url->setAutoCorrectionEnabled($enabled)``

Sets if the auto correction should be enabled (``true`` or ``false``). Modifies the value for the ``nfpr`` parameter.
