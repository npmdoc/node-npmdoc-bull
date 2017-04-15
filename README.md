# api documentation for  [bull (v2.2.6)](https://github.com/OptimalBits/bull#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-bull.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-bull) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-bull.svg)](https://travis-ci.org/npmdoc/node-npmdoc-bull)
#### Job manager

[![NPM](https://nodei.co/npm/bull.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/bull)

[![apidoc](https://npmdoc.github.io/node-npmdoc-bull/build/screenCapture.buildCi.browser.apidoc.html.png)](https://npmdoc.github.io/node-npmdoc-bull/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-bull/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-bull/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "OptimalBits"
    },
    "bugs": {
        "url": "https://github.com/OptimalBits/bull/issues"
    },
    "dependencies": {
        "bluebird": "^3.5.0",
        "bull-redlock": "^2.2.1",
        "debuglog": "^1.0.0",
        "disturbed": "^1.0.6",
        "ioredis": "^2.5.0",
        "lodash": "^4.17.4",
        "semver": "^5.3.0",
        "uuid": "^3.0.1"
    },
    "description": "Job manager",
    "devDependencies": {
        "expect.js": "^0.3.1",
        "gulp": "^3.9.1",
        "gulp-eslint": "^2.1.0",
        "mocha": "^2.5.3",
        "sinon": "^1.17.7"
    },
    "directories": {},
    "dist": {
        "shasum": "568baae0dc0bbcc77d5f1d2fea545a336be7603e",
        "tarball": "https://registry.npmjs.org/bull/-/bull-2.2.6.tgz"
    },
    "gitHead": "94421ac8e8be62467a66abbafab05239e4b55384",
    "homepage": "https://github.com/OptimalBits/bull#readme",
    "keywords": [
        "job",
        "queue",
        "task",
        "parallel"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "manast"
        }
    ],
    "name": "bull",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git://github.com/OptimalBits/bull.git"
    },
    "scripts": {
        "postpublish": "git push && git push --tags",
        "test": "gulp && mocha test/test_* --reporter spec --timeout 5000"
    },
    "version": "2.2.6"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module bull](#apidoc.module.bull)
1.  [function <span class="apidocSignatureSpan"></span>bull (name, redisPort, redisHost, redisOptions)](#apidoc.element.bull.bull)
1.  [function <span class="apidocSignatureSpan">bull.</span>super_ (pubClient, subClient)](#apidoc.element.bull.super_)
1.  [function <span class="apidocSignatureSpan">bull.</span>toString ()](#apidoc.element.bull.toString)
1.  object <span class="apidocSignatureSpan">bull.</span>super_.prototype

#### [module bull.super_](#apidoc.module.bull.super_)
1.  [function <span class="apidocSignatureSpan">bull.</span>super_ ()](#apidoc.element.bull.super_.super_)

#### [module bull.super_.prototype](#apidoc.module.bull.super_.prototype)
1.  [function <span class="apidocSignatureSpan">bull.super_.prototype.</span>distEmit (evt)](#apidoc.element.bull.super_.prototype.distEmit)
1.  [function <span class="apidocSignatureSpan">bull.super_.prototype.</span>off (evt, listener)](#apidoc.element.bull.super_.prototype.off)
1.  [function <span class="apidocSignatureSpan">bull.super_.prototype.</span>on (evt, listener, isGlobal)](#apidoc.element.bull.super_.prototype.on)
1.  [function <span class="apidocSignatureSpan">bull.super_.prototype.</span>removeListener (evt, listener)](#apidoc.element.bull.super_.prototype.removeListener)



# <a name="apidoc.module.bull"></a>[module bull](#apidoc.module.bull)

#### <a name="apidoc.element.bull.bull"></a>[function <span class="apidocSignatureSpan"></span>bull (name, redisPort, redisHost, redisOptions)](#apidoc.element.bull.bull)
- description and source-code
```javascript
function Queue(name, redisPort, redisHost, redisOptions){
  if(!(this instanceof Queue)){
    return new Queue(name, redisPort, redisHost, redisOptions);
  }

  if(_.isObject(redisPort)) {
    var opts = redisPort;
    var redisOpts = opts.redis || {};
    redisPort = redisOpts.port;
    redisHost = redisOpts.host;
    redisOptions = redisOpts.opts || {};
    redisOptions.db = redisOpts.DB || redisOpts.DB;
  } else if(parseInt(redisPort) == redisPort) {
    redisPort = parseInt(redisPort);
    redisOptions =  redisOptions || {};
  } else if(_.isString(redisPort)) {
    try {
      var redisUrl = url.parse(redisPort);
      assert(_.isObject(redisHost) || _.isUndefined(redisHost),
          'Expected an object as redis option');
      redisOptions =  redisHost || {};
      redisPort = redisUrl.port;
      redisHost = redisUrl.hostname;
      if (redisUrl.auth) {
        redisOptions.password = redisUrl.auth.split(':')[1];
      }
    } catch (e) {
      throw new Error(e.message);
    }
  }

  redisOptions = redisOptions || {};

  function createClient(type) {
    var client;
    if(_.isFunction(redisOptions.createClient)){
      client = redisOptions.createClient(type);
    }else{
      client = new redis(redisPort, redisHost, redisOptions);
    }
    return client;
  }

  redisPort = redisPort || 6379;
  redisHost = redisHost || '127.0.0.1';

  var _this = this;

  this.name = name;
  this.keyPrefix = redisOptions.keyPrefix || 'bull';

  //
  // We cannot use ioredis keyPrefix feature until we
  // stop creating keys dynamically in lua scripts.
  //
  delete redisOptions.keyPrefix;

  //
  // Create queue client (used to add jobs, pause queues, etc);
  //
  this.client = createClient('client');

  getRedisVersion(this.client).then(function(version){
    if(semver.lt(version, MINIMUM_REDIS_VERSION)){
      throw new Error('Redis version needs to be greater than ' + MINIMUM_REDIS_VERSION + '. Current: ' + version);
    }
  }).catch(function(err){
    _this.emit('error', err);
  });

  //
  // Keep track of cluster clients for redlock
  //
  this.clients = [this.client];
  if (redisOptions.clients) {
    this.clients.push.apply(this.clients, redisOptions.clients);
  }
  this.redlock = {
    driftFactor: REDLOCK_DRIFT_FACTOR,
    retryCount: REDLOCK_RETRY_COUNT,
    retryDelay: REDLOCK_RETRY_DELAY
  };
  _.extend(this.redlock, redisOptions.redlock || {});

  //
  // Create blocking client (used to wait for jobs)
  //
  this.bclient = createClient('block');

  //
  // Create event subscriber client (receive messages from other instance of the queue)
  //
  this.eclient = createClient('subscriber');

  this.delayTimer = null;
  this.processing = 0;
  this.retrieving = 0;

  this.LOCK_RENEW_TIME = LOCK_RENEW_TIME;
  this.LOCK_DURATION = LOCK_DURATION;
  this.STALLED_JOB_CHECK_INTERVAL = STALLED_JOB_CHECK_INTERVAL;
  this.MAX_STALLED_JOB_COUNT = MAX_STALLED_JOB_COUNT;

  // bubble up Redis error events
  [this.client, this.bclient, this.eclient].forEach(function (client) {
    client.on('error', _this.emit.bind(_this, 'error'));
  });

  // keeps track of active timers. used by close() to
  // ensure that disconnect() is deferred until all
  // scheduled redis commands have been executed
  this.timers = new TimerManager();

  // emit ready when redis connections ready
  var initializers = [this.client, this.bclient, this.eclient].map(function (client) {
    return new Promise(function(resolve, reject) {
      client.once('ready', resolve);
      client.once('error', reject);
    });
  });

  this._initializing = Promise.all(initializers).then(function(){
    return Promise.join(
      _this.eclient.subscribe(_this.toKey('delayed')),
      _this.eclient.subscribe(_this.toKey('paused'))
    );
  }).then(function(){
    debuglog(name + ' queue ready');
    _this.emit('ready');
  }, function(err){
    console.error('Error initializing queue:', err);
  });

  Disturbed.call(this, _this.client, _this.eclient);

  //
  // Listen distributed queue events
  //
  listenDistEvent('stalled'); //
  listenDistEvent('active'); //
  listenDist ...
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.bull.super_"></a>[function <span class="apidocSignatureSpan">bull.</span>super_ (pubClient, subClient)](#apidoc.element.bull.super_)
- description and source-code
```javascript
super_ = function (pubClient, subClient) {
  var _this = this;
  EventEmitter.call(this);

  this.uuid = uuid();
  this.pubClient = pubClient;
  this.subClient = subClient;

  subClient.on('message', function (channel, msg) {

    var count = _this.listenerCount(channel);
    if (count) {
      var args;
      try {
        args = JSON.parse(msg);
      } catch (err) {
        console.error('Parsing event message', err);
      }

      if (args[0] !== _this.uuid) {
        args[0] = channel;
        _this.emit.apply(_this, args);
      }
    }
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.bull.toString"></a>[function <span class="apidocSignatureSpan">bull.</span>toString ()](#apidoc.element.bull.toString)
- description and source-code
```javascript
toString = function () {
    return toString;
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.bull.super_"></a>[module bull.super_](#apidoc.module.bull.super_)

#### <a name="apidoc.element.bull.super_.super_"></a>[function <span class="apidocSignatureSpan">bull.</span>super_ ()](#apidoc.element.bull.super_.super_)
- description and source-code
```javascript
function EventEmitter() {
  EventEmitter.init.call(this);
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.bull.super_.prototype"></a>[module bull.super_.prototype](#apidoc.module.bull.super_.prototype)

#### <a name="apidoc.element.bull.super_.prototype.distEmit"></a>[function <span class="apidocSignatureSpan">bull.super_.prototype.</span>distEmit (evt)](#apidoc.element.bull.super_.prototype.distEmit)
- description and source-code
```javascript
distEmit = function (evt) {
  var _this = this;
  var args = Array.prototype.slice.call(arguments);
  this.emit.apply(this, args);

  args[0] = this.uuid;

  // Emit to other nodes
  return new Promise(function (resolve, reject) {
    _this.pubClient.publish(evt, JSON.stringify(args), function (err) {
      if (err) {
        reject(err);
      } else {
        resolve();
      }
    });
  });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.bull.super_.prototype.off"></a>[function <span class="apidocSignatureSpan">bull.super_.prototype.</span>off (evt, listener)](#apidoc.element.bull.super_.prototype.off)
- description and source-code
```javascript
off = function (evt, listener) {
  var _this = this;
  var args = Array.prototype.slice.call(arguments);
  EventEmitter.prototype.removeListener.apply(this, args);

  // TODO: we should take into consideration isGlobal.
  if (!_this.listenerCount(evt)) {
    _this.subClient.unsubscribe(evt);
  }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.bull.super_.prototype.on"></a>[function <span class="apidocSignatureSpan">bull.super_.prototype.</span>on (evt, listener, isGlobal)](#apidoc.element.bull.super_.prototype.on)
- description and source-code
```javascript
on = function (evt, listener, isGlobal) {
  var _this = this;
  var args = Array.prototype.slice.call(arguments);
  EventEmitter.prototype.on.apply(this, args);

  if (isGlobal) {
    return new Promise(function (resolve, reject) {
      _this.subClient.subscribe(args[0], function (err) {
        if (err) {
          reject(err);
        } else {
          resolve();
        }
      });
    })
  }
  return Promise.resolve(void 0);
}
```
- example usage
```shell
...
queue.resume().then(function(){
  // queue is resumed now
})
'''

A queue emits also some useful events:
'''javascript
.on('ready', function() {
  // Queue ready for job
  // All Redis connections are done
})
.on('error', function(error) {
  // Error
})
.on('active', function(job, jobPromise){
...
```

#### <a name="apidoc.element.bull.super_.prototype.removeListener"></a>[function <span class="apidocSignatureSpan">bull.super_.prototype.</span>removeListener (evt, listener)](#apidoc.element.bull.super_.prototype.removeListener)
- description and source-code
```javascript
removeListener = function (evt, listener) {
  var _this = this;
  var args = Array.prototype.slice.call(arguments);
  EventEmitter.prototype.removeListener.apply(this, args);

  // TODO: we should take into consideration isGlobal.
  if (!_this.listenerCount(evt)) {
    _this.subClient.unsubscribe(evt);
  }
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
