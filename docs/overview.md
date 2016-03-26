Overview
========

This overview will help you to understand how the library is built and what are its main components.

Install
-------

Two work with SERPS you need two things:

- One or more search engine clients you want to parse
- An http clients


[Composer](https://getcomposer.org/) is required to manage the necessary dependencies.

> <sub>Example with the **Google client** and the **Curl http client**</sub>

```json
{
    "require": {
        "serps/search-engine-google": "*",
        "serps/http-client-curl": "*"
    }
}
```

Search Engine client
--------------------
In a regular workflow a search engine client allows to:

- Manipulate an url and generate a request specific to the search engine
- Retrieve the response from the search engine
- Parse this response to a standard sets of results

Each **search engine** has its set of specificities and thus each search engine implementation has its own dedicated guide.

Check the list of from the top menu.

Http client
-----------

Working with search engines involves to work with **http requests** to be able to interact with them.
Ussually the **search engine client** will need a http client to work correctly.

> <sub>Example with the **google client** and the **curl http client**</sub>

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;

    $googleClient = new GoogleClient(new CurlClient());
```

Check the list of [**available http clients**](http-client.md).

Proxies
-------

When you deal with a very **large number of requests** solving captcha is not enough, you will need to send requests
through proxies.

This is a major feature of scraping and we placed proxies at the very heart of the library. 
We made the choice to make each request being proxy aware. 
This way with a single client you can use as many proxies as you want.


> <sub>Example of **proxy** usage with the google client</sub>

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleClient->query($googleUrl, $proxy);
```

Read more about [**proxies**](proxies.md).


Captcha
-------

As said before, most of time search engines don't want you to parse them, additionally if you submit a lot of 
requests to them, they might - *they will* - detect you as a bot and they will send you a **captcha** that you have
to solve before you continue.


> <sub>Example of **captcha handling**s with the google client</sub>

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\Exception\CaptchaException;
    use Serps\Exception\CaptchaException;

    $googleClient = new GoogleClient(new CurlClient());
    
    try{
        $googleClient->query($googleUrl, $proxy);
    }catch(CaptchaException $e){  
        $captcha = $e->getCaptcha();
    }
```

Read more about [**captcha**](captcha.md).


### Proxies and captcha

When a proxy got blocked by a captcha it's important to solve the captcha with the proxy, if you don't, the search
engine might detect it's not the same ip that solved the captcha and wont accept to solve it.


