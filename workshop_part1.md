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
		}.catch(function(error) {
		    console.error(error);
		});
	```

	You should see an alert when you load the page.

3. Load the user login data

    We simulate asking the server if we have successfully logged in
    by providing a login-user-pass.json file you can retrieve.

    You need to handle both success and failure cases (e.g. the 
    file doesn't exist and your request returns 404).
    
    Create a function that returns a promise that resolves with the
    response of the url request. Use this get function if you've 
    forgotten/don't know how to do xhrs.
    
```javascript
function get(url) {
	var xhr = new XMLHttpRequest();
	var promise = new Promise((resolve, reject) => {
		xhr.onreadystatechange = () => {
			if (xhr.readyState === 4 && xhr.status === 200) {
				resolve(xhr.response);
			} else if (xhr.readyState === 4 && xhr.status === 404) {
				reject(new Error(`Requested URL (${url}) unavailable`));
			}
		}
	});

	xhr.open('GET', url);
	xhr.send();

	return promise;
}
```
    
    export a `loadData` function that loads /data/login-user-pass.json
    returns the promise.
    
    In index.html, call the loadData function and log the data
    and deal with any error.
    
    Simulate an error by renaming the json file and reload to check
    that you handle the error.
    
4.  Load all of the .json paths in /data using Promise.all
    
5.  Make your async code look synchronous
   
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
