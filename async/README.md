```
function request(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      reject(`Network error when trying to reach ${url}`);
    }, 500);
  });
}

function requestWithRetry(url, retryCount, currentTries = 1) {
  return new Promise((resolve, reject) => {
    if (currentTries <= retryCount) {
      const timeout = (Math.pow(2, currentTries) - 1) * 100;
      request(url)
        .then(resolve)
        .catch((error) => {
          setTimeout(() => {
            console.log('Error: ', error);
            console.log(`Waiting ${timeout} ms`);
            requestWithRetry(url, retryCount, currentTries + 1);
          }, timeout);
        });
    } else {
      console.log('No retries left, giving up.');
      reject('No retries left, giving up.');
    }
  });
}

requestWithRetry('http://localhost:3000')
  .then((res) => {
    console.log(res)
  })
  .catch(err => {
    console.error(err)
  });
  //https://blog.risingstack.com/mastering-async-await-in-nodejs/
```

vs 

```
function wait (timeout) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve()
    }, timeout);
  });
}

async function requestWithRetry (url) {
  const MAX_RETRIES = 10;
  for (let i = 0; i <= MAX_RETRIES; i++) {
    try {
      return await request(url);
    } catch (err) {
      const timeout = Math.pow(2, i);
      console.log('Waiting', timeout, 'ms');
      await wait(timeout);
      console.log('Retrying', err.message, i);
    }
  }
}
```

Read more at https://blog.risingstack.com/mastering-async-await-in-nodejs/
