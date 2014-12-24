### SocketServer for Unix Domain Socket

Here is the sample code of `app.ls`:

```coffeescript
#!/usr/bin/env lsc
require! <[net byline]>

server = net.createServer (c) ->
  console.log "client connected"
  console.log "remoteAddress = #{c.remoteAddress}"
  console.log "remotePort = #{c.remotePort}"
  console.log "localPort = #{c.localPort}"
  console.log ""
  c.on 'end', -> console.log "client disconnected"
  lineReader = byline.createStream c
  lineReader.on 'data', (line) -> console.log "receive: #{line}"
  c.write 'hello\r\n'


unixSocket = "/tmp/app.sock"

server.listen unixSocket, ->
  console.log "listening `#{unixSocket}` ..."
```

2nd step, run it to listen to a Unix domain socket (`/tmp/app.sock`): 

`rm -f /tmp/app.sock & ./app.ls`.

3rd step, mount the unix socket to a TCP server with **socat**:

`socat -d -d TCP-LISTEN:12345,reuseaddr,fork UNIX-CONNECT:/tmp/app.sock`

4th step, open 2 new terminal windows, and connect to that TCP server via `telnet`:

`telnet localhost 12345`

When the 2 clients are connected to TCP server, and disconnect.

```text
# telnet 192.168.1.175 12345
Trying 192.168.1.175...
Connected to yasai-a820663d7d29.lan.
Escape character is '^]'.
hello
aa
bb
^]
telnet> quit
Connection closed.
```

```text
# telnet 192.168.1.175 12345
Trying 192.168.1.175...
Connected to yasai-a820663d7d29.lan.
Escape character is '^]'.
hello
cc
dd
^]
telnet> quit
Connection closed.
```


Here are the outputs of **socat**:

```text
# socat -d -d TCP-LISTEN:12345,reuseaddr,fork UNIX-CONNECT:/tmp/app.sock

2014/12/24 12:00:01 socat[4836] N listening on AF=2 0.0.0.0:12345
2014/12/24 12:00:06 socat[4836] N accepting connection from AF=2 192.168.1.197:56936 on AF=2 192.168.1.175:12345
2014/12/24 12:00:06 socat[4836] N forked off child process 4844
2014/12/24 12:00:06 socat[4836] N listening on AF=2 0.0.0.0:12345
2014/12/24 12:00:06 socat[4844] N opening connection to AF=1 "/tmp/app.sock"
2014/12/24 12:00:06 socat[4844] N successfully connected from local address AF=1 "\xB6\x1@\xEE\x7E"
2014/12/24 12:00:06 socat[4844] N starting data transfer loop with FDs [4,4] and [3,3]
2014/12/24 12:00:09 socat[4836] N accepting connection from AF=2 192.168.1.197:56937 on AF=2 192.168.1.175:12345
2014/12/24 12:00:09 socat[4836] N forked off child process 4845
2014/12/24 12:00:09 socat[4836] N listening on AF=2 0.0.0.0:12345
2014/12/24 12:00:09 socat[4845] N opening connection to AF=1 "/tmp/app.sock"
2014/12/24 12:00:09 socat[4845] N successfully connected from local address AF=1 "\xB6\x1@\xEE\x7E"
2014/12/24 12:00:09 socat[4845] N starting data transfer loop with FDs [4,4] and [3,3]
2014/12/24 12:00:18 socat[4844] N socket 1 (fd 4) is at EOF
2014/12/24 12:00:18 socat[4844] N socket 1 (fd 4) is at EOF
2014/12/24 12:00:18 socat[4844] N socket 2 (fd 3) is at EOF
2014/12/24 12:00:18 socat[4844] N exiting with status 0
2014/12/24 12:00:24 socat[4845] N socket 1 (fd 4) is at EOF
2014/12/24 12:00:24 socat[4845] N socket 1 (fd 4) is at EOF
2014/12/24 12:00:24 socat[4845] N socket 2 (fd 3) is at EOF
2014/12/24 12:00:24 socat[4845] N exiting with status 0
```

Here are the console outputs of `app.ls`:

```text
# rm -f /tmp/app.sock & ./app.ls

[1] 4837
listening `/tmp/app.sock` ...
client connected
remoteAddress = undefined
remotePort = undefined
localPort = undefined

client connected
remoteAddress = undefined
remotePort = undefined
localPort = undefined

receive: aa
receive: bb
client disconnected
receive: cc
receive: dd
client disconnected
```