# Rest MV

This is the beginning of a restmv api.  This version is starting
fully as a jBASE function using built in jBASE objects.

A future version should either use a ifdiff package or use wobj
with built in multi-patform capability

## End point

http(s)://{host}/api/restmv/{version}

## Encoding

Default encoding is type: array.  The record will be stored in a 3 level json array.  The second and third level arrays will ONLY be used if the pick record has multi-values.

Example
```
Pick Data
1> First Name
2> Last Name
3> STREET]CITY]STATE]ZIP
4> 4-1-1\4-1-2]4-2-1\4-2-2

In json
{ "data": [
   "First Name",
   "Last Name",
   [ "STREET", "CITY", "STATE", "ZIP" ],
   [ [ "4-1-1", "4-1-2"], [ "4-2-1", "4-2-2" ]]
]}
```

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

## Locking

No locking is implemented at this point.  Since rest is not persistent normal locking mechanisms will not work.  If locking
is needed we will need to implement a locking daemon that will hold the locks for us.  jBASE long term has a non-persistent
locking scheme but it is not complete yet.

## Methods

https(s)://{host}/api/restmv/{version}/file/{filename}/{itemname}

{filename}/{itemname} is actually full pathing.  This is typically
file/{account}/{file}/{item}
file/{account}/{dict file}/{item}
file/{account}/{file}/{file}/{item}


This endpoing will handle all file IO.  Normal http verbs action will be utilized for different crud actions

### Read a record

http(s)://{host}/api/restmv/{version}/file/{account}/{filename}/{itemname}
VERB: GET

Response
```
{ "status": "ok",
  "statusmsg":"status msg",
  "_id": "id",
  "type": "array - encoding type"
  "data": [ "array representation of data" ]
}
```

### Update a record

http(s)://{host}/api/restmv/{version}/file/{account}/{filename}/{itemname}
VERB: PUT (Post will not be used)

The body needs to contain our record as an array

```
{  "data": [ "array of data" ] }
```

### Delete a record

http(s)://{host}/api/restmv/{version}/file/{account}/{filename}/{itemname}
VERB: DELETE (Should this be more focused??)

### Rename

http(s)://{host}/api/restmv/{version}/rename/{account}/{filename}/{itemname}?newname={newname}

VERB: GET (Should this be more focused??)

### Create Directory/File

http(s)://{host}/api/restmv/{version}/create/{account}/{filename}/{itemname}?type=2

VERB: GET (Should this be more focused??)
* type 1 File
* type 2 Directory

### Login

http(s)://{host}/api/restmv/{version}/login
VERB: POST

Payload
```
{
   "UserId": "User Id",
   "Password": "Password",
   "ServerIP": "mvon",
   "Account": "Optional Account to log into"
}
```
Response
```
{
   "access_token":"wjwt Token"
}
```

# Under Construction/todo 

## Subroutine

http(s)://{host}/api/restmv/{version}/call/subroutinename
VERB: PUT (Post will not be used)

Params will be passed as json arrays.

```
{  "params": [ [ "param1" ], [ "param2" ],... ]
```

## Execute

http(s)://{host}/api/restmv/{version}/execute
VERB: PUT (Post will not be used)

We will need to handle all the execute options.  This is a very early spec sheet.

```
{  
   "GETLIST":"Name of list to do a GET-LIST first",
   "EXECUTE":"Execute command",
   "DATA ITEMS": [ "array of data statements","data2" ],
   "CAPTURING": "Captured output - Array format",
   "RETURNING": "Returning code"
}

```


