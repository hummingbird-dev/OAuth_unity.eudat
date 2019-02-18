# OAuth_unity.eudat

Create an OAuth client with unity.eudat. In this tutorial I simply use
HTTP requests via the Browser and via command line cURL. Thus it can
be adopted to other systems, e.g. PHP.

I recommend to read first/additionally the official documentation
[here](https://www.eudat.eu/services/userdoc/b2access-service-integration).

## Register client

First we go to
[https://unity.eudat-aai.fz-juelich.de/home/home](https://unity.eudat-aai.fz-juelich.de/home/home)
and click on the top right link "Register a new account". From the
pop-up menu we choose "OAuth 2.0 Client Registration Form". Now we
fill the respective fields, whereas it is important to set "OAuth
client allowed grants" to "authorizationCode". Be sure to remember the
Password, we will need it later. The "OAuth client return URL" can be
specified now or later. After successful registration we can login to
the account and do things like changing the password or setting return
URL's. Accordingly we logout.

## Redirect to authorization server

Now we are sending the user to the authorization server and type the following into the Browser's address bar:

``` html
https://unity.eudat-aai.fz-juelich.de/oauth2-as/oauth2-authz?
client_id=myClientName&
redirect_uri=http://localhost/unity/callback&
scope=email&
response_type=code&
state=14N8PON3olyCEV3G75xXTdrY28MsX8UVPFczM4XB
```

The "client_id" is the User name, which we have chosen during the
above registration of the client. The "redirect_uri" must be identical
to the one inserted during the registration. The "state" parameter is a
random string and used to prevent against CRSF attacks and should be
saved, e.g. in a PHP session.

Now we login with one of our social identities, e.g. Google, Facebook,
GitHub, MarineID, etc. After the successful login we have to confirm
to submit personal information to the client.

We are now redirected back to our "redirect_uri" and find the following in the Browser's address bar:

``` html
http://localhost/unity/callback?
code=PpgFJJJdvIw3uv_X62SS2awhUWfIM7B5EM12mrupVHU&
state=14N8PON3olyCEV3G75xXTdrY28MsX8UVPFczM4XB
```

## Get access token from authorization server

To prevent against CSRF attacks we can now check if the authorization
server correctly returned the "state" parameter. With the "code"
parameter we can now request the access token and user information. In
this tutorial I use command line cURL for the request. Adapt it to
your system.

The crucial point here is now that we have to submit the credentials
base64 encoded and additionally use "Basic Authentication" in the HTTP
header, with the base64 encoded credentials. The needed "client_id" is
the user name and the "client_secret" is the password, which we have
chosen during the registration of the client. So first encode the
credentials to base64.

``` shell
client_id=myClientName
client_id_b64=`echo $client_id | base64`

client_secret=myClientSecret
client_secret_b64=`echo $client_secret | base64`

token_b64=`echo -ne "$client_id:$client_secret" | base64 --wrap 0`

redirect_uri=http://localhost/unity/callback

code=PpgFJJJdvIw3uv_X62SS2awhUWfIM7B5EM12mrupVHU
```

And now fire the request with cURL.

``` shell
curl -H "Authorization: Basic $token_b64" -d "client_id=$client_id_b64&client_secret=$client_secret_b64&redirect_uri=$redirect_uri&code=$code&grant_type=authorization_code" https://unity.eudat-aai.fz-juelich.de/oauth2/token
```

The answer now looks like the following:

``` shell
{"access_token":"bPwVaYhOzTl__dnAll5o9QtTABxw-661SQiqDbcVBBo",
"refresh_token":"Fj1vffVU7MuR3B8XvmF18PlCSjulD7K2rIJQawzQ6pA",
"token_type":"Bearer"}
```

## Get user information

With the access token we can now get information about the user:

``` shell
curl -H "Authorization: Bearer bPwVaYhOzTl__dnAll5o9QtTABxw-661SQiqDbcVBBo" https://unity.eudat-aai.fz-juelich.de/oauth2/userinfo
```

The response is then something like:

``` shell
{"sub":"244xxdbf-f665-404c-a29e-72403edczzb4",
"email":"sebastian.mieruch@gmx.de"}
```

Finally we have the email of the user and can login the user into our
system or create e.g. a new user.
























