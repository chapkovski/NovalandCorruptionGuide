======================
Installation
======================

Download Python
=====================
First, Python, the programming language we want to work with, needs to be downloaded.
`Python Download Page <https://www.python.org/downloads/>`_

Development Environment
=======================
Feel free to use any preferred development environment.
The following guide is primarily crafted for use with the 'PyCharm' development platform.

.. _pycharm-download:

Download PyCharm
------------------------
The advantage of PyCharm is that it has been developed especially for Python.

`PyCharm Download Page <https://www.jetbrains.com/de-de/pycharm/download/>`_

oTree
========================
Novaland is a project build on the 'oTree' plattform.
oTree is a platform for conducting behavioral research and experiments.

Install oTree
-----------------------
To install oTree in PyCharm, follow these steps:

    1. Open the 'Terminal' window in PyCharm

    2. Type:

    .. code-block:: console

        pip install otree

    in the terminal and press enter

    3. The installation process of oTree in PyCharm will now begin


GitHub
========================
The project is hosted on GitHub.
Open a new PyCharm project and go to VCS --> Get from version control.
Enter the Novaland repository URL in the field "URL" and choose a folder where you save the project.
PyCharm will fetch the latest Novaland version from the repository.

The Novaland repository URL is: https://github.com/EmpPowiUDE/solidarity_game

(Optional) Enable Django Support
----------------------
You can enable Django support in PyCharm for the Novaland project to e.g. see code completion suggestions in the HTML templates.

    1. Open PyCharm

    2. Click on 'file' in the menu

    3. Choose 'Settings'

    4. Go to 'Languages & Frameworks'

    5. Choose Django

    6. Press the 'Enable Django Support' button or check if the button is already enabled

    7. Under 'Django project root' select the project on your computer


Select Interpreter
=====================
To use Python for the project, an interpreter needs to be chosen.

    1. Open 'File'
    2. Go to settings
    3. Search 'interpreter'
    4. Choose the 'Python interpreter' in the project
    5. Click on 'add interpreter'
    6. Choose 'Add Local Interpreter'
    7. Select the appropriate version of Python installed on your system as the interpreter for the project.
    8. Restart PyCharm

Additional installation
=========================

In Novaland, we use various libraries, so we still need to install "email_validator" and "psycopg2" via the terminal.
To install these libraries in PyCharm, you can use the package manager called "pip".
Pip is a command-line tool used for installing Python packages and libraries.
To install "email_validator" and "psycopg2", you can open the terminal in PyCharm and type the following commands:

email_validator
--------------------
"email_validator" is a Python library used for validating email addresses.
It can be used to check if an email address has a valid format and if it belongs to a valid domain.
The library provides several methods to validate email addresses, such as "validate_email()" and "is_email()" functions.

Installation
.. code-block:: console

    pip install email_validator

psycopg2
---------------------
"psycopg2" is a Python library used to connect to PostgreSQL databases.
It provides a way to interact with a PostgreSQL database from a Python script, allowing you to execute queries, insert or update data, and more.

.. code-block:: console

    pip install psycopg2

These commands will download and install the libraries and their dependencies automatically.
Once the installation is complete, you can import these libraries in your Python code and start using them.

oTree.zip
==================

Once everything is installed, if you are collaborating with others and want to share projects with each other, you can use .otreezip files.
.otreezip files are compressed files that contain all the files and folders necessary to transport or share an oTree project easily.
They allow you to package your entire project into a single file that can be easily transferred to another computer or shared with others.

Create a oTree.zip
--------------------

1. Open the oTree project.
2. Enter the following command in the terminal:

.. code-block:: console

    otree zip

3. The system will then save the project in the project folder.

Open a oTree.zip
--------------------------

To open an otree.zip file, follow these steps:

1. Save the otree.zip file in the folder where you want the project to be located.

2. Open the oTree project.

3. Use the cd command to navigate to the appropriate folder:

.. code-block:: console

    cd C:\Documents\Novaland

4. Enter the command otree unzip followed by the name of the otree.zip file in the terminal:

.. code-block:: console

    otree unzip ProjectName.otreezip

5. Open the new folder in PyCharm.