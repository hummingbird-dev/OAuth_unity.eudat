# OAuth_unity.eudat

Create an OAuth client with unity.eudat. In this tutorial I simply use
HTTP requests via the Browser and via command line cURL. Thus it can
be adopted to other systems, e.g. PHP.

I recommend to read first/additionally the official documentation
[here](https://www.eudat.eu/services/userdoc/b2access-service-integration).

## Register client

First we go to
[https://unity.eudat-aai.fz-juelich.de/home/home](https://unity.eudat-aai.fz-juelich.de/home/home)
and click on the top left link "Register a new account". From the
pop-up menu we choose "OAuth 2.0 Client Registration Form". Now we
fill the respective fields, whereas it is important to set "OAuth
client allowed grants" to "authorizationCode". Be sure to remember the
Password, we will need it later. The "OAuth client return URL" can be
specified now or later. After successful registration we can login to
the account and do things like changing the password or setting return
URL's.

## Redirect to authorization server

Now we are sending the user to the authorization server and type the following into the Browser's address bar:

``` html
https://unity.eudat-aai.fz-juelich.de/oauth2-as/oauth2-authz?client_id=myClientName&redirect_uri=http://localhost/unity/callback&scope=email&response_type=code&state=14N8PON3olyCEV3G75xXTdrY28MsX8UVPFczM4XB
```





























