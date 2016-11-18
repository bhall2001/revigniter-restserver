# revigniter-restserver
A RESTful Server for revIgniter.

## DOES NOT WORK AS OF 2016-OCT-26
This is a very early development state at the moment. Github is being used to store the project in the hopes of someone wanting to jump onboard to help with development.

## Requirements
  * Livecode Server 8.1.0 or above
  * revIgniter 1.9.4


## Installation

Copy the application/libraries/Format.lc and application/libraries/REST_Controller.lc files into your revIgniter application's directory. Additionally, copy the application/config/rest.lc file to your application's configuration directory.

Cloning this repository gives you a complete revIgniter installation. You can put this project in it's own sub directory for easy access.
```
~/revigniter-restserver/
```

## Dependencies

The RestController library requires the Livecode 8 JSON Livecode Builder Library to be installed.

## Handling Requests

Making an HTTP GET call to index.lc/books, for instance, calls the books_get function in the books.lc controller.

This allows you to implement a RESTful interface easily:

In the books.lc controller file, you add functions for for HTML Request verbs that are handled...

```
function books_get
    // Display all books or a single book if an id is provided
end book_get

function books_post
    // Create a new book
end book_post

function books_delete
    // Delete a book
end book_delete
```

RestController also supports PUT and DELETE methods, allowing you to support a truly RESTful interface.

RestController supports a bunch of different request/response formats, including XML, JSON, LSON, CSV and HTML. By default, the library checks the URL and looks for a format either as an extension or as a separate segment.

This means your URLs can look like this:

```
http://example.com/index.lc/v1/books.json
http://example.com/index.lc/v1/books/id/1/format/lson
http://example.com/index.lc/v1/books?format=xml
```
(default format if none specified is JSON)


The recommend approach is using the HTTP Accept header:

```
$ curl -H "Accept: application/json" http://example.com
```
Any responses you make from the function (see responses for more on this) are returned in the designated format.

## Responses

Stay Tuned...

## Authentication

The RestController Library also provides rudimentary support for HTTP basic authentication and/or the securer HTTP digest access authentication.

You can enable basic authentication by setting the gConfig["rest_auth"] to "basic". The gConfig["rest_valid_logins"] directive is used to set the usernames and passwords that are able to log in to your system. The Library will automatically send all the correct headers to trigger the authentication dialogue:
```
put "password" into gConfig["rest_valid_logins"]["username"]
put "secure123" into gConfig["rest_valid_logins"]["other_person"]
```
Enabling digest auth is similarly easy. Configure your desired logins in the config file like above, and set gConfig["rest_auth"] to "digest". The class will automatically send out the headers to enable digest auth.

All methods of authentication can be secured further by using an IP whitelist. If you enable gConfig["rest_ip_whitelist_enabled"] in your config file, you can then set a list of allowed IPs.

Any client connecting to your API will be checked against the whitelisted IP list. If they're on the list, they'll be allowed access. If not, they will not be able to access the API. The whitelist is a comma-separated string:
```
put "123.456.789.0, 987.654.32.1" into gConfig["rest_ip_whitelist"]
```
Your localhost IPs (127.0.0.1 and 0.0.0.0) are allowed by default.

Any client connecting to your API will be checked against the blacklisted IP list. If they're on the list, they will NOT be allowed access to the API. The blacklist is a comma-separated string:
```
put "123.456.666.0, 987.654.93.1" into gConfig["rest_ip_blacklist"]
```

## API Keys

In addition to the authentication methods above, the REST_Controller class also supports the use of API keys. Enabling API keys is easy. Turn it on in your config/rest.php file:
```
put true into gConfig["rest_enable_keys"]
```
You'll need to create a new database table to store and access the keys. RestController REQUIRES you have a table that looks like this when using API keys:
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
The Library looks for an HTTP header with the API key on each request. An invalid or missing API key results in an HTTP 403 Forbidden.

By default, the HTTP will be X-API-KEY. This can be configured in config/rest.php.
```
$ curl -X POST -H "X-API-KEY: some_key_here" http://example.com/books
```

Additional security is placed on API keys by enabling rate limits for each key. Each call to the API from an authorized key increments a counter for that key. When the number of calls per hour reaches the key's limit, the requester is notified they have reached their hourly limit.
 
## Other Documentation / Tutorials

NetTuts: Working with RESTful Services in CodeIgniter (explains the CodeIngniter Class)

## Contributions

This project is based on the Codeigniter restserver originally written by Phil Sturgeon and now maintained by Chris Kacerguis.

Please help if you can using pull requests on Github. The project is located at https://github.com/bhall2001/revigniter-restserver

Agile project management provided by overv.io. Check it out at https://overv.io/bhall2001/revigniter-restserver/

GitHub license
TBD