# MercadoLibre's Python SDK

Fork from [the official Python SDK for MercadoLibre's Platform](https://github.com/mercadolibre/python-sdk).

## How do I install it?

       pip install git+git://github.com/calazans10/python-sdk.git

## How do I use it?

The first thing to do is to instance a ```Meli``` class. You'll need to give a ```clientId``` and a ```clientSecret```. You can obtain both after creating your own application. For more information on this please read: [creating an application](http://developers.mercadolibre.com/application-manager/)

### Including the Lib
Include the lib meli in your project

```python
from meli import Meli
```
Start the development!

### Create an instance of Meli class
Simple like this
```python
meli = Meli(client_id=1234, client_secret="a secret")
```
With this instance you can start working on MercadoLibre's APIs.

There are some design considerations worth to mention.

1. This SDK is just a thin layer on top of an http client to handle all the OAuth WebServer flow for you.

2. There is JSON parsing. this SDK will include [json](http://docs.python.org/2/library/json.html) for internal usage.

3. If you already have the access_token and the refresh_token you can pass in the constructor

```python
meli = Meli(client_id=1234, client_secret"a secret", access_token="Access_Token", refresh_token="Refresh_Token")
```

## How do I redirect users to authorize my application?

This is a 2 step process.

First get the link to redirect the user. This is very easy! Just:

```python
redirectUrl = meli.auth_url(redirect_URI="http://somecallbackurl")
```

This will give you the url to redirect the user. You need to specify a callback url which will be the one that the user will redirected after a successfull authrization process.

Once the user is redirected to your callback url, you'll receive in the query string, a parameter named ```code```. You'll need this for the second part of the process.

```python
meli.authorize(code="the received code", redirect_URI="http://somecallbackurl")
```

This will get an ```access_token``` and a ```refresh_token``` (is case your application has the ```offline_access```) for your application and your user.

At this stage your are ready to make call to the API on behalf of the user.

#### Making GET calls

```python
params = {'access_token' : meli.access_token}
response = meli.get(path="/users/me", params=params)
```

#### Making POST calls

```python
params = {'access_token' : meli.access_token}

  #this body will be converted into json for you
body = {'foo'  : 'bar', 'bar' : 'foo'}

response = meli.post(path="/items", body=body, params=params)
```

#### Making PUT calls

```python
params = {'access_token' : meli.access_token}

  #this body will be converted into json for you
body = {'foo'  : 'bar', 'bar' : 'foo'}

response = meli.put(path="/items/123", body=body, params=params)
```

#### Making DELETE calls
```python
params = {'access_token' : meli.access_token}
response = meli.delete(path="/questions/123", params=params)
```

## Contributing

1. [Fork it](https://github.com/calazans10/python-sdk/fork)
2. Create your feature branch (git checkout -b my-new-feature)
3. Commit your changes (git commit -am 'Add some feature')
4. Push to the branch (git push origin my-new-feature)
5. Create a new Pull Request
