=========================================
The :code:`__init__.py` files
=========================================

The __init__.py files are the main files of the apps. They contain the code that defines the structure and functionality of the app. The __init__.py files are used to define the pages, models, and other components of the app. The __init__.py files are divided into several sections, each of which is used to define a specific aspect of the app.

The **sections of the __init__.py files** are as follows:

#. **Imports**: At the beginning of the file, all necessary libraries and modules are imported. This includes the oTree libraries, as well as any other libraries that are used in the app.

#. **Models**: The models section defines the data structure of the app. This includes the variables that are used to store data. oTree defines a set of classes and functions for defining these data storage models.

#. **Pages**: In this section, the content and behavior of the pages are defined. Each page is defined as a class, which inherits from the oTree Page parent class. The page classes define the content and behavior of the pages, including the variables that are used to store data, the functions that are used to process data. Each page class corresponds to one HTML file that defines the layout and design of the page.

#. **Page sequence**: At the end of the file, the page sequence is defined. This is a list of the pages that will be displayed in the app, in the order in which they will be displayed.

* **Additional constants and functions**: In addition to these sections, the __init__.py files may also include additional constants and functions that are used in the app. An example for this is the :code:`create_session` function, which was used in this study to conduct the treatment randomization at the beginning of the main app.

For a more detailed documentation on the structure of the __init__.py files, please refer to the `official oTree documentation <https://otree.readthedocs.io/en/latest/index.html>`_.