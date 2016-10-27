# revigniter-restserver
A RESTful Server for revIgniter.

## DOES NOT WORK AS OF 2016-OCT-26
This is a very early development state at the moment. Github is being
used to store the project in the hopes of someone wanting to jump
onboard to help with development.

## Requirements
Livecode Server 8.0.2 or above
revIgniter 1.9.3


## Installation

Drag and drop the application/libraries/Format.php and application/libraries/REST_Controller.php files into your application's directories. To use require_once it at the top of your controllers to load it into the scope. Additionally, copy the rest.php file from application/config in your application's configuration directory.

## Handling Requests

If you're making an HTTP GET call to index.lc/books, for instance, it would call index_get function in the books controller.

This allows you to implement a RESTful interface easily:

In the books.lc controller file...

```
function index_get
    // Display all books
end index_get

function index_post
    // Create a new book
end index_post
```

REST_Controller also supports PUT and DELETE methods, allowing you to support a truly RESTful interface.

REST_Controller supports a bunch of different request/response formats, including XML, JSON, LSON, CSV and HTML. By default, the class will check the URL and look for a format either as an extension or as a separate segment.

This means your URLs can look like this:

```
http://example.com/index.lc/example-api/books.json
http://example.com/index.lc/example-api/book/id/1/format/lson
http://example.com/index.lc/books?format=json
```

The recommend approach is using the HTTP Accept header:

```
$ curl -H "Accept: application/json" http://example.com
```
Any responses you make from function (see responses for more on this) will be returned in the designated format.

## Responses

Stay Tuned...

## Authentication

This class also provides rudimentary support for HTTP basic authentication and/or the securer HTTP digest access authentication.

You can enable basic authentication by setting the gConfig["rest_auth"] to "basic". The gConfig["rest_valid_logins"] directive can then be used to set the usernames and passwords able to log in to your system. The class will automatically send all the correct headers to trigger the authentication dialogue:
```
put "password" into gConfig["rest_valid_logins"]["username"]
put "secure123" into gConfig["rest_valid_logins"]["other_person"]
```
Enabling digest auth is similarly easy. Configure your desired logins in the config file like above, and set gConfig["rest_auth"] to "digest". The class will automatically send out the headers to enable digest auth.

If you're tying this library into an AJAX endpoint where clients authenticate using PHP sessions then you may not like either of the digest nor basic authentication methods. In that case, you can tell the REST Library what PHP session variable to check for. If the variable exists, then the user is authorized. It will be up to your application to set that variable. You can define the variable in $config['auth_source']. Then tell the library to use a php session variable by setting $config['rest_auth'] to session.

All three methods of authentication can be secured further by using an IP whitelist. If you enable gConfig["rest_ip_whitelist_enabled"] in your config file, you can then set a list of allowed IPs.

Any client connecting to your API will be checked against the whitelisted IP list. If they're on the list, they'll be allowed access. If not, sorry, no can do hombre. The whitelist is a comma-separated string:
```
put "123.456.789.0, 987.654.32.1" into gConfig["rest_ip_whitelist"]
```
Your localhost IPs (127.0.0.1 and 0.0.0.0) are allowed by default.

## API Keys

In addition to the authentication methods above, the REST_Controller class also supports the use of API keys. Enabling API keys is easy. Turn it on in your config/rest.php file:
```
put TRUE into gConfig["rest_enable_keys"]
```
You'll need to create a new database table to store and access the keys. REST_Controller will REQUIRES you have a table that looks like this if you are using API keys:
```
CREATE TABLE `keys` (
    `id` INT(11) NOT NULL AUTO_INCREMENT,
    `key` VARCHAR(40) NOT NULL,
    `level` INT(2) NOT NULL,
    `ignore_limits` TINYINT(1) NOT NULL DEFAULT '0',
    `date_created` INT(11) NOT NULL,
    PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
The class will look for an HTTP header with the API key on each request. An invalid or missing API key will result in an HTTP 403 Forbidden.

By default, the HTTP will be X-API-KEY. This can be configured in config/rest.php.
```
$ curl -X POST -H "X-API-KEY: some_key_here" http://example.com/books
```
## Other Documentation / Tutorials

NetTuts: Working with RESTful Services in CodeIgniter

## Contributions

This project is based on the Codeigniter restserver originally written by Phil Sturgeon and now maintained by Chris Kacerguis.

Please help if you can using pull requests on Github. The project is located at https://github.com/bhall2001/revigniter-restserver

Agile project management provided by overv.io. Check it out at https://overv.io/bhall2001/revigniter-restserver/

GitHub license
MIT