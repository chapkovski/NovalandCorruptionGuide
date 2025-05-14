.. _randomise-values-page:

=====================================
Randomise Values for the participant
=====================================
There are several ways to randomise treatments in oTree.
In Novaland, we use a pre-defined table with randomised values that determine treatments.
This way, we prevent errors that may occur due to randomisation during the experiment.

Create a CSV Data
____________________________________

We use the CSV format for inputting and outputting data from the program.
There are many different tools that can be used to create CSV files, including Excel, for example.

Here is an example of data in a CSV file:

.. code-block:: console

    ID, Income, Corruption, Jobless, Fireaffected
    0,2,1,1,0
    1,2,0,1,0
    2,0,0,0,0
    3,1,0,0,0
    ...

For example, a CSV file may contain fields such as ID, Income, Corruption, Jobless, and Fireaffected, where the first row contains the names of the fields, and the subsequent rows contain the data values.
This type of document can be created with a simple text editor, but it's important to save the document with the ".csv" extension to ensure that it is properly recognized as a CSV file.

.. raw:: html

   <a href="_static/ExampleCSV.csv" download>Download CSV file</a>

.. image:: docs/source/_static/CSVEXAMPLE.PNG
    :width: 400

For more information about :ref:`csv-page` files

Import a CSV file
_______________________

Now you can place the CSV file in the Novaland folder and use it from there.

When you open a CSV file in PyCharm, it provides you with a preview of the file's contents, allowing you to verify the data and check for any errors you may have made.

.. image:: docs/source/_static/CSV_Data.PNG


To be able to use the individual values in your project, we will write some code in the init.py file, in which the program reads the values and assigns them to each participant.
First, we need to import the CSV library:

.. code-block:: console

    import csv


Then we write a for-loop that reads all the data from the table and assigns it to the participants.

For Example:

.. code-block:: console

    def creating_session(subsession: Subsession):
        f = open('participants_data.csv', encoding='utf-8-sig')

        rows = list(csv.DictReader(f))
        players = subsession.get_players()
        for i in range(len(players)):
            row = rows[i]
            player = players[i]
            player.Income = int(row['Income'])
            player.Corruption = int(row['Corruption'])
            player.Jobless = int(row['Jobless'])
            player.FireAffected = int(row['FireAffected'])
            player.participant.CSVIncome = int(row['Income'])
            player.participant.CSVCorruption = int(row['Corruption'])
            player.participant.CSVJobless = int(row['Jobless'])
            player.participant.CSVFireAffected = int(row['FireAffected'])



Explanation about the Example Code:

.. code-block:: console

    f = open('participants_data.csv', encoding='utf-8-sig')


The 'open' function is used to access a file called 'participants_data.csv'.
It has two parts: the first part is the name of the file, and the second part is how the file should be opened.
The 'encoding' parameter tells Python which character encoding is used in the file.
In this case, 'utf-8-sig' is used, which is a way of encoding that includes a special marker at the beginning of the file.

Once the file is opened, the returned file object is assigned to the variable 'f'. The file object can then be used to read data from the file using various methods.

.. code-block:: console

    rows = list(csv.DictReader(f))


The 'DictReader' function reads a CSV file and returns a special type of list that is made up of dictionaries. Each dictionary represents a row in the CSV file, where the keys are the column headers and the values are the corresponding values in each row.
This makes it easy to access the data by column name instead of position.
In this code, the 'f' variable represents the CSV file that was opened earlier, and the resulting list of dictionaries is stored in the 'rows' variable for later use.

.. code-block:: console

    players = subsession.get_players()

The line retrieves a list of all the players in the current subsession.
This is done by calling the "get_players()" method on the subsession object, which returns a list of player objects.

.. code-block:: console

    for i in range(len(players)):
        row = rows[i]
        player = players[i]
        player.Income = int(row['Income'])
        player.Corruption = int(row['Corruption'])
        player.Jobless = int(row['Jobless'])
        player.FireAffected = int(row['FireAffected'])
        player.participant.CSVIncome = int(row['Income'])
        player.participant.CSVCorruption = int(row['Corruption'])
        player.participant.CSVJobless = int(row['Jobless'])
        player.participant.CSVFireAffected = int(row['FireAffected'])


The function then gets a list of players from the "subsession" object and loops over them.
For each player, it retrieves the corresponding row of data from the list of dictionaries and sets the player's attributes (Income, Corruption, Jobless, FireAffected) to the values from the row.
The function also sets the values from the row to these participant values (CSVIncome, CSVCorruption, CSVJobless, CSVFireAffected).