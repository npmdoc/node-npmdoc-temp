# api documentation for  [temp (v0.8.3)](https://github.com/bruce/node-temp)  [![npm package](https://img.shields.io/npm/v/npmdoc-temp.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-temp) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-temp.svg)](https://travis-ci.org/npmdoc/node-npmdoc-temp)
#### Temporary files and directories

[![NPM](https://nodei.co/npm/temp.png?downloads=true)](https://www.npmjs.com/package/temp)

[![apidoc](https://npmdoc.github.io/node-npmdoc-temp/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-temp_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-temp/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-temp/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-temp/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Bruce Williams",
        "email": "brwcodes@gmail.com"
    },
    "bugs": {
        "url": "https://github.com/bruce/node-temp/issues"
    },
    "dependencies": {
        "os-tmpdir": "^1.0.0",
        "rimraf": "~2.2.6"
    },
    "description": "Temporary files and directories",
    "devDependencies": {},
    "directories": {
        "lib": "lib"
    },
    "dist": {
        "shasum": "e0c6bc4d26b903124410e4fed81103014dfc1f59",
        "tarball": "https://registry.npmjs.org/temp/-/temp-0.8.3.tgz"
    },
    "engines": [
        "node >=0.8.0"
    ],
    "gitHead": "76966b174840a540c8d1566507f2afcad99a3afa",
    "homepage": "https://github.com/bruce/node-temp",
    "keywords": [
        "temporary",
        "tmp",
        "temp",
        "tempdir",
        "tempfile",
        "tmpdir",
        "tmpfile"
    ],
    "license": "MIT",
    "main": "./lib/temp",
    "maintainers": [
        {
            "name": "bruce",
            "email": "bruce@codefluency.com"
        }
    ],
    "name": "temp",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/bruce/node-temp.git"
    },
    "scripts": {
        "test": "node test/temp-test.js"
    },
    "tags": [
        "temporary",
        "temp",
        "tempfile",
        "tempdir",
        "tmpfile",
        "tmpdir",
        "security"
    ],
    "version": "0.8.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module temp](#apidoc.module.temp)
1.  [function <span class="apidocSignatureSpan">temp.</span>cleanup (callback)](#apidoc.element.temp.cleanup)
1.  [function <span class="apidocSignatureSpan">temp.</span>cleanupSync ()](#apidoc.element.temp.cleanupSync)
1.  [function <span class="apidocSignatureSpan">temp.</span>createWriteStream (affixes)](#apidoc.element.temp.createWriteStream)
1.  [function <span class="apidocSignatureSpan">temp.</span>mkdir (affixes, callback)](#apidoc.element.temp.mkdir)
1.  [function <span class="apidocSignatureSpan">temp.</span>mkdirSync (affixes)](#apidoc.element.temp.mkdirSync)
1.  [function <span class="apidocSignatureSpan">temp.</span>open (affixes, callback)](#apidoc.element.temp.open)
1.  [function <span class="apidocSignatureSpan">temp.</span>openSync (affixes)](#apidoc.element.temp.openSync)
1.  [function <span class="apidocSignatureSpan">temp.</span>path (rawAffixes, defaultPrefix)](#apidoc.element.temp.path)
1.  [function <span class="apidocSignatureSpan">temp.</span>track (value)](#apidoc.element.temp.track)
1.  string <span class="apidocSignatureSpan">temp.</span>dir



# <a name="apidoc.module.temp"></a>[module temp](#apidoc.module.temp)

#### <a name="apidoc.element.temp.cleanup"></a>[function <span class="apidocSignatureSpan">temp.</span>cleanup (callback)](#apidoc.element.temp.cleanup)
- description and source-code
```javascript
function cleanup(callback) {
  if (!tracking) {
    if (callback) {
      callback(new Error("not tracking"));
    }
    return;
  }
  cleanupFiles(function(fileErr, fileCount) {
    if (fileErr) {
      if (callback) {
        callback(fileErr, {files: fileCount})
      }
    } else {
      cleanupDirs(function(dirErr, dirCount) {
        if (callback) {
          callback(dirErr, {files: fileCount, dirs: dirCount});
        }
      });
    }
  });
}
```
- example usage
```shell
...
'''javascript
> temp.cleanupSync();
{ files: 1,
  dirs:  0 }
'''

'''javascript
> temp.cleanup(function(err, stats) {
    console.log(stats);
  });
{ files: 1,
  dirs:  0 }
'''

Note: If you're not tracking, an error ("not tracking") will be passed
...
```

#### <a name="apidoc.element.temp.cleanupSync"></a>[function <span class="apidocSignatureSpan">temp.</span>cleanupSync ()](#apidoc.element.temp.cleanupSync)
- description and source-code
```javascript
function cleanupSync() {
  if (!tracking) {
    return false;
  }
  var fileCount = cleanupFilesSync();
  var dirCount  = cleanupDirsSync();
  return {files: fileCount, dirs: dirCount};
}
```
- example usage
```shell
...

When tracking, you can run 'cleanup()' and 'cleanupSync()' anytime
('cleanupSync()' will be run for you on process exit). An object will
be returned (or passed to the callback) with cleanup counts and
the file/directory tracking lists will be reset.

'''javascript
> temp.cleanupSync();
{ files: 1,
  dirs:  0 }
'''

'''javascript
> temp.cleanup(function(err, stats) {
console.log(stats);
...
```

#### <a name="apidoc.element.temp.createWriteStream"></a>[function <span class="apidocSignatureSpan">temp.</span>createWriteStream (affixes)](#apidoc.element.temp.createWriteStream)
- description and source-code
```javascript
function createWriteStream(affixes) {
  var filePath = generateName(affixes, 's-');
  var stream = fs.createWriteStream(filePath, {flags: RDWR_EXCL, mode: 0600});
  deleteFileOnExit(filePath);
  return stream;
}
```
- example usage
```shell
...

'''javascript
var temp = require('temp');

// Automatically track and cleanup files at exit
temp.track();

var stream = temp.createWriteStream();
stream.write("Some data");
// Maybe do some other things
stream.end();
'''

### Affixes
...
```

#### <a name="apidoc.element.temp.mkdir"></a>[function <span class="apidocSignatureSpan">temp.</span>mkdir (affixes, callback)](#apidoc.element.temp.mkdir)
- description and source-code
```javascript
function mkdir(affixes, callback) {
  var dirPath = generateName(affixes, 'd-');
  fs.mkdir(dirPath, 0700, function(err) {
    if (!err) {
      deleteDirOnExit(dirPath);
    }
    if (callback) {
      callback(err, dirPath);
    }
  });
}
```
- example usage
```shell
...

// Automatically track and cleanup files at exit
temp.track();

// For use with ConTeXt, http://wiki.contextgarden.net
var myData = "\\starttext\nHello World\n\\stoptext";

temp.mkdir('pdfcreator', function(err, dirPath) {
var inputPath = path.join(dirPath, 'input.tex')
fs.writeFile(inputPath, myData, function(err) {
  if (err) throw err;
  process.chdir(dirPath);
  exec("texexec '" + inputPath + "'", function(err) {
    if (err) throw err;
    fs.readFile(path.join(dirPath, 'input.pdf'), function(err, data) {
...
```

#### <a name="apidoc.element.temp.mkdirSync"></a>[function <span class="apidocSignatureSpan">temp.</span>mkdirSync (affixes)](#apidoc.element.temp.mkdirSync)
- description and source-code
```javascript
function mkdirSync(affixes) {
  var dirPath = generateName(affixes, 'd-');
  fs.mkdirSync(dirPath, 0700);
  deleteDirOnExit(dirPath);
  return dirPath;
}
```
- example usage
```shell
...





var temp = require('temp').track();

var p = temp.mkdirSync("shouldBeDeletedOnExitNotJasmine");
console.log('created dir ' + p);
...
```

#### <a name="apidoc.element.temp.open"></a>[function <span class="apidocSignatureSpan">temp.</span>open (affixes, callback)](#apidoc.element.temp.open)
- description and source-code
```javascript
function open(affixes, callback) {
  var filePath = generateName(affixes, 'f-');
  fs.open(filePath, RDWR_EXCL, 0600, function(err, fd) {
    if (!err) {
      deleteFileOnExit(filePath);
    }
    if (callback) {
      callback(err, {path: filePath, fd: fd});
    }
  });
}
```
- example usage
```shell
...
// Automatically track and cleanup files at exit
temp.track();

// Fake data
var myData = "foo\nbar\nfoo\nbaz";

// Process the data (note: error handling omitted)
temp.open('myprefix', function(err, info) {
if (!err) {
  fs.write(info.fd, myData);
  fs.close(info.fd, function(err) {
    exec("grep foo '" + info.path + "' | wc -l", function(err, stdout) {
      util.puts(stdout.trim());
    });
  });
...
```

#### <a name="apidoc.element.temp.openSync"></a>[function <span class="apidocSignatureSpan">temp.</span>openSync (affixes)](#apidoc.element.temp.openSync)
- description and source-code
```javascript
function openSync(affixes) {
  var filePath = generateName(affixes, 'f-');
  var fd = fs.openSync(filePath, RDWR_EXCL, 0600);
  deleteFileOnExit(filePath);
  return {path: filePath, fd: fd};
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.temp.path"></a>[function <span class="apidocSignatureSpan">temp.</span>path (rawAffixes, defaultPrefix)](#apidoc.element.temp.path)
- description and source-code
```javascript
path = function (rawAffixes, defaultPrefix) {
  var affixes = parseAffixes(rawAffixes, defaultPrefix);
  var now = new Date();
  var name = [affixes.prefix,
              now.getYear(), now.getMonth(), now.getDate(),
              '-',
              process.pid,
              '-',
              (Math.random() * 0x100000000 + 1).toString(36),
              affixes.suffix].join('');
  return path.join(affixes.dir || exports.dir, name);
}
```
- example usage
```shell
...
### Just a path, please

If you just want a unique name in your temporary directory, use
'path':

'''javascript
var fs = require('fs');
var tempName = temp.path({suffix: '.pdf'});
// Do something with tempName
'''

Note: The file isn't created for you, and the mode is not changed  -- and it
will not be removed automatically at exit.  If you use 'path', it's
all up to you.
...
```

#### <a name="apidoc.element.temp.track"></a>[function <span class="apidocSignatureSpan">temp.</span>track (value)](#apidoc.element.temp.track)
- description and source-code
```javascript
track = function (value) {
  tracking = (value !== false);
  return module.exports; // chainable
}
```
- example usage
```shell
...
, fd: theFileDescriptor
}
'''

In this example we write to a temporary file and call out to 'grep' and
'wc -l' to determine the number of time 'foo' occurs in the text.  The
temporary file is chmod'd '0600' and cleaned up automatically when the
process at exit (because 'temp.track()' is called):

'''javascript
var temp = require('temp'),
fs   = require('fs'),
util  = require('util'),
exec = require('child_process').exec;
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
