Project-wide files
===================================================

Global settings
----------------
The global settings of the project are defined in the :file:`settings.py` file. This file contains the configuration settings for the entire project, including database settings, static files, and other global settings that apply to all apps within the project. It is important to ensure that this file is correctly configured to ensure the proper functioning of the project.

Room
^^^^^^^^^^^^^^^^^^
There are a couple of options on how to launch an oTree project for a data collection. Here, setting up a "room" in which the questionnaire is embedded is the most robust one and thus used for this project. Several of these rooms were defined, each of which can be used to launch a separate instance of the questionnaire. The room used in the final data collection is called "study" and is defined as follows.

.. code-block:: python
    :linenos:


    ROOMS = [
        dict(
            name='study',
            display_name="Study"
        ),
    ]


Session
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Within a room, we have to define a session. A session is a single instance of the questionnaire that participants can join. Each session can have its own configuration, including the number of participants, the apps included in the session, and other settings. The session used in the final data collection is called "Full_game" and is defined as follows.

.. code-block:: python
    :linenos:


    SESSION_CONFIGS = [
    dict(
        name='Full_game',
        app_sequence=['intro', 'main', 'election', 'post'], # defines the apps used in the session
        num_demo_participants=200, #used for development only
    ),
    ]


Participant redirection to sample provider
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The participants of our study were recruited from the sample provider Prolific. From the providers' website, they were directed to our survey and they were redirected back after completing the questionnaire. To ensure that the participants are redirected back to the provider's website after completing the questionnaire, we need to define a return URL in the session configuration. This URL is used to redirect participants back to the provider's website after they have completed the questionnaire. The return URL is defined dynamically, as each participant was assigned a unique URL after completion.

.. code-block:: python
    :linenos:


    PROLIFIC_RETURN_CODE = environ.get('PROLIFIC_RETURN_CODE', 'NO_CODE')

    SESSION_CONFIG_DEFAULTS = dict(
        prolific_return_url=f"https://app.prolific.com/submissions/complete?cc={PROLIFIC_RETURN_CODE}", # unique redirection URL for each participant
        for_prolific=True,
        real_world_currency_per_point=1.00, participation_fee=0.00, doc="" # not relevant
    )

There are more options defined in the :file:`settings.py`, but those are not relevant for conducting the data collection and are thus not explained here. For a complete overview of the settings, please refer to the `official oTree documentation <https://otree.readthedocs.io/en/latest/index.html>`_.


Aesthetics of the web questionnaire
---------------------------------------------
The aesthetics of the web questionnaire are defined in the :file:`_templates/global/Page.html` file. This file contains the HTML code that defines the overall layout and design of the questionnaire pages. It includes style elements such fonts, colors, and outlays used on each project. To ensure a consistent appearance of the questionnaire's web pages, nearly all pages build on this template and only specify their content and functionality in their designated HTML files, of which one exist for each page of the questionnaire.
