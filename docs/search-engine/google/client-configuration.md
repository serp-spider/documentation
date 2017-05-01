page_description: Tune the google client to fit your own needing.

# Google Client Configuration

<p>
</p>

---

Back to the [**general google documentation**](../google.md).

---

## Query google

To query google you need to have a working google client. 

When a query is emitted by the google client there are 2 main components involved: a [browser](/browser.md) 
that serves to create the http request and a [google url](google-url.md).

Here is a bare example of how it works:

```php

use Serps\SearchEngine\Google\GoogleClient;
use Serps\HttpClient\CurlClient;
use Serps\SearchEngine\Google\GoogleUrl;
use Serps\Core\Browser\Browser;

// 1. First you need to create a browser instance

$browser = new Browser(new CurlClient());


// 2. Then we create an google url for the search term "simpsons"

$googleUrl = new GoogleUrl();
$googleUrl->setSearchTerm('simpsons');


// 3. We create a google client with our browser

$googleClient = new GoogleClient($browser);

// 4. Finally we send the query to google with our url

$googleData = $googleClient->query($googleUrl);

```

Note that you are not required to give a browser when creating the googleClient and you can only specify it 
at request time. In this case steps 3 and 4 become:

```php

// 3. We create a google client without the browser

$googleClient = new GoogleClient();

// 4. Finally we send the query to google with our url and our browser

$googleData = $googleClient->query($googleUrl, $browser);

```

## Configure the request

At the first release of the google client it was possible to modify user agent, proxy, language headers and cookies 
on the google client. For better management of requests all this logic has moved on the browser since version ``0.2``. 
To learn more on this configuration, visit the  [browser documentation](/browser.md).


## Solve a Captcha

Solving captcha is not implemented at the moment


<br/>

Next step: [**parse a page**](parse-page.md)