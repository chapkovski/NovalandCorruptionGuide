=========================================
Offline Bots
=========================================

The offline bots are exclusively utilized to run the study on the local server through the bots.
Consequently, all collected data is stored on the local server.

Activate Browser Bots
__________________________
To enable the use of browser bots, add the following line to your app's Settings.py file:

.. code-block:: console

    use_browser_bots = True

To illustrate this with an example within the project:

.. code-block:: console

    SESSION_CONFIGS = [
    dict(
        name='Pretest_Novaland',
        display_name='Novaland',
        app_sequence=['phase_1', 'phase_2', 'phase_3', 'phase_4', 'phase_5', "final"],
        num_demo_participants=3,
        use_browser_bots=False,
    )

Create Bot Pages
__________________________
To incorporate bot functionalities into individual apps within the project, a tests.py Python file must be created within each app.
In this file, all values need to be added to instruct oTree bots about their actions. We populate this file with the values that the bots are meant to input.

To begin, we import the necessary packages to leverage all the functions and to obtain randomized values:

.. code-block:: console

    from . import *
    import random

Firstly, we need to create a bot class:

.. code-block:: console

    class PlayerBot(Bot):
    def play_round(self):
        ...

Next, for each page integrated within the app, we need to assign values to the defined form fields on those pages.
We sequence all the pages together using the 'yield' keyword, instructing the program on its actions.
When encountering a form field on a page, we provide the field name and its corresponding value to the program:

.. code-block:: console

   yield Page1, {"Formfield_Name": "Formfield_Value"}

To randomize response options, we can utilize the random function:


.. code-block:: console

    yield Page2, {"Formfield_Name": random.choice(["Formfield_Value1", "Formfield_Value2", "Formfield_Value3", ...])}


If a page doesn't contain a form field value but instead has a "Continue" button with a specific ID that the bot can access and evaluate to True, the page can be progressed automatically:

HTML Button:

.. code-block:: console

    <button class="button" type="submit" id="continue-btn">Continue</button>

We now use this button's ID, which in this example is "continue-btn", within the bot code:

.. code-block:: console

    yield Submission(Page3, {"continue-btn": True}, check_html=False)


Furthermore, it's crucial to instruct the bot to submit the page using the Submission function.

Run Offline Bots
________________________
To run the offline bots, simply start the offline server using the command 'otree devserver' in the terminal and open a page from the program.