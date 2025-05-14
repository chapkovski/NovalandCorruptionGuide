=========================================
Online Command Line Bots
=========================================

You can also create bots on Heroku from a local server or directly from your PyCharm program.

.. _otree-rest-key:

Otree Rest Key
____________________

In order for your local PyCharm system to access Heroku, you need to add the "OTREE REST KEY" on the Heroku server where the project is hosted.

1.	Open Heroku and log in.
2.	Open the respective app where you want to run the bots.
3.	In the app's dashboard, select "Settings" from the top menu bar.
4.	Under Config Vars, you'll find a button labeled "Reveal Config Vars".
5.	Add a new variable. The key must be "OTREE_REST_KEY", and the value should be the password you have chosen.

.. image:: docs/source/_static/Doku12.png
  :width: 800

Use Command line for Online Bots on Heroku
____________________________________________

Now, we need to set the OTREE_REST_KEY on our local server as an environment variable to access Heroku with our bots.
For this purpose, we use the following code:

.. code-block:: console

    $env:OTREE_REST_KEY="YOUR_PASSWORD"


With the environment variable set, the bridge to Heroku is established, and now we can start the browser bots.
The code to do so consists of the first part of the browser bot command:

.. code-block:: console

    otree browser_bots Your_App_Name

Then, the server needs to be assigned:

.. code-block:: console

    otree browser_bots Pretest_Novaland --server-url=https://pilotnovaland2022.herokuapp.com/room/Novaland

In this case, you should add the URL of your app.
Upon confirming the input, a certain number of browser instances will open, carrying out the bot run on Heroku.