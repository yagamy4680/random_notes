
{{toc}}

## BackboneREST sample codes

```ruby
require! <[async express body-parser]>
{Model} = require 'backbone'
OrmSync = require 'backbone-orm' .sync
MongoSync = require 'backbone-mongo' .sync
RestController = require 'backbone-rest'

MONGO_URL = "mongodb://localhost:27017/pbs"

class Developer extends Model
  urlRoot: "#{MONGO_URL}/developers"
  sync: MongoSync Developer

app = express!
app.use bodyParser.json!

rest = new RestController app, {
  model_type: Developer,
  route: '/developers', 
  templates: {
    show: { $select: ['id', 'name', 'email', 'pass_type'] },
    show_with_staff: { $select: ['id', 'name', 'email']}
  },
  default_template: 'show'
}

app.listen 8080, (err) ->
  return console.log "failed to start http server at 8080, err: #{err}" if err
  console.log "listening 8080 ..."
```


## Create a new Developer

Command-line:

```bash
http --verbose http://192.168.1.16:8080/developers \
  name='t2t.test' \
  email='test@dhvac.io' \
  password='aaa' \
  pass_type='pass.io.t2t.yasai.formal2'
```

Outputs:

```text
POST /developers HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate
Content-Length: 107
Content-Type: application/json; charset=utf-8
Host: 192.168.1.16:8080
User-Agent: HTTPie/0.8.0

{
    "email": "test@dhvac.io",
    "name": "t2t.test",
    "pass_type": "pass.io.t2t.yasai.formal2",
    "password": "aaa"
}

HTTP/1.1 200 OK
Cache-Control: no-cache
Connection: keep-alive
Content-Length: 51
Content-Type: application/json; charset=utf-8
Date: Sun, 25 Jan 2015 05:09:36 GMT
X-Powered-By: Express

{
    "id": "54c47a909df6caf8067ef0c4",
    "name": "t2t.test"
}
```

## Show all developers, with selected template `show_with_staff`

Command-line:

```bash
http --verbose GET http://192.168.1.16:8080/developers \$template==show_with_staff
```

Outputs:

```text
GET /developers?%24template=show_with_staff HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate
Host: 192.168.1.16:8080
User-Agent: HTTPie/0.8.0

HTTP/1.1 200 OK
Cache-Control: no-cache
Connection: keep-alive
Content-Length: 404
Content-Type: application/json; charset=utf-8
Date: Sun, 25 Jan 2015 05:10:25 GMT
ETag: W/"194-7e8f440d"
X-Powered-By: Express

[
    {
        "email": "test@t2t.io",
        "id": "54c251aa482e61cd0755dd8f",
        "name": "t2t.test"
    },
    {
        "email": "pass@dhvac.io",
        "id": "54c251aa482e61cd0755dd90",
        "name": "t2t.yasai"
    },
    {
        "id": "54c477f29df6caf8067ef0c0"
    },
    {
        "id": "54c4780b9df6caf8067ef0c1"
    },
    {
        "id": "54c4780e9df6caf8067ef0c2"
    },
    {
        "email": "test@dhvac.io",
        "id": "54c479a99df6caf8067ef0c3",
        "name": "aabbcc"
    },
    {
        "email": "test@dhvac.io",
        "id": "54c47a909df6caf8067ef0c4",
        "name": "t2t.test"
    }
]
```


## Update a developer

Command-line:

```bash
http --verbose PUT http://192.168.1.16:8080/developers/54c47a909df6caf8067ef0c4 name='aabbcc'
```

```text
PUT /developers/54c47a909df6caf8067ef0c4 HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate
Content-Length: 18
Content-Type: application/json; charset=utf-8
Host: 192.168.1.16:8080
User-Agent: HTTPie/0.8.0

{
    "name": "aabbcc"
}

HTTP/1.1 200 OK
Cache-Control: no-cache
Connection: keep-alive
Content-Length: 49
Content-Type: application/json; charset=utf-8
Date: Sun, 25 Jan 2015 05:11:26 GMT
X-Powered-By: Express

{
    "id": "54c47a909df6caf8067ef0c4",
    "name": "aabbcc"
}
```

