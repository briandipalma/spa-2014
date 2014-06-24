Topic - Ecmascript 6
--------------------

http://slides.com/kybernetikos/apocalypse-es6#/

Workshop - Communicator
-----------------------

We'll be building some of the packages that make up a simple chat
application. The first package we look at is the communicator, it's job will be
to provide the messages, list of contacts and contact status.  In a real
system this would communicate over websockets, but we'll just use
a simplified stub that loads the data from files.

Helpful references

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

To start

	git clone https://github.com/briandipalma/spa-2014-communicator

1. Open up `index.html`

	The `script` tag contents are as follows.
	
	Traceur compiles ES6+ to ES5. It's written in ES6 and compiles itself.
	https://github.com/google/traceur-compiler/
	
	ES6 module loader is a polyfill that implements the ES6 Loader factory/class.
	https://github.com/ModuleLoader/es6-module-loader
	
	SystemJS provides a polyfill for `System` the browser version of the ES6 module loader.
	It loads ES6 modules, AMD, CommonJS and global scripts.
	https://github.com/systemjs/systemjs/
	
2. Serve the package

	All these packages can be served by a static server rooted at the package directory.
	One is included in the package and can be launched with the command,

	```bash
	$ npm run serve
	```

	, executed in the root package directory.
	
	The included server will serve the package at http://127.0.0.1:8080

	You can use your own server if you wish - there is no need for special server logic for ES6 modules.

3. Make a module that alerts "Hello World!"

	Create `src/main.js` in your project. Inside it place,

	```javascript
	export function greet() {
		alert('Hello world');
	}
	```

	Use the System loader to load the `main` module.
	Inside `index.html` in the empty `script` tag at the end of the `head` element add


	```javascript
	System.import('src/main')
		.then(function(module) {
		    module.greet();
		}).catch(function(error) {
		    console.error(error);
		});
	```

	You should see an alert when you load the page.

4. Load the user login data

	We simulate asking the server if we have successfully logged in	by requesting `/data/login-user-pass.json`.

	In the `main` module add a function that when given a URL returns a promise that resolves with the URL response.
	Use this `get` function if you've forgotten/don't know how to do XHRs.

	```javascript
	function get(url) {
		var xhr = new XMLHttpRequest();
		xhr.open('GET', url);
		xhr.send();

		var promise = new Promise((resolve, reject) => {
			xhr.onload = () => {
				if (xhr.status === 200) {
					resolve(xhr.response);
				} else if (xhr.status === 404) {
					reject(new Error(`Requested URL (${url}) unavailable`));
				}
			}
		});

		return promise;
	}
	```

	Create a new function in the `main` module called `loadData`.
	Make `loadData` call `get` requesting the `/data/login-user-pass.json` resource and log the response.
	The System loader code in `index.html` shows how to handle a `Promise` resolution.

	To test `loadData` export it from the `main` module and call it in `index.html`.

5. Error handling

	The `get` function `reject`s the `Promise` if the request `status`  is 404.
	It uses template strings to interpolate the failed url into the error message.
	Handle the `Promise` rejection case by logging the returned error to `console.error`.
	The System loader code in `index.html` shows how to handle a `Promise` rejection.

	Simulate an error by modifying your url argument and reload to check that you log the error.

6. Load the remaining chat data using Promise.all

	Firstly correct the login url from the previous step!

	Once we have successfully logged in we need to retrieve the message and contacts data.
	This can be done in parallel using the static method `Promise.all`.

	`Promise.all(iterable)`
	> Returns a promise that resolves when all of the promises in iterable have resolved. The result is passed an
	> array of values from all the promises.

	An array is an iterable in ES6. These requests should only be made if the login request was successful.
	Request `data/contacts-user.json`, `data/chat-messages.json` and `data/recent-messages-user.json`.

	For this step you will need to know what happens when you return a promise within a promise resolved handler.
	
	```javascript
	myPromise
		.then((response) => {
			return anotherPromise //We return another promise here.
		})
		... //Deal with errors.
		.then((anotherPromiseValue) => {
			//The passed in value is the resolved value of anotherPromise.
		})
		...
	```

	The containing promise won't resolve until the promise returned in the `then` resolves.
	And when it does resolve it will resolve with the value of the promise returned in the `then`.

	You can log the returned data by chaining onto the error handling from the previous step.

	`Promise.prototype.catch(onRejected)`
	> Appends a rejection handler callback to the promise, and returns a new promise resolving to the return value
	> of the callback if it is called, or to its original fulfillment value if the promise is instead fulfilled.

7. Make your async code look synchronous

	You can combine promises with generators to write asynchronous
	code as if it were synchronous.
	
	This will be built into es7 (with await async functions, like c#)
	but for now, we can use a library to provide the same functionality
	
	Add the 'co-promise' library package, which is a commonJS package that works
	both in the browser and nodejs and depends on a 'co' package.
	
	Inside the index.html file before you call `System.import` add.
	
	```javascript
	System.config({
	    map: {
	        "co": "node_modules/co/index",
	        "co-promise": "node_modules/co-promise/index"
	    }
	});
	```
	
	This library has been brought into the project using npm and is already
	available.
	
	Until it's built into the language in es7, you have to use co to run your
	code.
	
	import co from "co-promise";
	
	Now rewrite your method to look like synchronous code.  Don't forget to
	handle error cases.
