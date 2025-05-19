======================
Add Pictures
======================
In oTree you can add images in the HTML templates.
It is recommended to optimize the images for online use to reduce server load and page's loading times

_static Folder
======================
Definition:
 The "static" folder in an oTree app serves as a storage location for static files such as images, stylesheets, and JavaScript files that are used in the application.
 These files are served by the web server without any modification and can be shared by multiple pages within the application.
 By using a static folder, you can separate your static assets from your application code, making it easier to maintain and update your application.

Add the picture to a Page
=============================
You need to use HTML code to add your image to the page.

Example:

.. code-block:: console

    <img src="{% static 'appName/PictureName.jpg' %}">


This is the HTML tag for displaying an image on a web page.

.. code-block:: console

    <img src=">

This is a Django template tag, which is used to include the URL for a static file in the HTML.
The static tag tells Django to look for the file in the "static" folder in your oTree app.

.. code-block:: console

    {% static ' %}

This is the location of the image file within the "static" folder in your oTree app.
- appName is the name of the oTree app that contains the image, and
- PictureName.jpg is the name of the image file.

.. code-block:: console

    appName/PictureName.jpg