Topic - Workflow
---------------------

http://slides.com/kybernetikos/pacman#/

Workshop - Integrated with other Custom Elements.
------------------------

After completing the first workshop

	https://github.com/briandipalma/spa-2014-communicator

and the second

	https://github.com/briandipalma/spa-2014-contacts-list

we can move onto integrating the packages we have worked on into an application.

To start

	git clone https://github.com/briandipalma/spa-2014-app

1. Combine with other Custom Elements.

All the custom elements are registered inside src\index.js including spa2014-contacts-list.

If you run npm run serve you can see a simple chat application without the spa2014-contacts-list element.

Open up the app template and add spa2014-contacts-list just under the logged-out div

You will see that the spa2014-contacts-list in the application is the old incomplete one. To use your newer version of it open a cmd window, navigate to your spa2014-contacts-list and enter npm link Then from the cmd window navigate to the spa-2014-app and as administrator in Windows run npm link spa-2014-contacts-list.
