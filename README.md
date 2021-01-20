# 💡 Programming Protips

_Got something to add? [Send a PR](https://github.com/magicmark/engineering-protips/pulls)!_

### Limit the amount of logic on a single line of code

TODO: Add rationale

### Limit the amount of logic in a try/catch block

Keep the contents of try/catch blocks to a minimum.

#### Example**:

**Instead of**:

```js
try {
  const config = yaml.safeLoad(fs.readFileSync(path.join(process.cwd(), configFile), 'utf8'));
  const { projectName, scanPaths } = config;
  const resolvedScanPaths = [];
  scanPaths.forEach(scanPath => {
    resolvedScanPaths.push(path.join(__dirname, scanPath));
  });
  ...
  ...
  27 lines later
  ...
} catch (e) {
  console.error('Could not read config file, assuming defaults.');
}
```

Prefer:

```js
let config;

try {
  config = fs.readFileSync(path.join(process.cwd(), configFile), 'utf8'));
} catch (e) {
  console.error('Could not read config file, assuming defaults.');
}
```

**Why?**

There's _lots_ of things could throw inside our original try block. We may accidentally silence and ignore unrelated errors.

(e.g. Maybe the config file _was_ found, but `scanPaths` wasn't specified - so `scanPaths.forEach` throws, but we silenced it! Uh oh!)

### Limit what you catch


Keeping the contents of the try block minimal means we avoid a giant "catch all" that silences errors we didn't mean to catch.


https://gist.github.com/jehugaleahsa/f3c43d41e68a6b4bc73d2d6cbaee876a#within-a-limited-scope

(lots of things could throw!)

I'd just wrap L64.


### DRY

TODO: Add rationale

### DRY

TODO: Add rationale

### Prefer writing pure functions where applicable

something something avoid side effects / global state

TODO: Add rationale

### Don't blindly copy/paste code

TODO: Add rationale

### Make custom error messages as useful as possible

TODO: Add rationale

**Further Reading / Prior Art**

- https://twitter.com/swyx/status/1329171738215686145

### Return early

TODO: Add rationale

### Don't try and outsmart the typechecker

TODO: Add rationale

### Read your stack trace. Scroll up.

### Don't pass functions too many arguments

- **Further Reading / Prior Art**

- http://wiki.c2.com/?TooManyParameters

### Don't pass functions the whole object
