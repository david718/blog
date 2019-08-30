---
title: (0824)node module http createServer function
date: 2019-08-24 18:08:59
category: TIL
---

node 기본 module인 http의 method  
createServer를 파보겠다

##createServer 기본 사용법
node의 공식 홈페이지에 가면 사용법은 아래와 같다  
`http.createServer([options][, requestlistener])`

source code는 아래와 같다

```js
const { Server } = require('_http_server')

function createServer(opts, requestListener) {
  return new Server(opts, requestListener)
}
```

options와 requestlistener라는 함수를 인자로 넣으면
Server라는 function에 인자로 똑같이 넣어서  
return받은 것을 return 해주고 있다

##Server
Server는 `_http_server` 라는 파일에 선언되어 있는 함수이다
source code는 아래와 같다

```js
const net = require('net')

function Server(options, requestListener) {
  if (!(this instanceof Server)) return new Server(options, requestListener)

  if (typeof options === 'function') {
    requestListener = options
    options = {}
  } else if (options == null || typeof options === 'object') {
    options = { ...options }
  } else {
    throw new ERR_INVALID_ARG_TYPE('options', 'object', options)
  }

  this[kIncomingMessage] = options.IncomingMessage || IncomingMessage
  this[kServerResponse] = options.ServerResponse || ServerResponse

  net.Server.call(this, { allowHalfOpen: true })

  if (requestListener) {
    this.on('request', requestListener)
  }

  // Similar option to this. Too lazy to write my own docs.
  // http://www.squid-cache.org/Doc/config/half_closed_clients/
  // http://wiki.squid-cache.org/SquidFaq/InnerWorkings#What_is_a_half-closed_filedescriptor.3F
  this.httpAllowHalfOpen = false

  this.on('connection', connectionListener)

  this.timeout = kDefaultHttpServerTimeout
  this.keepAliveTimeout = 5000
  this.maxHeadersCount = null
  this.headersTimeout = 40 * 1000 // 40 seconds
}
```

한 줄 씩 해석해보자

1. 만약 this가 Server의 instance가 아니면 Server를 다시 호출하여 return한다
2. 만약 options가 function이면 requestListener만 받은것처럼 동작해라
3. options가 null이나 object면 그대로 options에 넣어줘라
4. 둘다 아니면 error 뱉어라
5. kIncomingMessage, kServerResponse 값 할당
6. net 이라는 파일에서 module.exports 객체 안에 들어있는 Server 함수를 call을 활용해 호출
7. 만약 requestListener 가 존재하면 `request`상태 일 때 requestListener 실행
8. httpAllowHalfOpen 값은 false
9. `connection`상태 일 때 connectionListener 호출
10. timeout, keepAliveTimeout, maxHeadersCount, headersTimeout 값 할당

여기서 다시 알아볼 것은 6번이다

> 6. net 이라는 파일에서 module.exports 객체 안에 들어있는 Server 함수를 call을 활용해 호출

그럼 net에서 Server는 어떤 함수일까

##net 안에 Server function

```js
function Server(options, connectionListener) {
  if (!(this instanceof Server)) return new Server(options, connectionListener)

  EventEmitter.call(this)

  if (typeof options === 'function') {
    connectionListener = options
    options = {}
    this.on('connection', connectionListener)
  } else if (options == null || typeof options === 'object') {
    options = { ...options }

    if (typeof connectionListener === 'function') {
      this.on('connection', connectionListener)
    }
  } else {
    throw new ERR_INVALID_ARG_TYPE('options', 'Object', options)
  }

  this._connections = 0

  Object.defineProperty(this, 'connections', {
    get: deprecate(
      () => {
        if (this._usingWorkers) {
          return null
        }
        return this._connections
      },
      'Server.connections property is deprecated. ' +
        'Use Server.getConnections method instead.',
      'DEP0020'
    ),
    set: deprecate(
      val => (this._connections = val),
      'Server.connections property is deprecated.',
      'DEP0020'
    ),
    configurable: true,
    enumerable: false,
  })

  this[async_id_symbol] = -1
  this._handle = null
  this._usingWorkers = false
  this._workers = []
  this._unref = false

  this.allowHalfOpen = options.allowHalfOpen || false
  this.pauseOnConnect = !!options.pauseOnConnect
}
```

이것도 한줄 한줄 보자

