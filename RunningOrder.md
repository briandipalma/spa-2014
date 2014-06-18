Prerequisites
-------------

git, an editor, nodejs

Introduction - People
---------------------

I'm Brian from Caplin
I'm Adam from Improbable

Introduction - Talk
-------------------

One of simultaneously the best and worst things about web
development is that things are changing fast.

It didn't always used to be this way.  The JS and web world
stagnated for years, and lots of ugly and unfortunate things
have been simply the only way to do things for a long time.

Now, not only is it changing fast, we're currently getting
lined up for a period of unusually rapid change.

Whether you see this as an asteroid collision causing big 
chunks of current web development practice to go the way
of the dinosaurs and be replaced by a more mammalian way of
doing things, or a Cambrian explosion of new opportunities 
is up to you.

Introduction - Contents
-----------------------

We're going to be looking at some of the biggest changes and
giving you hands on experience of them.  We don't have time to
cover everything in depth, so this will necessarily be a quick
tour.

But in the near future, the changes will be significant. You'll
be coding in a language that looks and feels pretty different
from the Javascript we're used too.  We'll be organising our
code differently, many of the tools and workflows we already
have will die out, and new tools will take their place, and the
platform standards we're building on will have changed
significantly.

There's a quote from William Gibson that's pretty relevant to
a lot of these changes:

"The future is already here — it's just not very evenly
distributed."

Because the future is not very evenly distributed, we'll be
occasionally using shims and tools to cover over the holes.
 
As well as giving you a whistle stop tour of the features we
think are most important, we'll also be giving you some hands
on time.

Topic - Ecmascript 6
--------------------

http://slides.com/kybernetikos/apocolypse-es6

Workshop - Communicator
-----------------------

We'll be building some of the packages that make up a simple chat
application.

You will need references
    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
    
The first package we look at is the communicator, it's job will be
to provide the messages, list of contacts and status.  In a real
system this would communicate over websockets, but we'll just use
a simplified stub that loads the data from files.

    git clone https://github.com/briandipalma/spa-2014-communicator

1. Look at index.html

    Traceur for compiling ES6 to ES5.
        Give it ES6 it generates ES5, it's written in ES6 and compiles itself. Straight forward.
    
    The ES6 module loader polyfill https://github.com/ModuleLoader/es6-module-loader is a polyfill that implements the ES6 Loader factory/class.
        ES6 Loader is Promises based.
        It supports circular references.
        Live bindings between modules.
        Allows dynamic loading of content.
        A module ID is *not* a URL or a file path.

    A Specific module loader for browsers
        Polyfills the browser Loader instance
        
2. Make a module that alerts "Hello Module World!"

   Create a src/main.js in your project
   
    export function greet() {
        alert('Hello module world')
    }   

   In index.html use the System loader to load the hello.js module.
   
    System.import('src/main')
        .then(function(comsModule) {
            comsModule.greet()
        })
            
   You should see an alert when you load the page.
   
3. Load the user login data

    We simulate asking the server if we have successfully logged in
    by providing a login-user-pass.json file you can retrieve.

    You need to handle both success and failure cases (e.g. the 
    file doesn't exist and your request returns 404).
    
    Create a function that returns a promise that resolves with the
    response of the url request. Use this get function if you've 
    forgotten/don't know how to do xhrs.
    
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
    
    export a loadData function that loads /data/login-user-pass.json
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
    
        System.config({
            map: {
                "co": "node_modules/co/index",
                "co-promise": "node_modules/co-promise/index"
            }
        });
    
    This library has been brought into the project using npm and is already
    available.
    
    Until it's built into the language in es7, you have to use co to run your
    code.
    
       import co from "co-promise";
    
    Now rewrite your method to look like synchronous code.  Don't forget to
    handle error cases.


Topic - WebComponents
---------------------

http://slides.com/kybernetikos/webcomponents

Workshop - Contacts List
------------------------

The next piece that we'll be building is a custom element that shows a list of
contacts to go in our chat application.

    git clone https://github.com/briandipalma/spa-2014-contacts-list

1. Look at index.html

    We haven't used html imports/templates/shadow dom polyfills.
    
    The new script we've added is the custom-elements polyfill, which is part of
    the polymer project.
    
    System.config now has css and text loading plugin which allows the es6
    module loader to load css and text files.  The CSS plugin automatically
    attaches the css to the head as soon as you import the css module.  The
    text plugin returns the content of the file as a javascript string.
    
    The other libraries are for integration later.
    
    This custom element is already wired up to be notified of model changes.
    
    Look at ContactsListElement.js
    
    The state member variable holds the information that needs to be added to the DOM, it's an ES6 Map.
    It's keys are usernames and values are a objects with keys "status" (value of "online" or "offline") and
    "imageSource" which is a URL for a user avatar.
    
    render is the function that's called whenever the state changes. For simplicity you can
    discard the entire DOM everytime.
    
    A Custom Element is just a HTML element so standard DOM methods will work on it 
    (this).
    
2. Register the custom element implementation ContactsListElement in index.html
    under the name spa2014-contacts-list
    
3. Add a <spa2014-contacts-list> element to the body of the index.html
    Load up the index.html file in your browser and see the element in the inspector
    and check for the console logs.

4. In the render method, iterate over the state map and display its data in the
    DOM.
    
    Use a document fragment to hold the DOM to add while iterating over the Map.
    
    var documentFragment = document.createDocumentFragment();
    
    this.state.forEach((contactState, contact) => {
        //create DOM elements to hold the contact data
        //which consists of the contact avatar and contact name.
        //Append to documentFragment.
    });
    // append the documentFragment to your

5. Once the list of contacts is displayed create a template that represents the
    component.  The template should have a header with the text 'Contacts', an
    <hr> and a section for the contacts to be placed.

6. Change your render method to place the document fragment in the contacts section.
    You can save the contacts section during the attachedCallback as this.contactsSection
    
7. Add an event listener to yourself to handle click events.
    Use arrow functions to minimize  boilerplate for the callback.  Pass the
    mouse event in to the _onContactSelected method.

    Use destructuring to extract all the details from the event.
    Destructuring works to multiple levels.

    Look at the page in the browser, and click on one of the contact rows. You
    should see a log line in the console.

    Look at contactSelected in ContactsListActions.js to see the console log line
    which uses String templates.

Slides
------

Missing: Packagemanagement, http2.0 / spdy. 


Workshop - Integrate Custom Element
-----------------------------------
    
...

Conclude
--------

Missing: live reload, source maps
Missing: use es6, packages and webcomponents today.