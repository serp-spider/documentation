Overview
========

This overview will help you to understand how the library is built and what are its main components.

---

Install
-------

Two work with SERPS you need two things:

- One or more search engine clients you want to parse
- A http client


In addition [Composer](https://getcomposer.org/) is required to manage the necessary dependencies.

> <sub>composer.json example with the **Google client** and the **Curl http client**</sub>

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

These search engines are currently available:

- [Google](search-engine/google.md)


Http Client
-----------

Working with search engines involves to work with **http requests** to be able to interact with them.
Ussually the **search engine client** will need a http client to work correctly.

> <sub>Example with the **google client** and the **curl http client**</sub>

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;

    $googleClient = new GoogleClient(new CurlClient());
```

These http clients are currently available:

- [CURL](http-client/curl.md)
- [phantomJS](http-client/phantomJS.md)


Proxies
-------

As said before, most of time search engines don't want you to parse them.
When you deal with a very **large number of requests**, you will need to send requests
through proxies.

This is a major feature of scraping and we placed proxies at the very heart of the library. 
Each request is proxy aware. 
This way with a single client you can use as many proxies as you want.


> <sub>Example of **proxy** usage with the google client</sub>

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleClient->query($googleUrl, $proxy);
```