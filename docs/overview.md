page_description: Overview of SERPS, minimal informations to get started with scraping search engines - The search engine Spider.

Overview
========

This overview will help you to understand how the library is built and what are its main components.

---

Install
-------

To work with SERPS you need two things:

- One or more search engine clients you want to parse
- A http client


In addition [Composer](https://getcomposer.org/) is required to manage the necessary dependencies.

> <sub>composer.json example with the **Google client** and the **Curl http client**</sub>

```json
{
    "require": {
        "serps/core": "*",
        "serps/search-engine-google": "*",
        "serps/http-client-curl": "*"
    }
}
```

!!! Danger
    The library is still in alpha, no version is released yet that means that minor things can change until 
    the stable release and your code might become not compatible with the updates.

Search Engine client
--------------------
In a regular workflow a search engine client allows to:

- Manipulate an url and generate a request specific to the search engine
- Retrieve the response from the search engine
- Parse this response to a standard sets of results

Each **search engine** has its set of specificities and thus each search engine implementation has its own dedicated guide.

These search engines are currently available:

- [Google](search-engine/google.md)


Http Client
-----------

Working with search engines involves to work with **http requests**.
Usually the **search engine client** will need a http client to work correctly.

> <sub>Example with the **google client** and the **curl http client**</sub>

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;

    $googleClient = new GoogleClient(new CurlClient());
```


There are two kinds of http clients: those that return the raw html as returned from the search engine (e.g the curl client)
and the others that evaluate the javascript and update the DOM before before returning (e.g the phantomJS client)

These http clients are currently available:

- Raw clients:
    - [CURL](http-client/curl.md)
- Evaluating clients
    - [phantomJS](http-client/phantomJS.md)


Proxies
-------

Most of time search engines don't want you to parse them thus 
they use to block you with captcha when they think you are a bot
When you deal with a very **large number of requests**, you will need to send requests
through proxies.

This is a major feature of scraping and we placed proxies at the very heart of the library. 
Each request is proxy aware. 
This way, with a single client you can use as many proxies as you want.


> <sub>Example of **proxy** usage with the google client</sub>

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleClient->query($googleUrl, $proxy);
```

Captcha
-------

Even though you are using proxies and place all the efforts to act like an human, you might encounter the fatal captcha.

When you get **blocked** by a captcha request, it is very important to stop sending request to the search engine and to
solve the captcha before you continue. 

Dealing with captcha is not easy, at the current state the library can detect captcha but is not able to solve them
for you we are **currently working** on a captcha solver implementation.

!!! note
    Captcha are proxy specific, when solving a captcha that should be done with the proxy that was initially blocked

Cookies
-------

SERPS integrates cookie management, that allows to share cookies across many requests.

Cookie management is usually done at the search engine client level. You still want to know
how to manipulate cookies and cookiejars: 
[see cookie documentation](/cookies.md) 