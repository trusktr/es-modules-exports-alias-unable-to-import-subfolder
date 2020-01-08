Show that "exports" aliases in package.json cause ES Modules not to be able to import subfolders.

> Note, if you're not using the latest version of Node.js, you may need to add the `--experimental-modules` flag.

The following works, the file imports relative to `test-package/dist/` which is specified by value `"./": "./dist/"` of the `"exports"` field in `test-package/package.json`:

```
node testFooImport.js
```

The following throws an error wen the file tries to import from `test-package/subfolder/`, the because the value of `"exports"` causes the `subfolder`  invisible to the consumer of test-package:

```
node testBarImport.js
```

You'll see an error like this:


```
$ node testBarImport.js 
/home/trusktr/test/testBarImport.js:1
import {Bar} from 'test-package/subfolder/Bar.js'
^^^^^^

SyntaxError: Cannot use import statement outside a module
    at Module._compile (internal/modules/cjs/loader.js:892:18)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:973:10)
    at Module.load (internal/modules/cjs/loader.js:812:32)
    at Function.Module._load (internal/modules/cjs/loader.js:724:14)
    at Function.Module.runMain (internal/modules/cjs/loader.js:1025:10)
    at internal/main/run_main_module.js:17:11
```

Related issue: https://github.com/nodejs/node/issues/14970

