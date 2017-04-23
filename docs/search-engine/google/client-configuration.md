page_description: Tune the google client to fit your own needing.

# Google Client Configuration

<p>
</p>

---

Back to the [**general google documentation**](../google.md).

---

## User agent

**Setting an user agent is very important**. 
By default it will be the one from the http client, and google would easily detect your script as a bot.

To avoid that it happens configure an user agent:

```php
    $googleClient->request->setUserAgent('user agent string');
```

You should **always** set a **real** user agent. Here are a few user agent lists:

- [from Chrome](http://www.useragentstring.com/pages/Chrome/)
- [from Firefox](http://www.useragentstring.com/pages/Firefox/)
- [from Opera](http://www.useragentstring.com/pages/Opera/)
- [from IE](http://www.useragentstring.com/pages/Internet%20Explorer/)


## Proxy usage

You can use a proxy at the request time

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleUrl = new GoogleUrl();
    $google->setSearchTerm('simpsons');
    
    $cookieJar = new Proxy('1.1.1.1', 8080);
    
    $response = $googleClient->query($googleUrl, $proxy);
```

Read the [proxy doc](/proxy.md) to learn more about proxy creation


## Cookie usage

!!! Warning
    Cookies usage is still at prototype stage and all http engines do not support cookies yet.

The google client can share cookies across several request, thus your navigation will evolve and persist across
many requests.

Like proxies, cookies are request dependent

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;
    use Serps\Core\Cookie\ArrayCookieJar;
    

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleUrl = new GoogleUrl();
    $google->setSearchTerm('simpsons');
    
    $cookieJar = new ArrayCookieJar();
    
    // After the query to google cookie jar will evolve according to the cookies from the request
    $response = $googleClient->query($googleUrl, $cookieJar);
```


View the dedicated [cookie documentation](/cookies.md) to learn more about cookies manipulation.




## Solve a Captcha

Solving captcha is not implemented at the moment


<br/>

Next step: [**parse a page**](parse-page.md)