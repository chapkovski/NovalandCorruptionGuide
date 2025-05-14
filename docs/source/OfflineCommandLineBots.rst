=========================================
Offline Command Line Bots
=========================================
To execute Command Line Browser Bots that automatically open the app and run the bots, you need to have two terminals open.
In PyCharm, this can be easily achieved by clicking the plus button next to the first terminal.

.. image:: docs/source/_static/Doku11.png
  :width: 800

Firstly, open a server using the following command in one terminal:

.. code-block:: console

    otree devserver

Then, in the second terminal, initiate the Browser Bots using the command:

.. code-block:: console

    otree browser_bots AppName

Here's an example using "Novaland" app:

.. code-block:: console

    otree browser_bots Pretest_Novaland
