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

You should **always** set a **real** user agent. Here are a fow user agent lists:

- [from Chrome](http://www.useragentstring.com/pages/Chrome/)
- [from Firefox](http://www.useragentstring.com/pages/Firefox/)
- [from Opera](http://www.useragentstring.com/pages/Opera/)
- [from IE](http://www.useragentstring.com/pages/Internet%20Explorer/)


## Cookie usage

!!! Warning
    Cookies usage is still at prototype stage and all http engines do not support cookies yet.

The google client can share cookies across several request, thus the state of the client will evolve and persist across
many requests.

By default it is disabled, to enable it, simply do:

```php
    $googleClient->enableCookies();
```

And to disable it again:

```php
    $googleClient->disableCookies();
```

By default the cookie jar is empty, but you can pass a custom cookie jar:


```php
    $googleClient->setCookieJar($cookieJar);
```

This way, it's possible to share the cookie jar with other client:


```php
    $cookieJar = $googleClient->getCookieJar();
    $otherGoogleClient->setCookieJar($cookieJar);
```


View the dedicated [cookie documentation](/cookies.md) to learn more about cookies manipulation.



## Proxy usage

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



## Solve a Captcha

Solving captcha is not implemented at the moment


<br/>

Next step: [**parse a page**](parse-page.md)