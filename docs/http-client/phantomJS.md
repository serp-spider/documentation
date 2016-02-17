PhantomJS HTTP Client
=====================

[PhantomJS](http://phantomjs.org/) is a webkit implementation that helps to simulate the real browser.
By using this client you will execute the inner javascript code and make the DOM as real as in the true browser,
that can be required for some search engines to work properly.

> <sub>**Important notice**</sub>
>
> <cite>When you use phantomJS client and you submit a request from it the resulting DOM 
> might be different of the source code returned by a server because
> javascript is executed before returning the DOM.</cite>



Installation
------------

The client is available with the package 
[serps/http-client-phantomjs](https://packagist.org/packages/serps/http-client-phantomjs): 

``$ composer require 'serps/http-client-phantomjs'``

### Additional requirement

**[PhantomJS](http://phantomjs.org/) bianries have to be installed** to use the client. The process for installing
it depends on your environment, you will find further guides on the internet.

The package [jakoch/phantomjs-installer](https://github.com/jakoch/phantomjs-installer) 
can help you to manage phantomJS as a dependency of your project.

## Usage

```php
use Serps\HttpClient\PhantomJsClient;

// The client needs the path to the binary.
// By default it will use 'phantomjs'
$client = new PhantomJsClient(__DIR__ . '/bin/phantomjs');

// you can optionally add a proxy as a second parameter 
$response = $client->query($request);
```
