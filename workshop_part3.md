Topic - Workflow
---------------------

http://slides.com/kybernetikos/pacman#/

Workshop - Integrated with other Custom Elements.
-------------------------------------------------

After completing the first workshop

	https://github.com/briandipalma/spa-2014-communicator

and the second

	https://github.com/briandipalma/spa-2014-contacts-list

we can move onto integrating the packages we have worked on into an application.

To start

	git clone https://github.com/briandipalma/spa-2014-app

1. Open `src/index.js`

	All the application Custom Elements are imported and registered in this module.

2. Serve the package

	All these packages can be served by a static server rooted at the package directory.
	One is included in the package and can be launched with the command.

	```bash
	$ npm run serve
	```

	The included server will serve the package at `http://127.0.0.1:8080`

3. Log in

	Navigating to `http://127.0.0.1:8080` will present you with a simple login form.
	Entering any username and password will work to login.
	Investigating the DOM will show some of the Custom Elements registered in `src/index.js`.

	The application template is in the `template/spaApplication.text` file.

4. Add the `spa2014-contacts-list` element

	To add the `spa2014-contacts-list` element to the application it must added to the template.
	Open up the app template and add a `spa2014-contacts-list` element just under the logged-out div.

	```html
	<div class="logged-out">
		<spa2014-contacts-list></spa2014-contacts-list>
	```

	Import the element and register it before you register `spa2014-app` in the `registerApplicationElements` method.

	```javascript
	import {ContactsListElement} from 'spa-2014-contacts-list';
	
	export function registerApplicationElements() {
		...
		document.registerElement('spa2014-contacts-list', ContactsListElement);
		...
	}
	```

	Reloading the application will show an old version of the `spa2014-contacts-list`.

5. All the custom elements are registered inside src\index.js including spa2014-contacts-list.

If you run npm run serve you can see a simple chat application without the spa2014-contacts-list element.



You will see that the spa2014-contacts-list in the application is the old incomplete one. To use your newer version of it open a cmd window, navigate to your spa2014-contacts-list and enter npm link Then from the cmd window navigate to the spa-2014-app and as administrator in Windows run npm link spa-2014-contacts-list.
