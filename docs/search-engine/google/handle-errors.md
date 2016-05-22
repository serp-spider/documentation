# Handle erros from the google client
 
 <p></p>

---

Back to the [**general google documentation**](../google.md).

---

Working with google involves to make http request, to get an output from google and to parse this output. 
All of these steps are not error free, you can encounter a network error, an error from google server, get a captcha
or even face some updated google serp not supported by your version of the library.

Errors are managed with exceptions, that makes them easy to handle.

## Request error

There are several reasons for a request error to trigger:

- A network error
- A http error
- A captcha error

All these errors can be grouped under the same exception type:

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;
    use Serps\Exception\RequestError\RequestErrorException;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleUrl = new GoogleUrl();
    $googleUrl->setSearchTerm('simpsons');

    try{
        $response = $googleClient->query($googleUrl);
    }catch(RequestErrorException $e){
        // Some error with the request
        $errorInfo = $e->getMessage();
    }
```

It's possible to make the distinction between each of them, see bellow.

### Network errors

Network errors occur when it was not possible to create a valid http connexion with google. They use to be triggered
by http client, and they can be different for each http client.

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;
    use Serps\Exception\RequestError\NetworkErrorException;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleUrl = new GoogleUrl();
    $googleUrl->setSearchTerm('simpsons');

    try{
        $response = $googleClient->query($googleUrl);
    }catch(NetworkErrorException $e){
        // Something wrong happened with network
        $errorInfo = $e->getMessage();
    }
```

### Invalide Response

A invalid response happens when the http server returned a invalid response (status code 404, 500...).

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;
    use Serps\Exception\RequestError\InvalidResponseException;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleUrl = new GoogleUrl();
    $googleUrl->setSearchTerm('simpsons');

    try{
        $response = $googleClient->query($googleUrl);
    }catch(InvalidResponseException $e){
        // Http response is not valid
        $errorInfo = $e->getMessage();
    }
```

### Captcha errors

A captcha error happens when the the server asks for a captcha to continue.

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;
    use Serps\Exception\RequestError\CaptchaException;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleUrl = new GoogleUrl();
    $googleUrl->setSearchTerm('simpsons');

    try{
        $response = $googleClient->query($googleUrl);
    }catch(CaptchaException $e){
        // Captcha required
        $errorInfo = $e->getMessage();
    }
```

## DOM errors

Once you got the data from google, you are ready to parse the DOM, but you are not safe against google DOM updates,
Google might have updated its DOM structure and your version of the library is maybe not updated. That's what DOM 
exceptions are made for.

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;
    use Serps\SearchEngine\Google\Exception\InvalidDOMException;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleUrl = new GoogleUrl();
    $googleUrl->setSearchTerm('simpsons');

    $response = $googleClient->query($googleUrl);
    
    try{
        $results = $response->getNaturalResults();
    
        foreach($results as $result){
            // parse results
        }
    }catch(InvalidDOMException $e){
        // Something bad happened while parsing
        // Maybe an update of the library is needed, 
        // the exception message maybe tells more
        $errorInfo = $e->getMessage();
    }
```

## More complete example of error handling

```php
    use Serps\SearchEngine\Google\GoogleClient;
    use Serps\HttpClient\CurlClient;
    use Serps\SearchEngine\Google\GoogleUrl;
    use Serps\Exception\RequestError\RequestErrorException;
    use Serps\Exception\RequestError\HttpResponseErrorException;
    use Serps\Exception\RequestError\CaptchaException;
    use Serps\SearchEngine\Google\Exception\InvalidDOMException;

    $googleClient = new GoogleClient(new CurlClient());
    
    $googleUrl = new GoogleUrl();
    $googleUrl->setSearchTerm('simpsons');

    $response = null;
    
    try{
        $response = $googleClient->query($googleUrl);
    }catch(HttpResponseErrorException $e){
        // Http response is not valid, maybe an error from google or your url
        $errorInfo = $e->getMessage();
    }catch(CaptchaException $e){
        // Captcha needs to be solved
    }catch(RequestErrorException $e){
        // Other request error are handled here, maybe something wrong with your network
        $errorInfo = $e->getMessage();
    }
    
    if($response){
        try{
            $results = $response->getNaturalResults();
            foreach($results as $result){
                // parse results
            }
        }catch(InvalidDOMException $e){
            // Something bad happened while parsing
            // Maybe an update of the library is needed, 
            // the exception message will tell more about
            $errorInfo = $e->getMessage();
        }
    }
```