page_description: Create and use proxy to use them with your browser implementation
 
 
Proxy
=====

Guide to proxy management in SERPS

---


Proxies are standardized in serps and this short guide will show you how to create them.


## Create a proxy

Proxy creation is possible in two ways. You can either build it from a **simple proxy string**, or with **proxy parts**


### Create a proxy from a string

Assuming we have the proxy as a string, for instance with ``196.168.192.168:8080``, you can use this string to 
create the proxy:


```php
 use Serps\Core\Http\Proxy;

 $proxy = Proxy::createFromString('196.168.192.168:8080');
```

By default the proxy will be a ``HTTP`` proxy but serps proxy also accepts ``HTTPS``,
 ``SOCKS4`` and ``SOCKS5`` proxies, see with examples:

```php
 use Serps\Core\Http\Proxy;
 
 $httpProxy = Proxy::createFromString('http://196.168.192.168:8080');
 $httpsProxy = Proxy::createFromString('https://196.168.192.168:8080');
 $socks4Proxy = Proxy::createFromString('socks4://196.168.192.168:8080');
 $socks5Proxy = Proxy::createFromString('socks5://196.168.192.168:8080');
```

You can also add **authentication** details (by default no authentication is set). You can do it by using the form
**authentication**@**host**:

```php
    use Serps\Core\Http\Proxy;
    
    $proxy = Proxy::createFromString('https://user:password@196.168.192.168:8080');
```


### Create a proxy from parts

Now let's consider that you have some proxy parts (ip, port...) that are not put together as a string, you can use
them directly with the proxy constructor to create a new proxy:

```php
    use Serps\Core\Http\Proxy;
   
    $ip   = '192.168.192.168';
    $port = 8080;
     
    $proxy = new Proxy($ip, $port);
```

By default authentication is disabled and the proxy is an ``HTTP`` proxy but you can provide these details:

```php
    use Serps\Core\Http\Proxy;
   
     $type = 'HTTP';
     $ip   = '192.168.192.168';
     $port = 8080;
     $user = 'user';
     $pass = 'password';
     
    $proxy = new Proxy($ip, $port, $user, $pass, $type);
```


## What to do with proxies

These proxies are usable with any http client. Find example on how to use them:

- [with google client](search-engine/google/client-configuration/#proxy-usage)