1. 만약 Server의 instance가 아니라면 Server를 return하자
2. EventEmitter를 Server에 묶어서 호출하자
3. 중간에 if문은 arguments로 option이 안들어왔을때 처리하는 구문
4. `_.connections` 값을 0으로 할당
5. Server 객체의 connetions 라는 새로운 속성 정의
6. 나머지 값들 할당

##Server.listen() source code

```js
Server.prototype.listen = function(...args) {
  const normalized = normalizeArgs(args)
  var options = normalized[0]
  const cb = normalized[1]

  if (this._handle) {
    throw new ERR_SERVER_ALREADY_LISTEN()
  }

  if (cb !== null) {
    this.once('listening', cb)
  }
  const backlogFromArgs =
    // (handle, backlog) or (path, backlog) or (port, backlog)
    toNumber(args.length > 1 && args[1]) || toNumber(args.length > 2 && args[2]) // (port, host, backlog)

  options = options._handle || options.handle || options
  const flags = getFlags(options.ipv6Only)
  // (handle[, backlog][, cb]) where handle is an object with a handle
  if (options instanceof TCP) {
    this._handle = options
    this[async_id_symbol] = this._handle.getAsyncId()
    listenInCluster(this, null, -1, -1, backlogFromArgs)
    return this
  }
  // (handle[, backlog][, cb]) where handle is an object with a fd
  if (typeof options.fd === 'number' && options.fd >= 0) {
    listenInCluster(this, null, null, null, backlogFromArgs, options.fd)
    return this
  }

  // ([port][, host][, backlog][, cb]) where port is omitted,
  // that is, listen(), listen(null), listen(cb), or listen(null, cb)
  // or (options[, cb]) where options.port is explicitly set as undefined or
  // null, bind to an arbitrary unused port
  if (
    args.length === 0 ||
    typeof args[0] === 'function' ||
    (typeof options.port === 'undefined' && 'port' in options) ||
    options.port === null
  ) {
    options.port = 0
  }
  // ([port][, host][, backlog][, cb]) where port is specified
  // or (options[, cb]) where options.port is specified
  // or if options.port is normalized as 0 before
  var backlog
  if (typeof options.port === 'number' || typeof options.port === 'string') {
    if (!isLegalPort(options.port)) {
      throw new ERR_SOCKET_BAD_PORT(options.port)
    }
    backlog = options.backlog || backlogFromArgs
    // start TCP server listening on host:port
    if (options.host) {
      lookupAndListen(
        this,
        options.port | 0,
        options.host,
        backlog,
        options.exclusive,
        flags
      )
    } else {
      // Undefined host, listens on unspecified address
      // Default addressType 4 will be used to search for master server
      listenInCluster(
        this,
        null,
        options.port | 0,
        4,
        backlog,
        undefined,
        options.exclusive
      )
    }
    return this
  }

  // (path[, backlog][, cb]) or (options[, cb])
  // where path or options.path is a UNIX domain socket or Windows pipe
  if (options.path && isPipeName(options.path)) {
    var pipeName = (this._pipeName = options.path)
    backlog = options.backlog || backlogFromArgs
    listenInCluster(
      this,
      pipeName,
      -1,
      -1,
      backlog,
      undefined,
      options.exclusive
    )

    if (!this._handle) {
      // Failed and an error shall be emitted in the next tick.
      // Therefore, we directly return.
      return this
    }

    let mode = 0
    if (options.readableAll === true) mode |= PipeConstants.UV_READABLE
    if (options.writableAll === true) mode |= PipeConstants.UV_WRITABLE
    if (mode !== 0) {
      const err = this._handle.fchmod(mode)
      if (err) {
        this._handle.close()
        this._handle = null
        throw errnoException(err, 'uv_pipe_chmod')
      }
    }
    return this
  }

  if (!('port' in options || 'path' in options)) {
    throw new ERR_INVALID_ARG_VALUE(
      'options',
      options,
      'must have the property "port" or "path"'
    )
  }

  throw new ERR_INVALID_OPT_VALUE('options', inspect(options))
}
```

##createServer 호출해서 서버 만든 예시

```js
const server = http.createServer((req, res) => {
  let url = req.url

  if (req.url === '/') {
    url = '/index.html'
  }
  if (req.url === '/favicon.ico') {
    res.writeHead(400)
    res.end()
    return
  }

  res.writeHead(200)
  res.end(fs.readFileSync(__dirname + url))
})

server.listen(3000, () => {
  console.log('server is running')
})
```
