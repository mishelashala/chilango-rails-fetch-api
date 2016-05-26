# Fetch API
The XMLHttpRequest on steroids

## XMLHttpRequest Object
```javascript
const Request = new XMLHttpRequest();

Request.setRequestHeader('Accept', 'application/json');

Request.onreadystatechange = function () {
	if (xhttp.readyState == 4 && xhttp.status == 200) {
	    document.getElementById("demo").innerHTML = xhttp.responseText;
  }
};

Request.open('GET', 'some_resource.json', true);
Request.send();
```

## Promises
[Here's](https://www.promisejs.org/) an explanation of why you should use promises.

The core idea behind promises is that a promise represents the result of an asynchronous operation. A promise is in one of three different states:

- pending: initial state
- fulfilled: success
- rejected: fuck da sh*t

Once a promise is fulfilled or rejected, it is immutable (i.e. it can never change again).

```javascript
const getData = new Promise(resolvePromise);

const resolvePromise = function (fulfill, reject) {
	ajaxRequest('some-resosource.json', function (err, data) {
		if (err) {
			return reject(err);
		}
		
		fulfill(data);
	});
};

// a promise: fulfilled | rejected
const promisedData = getData('some-resource.json');
```
### Refactor da coud.
```javascript
const getData = function (uri) {
	return new Promise(function (fulfill, reject) {
		// sync request
		const data = ajaxRequest(uri);

		if (!data) {
			return reject(new Error('No data returned'));
		}

		fulfill(data);
	});
};

// a promise: fulfilled | rejected
const promisedData = getData('some_resource.json');

const remaining = promisedData.then(function (data) {
	console.log(data);
});

console.log(remaining); // Promise < pending >
```
### Something went wrong

```javascript
const promisedData = getData('some_resource.json');

const remaining = promisedData.then(function (data) {
	console.log(data);
});

console.log(remaining); // Promise < rejected >

remaining.catch(function (err) {
	console.error(err.message); // 'No data returned'
});
```

### Moar promises!

```javascript
const promisedData = getData('some_resource.json');

const remaining = promisedData.then(function (data) {
	data.some_extra_field = 'some-extra-data';
	return data
});

const moar_remaining = remaining.then(function (data) {
	console.log(data); // { some_extra_field: 'some-extra-data', ... };
});

moar_remaining.catch(function (err) {
	// If rejected, do something
});
```
### Resolution algorithm

![promise-algorithm](https://idiotwu.me/content/images/2015/03/promise-then-catch-flow.png)

### Refactor da coud.
```javascript
const promisedData = getData('some_resource.json');

promisedData
	.then(function (data) {
		data.some_extra_field = 'some-extra-data';
		return data
	})
	.then(function (data) {
		console.log(data); // { some_extra_field: 'some-extra-data', ... };
	})
	.catch(function (err) {
		console.error(err.message);
	});
```
### Moar reafactor!!!
```javascript
getData('some_resource.json')
	.then(transformData)
	.then(logData)
	.catch(handleError);
```
## Finally: fetch
[Fetch API] provides an interface for fetching resources... but the new API provides a more powerful and flexible feature set.

### Interfaces
- Response
- Request
- Headers

```javascript
const headers = new Headers();
headers.append('Accept', 'application/json');

const request = new Request('some-uri', headers);

const response = new Response({ data: { /* ... */} });
```

### How to use it
```javascript
// GlobalFetch.fetch | window.fetch | fetch
fetch('some-resource.json')
	.then((res) => res.json())
	.then((data) => console.log(data))
	.catch((err) => console.error(err.message));
```

### Not only JSON

- ArrayBuffer
- Blob
  - URI
  - File
- Body.json
- Text

## Support
**Chrome** supports everything...
**IE** don't support anything...
**The rest**... who cares?

### Polifyll
[browser](https://www.npmjs.com/package/whatwg-fetch)
[node](https://github.com/bitinn/node-fetch)

# Thanks!