## Delete a developer

Command-line:

```bash
http --verbose DELETE http://192.168.1.16:8080/developers/54c47a909df6caf8067ef0c4
```

Outputs:

```text
DELETE /developers/54c47a909df6caf8067ef0c4 HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate
Content-Length: 0
Host: 192.168.1.16:8080
User-Agent: HTTPie/0.8.0

HTTP/1.1 200 OK
Cache-Control: no-cache
Connection: keep-alive
Content-Length: 2
Content-Type: application/json; charset=utf-8
Date: Sun, 25 Jan 2015 05:13:45 GMT
X-Powered-By: Express

{}
```

## Add a developer and its own metapass

We define 2 models `Developer` and `MetaPass`. The prior one has many of the later ones. 

```ruby
class Developer extends Model
  urlRoot: "#{MONGO_URL}/developers"
  schema:
    metapasses: -> ['hasMany', MetaPass]
  sync: MongoSync Developer

class MetaPass extends Model
  urlRoot: "#{MONGO_URL}/metapasses"
  schema:
    developer: -> ['belongsTo', Developer]
  sync: MongoSync MetaPass

r = express!
r.use (req, res, next) ->
  console.log "[r] #{req.url}, #{req.socket.localPort}, #{req.method}"
  next!

r0 = new RestController r, {
  model_type: Developer,
  route: '/developers', 
  templates: {
    show: { $select: ['id', 'name', 'email', 'pass_type'] },
    show_with_staff: { $select: ['id', 'name', 'email']}
  },
  default_template: 'show'
}
```

Then, insert following data to REST end point:

```ruby
name: 't2t.test'
email: 'test@t2t.io'
password: '123456'
pass_type_identifier: 'pass.io.t2t.yasai.ruby'
team_identifier: 'P9NZ6CT96Z'
keys_archive_id: 'aabbcc'
keys_password: '123456'
metapasses:
  * uuid: 'aaa'
    qrcode_url: 'good'
  * uuid: 'bbb'
    qrcode_url: 'yes'%
```

```bash
cat data.ls | lsc -jp | http --verbose http://192.168.1.16:8080/rest/developers
POST /rest/developers HTTP/1.1
Accept: application/json
Accept-Encoding: gzip, deflate
Content-Length: 365
Content-Type: application/json; charset=utf-8
Host: 192.168.1.16:8080
User-Agent: HTTPie/0.8.0

{
    "email": "test@t2t.io",
    "keys_archive_id": "aabbcc",
    "keys_password": "123456",
    "metapasses": [
        {
            "qrcode_url": "good",
            "uuid": "aaa"
        },
        {
            "qrcode_url": "yes",
            "uuid": "bbb"
        }
    ],
    "name": "t2t.test",
    "pass_type_identifier": "pass.io.t2t.yasai.ruby",
    "password": "123456",
    "team_identifier": "P9NZ6CT96Z"
}

HTTP/1.1 200 OK
Cache-Control: no-cache
Connection: keep-alive
Content-Length: 73
Content-Type: application/json; charset=utf-8
Date: Sun, 25 Jan 2015 08:56:30 GMT
X-Powered-By: Express

{
    "email": "test@t2t.io",
    "id": "54c4afbe388468030a75d1bd",
    "name": "t2t.test"
}
```

After insertion, when we query the Mongodb, it's found that only ONE metapass data is inserted into database, and this record is updated for twice.

```bash
db.metapasses.find()

{ "_id" : ObjectId("54c4ae5f388468030a75d1bc"), 
	"uuid" : "aaa", 
	"qrcode_url" : "good", 
	"developer_id" : "54c4ae5e388468030a75d1bb", 
	"_rev" : 2 }
```

