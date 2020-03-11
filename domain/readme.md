# Domain

```
const Domain = require('domain');
const d = Domain.create();

d.once('error', err => {
  console.error('Error was trapped by the domain:', err);
});

d.run(exec);

/*  Using the global uncaughtException and unhandledRejection handlers  - does not work on these errors inside a setTimeout */
```

For more on exception handling, read https://stackoverflow.com/questions/7310521/node-js-best-practice-exception-handling or https://nodejs.dev/error-handling-in-nodejs 
