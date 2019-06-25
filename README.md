# Rest MV

This is the beginning of a restmv api.  This version is starting
fully as a jBASE function using built in jBASE objects.

A future version should either use a ifdiff package or use wobj
with built in multi-patform capability

## End point

http(s)://{host}/api/restmv/{version}

## Encoding

Due to encoding issues around char(254) we will be encoding all Multi-Value records.  Current MVConnect has built
in char(254) -> new line conversions that make returning a char(254) impossible.  In addition direct returning
of a char(254) can cause issues with clients that are expecting UTF-8.  Due to the time to build out this library we
will be utilizing a HEX encoding that then returns NON utf-8 responses.  This output will be the DEFAULT and in the
future we will utilize a content type heading to allow others (such as BASE64).

Due to this the default for now is

ACCEPT: application/json   
X-RESTMV-MVENCODING: HEX

If these are left off then the above defaults will be used.  If you request something else then the request will
be rejected.

## Response/Body

MVConnect at this point generates a error if NO content is produced.  Due to this limitation all services will return a
generic payload

```
{ "status": "ok",
  "statusmsg":"status msg",
  .. Additional fields
}
```

## Versioning

Basic versioning will be included.  This will be a simple router at the top that redirects the ENTIRE call to the
specific versioning routine.

## Error Handling

This function will return valid status codes in addition to the default body.  The body results with either be OK/Error while the
statusmsg will give a more pick based response.  See this place for assignment of error messages.

https://blog.restcase.com/rest-api-error-codes-101/

At this point we are going to use the Twilio design

```
{
   "status":401,
   "message":"Error message",
   "more_info":"url link"
}
```

## Methods

https(s)://{host}/api/restmv/{version}/crud/{filename}/{itemname}

This endpoing will handle all file IO.  Normal http verbs action will be utilized for different crud actions

### Read a record

http(s)://{host}/api/restmv/{version}/crud/{filename}/{itemname}
VERB: GET

Response
```
{ "status": "ok",
  "statusmsg":"status msg",
  "record":"{hex representation of record}"
}
```

### Update a record

http(s)://{host}/api/restmv/{version}/crud/{filename}/{itemname}
VERB: PUT (Post will not be used)

The body needs to contain our record in hex

```
{  "record": "{hex of data} }
```

### Delete a record

http(s)://{host}/api/restmv/{version}/crud/{filename}/{itemname}
VERB: DELETE (Should this be more focused??)




