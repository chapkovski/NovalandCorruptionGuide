===================================================================
Saving and Using Participant Input in oTree
===================================================================

To save the input from participants and use it later in an oTree app, you can create fields in the app's database models. For example, to store a variable for a single participant, you can define fields in the Player class like this:

.. code-block:: console

    class Player(BasePlayer):
        ParticipantName = models.StringField()
        ParticipantAge = models.IntegerField()

In this case, the ParticipantName field can store text variables for the participant, while the ParticipantAge field can store integer variables or whole numbers.

.. _form-fields:

Formfields
_________________________________
To retrieve saved variables on other pages of the app, use the Player variable.
To define form inputs and specify their appearance, use the formfields function provided by oTree.
With this function, you can customize the form and appearance of field objects.

We provide an example below:

.. code-block:: console

    NewVariable = models.StringField(
            choices=[["Male", "M"], ["Female", "F"], ["Non-Binary", "NB"]],
                    label="Please select your gender",
                    widget=widgets.RadioSelect
            )


This code defines a StringField that represents a string value, with choices that are displayed to the participant using a RadioSelect widget.
The label argument specifies the text displayed next to the input field on the participant's page.
The choices argument is a list of sub-lists where each sub-list contains two elements: a string to be displayed to the participant and a string value that represents the selected choice.

The general representation is as follows:

.. code-block:: console

    NewVariable = models.StringField(
        choices=[["ParticipantView1", "BackendValue1"], ["ParticipantView2", "BackendValue2"]],
                label="This is the title or the description of the input from the participant",
                widget=widgets.RadioSelect
        )


This defines a StringField with two available choices represented by the strings "ParticipantView1" and "ParticipantView2", corresponding to backend values "BackendValue1" and "BackendValue2", respectively.
The label argument provides the text displayed next to the input field on the participant's page."
