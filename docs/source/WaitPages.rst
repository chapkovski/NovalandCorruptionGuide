======================
Wait Pages
======================
Wait pages in oTree are pages that help coordinate the timing of an experiment with multiple participants.
Basically, it makes sure everyone is on the same page and ready to move forward with the next step of the task at the same time.

It is a type of page in oTree that's defined in the app's init.py file.
When a participant reaches the wait page, they'll see a message saying they're waiting for other participants to catch up.
You can change what the wait page looks like and what it says.

Standard Wait Pages
======================
You can use oTree's own wait pages.
All you need to do is create a new page, or repurpose an existing one.
Go to your __init__.py file and add WaitPage inside the brackets of the class that you want to be the wait page. This way, oTree will recognize it as a wait page and you can assign it new functions.

.. code-block:: console

   class PageName(WaitPage):

WaitPage Function
=========================
Here are a few helpful features.

is_displayed
_________________________
Returns a Boolean indicating whether the wait page should be displayed to the participant.
You can use this function to conditionally display the wait page based on certain conditions, such as the participant's previous answers.

Example:

.. code-block:: console

    class PageName(Page):
        @staticmethod
        def is_displayed(player: Player):
            return player.income < 1000


The method returns True if the value of the income attribute of the player is smaller than 1000.
If the method returns True the page will be displayed.
If the method returns False the page will not be displayed.

more functions
__________________________
Visit `this page <https://otree.readthedocs.io/en/latest/multiplayer/waitpages.html>`_ for more Wait Page features.

Customized Wait Page
========================
In Novaland there are mainly Wait Pages which depend on a certain time of day to prevent participants from proceeding
before and after a certain time.

The first step is to create a normal wait page.
On this page, we include a timer and a variable that both need to be fulfilled for the page to be displayed and disappear automatically after a certain time.
The variable is created in the settings.py file and as a result, can have an impact on all pages and participants, regardless of the app.
This variable can then be individually configured when creating a session.

Session config variable
__________________________

In this case we just want to tell if the timer should be activated or not.
We can create the variables we need for the date and time check via the config setting of the project.
For this you have to go to the setting.py file and define your variables in the SESSION_CONFIG item inside the 'dict'.

settings.py file:

.. code-block:: console

    SESSION_CONFIGS = [
    dict(
        waitingPageActivated=False,
        deactivateWaitingPageTime='1800',
        tooLatePageActivated=False,
        tooLateTimePage='1830',
        dateNovaland='0911',
        doc="Time: 'hhmm'; utc = now - 1; date: 'ddmm "
    )

waitingPageActivated:
This variable activate or deactivate the waitpage.

deactivateWaitingPageTime:
This indicates the time at which the wait page should disappear.
The input reflects the time, where 1830 is equivalent to 6:30 PM.

tooLatePageActivated:
This is a page that appears when a person takes longer than expected.
It must also be activated for the event to take place.

tooLatePageTime:
This variable specifies the time threshold at which a page will be displayed, informing the participant that they have exceeded the allotted time.

dateNovaland:
This is the date on which the study will take place.
In our example, '09' represents the day and '11' represents the month.
Attempting to participate before or after this date will render it impossible for the participant to do so.


datetime
____________________________________
To make the page dependent on a specific time and date, we need to use the 'datetime' Python module.
It allows us to create objects for date and time information and perform operations like conversion between different date and time formats, calculation of time differences, and manipulation of date and time information.
We first need to import it.

.. code-block:: console

    import datetime


Now we can use datetime to specify a particular date and time.

Date only:

.. code-block:: console

    datetime.datetime(Year, Month, Day)


Time only:

.. code-block:: console

    datetime.datetime(Hours, Minutes, Seconds)


Date and Time:

.. code-block:: console

    datetime.datetime(Year, Month, Day, Hours, Minutes, Seconds)


To check whether the given conditions have been met, we need to obtain the date and time of the participating individuals:

.. code-block:: console

    datetime.datetime.now()


Activate the WaitPage
______________________________
We can now combine 'datetime' and session config variables in our init.py file to make displaying a  WaitPage dependent on them.

The session config variables can be retrieved via this Python code:

.. code-block:: console

        player.session.config['SessionConfigValueName']


Example

.. code-block:: console

    class TooLatePage(Page):
        @staticmethod
        def is_displayed(player: Player):
        if player.session.config['SessionConfigValueName'] == True and datetime.datetime.now() > datetime.datetime(2022, int(
                        player.session.config['dateNovaland'][2:4]), int(player.session.config['dateNovaland'][:2]), int(
                        player.session.config['tooLateTimePage'][:2]), int(
                        player.session.config['tooLateTimePage'][2:4]), 0):
            return True
        else:
            return False

We can check in the is_displayed function if our conditions are met. If they are true, the page will be displayed, otherwise it will not.


.. code-block:: console

    def is_displayed(player: Player):
        if ....
          return True
        else:
            return False


The first part of our condition is to check if the WaitPage has been activated in the session.

.. code-block:: console

    player.session.config['waitingPageActivated'] == True


The second part is to check if the time and date of the participant has exceeded the time we have specified.

.. code-block:: console

    datetime.datetime.now() > datetime.datetime(2022, int(                  # Year
            player.session.config['dateNovaland'][2:4]), int(               # Month
            player.session.config['dateNovaland'][:2]), int(                # Day
            player.session.config['zuSpaetTimePhase4'][:2]), int(           # Hour
            player.session.config['zuSpaetTimePhase4'][2:4]), 0)            # Minutes



timer to refresh the site
__________________________

You can insert a timer in the HTML template of the waiting page to reload the page in a specified amount of time.

.. code-block:: console

    <meta http-equiv="refresh" content="10">


The HTML code you provided is a meta tag that instructs the browser to refresh the current web page after a certain amount of time has passed.
In this case, the "content" attribute is set to "10", which means the page will automatically refresh after 10 seconds.