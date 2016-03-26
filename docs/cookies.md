Cookies
=======

Guide for cookie and cookiejar manipulation 

---


SERPS offers a convenient tools that emulate a cookie jar and allow to persist cookies across many requests.
 
The following examples introduce the work with cookies

## Create a cookie

Parameters for creating a cookie:

- ``name``: the name of a cookie.
- ``value``: the value of a cookie.
- ``flags``: cookie flags. Available flags are:
    - ``path``
    - ``domain``
    - ``expires``
    - ``discard``
    - ``secure``
    
```php
use Serps\Core\Cookie\Cookie;

$cookie = new Cookie('baz', 'bar', [
    'domain' => 'foo.bar',
    'path' => '/',
    'expires' => time() + 1000,
]);
```

## Populate a Cookie Jar

```php
use Serps\Core\Cookie\ArrayCookieJar;

$cookieJar = new ArrayCookieJar();

// Add the cookie 'foo' with value 'bar' for the domain 'foo.bar'
$cookie = new Cookie('foo', 'bar', ['domain' => 'foo.bar']);
$cookieJar->set($cookie);

// Add the cookie 'baz' with value 'bar' for the domain 'foo.bar'
$cookie = new Cookie('baz', 'bar', ['domain' => 'foo.bar']);
$cookieJar->set($cookie);
```

## Retrieves cookies

The ``all`` method is responsible for getting cookies matching some filters

Method parameters:

- ``domain``: filters the cookies matching the given domain. Pass null to match all domains.
- ``path``: filters the cookies matching the given path. Pass null to match all paths.
- ``name``: filters the cookies matching the given name. Pass null to match all names.
- ``skipDiscardable``: Set to TRUE to skip cookies with the Discard attribute. Default FALSE.
- ``skipExpired``: Set to FALSE to include expired. Default TRUE.


```php
// Retrieves all cookies
$cookies = $cookieJar->all();

// Retrieves all cookies matching the domain "foo.bar"
$cookies = $cookieJar->all("foo.bar");


// Retrieves the cookie named "foo" for the domain "foo.bar", including expired cookies
$cookies = $cookieJar->all("foo.bar", "/", "foo", false, false);
```

### Retrieve cookie for a request

It's possible to automatically retrieve cookies that match a given PSR-7 request:

```php
// retrieves all cookies matching the request
$cookies = $cookieJar->getMatchingCookies($request);
```


## Remove cookies

```php
// Remove all cookies
$cookies = $cookieJar->remove();

// Remove all cookies matching the domain "foo.bar"
$cookies = $cookieJar->remove("foo.bar");

// Remove the cookie named "foo" for the domain "foo.bar"
$cookies = $cookieJar->remove("foo.bar", "/", "foo");
```

### Remove temporary cookies

```php
// Remove all temporary cookies
$cookies = $cookieJar->removeTemporary();
```

### Remove expired cookies

```php
// Remove all cookies that are expired
$cookies = $cookieJar->removeExpired();
```
