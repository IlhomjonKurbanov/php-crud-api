# PHP-CRUD-API (v2)

Single file PHP 7 script that adds a REST API to a MySQL 5.5 InnoDB database. PostgreSQL 9.1 and MS SQL Server 2012 are fully supported. 

NB: This is the [TreeQL](https://treeql.org) reference implementation in PHP.

NB: Are you looking for v1? It is here: https://github.com/mevdschee/php-crud-api/tree/v1

Related projects:

  - [PHP-API-AUTH](https://github.com/mevdschee/php-api-auth): Single file PHP script that is an authentication provider for PHP-CRUD-API (v2)
  - [PHP-SP-API](https://github.com/mevdschee/php-sp-api): Single file PHP script that adds a REST API to a SQL database.
  - [PHP-CRUD-UI](https://github.com/mevdschee/PHP-crud-ui): Single file PHP script that adds a UI to a PHP-CRUD-API (v1) project.
  - [VUE-CRUD-UI](https://github.com/nlware/vue-crud-ui): Single file Vue.js script that adds a UI to a PHP-CRUD-API (v1) project.
  
There are also ports of this script in:

- [Java JDBC by Ivan Kolchagov](https://github.com/kolchagov/java-crud-api) (v1)
- [Java Spring Boot + jOOQ](https://github.com/mevdschee/java-crud-api/tree/master/full) (v2: work in progress)

There are also proof-of-concept ports of this script that only support basic REST CRUD functionality in:
[PHP](https://github.com/mevdschee/php-crud-api/blob/master/extras/core.php),
[Java](https://github.com/mevdschee/java-crud-api/blob/master/core/src/main/java/com/tqdev/CrudApiHandler.java),
[Go](https://github.com/mevdschee/go-crud-api/blob/master/api.go),
[C# .net core](https://github.com/mevdschee/core-data-api/blob/master/Program.cs),
[Node.js](https://github.com/mevdschee/js-crud-api/blob/master/app.js) and
[Python](https://github.com/mevdschee/py-crud-api/blob/master/api.py).

## Requirements

  - PHP 7.0 or higher with PDO drivers for MySQL, PgSQL or SqlSrv enabled
  - MySQL 5.6 / MariaDB 10.0 or higher for spatial features in MySQL
  - PostGIS 2.0 or higher for spatial features in PostgreSQL 9.1 or higher
  - SQL Server 2012 or higher (2017 for Linux support)

## Installation

This is a single file application! Upload "`api.php`" somewhere and enjoy!

For local development you may run PHP's built-in web server:

    php -S localhost:8080

Test the script by opening the following URL:

    http://localhost:8080/api.php/records/posts/1

Don't forget to modify the configuration at the bottom of the file.

## Configuration

Edit the following lines in the bottom of the file "`api.php`":

    $config = new Config([
        'username' => 'xxx',
        'password' => 'xxx',
        'database' => 'xxx',
    ]);

These are all the configuration options and their default value between brackets:

- "driver": `mysql`, `pgsql` or `sqlsrv` (`mysql`)
- "address": Hostname of the database server (`localhost`)
- "port": TCP port of the database server (defaults to driver default)
- "username": Username of the user connecting to the database (no default)
- "password": Password of the user connecting to the database (no default)
- "database": Database the connecting is made to (no default)
- "middlewares": List of middlewares to load (`cors`)
- "controllers": List of controllers to load (`records,openapi`)
- "openApiBase": OpenAPI info (`{"info":{"title":"PHP-CRUD-API","version":"1.0.0"}}`)
- "cacheType": `TempFile`, `Redis`, `Memcache`, `Memcached` or `NoCache` (`TempFile`)
- "cachePath": Path/address of the cache (defaults to system's temp directory)
- "cacheTime": Number of seconds the cache is valid (`10`)
- "debug": Show errors in the "X-Debug-Info" header (`false`)

## Compilation

The code resides in the "`src`" directory. You can access it at the URL:

    http://localhost:8080/src/records/posts/1

You can compile all files into a single "`api.php`" file using:

    php build.php

NB: The script appends the classes in alphabetical order (directories first).

## Limitations

These limitation were also present in v1:

  - Primary keys should either be auto-increment (from 1 to 2^53) or UUID
  - Composite primary or foreign keys are not supported
  - Complex writes (transactions) are not supported
  - Complex queries calling functions (like "concat" or "sum") are not supported
  - Database must support and define foreign key constraints
  
## Features

These features match features in v1 (see branch "v1"):

  - [x] Single PHP file, easy to deploy.
  - [x] Very little code, easy to adapt and maintain
  - [ ] ~~Streaming data, low memory footprint~~
  - [x] Supports POST variables as input (x-www-form-urlencoded)
  - [x] Supports a JSON object as input
  - [x] Supports a JSON array as input (batch insert)
  - [ ] ~~Supports file upload from web forms (multipart/form-data)~~
  - [ ] ~~Condensed JSON output: first row contains field names~~
  - [x] Sanitize and validate input using callbacks
  - [x] Permission system for databases, tables, columns and records
  - [x] Multi-tenant database layouts are supported
  - [x] Multi-domain CORS support for cross-domain requests
  - [x] Support for reading joined results from multiple tables
  - [x] Search support on multiple criteria
  - [x] Pagination, seeking, sorting and column selection
  - [x] Relation detection with nested results (belongsTo, hasMany and HABTM)
  - [ ] ~~Relation "transforms" (of condensed JSON) for PHP and JavaScript~~
  - [x] Atomic increment support via PATCH (for counters)
  - [x] Binary fields supported with base64 encoding
  - [x] Spatial/GIS fields and filters supported with WKT
  - [ ] ~~Unstructured data support through JSON/JSONB~~
  - [x] Generate API documentation using OpenAPI tools
  - [x] Authentication via JWT token or username/password
  - [ ] ~~SQLite support~~

 NB: No checkmark means: not yet implemented. Striken means: will not be implemented.

### Extra Features

These features are new and were not included in v1.

  - Does not reflect on every request (better performance)
  - Complex filters (with both "and" & "or") are supported
  - Support for output of database structure in JSON
  - Support for boolean and binary data in all database engines
  - Support for relational data on read (not only on list operation)
  - Support for middleware to modify all operations (also list)
  - Error reporting in JSON with corresponding HTTP status
  - Support for basic authentication and via auth provider (JWT)
  - Support for basic firewall functionality

## Middleware

You can enable the following middleware using the "middlewares" config parameter:

- "firewall": Limit access to specific IP addresses
- "cors": Support for CORS requests (enabled by default)
- "xsrf": Block XSRF attacks using the 'Double Submit Cookie' method
- "ajaxOnly": Restrict non-AJAX requests to prevent XSRF attacks
- "jwtAuth": Support for "JWT Authentication"
- "basicAuth": Support for "Basic Authentication"
- "authorization": Restrict access to certain tables or columns
- "validation": Return input validation errors for custom rules
- "sanitation": Apply input sanitation on create and update
- "multiTenancy": Restricts tenants access in a multi-tenant scenario
- "customization": Provides handlers for request and response customization

The "middlewares" config parameter is a comma separated list of enabled middlewares.
You can tune the middleware behavior using middleware specific configuration parameters:

- "firewall.reverseProxy": Set to "true" when a reverse proxy is used ("")
- "firewall.allowedIpAddresses": List of IP addresses that are allowed to connect ("")
- "cors.allowedOrigins": The origins allowed in the CORS headers ("*")
- "cors.allowHeaders": The headers allowed in the CORS request ("Content-Type, X-XSRF-TOKEN")
- "cors.allowMethods": The methods allowed in the CORS request ("OPTIONS, GET, PUT, POST, DELETE, PATCH")
- "cors.allowCredentials": To allow credentials in the CORS request ("true")
- "cors.exposeHeaders": Whitelist headers that browsers are allowed to access ("")
- "cors.maxAge": The time that the CORS grant is valid in seconds ("1728000")
- "xsrf.excludeMethods": The methods that do not require XSRF protection ("OPTIONS,GET")
- "xsrf.cookieName": The name of the XSRF protection cookie ("XSRF-TOKEN")
- "xsrf.headerName": The name of the XSRF protection header ("X-XSRF-TOKEN")
- "ajaxOnly.excludeMethods": The methods that do not require AJAX ("OPTIONS,GET")
- "ajaxOnly.headerName": The name of the required header ("X-Requested-With")
- "ajaxOnly.headerValue": The value of the required header ("XMLHttpRequest")
- "jwtAuth.mode": Set to "optional" if you want to allow anonymous access ("required")
- "jwtAuth.header": Name of the header containing the JWT token ("X-Authorization")
- "jwtAuth.leeway": The acceptable number of seconds of clock skew ("5")
- "jwtAuth.ttl": The number of seconds the token is valid ("30")
- "jwtAuth.secret": The shared secret used to sign the JWT token with ("")
- "jwtAuth.algorithms": The algorithms that are allowed, empty means 'all' ("")
- "jwtAuth.audiences": The audiences that are allowed, empty means 'all' ("")
- "jwtAuth.issuers": The issuers that are allowed, empty means 'all' ("")
- "basicAuth.mode": Set to "optional" if you want to allow anonymous access ("required")
- "basicAuth.realm": Text to prompt when showing login ("Username and password required")
- "basicAuth.passwordFile": The file to read for username/password combinations (".htpasswd")
- "authorization.tableHandler": Handler to implement table authorization rules ("")
- "authorization.columnHandler": Handler to implement column authorization rules ("")
- "authorization.recordHandler": Handler to implement record authorization filter rules ("")
- "validation.handler": Handler to implement validation rules for input values ("")
- "sanitation.handler": Handler to implement sanitation rules for input values ("")
- "multiTenancy.handler": Handler to implement simple multi-tenancy rules ("")
- "customization.beforeHandler": Handler to implement request customization ("")
- "customization.afterHandler": Handler to implement response customization ("")

If you don't specify these parameters in the configuration, then the default values (between brackets) are used.

## TreeQL, a pragmatic GraphQL

TreeQL allows you to create a "tree" of JSON objects based on your SQL database structure (relations) and your query.

It is loosely based on the REST standard and also inspired by json:api.

### CRUD + List

The example posts table has only a a few fields:

    posts  
    =======
    id     
    title  
    content
    created

The CRUD + List operations below act on this table.

#### Create

If you want to create a record the request can be written in URL format as: 

    POST /records/posts

You have to send a body containing:

    {
        "title": "Black is the new red",
        "content": "This is the second post.",
        "created": "2018-03-06T21:34:01Z"
    }

And it will return the value of the primary key of the newly created record:

    2

#### Read

To read a record from this table the request can be written in URL format as:

    GET /records/posts/1

Where "1" is the value of the primary key of the record that you want to read. It will return:

    {
        "id": 1
        "title": "Hello world!",
        "content": "Welcome to the first post.",
        "created": "2018-03-05T20:12:56Z"
    }

On read operations you may apply joins.

#### Update

To update a record in this table the request can be written in URL format as:

    PUT /records/posts/1

Where "1" is the value of the primary key of the record that you want to update. Send as a body:

    {
        "title": "Adjusted title!"
    }

This adjusts the title of the post. And the return value is the number of rows that are set:

    1

#### Delete

If you want to delete a record from this table the request can be written in URL format as:

    DELETE /records/posts/1

And it will return the number of deleted rows:

    1

#### List

To list records from this table the request can be written in URL format as:

    GET /records/posts

It will return:

    {
        "records":[
            {
                "id": 1,
                "title": "Hello world!",
                "content": "Welcome to the first post.",
                "created": "2018-03-05T20:12:56Z"
            }
        ]
    }

On list operations you may apply filters and joins.

### Filters

Filters provide search functionality, on list calls, using the "filter" parameter. You need to specify the column
name, a comma, the match type, another commma and the value you want to filter on. These are supported match types:

  - "cs": contain string (string contains value)
  - "sw": start with (string starts with value)
  - "ew": end with (string end with value)
  - "eq": equal (string or number matches exactly)
  - "lt": lower than (number is lower than value)
  - "le": lower or equal (number is lower than or equal to value)
  - "ge": greater or equal (number is higher than or equal to value)
  - "gt": greater than (number is higher than value)
  - "bt": between (number is between two comma separated values)
  - "in": in (number or string is in comma separated list of values)
  - "is": is null (field contains "NULL" value)

You can negate all filters by prepending a "n" character, so that "eq" becomes "neq". 
Examples of filter usage are:

    GET /records/categories?filter=name,eq,Internet
    GET /records/categories?filter=name,sw,Inter
    GET /records/categories?filter=id,le,1
    GET /records/categories?filter=id,ngt,2
    GET /records/categories?filter=id,bt,1,1

Output:

    {
        "records":[
            {
                "id": 1
                "name": "Internet"
            }
        ]
    }

In the next section we dive deeper into how you can apply multiple filters on a single list call.

### Multiple filters

Filters can be a by applied by repeating the "filter" parameter in the URL. For example the following URL: 

    GET /records/categories?filter=id,gt,1&filter=id,lt,3

will request all categories "where id > 1 and id < 3". If you wanted "where id = 2 or id = 4" you should write:

    GET /records/categories?filter1=id,eq,2&filter2=id,eq,4
    
As you see we added a number to the "filter" parameter to indicate that "OR" instead of "AND" should be applied.
Note that you can also repeat "filter1" and create an "AND" within an "OR". Since you can also go one level deeper
by adding a letter (a-f) you can create almost any reasonably complex condition tree.

NB: You can only filter on the requested table (not on it's included) and filters are only applied on list calls.

### Column selection

By default all columns are selected. With the "include" parameter you can select specific columns. 
You may use a dot to separate the table name from the column name. Multiple columns should be comma separated. 
An asterisk ("*") may be used as a wildcard to indicate "all columns". Similar to "include" you may use the "exclude" parameter to remove certain columns:

```
GET /records/categories/1?include=name
GET /records/categories/1?include=categories.name
GET /records/categories/1?exclude=categories.id
```

Output:

```
    {
        "name": "Internet"
    }
```

NB: Columns that are used to include related entities are automatically added and cannot be left out of the output.

### Ordering

With the "order" parameter you can sort. By default the sort is in ascending order, but by specifying "desc" this can be reversed:

```
GET /records/categories?order=name,desc
GET /records/categories?order=id,desc&order=name
```

Output:

```
    {
        "records":[
            {
                "id": 3
                "name": "Web development"
            },
            {
                "id": 1
                "name": "Internet"
            }
        ]
    }
```

NB: You may sort on multiple fields by using multiple "order" parameters. You can not order on "joined" columns.

### Pagination

The "page" parameter holds the requested page. The default page size is 20, but can be adjusted (e.g. to 50):

```
GET /records/categories?order=id&page=1
GET /records/categories?order=id&page=1,50
```

Output:

```
    {
        "records":[
            {
                "id": 1
                "name": "Internet"
            },
            {
                "id": 3
                "name": "Web development"
            }
        ],
        "results": 2
    }
```

NB: Pages that are not ordered cannot be paginated.

### Joins

Let's say that you have a posts table that has comments (made by users) and the posts can have tags.

    posts    comments  users     post_tags  tags
    =======  ========  =======   =========  ======= 
    id       id        id        id         id
    title    post_id   username  post_id    name
    content  user_id   phone     tag_id
    created  message

When you want to list posts with their comments users and tags you can ask for two "tree" paths:

    posts -> comments  -> users
    posts -> post_tags -> tags

These paths have the same root and this request can be written in URL format as:

    GET /records/posts?join=comments,users&join=tags

Here you are allowed to leave out the intermediate table that binds posts to tags. In this example
you see all three table relation types (hasMany, belongsTo and hasAndBelongsToMany) in effect:

- "post" has many "comments"
- "comment" belongs to "user"
- "post" has and belongs to many "tags"

This may lead to the following JSON data:

    {
        "records":[
            {
                "id": 1,
                "title": "Hello world!",
                "content": "Welcome to the first post.",
                "created": "2018-03-05T20:12:56Z",
                "comments": [
                    {
                        id: 1,
                        post_id: 1,
                        user_id: {
                            id: 1,
                            username: "mevdschee",
                            phone: null,
                        },
                        message: "Hi!"
                    },
                    {
                        id: 2,
                        post_id: 1,
                        user_id: {
                            id: 1,
                            username: "mevdschee",
                            phone: null,
                        },
                        message: "Hi again!"
                    }
                ],
                "tags": []
            },
            {
                "id": 2,
                "title": "Black is the new red",
                "content": "This is the second post.",
                "created": "2018-03-06T21:34:01Z",
                "comments": [],
                "tags": [
                    {
                        id: 1,
                        message: "Funny"
                    },
                    {
                        id: 2,
                        message: "Informational"
                    }
                ]
            }
        ]
    }

You see that the "belongsTo" relationships are detected and the foreign key value is replaced by the referenced object.
In case of "hasMany" and "hasAndBelongsToMany" the table name is used a new property on the object.

### Batch operations

When you want to create, read, update or delete you may specify multiple primary key values in the URL.
You also need to send an array instead of an object in the request body for create and update. 

To read a record from this table the request can be written in URL format as:

    GET /records/posts/1,2

The result may be:

    [
            {
                "id": 1,
                "title": "Hello world!",
                "content": "Welcome to the first post.",
                "created": "2018-03-05T20:12:56Z"
            },
            {
                "id": 2,
                "title": "Black is the new red",
                "content": "This is the second post.",
                "created": "2018-03-06T21:34:01Z"
            }
    ]

Similarly when you want to do a batch update the request in URL format is written as:

    PUT /records/posts/1,2

Where "1" and "2" are the values of the primary keys of the records that you want to update. The body should 
contain the same number of objects as there are primary keys in the URL:

    [   
        {
            "title": "Adjusted title for ID 1"
        },
        {
            "title": "Adjusted title for ID 2"
        }        
    ]

This adjusts the titles of the posts. And the return values are the number of rows that are set:

    1,1

Which means that there were two update operations and each of them had set one row. Batch operations use database
transactions, so they either all succeed or all fail (successful ones get roled back).

### Spatial support

For spatial support there is an extra set of filters that can be applied on geometry columns and that starting with an "s":

  - "sco": spatial contains (geometry contains another)
  - "scr": spatial crosses (geometry crosses another)
  - "sdi": spatial disjoint (geometry is disjoint from another)
  - "seq": spatial equal (geometry is equal to another)
  - "sin": spatial intersects (geometry intersects another)
  - "sov": spatial overlaps (geometry overlaps another)
  - "sto": spatial touches (geometry touches another)
  - "swi": spatial within (geometry is within another)
  - "sic": spatial is closed (geometry is closed and simple)
  - "sis": spatial is simple (geometry is simple)
  - "siv": spatial is valid (geometry is valid)

These filters are based on OGC standards and so is the WKT specification in which the geometry columns are represented.

### Authentication

Authentication is done by means of sending a "Authorization" header. It identifies the user and stores this in the `$_SESSION` super global. 
This variable can be used in the authorization handlers to decide wether or not sombeody should have read or write access to certain tables, columns or records.
Currently there are two types of authentication supported: "Basic" and "JWT". This functionality is enabled by adding the 'basicAuth' and/or 'jwtAuth' middleware.

#### Basic authentication

The Basic type supports a file that holds the users and their (hashed) passwords separated by a colon (':'). 
When the passwords are entered in plain text they fill be automatically hashed.
The authenticated username will be stored in the `$_SESSION['username']` variable.
You need to send an "Authorization" header containing a base64 url encoded and colon separated username and password after the word "Basic".

    Authorization: Basic dXNlcm5hbWUxOnBhc3N3b3JkMQ

This example sends the string "username1:password1".

#### JWT authentication

The JWT type requires another (SSO/Identity) server to sign a token that contains claims. 
Both servers share a secret so that they can either sign or verify that the signature is valid.
Claims are stored in the `$_SESSION['claims']` variable. You need to send an "X-Authorization" 
header containing a base64 url encoded and dot separated token header, body and signature after
the word "Bearer" ([read more about JWT here](https://jwt.io/)). The standard says you need to
use the "Authorization" header, but this is problematic in Apache and PHP.

    X-Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWUsImlhdCI6IjE1MzgyMDc2MDUiLCJleHAiOjE1MzgyMDc2MzV9.Z5px_GT15TRKhJCTHhDt5Z6K6LRDSFnLj8U5ok9l7gw

This example sends the signed claims:

    {
      "sub": "1234567890",
      "name": "John Doe",
      "admin": true,
      "iat": "1538207605",
      "exp": 1538207635
    }

NB: The JWT implementation only supports the hash based algorithms HS256, HS384 and HS512.

## Authorizing operations

The Authorization model acts on "operations". The most important ones are listed here:

    method path                  - operation - description
    ----------------------------------------------------------------------------------------
    GET    /records/{table}      - list      - lists records
    POST   /records/{table}      - create    - creates records
    GET    /records/{table}/{id} - read      - reads a record by primary key
    PUT    /records/{table}/{id} - update    - updates columns of a record by primary key
    DELETE /records/{table}/{id} - delete    - deletes a record by primary key
    PATCH  /records/{table}/{id} - increment - increments columns of a record by primary key

The "`/openapi`" endpoint will only show what is allowed in your session. It also has a special 
"document" operation to allow you to hide tables and columns from the documentation.
    
For endpoints that start with "`/columns`" there are the operations "reflect" and "remodel". 
These operations can display or change the definition of the database, table or column. 
This functionality is disabled by default and for good reason (be careful!). 
Add the "columns" controller in the configuration to enable this functionality.

### Authorizing tables, columns and records

By default all tables and columns are accessible. If you want to restrict access to some tables you may add the 'authorization' middleware 
and define a 'authorization.tableHandler' function that returns 'false' for these tables.

    'authorization.tableHandler' => function ($operation, $tableName) {
        return $tableName != 'license_keys';
    },

The above example will restrict access to the table 'license_keys' for all operations.

    'authorization.columnHandler' => function ($operation, $tableName, $columnName) {
        return !($tableName == 'users' && $columnName == 'password');
    },

The above example will restrict access to the 'password' field of the 'users' table for all operations.

    'authorization.recordHandler' => function ($operation, $tableName) {
        return ($tableName == 'users') ? 'filter=username,neq,admin' : '';
    },

The above example will disallow access to user records where the username is 'admin'. 
This construct adds a filter to every executed query. 

NB: You need to handle the creation of invalid records with a validation (or sanitation) handler.

### Sanitizing input

By default all input is accepted and sent to the database. If you want to strip (certain) HTML tags before storing you may add 
the 'sanitation' middleware and define a 'sanitation.handler' function that returns the adjusted value.

    'sanitation.handler' => function ($operation, $tableName, $column, $value) {
        return is_string($value) ? strip_tags($value) : $value;
    },

The above example will strip all HTML tags from strings in the input.

### Validating input

By default all input is accepted. If you want to validate the input, you may add the 'validation' middleware and define a 
'validation.handler' function that returns a boolean indicating whether or not the value is valid.

    'validation.handler' => function ($operation, $tableName, $column, $value, $context) {
        return ($column['name'] == 'post_id' && !is_numeric($value)) ? 'must be numeric' : true;
    },

When you edit a comment with id 4 using:

    PUT /records/comments/4

And you send as a body:

    {"post_id":"two"}

Then the server will return a '422' HTTP status code and nice error message:

    {
        "code": 1013,
        "message": "Input validation failed for 'comments'",
        "details": {
            "post_id":"must be numeric"
        }
    }

You can parse this output to make form fields show up with a red border and their appropriate error message.

### Multi-tenancy support

You may use the "multiTenancy" middleware when you have a multi-tenant database. 
If your tenants are identified by the "customer_id" column you can use the following handler:

    'multiTenancy.handler' => function ($operation, $tableName) {
        return ['customer_id' => 12];
    },

This construct adds a filter requiring "customer_id" to be "12" to every operation (except for "create").
It also sets the column "customer_id" on "create" to "12" and removes the column from any other write operation.

### Customization handlers

You may use the "customization" middleware to modify request and response and implement any other functionality.

    'customization.beforeHandler' => function ($operation, $tableName, $request, $environment) {
        $environment->start = microtime(true);
    },
    'customization.afterHandler' => function ($operation, $tableName, $response, $environment) {
        $response->addHeader('X-Time-Taken', microtime(true) - $environment->start);
    },

The above example will add a header "X-Time-Taken" with the number of seconds the API call has taken.

### File uploads

File uploads are supported through the [FileReader API](https://caniuse.com/#feat=filereader).

## OpenAPI specification

On the "/openapi" end-point the OpenAPI 3.0 (formerly called "Swagger") specification is served. 
It is a machine readable instant documentation of your API. To learn more, check out these links:

- [Swagger Editor](https://editor.swagger.io/) can be used to view and debug the generated specification.
- [OpenAPI specification](https://swagger.io/specification/) is a manual for creating an OpenAPI specification.
- [Swagger Petstore](https://petstore.swagger.io/) is an example documentation that is generated using OpenAPI.

## Cache

There are 4 cache engines that can be configured by the "cacheType" config parameter:

- TempFile (default)
- Redis
- Memcache
- Memcached

You can install the dependencies for the last three engines by running:

    sudo apt install php-redis redis
    sudo apt install php-memcache memcached
    sudo apt install php-memcached memcached

The default engine has no dependencies and will use temporary files in the system "temp" path.

You may use the "cachePath" config parameter to specify the file system path for the temporary files or
in case that you use a non-default "cacheType" the hostname (optionally with port) of the cache server.

## Types

These are the supported types with their default length/precision/scale:

character types
- varchar(255)
- clob

boolean types:
- boolean

integer types:
- integer
- bigint

floating point types:
- float
- double

decimal types:
- decimal(19,4)

date/time types:
- date
- time
- timestamp

binary types:
- varbinary(255)
- blob

other types:
- geometry /* non-jdbc type, extension with limited support */

## 64 bit integers in JavaScript

JavaScript does not support 64 bit integers. All numbers are stored as 64 bit floating point values. The mantissa of a 64 bit floating point number is only 53 bit and that is why all integer numbers bigger than 53 bit may cause problems in JavaScript.

## Errors

The following errors may be reported:

- 1000: Route not found (404 NOT FOUND)
- 1001: Table not found (404 NOT FOUND)
- 1002: Argument count mismatch (422 UNPROCESSABLE ENTITY)
- 1003: Record not found (404 NOT FOUND)
- 1004: Origin is forbidden (403 FORBIDDEN)
- 1005: Column not found (404 NOT FOUND)
- 1006: Table already exists (409 CONFLICT)
- 1007: Column already exists (409 CONFLICT)
- 1008: Cannot read HTTP message (422 UNPROCESSABLE ENTITY)
- 1009: Duplicate key exception (409 CONFLICT)
- 1010: Data integrity violation (409 CONFLICT)
- 1011: Authentication required (401 UNAUTHORIZED)
- 1012: Authentication failed (403 FORBIDDEN)
- 1013: Input validation failed (422 UNPROCESSABLE ENTITY)
- 1014: Operation forbidden (403 FORBIDDEN)
- 1015: Operation not supported (405 METHOD NOT ALLOWED)
- 1016: Temporary or permanently blocked (403 FORBIDDEN)
- 1017: Bad or missing XSRF token (403 FORBIDDEN)
- 1018: Only AJAX requests allowed (403 FORBIDDEN)
- 1019: File upload failed (422 UNPROCESSABLE ENTITY)
- 9999: Unknown error (500: INTERNAL SERVER ERROR)

The following JSON structure is used:

    {
        "code":1002,
        "message":"Argument count mismatch in '1'"
    }

NB: Any non-error response will have status: 200 OK

## Tests

I am testing mainly on Ubuntu and I have the following test setups:

  - (Docker) Debian 9 with PHP 7.0, MariaDB 10.1, PostgreSQL 9.6 (PostGIS 2.3)
  - (Docker) Ubuntu 16.04 with PHP 7.0, MariaDB 10.0, PostgreSQL 9.5 (PostGIS 2.2) and SQL Server 2017
  - (Docker) Ubuntu 18.04 with PHP 7.2, MySQL 5.7, PostgreSQL 10.4 (PostGIS 2.4)

This covers not all environments (yet), so please notify me of failing tests and report your environment. 
I will try to cover most relevant setups in the "docker" folder of the project.

### Running

To run the functional tests locally you may run the following command:

    php test.php

This runs the functional tests from the "tests" directory. It uses the database dumps (fixtures) and
database configuration (config) from the corresponding subdirectories.

## Nginx config example
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.php index.html index.htm index.nginx-debian.html;
    server_name server_domain_or_IP;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ [^/]\.php(/|$) {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        try_files $fastcgi_script_name =404;
        set $path_info $fastcgi_path_info;
        fastcgi_param PATH_INFO $path_info;
        fastcgi_index index.php;
        include fastcgi.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

### Docker

Install docker using the following commands and then logout and login for the changes to take effect:

    sudo apt install docker.io
    sudo usermod -aG docker ${USER}

To run the docker tests run "build_all.sh" and "run_all.sh" from the docker directory. The output should be:

    ================================================
    Debian 9 (PHP 7.0)
    ================================================
    [1/4] Starting MariaDB 10.1 ..... done
    [2/4] Starting PostgreSQL 9.6 ... done
    [3/4] Starting SQLServer 2017 ... skipped
    [4/4] Cloning PHP-CRUD-API v2 ... skipped
    ------------------------------------------------
    mysql: 83 tests ran in 378 ms, 0 failed
    pgsql: 83 tests ran in 284 ms, 0 failed
    sqlsrv: skipped, driver not loaded
    ================================================
    Ubuntu 16.04 (PHP 7.0)
    ================================================
    [1/4] Starting MariaDB 10.0 ..... done
    [2/4] Starting PostgreSQL 9.5 ... done
    [3/4] Starting SQLServer 2017 ... done
    [4/4] Cloning PHP-CRUD-API v2 ... skipped
    ------------------------------------------------
    mysql: 83 tests ran in 381 ms, 0 failed
    pgsql: 83 tests ran in 290 ms, 0 failed
    sqlsrv: 83 tests ran in 4485 ms, 0 failed
    ================================================
    Ubuntu 18.04 (PHP 7.2)
    ================================================
    [1/4] Starting MySQL 5.7 ........ done
    [2/4] Starting PostgreSQL 10.4 .. done
    [3/4] Starting SQLServer 2017 ... skipped
    [4/4] Cloning PHP-CRUD-API v2 ... skipped
    ------------------------------------------------
    mysql: 83 tests ran in 364 ms, 0 failed
    pgsql: 83 tests ran in 294 ms, 0 failed
    sqlsrv: skipped, driver not loaded

The above test run (including starting up the databases) takes less than one minute on my machine.

    $ ./run.sh 
    1) debian9
    2) ubuntu16
    3) ubuntu18
    > 3
    ================================================
    Ubuntu 18.04 (PHP 7.2)
    ================================================
    [1/4] Starting MySQL 5.7 ........ done
    [2/4] Starting PostgreSQL 10.4 .. done
    [3/4] Starting SQLServer 2017 ... skipped
    [4/4] Cloning PHP-CRUD-API v2 ... skipped
    ------------------------------------------------
    mysql: 83 tests ran in 364 ms, 0 failed
    pgsql: 83 tests ran in 294 ms, 0 failed
    sqlsrv: skipped, driver not loaded
    root@b7ab9472e08f:/php-crud-api# 

As you can see the "run.sh" script gives you access to a prompt in a chosen the docker environment.
In this environment the local files are mounted. This allows for easy debugging on different environments.
You may type "exit" when you are done.
