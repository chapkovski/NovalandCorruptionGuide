===================
Buttons
===================
Buttons can be equipped with different functions. These are being triggered when the button is pressed.

Standard oTree Button
------------------------
If you want to give the participants the possibility to call the next page, you can use oTree's own Django button.
This allows you to have all the built-in form fields filled out by the participants before they can call the next page.

.. code-block:: console
    :linenos:

    {% next button %}

Costumized Button
=====================
Customized buttons are essential in creating a personalized and engaging user experience for participants in an oTree app. Unlike a standard "Next" button, a customized button can perform a variety of functions depending on the app's requirements.

HTML Button Overview
=====================
A customized button in HTML code consists of several elements. The button is enclosed within the 'Button' tag, which enables certain functions such as clickability.

The ground structure
------------------------
For the visual representation of the button and for the integration of the functions, the button is written inside the 'Button' tag, which enables certain functions.

.. code-block:: console

    <button Button Properties> Button Text </button>


The style of the button
--------------------------
The appearance of the button is controlled by a stylesheet that can be referenced using the 'class' attribute, as shown below:

.. code-block:: console

    <button class='button'> Button Text </button>

Give it a function
------------------------
To give a customized button a function, we can define a JavaScript function and bind it to the button's onclick event.
The function can perform any desired action, such as displaying a message, submitting a form, or navigating to a different page.

Here is an example of how to define a JavaScript function and bind it to a customized button:

.. code-block:: console

    <button class='button' onclick='myFunction()'> Button Text </button>

    <script>
    function myFunction() {
        # Add your code here
    }
    </script>

The JavaScript function is defined in a script tag, which can be placed either in the HTML file or in a separate file in the 'static' folder of the oTree app.
By defining the function in a separate file, it can be reused across multiple pages of the app.