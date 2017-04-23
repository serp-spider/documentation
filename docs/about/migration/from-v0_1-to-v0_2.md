Migration from v0.1 to v0.2
===========================

!!! Warning "Version not released yet"
    Be aware that this version is yet under development and this migration guide is a draft that might evolve until
    the version is published.

Version 0.2 overview
--------------------

The version 0.2 brings some improvements that target the future of the library. 

- The version brings better tools to work with captcha. 
- Some parts from google package are moved to core package (like css parser)
in order to make them ready for implementing new search engines. These change should be invisible to you.
- It makes browser emulation more transparent by implementing a browser class that aims to mimic real browsers.
- Url interface was refactored to be more flexible.
- There are also other changes, see the following guide for a review of the upgrade.



## Dependencies

Css parser moved from google package to core:

- ``serps/search-engine-google`` does not depend on ``symfony/css-selector`` anymore
- ``serps/core`` now depends on ``symfony/css-selector:^2|^3``

Remove PSR-7 implementation:

- **IN PROGRESS** ``serps/search-engine-google`` does not depend on ``zendframework/zend-diactoros`` anymore,
instead you need to add either ``zendframework/zend-diactoros`` or ``guzzle/psr7`` to your dependency. 

## Request Builder

Being said in the previous paragraph, serps does not provide a PSR-7 implementation anymore. Until now we forced the
users to use zend-diactoros, now the choice of the PSR-7 implementation will be up to the user. 
The goal was to let user choose what implementation to depend on. To make it worth serps now provides 
a request builder that will automatically detect what package is installed in your dependencies and create
a request object for you:

```php
    use Serps\Core\Psr7\RequestBuilder;
    
    $request = RequestBuilder::buildRequest('http://foo.bar', 'GET');
```

In most cases this API change will be invisible to you.


## Google: client update

This is probably the most sensible change from the upgrade. The google client changed the way it builds requests.

Instead of giving it a **http client** at construction and **proxy** and **cookie jar** at request time, you will now
give it a default browser instance that wraps all of these three items and that can be overriden at request time.
In other words the constructor moved from 

```php
    new GoogleClient(HttpClientInterface $client)
```
     
to 

```php
    new GoogleClient(BrowserInterface $browser = null)
``` 


And the query method moved from 

```php
    GoogleClient::query(GoogleUrlInterface $googleUrl, Proxy $proxy = null, CookieJarInterface $cookieJar = null)
```
                                                                                                                    
to 

```php
    GoogleClient::query(GoogleUrlInterface $googleUrl, BrowserInterface $browser = null)
```

In addition property ``$googleClient->request``  and method ``GoogleClient::getRequestBuilder()`` do not exist anymore,
they are fully replaced by the browser implementation.

The reason of this change is that google client was managing the request by itself. Making it hard to keep a consistent
browser behaviour from different places and that was an issue for managing captcha that need to have the same
browser environment (user-agent string, cookies, proxy...).
 
Now the google client acts more like a real browser, leaving all the request logic to the browser and it will only
manage the url given to the browser and the response returned from the browser.

Here is an example of how to use it before and after.

Before:

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;
    
    $userAgent = "Mozilla/5.0 (Windows NT 10.0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.93 Safari/537.36";
    $googleClient->request->setUserAgent($userAgent);
    
    $googleClient = new GoogleClient(new CurlClient());

    $response = $googleClient->query($googleUrl, $someProxy, $someCookies);
```

After:

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\Core\Browser\Browser;
    use Serps\SearchEngine\Google\GoogleUrl;
    
    $userAgent = 'Mozilla/5.0 (Windows NT 10.0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/40.0.2214.93 Safari/537.36';
    $language = 'en-US,en;q=0.8';
    
    $browser = new Browser(new CurlClient(), $userAgent, $language, $someProxy, $someCookies);
   
    $googleClient = new GoogleClient($browser);
    $response = $googleClient->query($googleUrl);
    
    // Alternatively you can pass browser at request time:
    $googleClient = new GoogleClient();
    $response = $googleClient->query($googleUrl, $browser);
```

As you can see the browser instance manages most of what a real browser would manage (at least for request):

- User agent string
- Language headers (in previous version that was automatically set from the language detected in the google url) 
- Additional headers
- Proxy
- Cookie

!!! Warning
    Although the class is named ``Browser`` it does not mean that it will parse the css and javascript of the page.
    The name browser was chosen because it's an object that encapsulates all the request logic and that is able to 
    manage cookies and proxies as a real browser would do. 
    
    **In any case** this class is capable to parse javascript and css or to render the html by itself .


## Google: CSS Parser

The css parser is not available from google package anymore, if you were using it you will need to rely on the 
replacement from the core package.

Before:

```php
Serps\SearchEngine\Google\Css:::toXPath('.expression');
```

After:

```php
Serps\Core\Dom\Css:::toXPath('.expression');
```

## Google: image results

Thanks to the addition of the [MediaInterface](#MediaInterface) serps makes it easy to work with image results parsed 
from google. 

With the previous version image result format was unpredictable: that could be an url, or a base64 image... 
now you wont have to care anymore about this. The only thing in you need to care about is how to store the image.

Concretely the media interface will detect for you the type of image and gives you the possibility to get it
either as a stream, as binary data or as a base64 string or to save it in a file:

```
    $result->image->saveFile('myImage.png');
```

## Google: drop support for raw parser

At the very beginning of serps we though it would make sense to provide a raw parser for raw google pages 
(no-javascript pages), but there is actually no use for it. Even the results from curl are the same as the javascript ones
and this is due to the fact that the page needs to be evaluated to show a javascript disabled version (curl does 
not evaluate the page, thus it returns almost the same version as the javascript-enabled one).

Maintaining a raw parser was a lot of efforts for a very few results. We simply decided to drop support for the raw parser.

## Google: additional changes:

- ``Serps\SearchEngine\Google\GoogleDom`` now extends ``Serps\Core\Dom\WebPage`` 
- ``Serps\SearchEngine\Google\GoogleError`` now extends ``Serps\Core\Dom\WebPage`` 
    and does not extend ``Serps\SearchEngine\Google\GoogleDom`` anymore
- Google cards results are now supported
- Mobile page detection: GoogleSerp::isMobile() 
- Mobile results have now their own parser
- Large video have the CLASSICAL type as mentioned in the doc (bugfix)

## URL

The url interface changed. Most of the changes are internal and you should not be concerned by it.

In case you played with the url, 
see the [core changelog](https://github.com/serp-spider/core/blob/master/CHANGELOG.md) for more details

## Cookie expiration time

Cookie expiration time was not standard and was invalid most of time. Now cookie expiration is correctly managed.