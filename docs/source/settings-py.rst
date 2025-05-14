==============================
Settings.py file
==============================
The settings.py file in an oTree project contains various configuration settings and parameters for your oTree experiment or application.
These settings control various aspects of your experiment such as the number of rounds, the number of participants per group, the name of the application, the language to be used, and other settings related to the behavior and appearance of your experiment.
You can modify the settings.py file to customize your experiment to meet your specific needs.

SESSION_CONFIGS:
____________________________
In the context of an oTree project, SESSION_CONFIGS is a list of dictionaries in the settings.py file that define the different sessions or conditions in your experiment. Each dictionary in the list represents a different session configuration, and contains the various settings and parameters for that particular session, such as the number of rounds, the number of participants per group, and the name of the session.
The SESSION_CONFIGS list allows you to specify multiple different sessions with different configurations, and participants can be randomly assigned to one of these sessions during the experiment.
By using different session configurations, you can run experiments with different treatments, parameters, and conditions.

This is a list of dictionaries that define different sessions or conditions in your experiment. Each dictionary in the list represents a different session configuration, and contains the various settings and parameters for that particular session.
Here's an example of how you might use this function:

.. code-block:: console

    SESSION_CONFIGS = [
    # It is possible to name several apps.
    # App 1:
    {
        name='project_Name_1',                       # The name of the project
        display_name_1='Project_Display_name_1',     # The Name that is displayed when you start the app
        num_demo_participants=3,                     # Number of the participants
        app_sequence=['app_1', 'app_2],              # All apps that will be represented in this project.
    },
    # App 2:
    {
        name='project_name_2',
        display_name='Project_Display_name_2',
        num_demo_participants=5,
        app_sequence=['app_3', 'app_4],
    },]

**'name'**

 This is a string that gives the session a unique identifier.

.. code-block:: console

    name='ProjectName'


**'display_name'**

 This is a string that gives the session a human-readable name.

.. code-block:: console

    display_name='Novaland'

**'num_demo_participants'**

 This is an integer that sets the number of demo participants for the session.


.. code-block:: console

    num_demo_participants=3;


**'app_sequence'**

 This is a list of strings that determines the order in which apps will be run in the session.

.. code-block:: console

    app_sequence:['App_Name_1', 'App_Name_2', ...]



PARTICIPANT_FIELDS
_______________________
Participant fields can be used to store information about each participant in your experiment.
Each field is defined as a tuple, with the first element being the field name, and the second element being the field type.

The main difference with formfields is that Player variables can be used across the entire oTree project, not just within individual apps.
These fields store information about a single participant that can be used to personalize their experience or gather data for analysis within the app.

Example:
We create a variable in Settings.py that can be used for a participant for the whole project.
This data is stored as a participant field and therefore can be accessed from other apps.

Create participant value

    Settings.py:

    .. code-block:: console

        PARTICIPANT_FIELDS = ['ValueName1', 'ValueName2', ...]


Save value in the participant variable:

    __init__.py file in app:

    .. code-block:: console

        player.participant.ValueName1 = Value_1
        player.participant.ValueName2 = Value_2


The 'player' refers to the current player object, while 'participant' refers to the participant object associated with that player.
'ValueName1' and 'ValueName2' are custom attributes that have been set, and 'Value_1' and 'Value_2' are their respective values.
These values can be accessed using the same syntax throughout the experiment and can be used for tracking participant characteristics, storing experimental conditions, or creating customized feedback messages."

    __init__.py file in app:

    .. code-block:: console

        New_Value_1 = player.participant.ValueName1
        New_Value_2 = player.participant.ValueName2


SESSION_FIELDS
__________________
Session fields can be used to store information about each session in your experiment.
Each field is defined as a tuple, with the first element being the field name, and the second element being the field type.

The information stored in these fields can then be used in the oTree app to determine which treatments a particular session receives, or to save aggregate session data.
This allows you to centralize important information that will be referenced and utilized throughout the experiment, providing a unified and consistent source of data for all components of the project.

This field was used in Novaland mainly to aggregate information from all participants and store them all in one variable.


Example:

Create an Settings Field:

**settings.py file:**

.. code-block:: console

    SESSION_FIELDS = ['Variable_1', 'Variable_2', ...]

Save a value in a session field:

**__init__.py**

.. code-block:: console

    player.session.Variable_1 = Value_1
    player.session.Variable_2 = Value_2

Use a saved session value:

**__init__.py**

.. code-block:: console

    New_Value_1 = player.session.ValueName1
    New_Value_2 = player.session.ValueName2


LANGUAGE_CODE
____________________

This is a string value that sets the language used in your experiment.

.. code-block:: console

    LANGUAGE_CODE = 'de'

ADMIN_USERNAME
____________________
The ADMIN_USERNAME in the settings.py file in an oTree project refers to the username used by the administrator of the platform.
This username is used to log in to the oTree administration interface, which provides access to various tools and features for managing the platform, such as monitoring participant progress, viewing data, and controlling the flow of the experiment.
The ADMIN_USERNAME setting allows you to specify the username that will be used by the platform administrator.


Example:

.. code-block:: console

    ADMIN_USERNAME = 'admin'


ADMIN_PASSWORT
___________________
The ADMIN_PASSWORD is a setting in oTree that allows the researcher to access the administrative features of the experiment.
It is a unique password that should be kept secure, as anyone who knows the password can access and modify the experiment.
The password can be set in the settings.py file of the oTree project, and should be changed from the default setting for security purposes.

.. code-block:: console

    ADMIN_PASSWORT = 'your_password_here'

If you host your experiment remotely, you should not store your password in the code.
Instead, use 'environ.get'.
By using "environ.get", the project reads the password value from the Heroku environment variables.
This approach provides an added layer of security as the password is not hardcoded into the code and is not publicly visible.
The password is stored as an environment variable named "OTREE_ADMIN_PASSWORD".

.. code-block:: console

    ADMIN_PASSWORD = environ.get('OTREE_ADMIN_PASSWORD')


SECRET_KEY
____________________
The SECRET_KEY in oTree is a secret password used for securing data within an oTree project.
It is used to support cryptographic functions such as data encryption and prevention of data tampering.
The SECRET_KEY should never be publicly disclosed and should be kept securely.

.. code-block:: console

    SECRET_KEY = '2341734735143'

These numbers are just an example, you can use any numbers you like.

DEBUG
_____________________________
Debug is a Boolean value that controls whether oTree should run in debug mode or not.
In debug mode, detailed error messages are displayed and the performance is slower.


Example:

.. code-block:: console

    DEBUG = False


Static
=========================
A directory that contains any static assets such as images, fonts, or other files that are needed by the app.

Template
===========================
A directory that contains the HTML templates for the pages in the app, as well as any custom CSS and JavaScript files used by the templates.