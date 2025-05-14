======================
Create a new app
======================
In oTree, an app is a group of pages that are related and perform a specific functionality within an oTree experiment.

Apps can be used to isolate and organize specific parts of an experiment, making it easier to manage and navigate.
Each app has its own folder within the oTree project and contains specific files, such as HTML templates, Python modules, CSS and JavaScript files, that are required for the app's functionality.

oTree allows multiple apps to be used within an experiment, defining and managing different parts of the experiment.
Separating the experiment into apps allows changes to be made to one part of the experiment without affecting other parts.

Introduction
__________________

To create a new app in oTree, follow these steps:

1. Open the command prompt or terminal and navigate to the directory where your oTree project is located.
    To navigate to a specific directory using the command line, you can use the 'cd' command followed by the path of the directory you want to go to. If you're already in the parent directory, you can simply type 'cd' followed by the name of the subdirectory you want to navigate to.

    For example, if you want to navigate to a directory named 'my_folder' located in the 'Documents' directory, you would type 'cd Documents/my_folder'. If you're already in the 'Documents' directory, you can simply type 'cd my_folder'.

    The 'cd' command stands for 'change directory' and is a basic command used in the command line to navigate through the file system.

2. Run the following command to create a new app:

.. code-block:: console

    otree startapp app_name

Replace "app_name" with the name you want to give your app.

3. Enter a brief description of the app when prompted.
4. Upon completion, a new folder with your app's name will be created, containing the necessary files to build an app.
5. You can now modify the files in your newly created app folder and make the necessary changes to add the desired functionality.
6. To display the app in the admin panel, you need to add it to the settings.py file. Go to 'SESSION_CONFIGS' and add the app under the app_sequence. The exact name and order are important.
7. Save the changes and restart the oTree server. Your new app should now be available in your project.