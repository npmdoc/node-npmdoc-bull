# npmdoc-bull

#### api documentation for  [bull (v2.2.6)](https://github.com/OptimalBits/bull#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-bull.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-bull) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-bull.svg)](https://travis-ci.org/npmdoc/node-npmdoc-bull)

#### Job manager

[![NPM](https://nodei.co/npm/bull.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/bull)

- [https://npmdoc.github.io/node-npmdoc-bull/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-bull/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-bull/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-bull/build/apidoc.html)

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
    "version": "2.2.6",
    "bin": {}
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
