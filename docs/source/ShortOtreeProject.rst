======================
oTree Project
======================

An oTree project consists of various components such as settings.py, apps, models, views, templates, static files, participants, and sessions.
Each app is a component of the oTree project that represents a task or game for participants to complete.
The init.py file is a backend script that includes pre-built oTree functions and allows for custom classes and functions.
BaseConstants is a class used to define constants for the game, and oTree apps have three data models - Subsession, Group, and Player.

Click on this :ref:`otree-project` for a detailed description for the whole page.

Conceptual overview
=====================

Session
_______________________
This section provides a conceptual overview of important concepts in oTree, including the session, subsession, and page. The session is a fundamental concept that allows for randomization and data aggregation, and can be customized with various settings.
Each session is composed of one or more subsessions, which represent the behavior of a group of players in a single round of the game, and can be designed to allow for different treatments or conditions.

Subsession
_____________________
Subsessions represent the behavior of a group of players in a single round and allow for randomization of treatments and conditions across groups and rounds.

.. image:: docs/source/_static/Overview_1_vers2.png
  :width: 400

Page
___________________
Pages in oTree are the building blocks of an experiment and represent a single web page presented to the participant.
They can include different types of content and define the structure of the experiment.
Pages can also be customized with various settings, such as the timer length and display of a progress bar.
A page is a part of the subsession.

.. image:: docs/source/_static/Overview_1.png
  :width: 400

Object hierarchy
=====================
In oTree, the different components are organized into a hierarchy.

.. image:: docs/source/_static/Object_hierarchy.png
  :width: 400


- At the top is a session, which is made up of subsessions
- Each subsession contains multiple groups
- Each group contains multiple players
- Each player goes through several pages

You can access any higher-level component from a lower-level component:

For example, you can access a player's group or subsession, and a group's session or subsession.

.. code-block:: console

    player.participant
    player.group
    player.subsession
    player.session
    group.subsession
    group.session
    subsession.session

Group
_____________________
This class represents a group of players within a single round of the game.
It can be used to track information that is specific to the group as a whole, such as the group's score or the decisions that the group makes.

Player
______________________
This class represents an individual participant within a group in a single round of the game.
It can be used to track information that is specific to the player, such as their decisions or their earnings.

Fields
=================
In an oTree project, a "field" refers to a data attribute associated with a model class.
There are different types of fields that can be used in oTree models, such as:

+----------------------------+--------------------------------+
| Element                    |      Description               |
+============================+================================+
| IntegerField               |      for integer values        |
+----------------------------+--------------------------------+
| FloatField                 |      for decimal numbers       |
+----------------------------+--------------------------------+
| BooleanField               |      for true/false values     |
+----------------------------+--------------------------------+
| CurrencyField              |      for monetary values       |
+----------------------------+--------------------------------+
| StringField                |      for text strings          |
+----------------------------+--------------------------------+
