======================
Create a new page
======================

Creating a HTML file
=====================
To work on a new page in Novaland you need to create first a new page in an existing app.
To do this click the right mouse button on the app or one of the phases we have already defined, click on "New" and then on "HTML File".

.. code-block:: console

    [click the right mouse button on the app / phase] -> [New] -> [HTML File]

Once the new page is created, it needs to be linked to the backend.
To do so, a class needs to be created in the init.py file of one of your apps, which contains the exact name of the newly created page.
This class must inherit the properties of a page, indicated by marking it within parentheses.

.. code-block:: console

    class PageName(Page):
        pass

To display the newly created page in your app, it needs to be added to the page_sequence.
The order of the pages in the page_sequence is important as it determines the order in which the pages are displayed to the participants.

.. code-block:: console

    page_sequence = [PageName, PageName2, PageName3]

The instructions on the :ref:`basic-structure-page` are essential in order to work with a page in oTree.