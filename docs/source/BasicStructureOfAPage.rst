.. _basic-structure-page:

============================
Basic structure of the page
============================
The basic scheme of the already existing pages in Novaland looks like this:

`For more informations <https://otree.readthedocs.io/en/latest/pages.html>`_.

Django
=====================
The web framework used by oTree is Django.
For Novaland we basically need the functions to create a "block" in which we can receive and send data to the backend python code.
A Django code always follows this structure {% code %}.
Therefore we need a block start and a block end and they are written like this:

.. code-block:: console
    :linenos:

    {% block title %}
    ...
    {% endblock %}

The visual representation of the page
=====================================
In order for the newly created page to adapt to the style of the already existing pages, you must link the already prescribed 'stylesheet'. This is based on a CSS written code, which is integrated in the project under the "_static" folder.
Therefore, in the first step you need to access the Statics folder via Django.

.. code-block:: console
    :linenos:

    {% load staticfiles %}


In the second step, you link the stylesheet to your project using HTML.

.. code-block:: console
    :linenos:

    <link href="{% static 'NovalandStyle.css' %}" rel="stylesheet">


Here is a template to apply to the HTML site:

.. code-block:: console
    :linenos:

    {% block style %}
    {% load staticfiles %}
    <link href="{% static 'NovalandStyle.css' %}" rel="stylesheet">
    {% endblock %}

Add text
=====================
If you want to write a text on your page, it must be in a block.
A page always consists of a title and the content.

Title
----------------------
To convert the text you wrote to a title, you need to write it between the 'h1' element and retrieve the 'h1' class from the stylesheet.

Example:

.. code-block:: console
    :linenos:
    {% block title %}
    <h1 class="h1"> Title Text </h1>
    {% endblock %}

Content
----------------------
The content text is written in a different block than the title.
This is written between a 'p' element, i.e. a paragraph or text paragraph element.

.. code-block:: console
    :linenos:

    {% block content %}
    <p class="p"> Content Text </p>
    {% endblock %}

Basic HTML elements for working with text
-----------------------------------------

+----------------------------+--------------------------------+
| Element                    |      Description               |
+============================+================================+
| <br>                       |      Line break                |
+----------------------------+--------------------------------+
|   <strong> ... </strong>   |      Text displayed in bold    |
+----------------------------+--------------------------------+
| <i> ... </i>               |      Text displayed in italic  |
+----------------------------+--------------------------------+

Add functions
====================

JavaScript
-----------

With the help of JavaScript, various functions can be integrated on the page.
The JavaScript code is written between a 'script' element.

.. code-block:: console
    :linenos:

    <script> javascript code </script>

To see an example go to the :ref:`buttons-page` page.

init.py functions
=====================
The init.py file in oTree allows you to define functions that can be assigned to pages in the backend.
This means you can define JavaScript functions for the frontend and also define 'vars_for_template' to be used in the HTML templates.
'vars_for_template' returns a dictionary with variable names as keys and data as values, which can then be accessed and used in the HTML templates using the Django template language.

`For more informations <https://otree.readthedocs.io/en/latest/pages.html>`_.

validate E-Mails
---------------------
If, for example, a :ref:form-fields has been created, it can be equipped with additional functions.
When an email address is requested, functions can be added to validate whether the input is really an email.

.. code-block:: console

    from email_validator import validate_email, EmailNotValidError   # Input the email_validator to use it

    class PageName(Page):

    @staticmethod
    def error_message(player, value):
        try:
            # Validate.
            valid = validate_email(value["f26"].strip(), check_deliverability=False)
        except EmailNotValidError as e:
            # email is not valid, exception message is human-readable
            return 'The entered email address is invalid. Please enter your email address again.''

This is a static method that validates an email address entered in a form field.
It uses the "validate_email" function and returns an error message if the email is invalid.