Cheatsheet for Piscina:

1. **Basic Usage**
```javascript
const Piscina = require('piscina');

const piscina = new Piscina({
  filename: 'worker.js'
});

(async function() {
  const result = await piscina.runTask({ a: 4, b: 6 });
  console.log(result);  // Prints: 10
})();
```
In the worker.js file:
```javascript
module.exports = ({ a, b }) => a + b;
```

2. **Abort Example**
```javascript
// In index.js
const { Piscina, AbortError } = require('piscina');
const { resolve } = require('path');

const piscina = new Piscina({
  filename: resolve(__dirname, 'worker.js')
});

(async function() {
  try {
    const result = await piscina.runTask({}, { abortSignal });
    console.log(result);
  } catch (err) {
    if (err instanceof AbortError)
      console.log('Task was aborted');
  }
})();

// In worker.js
module.exports = () => {
  while (true) {
    // Infinite loop
  }
};
```

3. **Async Load Example**
```javascript
// In index.js
const { Piscina } = require('piscina');
const { resolve } = require('path');

const piscina = new Piscina({
  filename: resolve(__dirname, 'worker.js')
});

(async function() {
  const result = await piscina.runTask({ a: 4, b: 6 });
  console.log(result);  // Prints: 10
})();

// In worker.js
module.exports = async function ({ a, b }) {
  return a + b;
};
```

4. **ES Module Example**
```javascript
// In index.mjs
import { Piscina } from 'piscina';
import { resolve } from 'path';

const piscina = new Piscina({
  filename: resolve(__dirname, 'worker.mjs')
});

(async function() {
  const result = await piscina.runTask({ a: 4, b: 6 });
  console.log(result);  // Prints: 10
})();

// In worker.mjs
export default ({ a, b }) => a + b;
```

5. **Message Port Example**
```javascript
// In index.js
const { Piscina } = require('piscina');
const { resolve } = require('path');
const { MessageChannel } = require('worker_threads');

const piscina = new Piscina({
  filename: resolve(__dirname, 'worker.js')
});

(async function() {
  const { port1, port2 } = new MessageChannel();
  port1.on('message', (message) => {
    console.log(message);  // Prints: { result: 10 }
  });
  await piscina.runTask(port2);
})();

// In worker.js
module.exports = (port) => {
  port.postMessage({ result: 4 + 6 });
};
```

6. **Messages Example**
```javascript
// In index.js
const { Piscina } = require('piscina');
const { resolve } = require('path');

const piscina = new Piscina({
  filename: resolve(__dirname, 'worker.js')
});

(async function() {
  const result = await piscina.runTask({ a: 4, b: 6 });
  console.log(result);  // Prints: 10
})();

// In worker.js
module.exports = ({ a, b }) => a + b;
```

7. **Move Example**
```javascript
// In index.js
const { Piscina, move } = require('piscina');
const { resolve } = require('path');

const piscina = new Piscina({
  filename: resolve(__dirname, 'worker.js')
});

(async function() {
  const buffer = new ArrayBuffer(10);
  const result = await piscina.runTask(move(buffer));
  console.log(result.byteLength);  // Prints: 10
})();

// In worker.js
module.exports = (buffer) => buffer;
```

8. **Multiple Workers Example**
```javascript
// In index.js
const { Piscina } = require('piscina');
const { resolve } = require('path');

const piscina = new Piscina({
  filename: resolve(__dirname, 'add_worker.js')
});

(async function() {
  const result = await piscina.runTask({ a: 4, b: 6 });
  console.log(result);  // Prints: 10
})();

// In add_worker.js
module.exports = ({ a, b }) => a + b;
```

9. **N-API Example**
```javascript
// In index.js
const { Piscina } = require('piscina');
const { resolve } = require('path');

const piscina = new Piscina({
  filename: resolve(__dirname, 'worker.js')
});

(async function() {
  const result = await piscina.runTask({ a: 4, b: 6 });
  console.log(result);  // Prints: 10
})();

// In worker.js
const { add } = require('./build/Release/addon');
module.exports = ({ a, b }) => add(a, b);
```

10. **Named Example**
```javascript
// In index.js
const { Piscina } = require('piscina');
const { resolve } = require('path');

const piscina = new Piscina({
  filename: resolve(__dirname, 'worker.js')
});

(async function() {
  const result = await piscina.runTask({ name: 'Alice' });
  console.log(result);  // Prints: 'Hello, Alice!'
})();

// In worker.js
module.exports = ({ name }) => `Hello, ${name}!`;
```

