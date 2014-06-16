
Notes on the first part of the workshop.

git clone https://github.com/briandipalma/spa-2014-communicator

Explain the structure/contents of the project.

Open up index.html, you can see the polyfills that provide the ES6 functionality.

Traceur for compiling ES6 to ES5.

	Give it ES6 it generates ES5, it's written in ES6 and compiles itself. Straight forward.

The ES6 module loader https://github.com/ModuleLoader/es6-module-loader is a polyfill that implements the ES6 Loader factory/class.

	ES6 Loader is Promises based.
	It supports circular references.
	Live bindings between modules.
	Allows dynamic loading of content.
	A module ID is *not* a URL or a file path.

We have some files on the disk...
How to load these ES6 module files into the browser?
	
There will be a Loader instance for each ES runtime environment. Not part of ES spec.
So there will be one for Node one for the browser and one for any other ES environment.

So we will use the browser Loader instance, referred to as System.

https://github.com/systemjs/systemjs
	
	It polyfills the browser Loader instance

	System instanceof LoaderPolyfill
	true
	
We can do this because the module syntax can be thought of as sugar over the imperitive API that a Loader instance provides.

There is a mooted declarative HTML tag for modules <module> but that's still up in the air, so we won't use that.

To retrieve a module

	System.import('moduleID')

Module operations return promises to support Async/Dynamic loading.

	System.import('moduleID')
			.then(function(requestedModule) {
				//requestedModule is an object with all module exports.
			})
			.catch(function(importError) {
				console.error(importError);
			});
			
To test your environment create a module that exports a function which prints "Hello World" to the console.

Using Promises create and export a function that asynchronously loads the JSON from the data folder.
Use Arrow functions to simplify functions.
Handle the login failure case where the value returned for "loggedIn" is false.

Using Generators simplify the function by yielding your promises.
Use co to speed things up, this requires configuring the System loader with it's location.

			System.config({
				map: {
					"co": "node_modules/co/index"
				}
			});

co is a CommonJS node package, it works in both the browser and node. SystemJS can load CommonJS/AMD/ES6/Global code.

You can create a class with a generator function if you want to export a class.
