.. _otree-project:

======================
oTree Project
======================

An oTree project consists of several files and components, and this chapter provides a basic explanation of the most essential ones.
The purpose of this chapter is to provide a surface-level overview of all the components of oTree and briefly explain them.
This chapter does not have to be fully understood but rather serves as a reference for readers to look up if questions arise during programming or while reading this guide.

An oTree project consists of several components, including:

    **The settings.py file**

    This file contains various configuration settings for the oTree project, including the PARTICIPANT_FIELDS and SESSION_CONFIGS.

    **Apps**

    Apps are the individual components of an oTree project that define the different parts of the experiment. Each application has its own set of models, views, and templates.

    **Models**

    Models are Python classes that define the data structure for the application and store the data in a database.

    **Views**

    Views define the logic for how data is displayed and processed in the application.

    **Templates**

    Templates define the HTML files used to render the pages of the application.

    **Static files**

    These are any additional files required for the application, such as images, JavaScript files, and CSS stylesheets.

    **Participants**

    Participants are the individuals who participate in the experiment. The information about each participant is stored in the database, and can be accessed and used throughout the project.

    **Sessions**

    A session is a single instance of an oTree experiment. Participants are grouped into sessions, and each session has its own set of parameters, such as the number of participants and the timing of events.

Apps
====================
An oTree app is a component of an oTree project, and represents a single task or game that participants can complete in the experiment.
An oTree app typically consists of a series of pages, each of which presents a task or question for the participant to answer, as well as models and views that define the behavior and logic of the app.
The app may also include custom templates and CSS styles to control the appearance of the pages and elements, as well as custom JavaScript code to handle user interactions and events.
The structure and behavior of an oTree app is defined using the Python programming language, and the platform provides a set of tools and libraries to make it easy to create and manage the components of the app.


init.py
==============================
The __init__.py file in oTree is a Python script that includes some pre-built oTree functions.
Additionally, new classes and functions can be defined there, which can be accessed throughout the app.
It serves as the fundamental script for the backend, where everything comes together and most of the work is done.

BaseConstants
____________________
This class is used to define the constants that are used throughout the game.
These can include things like the number of rounds, the number of players per group, or the payoffs for each action.

Models
==============
An oTree app has 3 data models: Subsession, Group, and Player.
A player is part of a group, which is part of a subsession.

Conceptual overview
=====================

Session
_______________________
The session in oTree is a fundamental concept, as it allows for the randomization of treatments and conditions across groups and rounds, as well as for the aggregation of data across participants.
The session data can be accessed and analyzed using various built-in functions and methods, and can be exported for further analysis outside of oTree.
When designing an experiment in oTree, it is important to consider the structure of the session, including the number of participants, the number of groups, and the number of rounds.
The session can also be customized with various settings, such as the length of rounds, the payment scheme, and the language of the instructions.

Subsession
_____________________
Each session is composed of one or more subsessions, which represent the behavior of a group of players in a single round of the game.
The subsession is responsible for creating the groups of players and setting the rules for how they interact with each other.
By breaking the session into subsessions, the experiment can be designed to allow for different treatments or conditions to be randomised across groups and rounds.

.. image:: docs/source/_static/Overview_1_vers2.png
  :width: 400

Page
___________________
Pages in oTree are the building blocks of an experiment and represent a single web page presented to the participant.
They can include different types of content and define the structure of the experiment.
Pages can also be customized with various settings, such as the timer length and display of a progress bar.

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
