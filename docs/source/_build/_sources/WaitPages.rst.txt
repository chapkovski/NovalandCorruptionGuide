======================
Wait Pages
======================
Wait pages in oTree are pages that help coordinate the timing of an experiment with multiple participants.
Basically, it makes sure everyone is on the same page and ready to move forward with the next step of the task at the same time.

It is a type of page in oTree that's defined in the app's views.py file.
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
            return player.value == 1


The method returns the value of the value attribute of the player, which is a boolean indicating whether the value is equal to 1.
If the value is 1, the method returns True, which means that the page should be displayed.
If the value is not 1, the method returns False, which means that the page should not be displayed.

before_next_page
________________________
Runs code before the participant is redirected to the next page.
You can use this function to perform any necessary updates or calculations before the participant moves on.

Example:

.. code-block:: console

    class PageName(Page):
        @staticmethod
        def before_next_page(player: Player):
            player.group.total_earnings = sum([p.earnings for p in player.group.get_players()])

function calculates the total earnings of all participants in the group and stores the result in the total_earnings attribute of the group.

participant_label
_________________________
Returns a string that represents the participant, which is displayed on the wait page to other participants.

Example:

.. code-block:: console

    class PageName(Page):
        @staticmethod
        def participant_label(player: Player):
            return player.participant.code

Returns the code of the participant, which is used to identify the participant on the wait page.

vars_for_template
_______________________
Returns a dictionary of variables that will be passed to the wait page's template.
You can use this function to pass information from the Python code to the HTML template, such as the number of participants that have arrived at the wait page.

after_all_players_arrive
________________________
Runs code after all participants have arrived at the wait page.
You can use this function to perform any necessary updates or calculations after the group has assembled.

get_players_for_group
________________________
Returns the participants that are in the same group as the current participant.
You can use this function to perform group-level calculations or to display information specific to the participant's group.

group_by_arrival_time
_________________________
Determines how participants are grouped on the wait page.
You can use this function to group participants by the order in which they arrived at the wait page, or by some other criterion.

Customized Wait Page
========================
In Novaland there are mainly witePages, which depend on a certain time of day.

The first step is to create a normal wait page.
In this, we want to include a timer and a variable that both need to be fulfilled for the page to be displayed and disappear automatically after a certain time.
Die Variable wird in der settings.py erstellt und kann dadurch, Einfluss auf teilnehmenden Personen und allen Seiten, egal in welcher App.
Diese kann dann beim erstellen, der Session individuell eingestellt werden.

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

**waitingPageActivated**

This is the variable that we will use later in the script to activate the wait pages.


**deactivateWaitingPageTime**

This indicates the time at which the wait page should disappear.
The input reflects the time, where 1830 is equivalent to 6:30 PM.


**tooLatePageActivated**

This is a page that appears when a person takes longer than expected.
It must also be activated for the event to take place.


**tooLatePageTime**

This is the time at which a participant is considered late.


**dateNovaland**

This is the date on which the study will take place.
In our example, '09' represents the day and '11' represents the month.


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
We can now combine 'datetime' and session config variables in our init.py file to make our WaitPage dependent on them.

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

You can insert a timer in the HTML file of the waiting page to reload the page in a specified amount of time.

.. code-block:: console

    <meta http-equiv="refresh" content="10">


The HTML code you provided is a meta tag that instructs the browser to refresh the current web page after a certain amount of time has passed.
In this case, the "content" attribute is set to "10", which means the page will automatically refresh after 10 seconds.