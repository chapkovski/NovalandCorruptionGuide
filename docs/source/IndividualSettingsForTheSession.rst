=====================================
Individual settings for the session:
=====================================

The general session configuration can be set in the SESSION_CONFIGS in the settings.py file in the root directory of your Novaland project.

SESSION_CONFIGS is a list of dictionaries, with each dictionary containing key-value pairs representing settings for a particular session configuration.

.. code-block:: console

    SESSION_CONFIGS = [
        dict()
    ]

To define your own variables, you need to enter your values under SESSION_CONFIGS.
These values can then be changed when starting the app on Heroku under the Session Settings.

.. code-block:: console

    SESSION_CONFIGS = [
        dict(
            Page1Visible=False;
        )
    ]

For example, the provided variable 'Page1Visible' could be used to either show or hide certain pages using the 'is_displayed' function.
In this case, the 'is_displayed' function could look like:

.. code-block:: console

    class PageName(Page):
        @staticmethod
        def is_displayed(player: Player):
            if player.session.config['Page1Visible'] == True:
                return True
            else:
                return False


Individual Novaland Settings
_______________________________
Novaland's functionality is dependent on specific dates and times, as different phases occur at predetermined times.
To ensure that these phases are not accessible until the designated date and time, we define start times in settings.py.
These variables can be changed when starting a session via the admin panel-

An example might be:

.. code-block:: console

    SESSION_CONFIGS = [
        dict(
            name='phase_1',
            display_name='phase_1',
            app_sequence=['phase_1'],
            waitingPagePhase1=False,        # Activates/Deactivates the waiting page for the first Phase
            timeWaitingPagePhase1='1900',   # Determined when to see the first phase of the app for the participants
            waitingPagePhase4=False,        # Activates/Deactivates the waiting page for the second Phase
            timeWaitingPagePhase4='1930',   # Determined when to see the second phase of the app for the participants
            waitingPagePhase5=False,        # Activates/Deactivates the waiting page for the third Phase
            timeWaitingPagePhase5='1940',   # Determined when to see the third phase of the app for the participants
            dateNovaland='0911',            # Sets the date when Novaland should start
            doc="Time: 'hhmm'; date: 'ddmm"
        )
    ]

This information can be used to display different pages and phases for the participants at certain times.
To display a waiting page when the date and time have not yet been reached,
or to make it disappear when the conditions have been met, we define a page with an "is_displayed" function.
We also need to use the "datetime" function.

SESSION_CONFIGS variables are called like this:

.. code-block:: console

    player.session.config['SessionConfigDictVariableName']


Based on this, we wrote the "if" conditions for the "is_displayed" function:


.. code-block:: console

    class Phase_1_Waiting_Page_0(Page):
    @staticmethod
    def is_displayed(player: Player):
        if player.session.config['waitingPagePhase1'] and datetime.datetime.now() < datetime.datetime(2022, int(
                player.session.config['dateNovaland'][2:4]), int(player.session.config['dateNovaland'][:2]), int(
            player.session.config['timeWaitingPagePhase1'][:2]), int(
            player.session.config['timeWaitingPagePhase1'][2:4]), 0):
            return True
        else:
            return False

The method checks the "waitingPagePhase1" parameter in the session configuration and compares the current datetime with the date and time specified in the "timeWaitingPagePhase1" and "dateNovaland" parameters.
If the "waitingPagePhase1" parameter is True and the current datetime is before the specified datetime, the method returns True, indicating that the waiting page should be displayed.
Otherwise, the method returns False and the page is not displayed.
