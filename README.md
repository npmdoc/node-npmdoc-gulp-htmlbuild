# api documentation for  [gulp-htmlbuild (v0.4.2)](https://github.com/Janpot/gulp-htmlbuild)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-htmlbuild.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-htmlbuild) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-htmlbuild.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-htmlbuild)
#### Extract content from html documents and replace by build result

[![NPM](https://nodei.co/npm/gulp-htmlbuild.png?downloads=true)](https://www.npmjs.com/package/gulp-htmlbuild)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-htmlbuild/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-htmlbuild_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-htmlbuild/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-htmlbuild/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-htmlbuild/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "url": "https://github.com/Janpot"
    },
    "bugs": {
        "url": "https://github.com/Janpot/gulp-htmlbuild/issues"
    },
    "dependencies": {
        "event-stream": "*",
        "gulp-util": "~3.0.4",
        "pause-stream": "0.0.11",
        "readable-stream": "~1.1.10",
        "stream-stream": "~1.2.6"
    },
    "description": "Extract content from html documents and replace by build result",
    "devDependencies": {
        "chai": "~1.9.0",
        "gulp": "~3.8.10",
        "gulp-concat": "~2.1.7",
        "gulp-debug": "~0.2.0",
        "gulp-load-plugins": "^0.5.0",
        "gulp-mocha": "~0.4.1",
        "mocha": "~1.17.0"
    },
    "directories": {},
    "dist": {
        "shasum": "b41148f4943f38a785630fb89e7b685d46af4cfb",
        "tarball": "https://registry.npmjs.org/gulp-htmlbuild/-/gulp-htmlbuild-0.4.2.tgz"
    },
    "engines": {
        "node": ">=0.8.0",
        "npm": ">=1.2.10"
    },
    "gitHead": "20865300f18d6cd36d2e11d0bc3d3c7e7d125837",
    "homepage": "https://github.com/Janpot/gulp-htmlbuild",
    "keywords": [
        "gulpplugin"
    ],
    "licenses": [
        {
            "type": "MIT"
        }
    ],
    "main": "./lib/index.js",
    "maintainers": [
        {
            "name": "janpotoms",
            "email": "potoms.jan@gmail.com"
        }
    ],
    "name": "gulp-htmlbuild",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/Janpot/gulp-htmlbuild.git"
    },
    "scripts": {
        "test": "gulp mocha"
    },
    "version": "0.4.2"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-htmlbuild](#apidoc.module.gulp-htmlbuild)
1.  object <span class="apidocSignatureSpan">gulp-htmlbuild.</span>builder
1.  object <span class="apidocSignatureSpan">gulp-htmlbuild.</span>parser
1.  object <span class="apidocSignatureSpan">gulp-htmlbuild.</span>preprocess

#### [module gulp-htmlbuild.builder](#apidoc.module.gulp-htmlbuild.builder)
1.  [function <span class="apidocSignatureSpan">gulp-htmlbuild.builder.</span>build (config)](#apidoc.element.gulp-htmlbuild.builder.build)

#### [module gulp-htmlbuild.parser](#apidoc.module.gulp-htmlbuild.parser)
1.  [function <span class="apidocSignatureSpan">gulp-htmlbuild.parser.</span>_tokenize (line)](#apidoc.element.gulp-htmlbuild.parser._tokenize)
1.  [function <span class="apidocSignatureSpan">gulp-htmlbuild.parser.</span>parse (file)](#apidoc.element.gulp-htmlbuild.parser.parse)
1.  object <span class="apidocSignatureSpan">gulp-htmlbuild.parser.</span>_tokens

#### [module gulp-htmlbuild.preprocess](#apidoc.module.gulp-htmlbuild.preprocess)
1.  [function <span class="apidocSignatureSpan">gulp-htmlbuild.preprocess.</span>css (buildFn)](#apidoc.element.gulp-htmlbuild.preprocess.css)
1.  [function <span class="apidocSignatureSpan">gulp-htmlbuild.preprocess.</span>js (buildFn)](#apidoc.element.gulp-htmlbuild.preprocess.js)



# <a name="apidoc.module.gulp-htmlbuild"></a>[module gulp-htmlbuild](#apidoc.module.gulp-htmlbuild)



# <a name="apidoc.module.gulp-htmlbuild.builder"></a>[module gulp-htmlbuild.builder](#apidoc.module.gulp-htmlbuild.builder)

#### <a name="apidoc.element.gulp-htmlbuild.builder.build"></a>[function <span class="apidocSignatureSpan">gulp-htmlbuild.builder.</span>build (config)](#apidoc.element.gulp-htmlbuild.builder.build)
- description and source-code
```javascript
function build(config) {
  config = config || {};

  var buildStream = es.through(function write(block) {
    var buildFn = block.target && config[block.target],
        blockStream = new BlockStream(block);

    if (buildFn) {
      buildFn(blockStream);
    } else {
      blockStream.pipe(blockStream);
    }

    this.queue(blockStream.result);
  });

  return es.pipeline(
    buildStream,
    flatten(),
    es.join('\n')
  );
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.gulp-htmlbuild.parser"></a>[module gulp-htmlbuild.parser](#apidoc.module.gulp-htmlbuild.parser)

#### <a name="apidoc.element.gulp-htmlbuild.parser._tokenize"></a>[function <span class="apidocSignatureSpan">gulp-htmlbuild.parser.</span>_tokenize (line)](#apidoc.element.gulp-htmlbuild.parser._tokenize)
- description and source-code
```javascript
function tokenize(line) {
  var token = {
    line  : line,
    type  : null,
    target: null,
    args  : null
  };

  var blockStartMatch = /^(\s*)<!--\s*htmlbuild:([a-zA-Z]*)(.+?)\s*-->/.exec(line);

  if (blockStartMatch !== null) {
    token.type   = tokenTypes.BLOCK_START;
    token.indent = blockStartMatch[1];
    token.target = blockStartMatch[2];
    token.args = blockStartMatch[3] ? blockStartMatch[3].trim().split(' ') : null;
  } else if (/<!--\s*endbuild\s*-->/.test(line)) {
    token.type = tokenTypes.BLOCK_END;
  } else {
    token.type = tokenTypes.LINE;
  }

  return token;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.gulp-htmlbuild.parser.parse"></a>[function <span class="apidocSignatureSpan">gulp-htmlbuild.parser.</span>parse (file)](#apidoc.element.gulp-htmlbuild.parser.parse)
- description and source-code
```javascript
function parse(file) {

  var result = buffer();

  setImmediate(function () {

    function makeError(lineNumber, msg) {
      return new gutil.PluginError(PLUGIN_NAME, msg, {
        showStack: false,
        lineNumber: lineNumber,
        fileName: file.path
      });
    }

    var lines   = String(file.contents).split('\n'),
        tokens  = lines.map(tokenize),
        inBlock = false,
        error   = null;

    var currentBlock = new Block({
      lineNumber: 1
    });

    for (var i = 0; i < tokens.length; i++) {
      var lineNumber = i + 1,
          token      = tokens[i];

      if (token.type === tokenTypes.BLOCK_START) {
        if (inBlock) {
          error = makeError(lineNumber, 'Blocks can\'t be nested');
          return result.emit('error', error);
        } else {
          inBlock = true;
        }
      }

      if (token.type === tokenTypes.BLOCK_END) {
        if (inBlock) {
          inBlock = false;
        } else {
          error = makeError(lineNumber, 'Block has no start');
          return result.emit('error', error);
        }
      }

      var switchBlocks = (
        token.type === tokenTypes.BLOCK_START ||
        token.type === tokenTypes.BLOCK_END
      );

      if (switchBlocks) {
        result.write(currentBlock);
        currentBlock = new Block({
          line      : token.line,
          target    : token.target,
          indent    : token.indent,
          args      : token.args,
          lineNumber: lineNumber
        });
      } else {
        currentBlock.lines.push(token.line);
      }
    }

    if (inBlock) {
      error = makeError(currentBlock.lineNumber, 'Unclosed block');
      return result.emit('error', error);
    }

    result.end(currentBlock);
  });

  return result;
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.gulp-htmlbuild.preprocess"></a>[module gulp-htmlbuild.preprocess](#apidoc.module.gulp-htmlbuild.preprocess)

#### <a name="apidoc.element.gulp-htmlbuild.preprocess.css"></a>[function <span class="apidocSignatureSpan">gulp-htmlbuild.preprocess.</span>css (buildFn)](#apidoc.element.gulp-htmlbuild.preprocess.css)
- description and source-code
```javascript
css = function (buildFn) {

  function extractCss(line) {
    var matched = /(\s*)<link.+href=['"]([^"']+)["']/.exec(line);
    if (matched) {
      return preprocessUtil.pathFromUrl(matched[2]);
    }
  }

  return function (block) {

    function templateCss(path) {
      var template = block.indent + '<link rel="stylesheet" href="%s"/>';
      return util.format(template, path);
    }


    var extractCssStream = es.mapSync(extractCss),
        templateCssStream = es.mapSync(templateCss);


    block.pipe(extractCssStream);
    templateCssStream.pipe(block);

    buildFn(es.duplex(templateCssStream, extractCssStream));
  };
}
```
- example usage
```shell
...

a preprocessor you can use to wrap your buildfunction in. It extracts script paths from the block. In this case you also write paths
 to the block. They will get templated into link elements.

#### buildFn

a function that has the same form as a normal buildfunction, only the argument here is a stream that contains script paths. You
are expected to write script paths to this stream as well.

### htmlbuild.preprocess.css(buildFn)

a preprocessor you can use to wrap your buildfunction in. It extracts stylesheet paths from the block. In this case you also write
 paths to the block. They will get templated into link elements.

#### buildFn

a function that has the same form as a normal buildfunction, only the argument here is a stream that contains stylesheet paths.
You are expected to write paths to this stream as well.
...
```

#### <a name="apidoc.element.gulp-htmlbuild.preprocess.js"></a>[function <span class="apidocSignatureSpan">gulp-htmlbuild.preprocess.</span>js (buildFn)](#apidoc.element.gulp-htmlbuild.preprocess.js)
- description and source-code
```javascript
js = function (buildFn) {

  function extractScript(line) {
    var matched = /(\s*)<script.+src=['"]([^"']+)["']/.exec(line);
    if (matched) {
      return preprocessUtil.pathFromUrl(matched[2]);
    }
  }

  return function (block) {

    function templateScript(path) {
      var template = block.indent + '<script src="%s"></script>';
      return util.format(template, path);
    }


    var extractSrc = es.mapSync(extractScript),
        templateSrc = es.mapSync(templateScript);


    block.pipe(extractSrc);
    templateSrc.pipe(block);

    buildFn(es.duplex(templateSrc, extractSrc));
  };
}
```
- example usage
```shell
...


'''js
gulp.task('build', function () {
gulp.src(['./index.html'])
  .pipe(htmlbuild({
    // build js with preprocessor
    js: htmlbuild.preprocess.js(function (block) {

      // read paths from the [block] stream and build them
      // ...

      // then write the build result path to it
      block.write('buildresult.js');
      block.end();
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
