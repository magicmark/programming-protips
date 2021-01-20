# 💡 Programming Protips

_Got something to add? [Send a PR](https://github.com/magicmark/engineering-protips/pulls)!_

### Limit the amount of logic on a single line of code

TODO: Add rationale

### Limit the amount of logic in a try/catch block

Keep the contents of try/catch blocks to a minimum.

**Bad Example**

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

**Prefer**

```js
let config;

try {
  config = fs.readFileSync(path.join(process.cwd(), configFile), 'utf8'));
} catch (e) {
  console.error('Could not read config file, assuming defaults.');
}
```

#### Why?

There's _lots_ of things could throw inside our original try block. We may accidentally silence and ignore unrelated errors.

(e.g. Maybe the config file _was_ found, but `scanPaths` wasn't specified - so `scanPaths.forEach` throws, but we silenced it! Uh oh!)

### Limit what you catch

Be specific when catching an error. Rethrow all other errors.

**Bad Example**

```js
try {
  result = divideNumbers(3, 0);
} catch (e) {
  console.error("You can't divide by zero!");
}
```

**Prefer**

```js
try {
  result = divideNumbers(3, 0);
} catch (e) {
  if (e instanceof DivideByZeroError) {
    console.error("You can't divide by zero!");
  } else {
    throw e;
  }
}
```

#### Why?

Lots of things could throw!

The message displayed to users or recovery logic inside the catch block may only apply to a certain type of error. But the catch block may be triggered with more errors types than you expect! (e.g. Someone accidentally renames the divideNumbers function, and now we're also catching a `ReferenceError`!)

Avoid "catch all" blocks that gobble up errors we didn't intend to catch.

More reading: https://gist.github.com/jehugaleahsa/f3c43d41e68a6b4bc73d2d6cbaee876a#within-a-limited-scope

### Use user-defined error codes
### DRY

TODO: Add rationale

### DRY

TODO: Add rationale

### Prefer writing pure functions where applicable

something something avoid side effects / global state

TODO: Add rationale

### Preserve prior stack traces when throwing errors

When throwing a new error from inside a try/catch block, preserve the stack from the caught error.

**Bad Example**

```js
try {
  config = readFileSync(configFilePath, 'utf8');
} catch (e) {
  throw new Error("Could not read config file - ensure this file exists!");
}
```

**Prefer**

```js
try {
  config = readFileSync(configFilePath, 'utf8');
} catch (e) {
  throw new MultiError([e, "Could not read config file - ensure this file exists!"]);
}
```

#### Why?

We don't want to gobble up the stack trace from the caught error as it contains potentially useful information.

In our example, the original stack trace will show the location on disk that we tried to read the config file from. Throwing this information away makes debugging harder.

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
