======================
Dynamic Text
======================

In Novaland, it is crucial that the majority of the texts are displayed dynamically based on inputs, decisions, and events within Novaland.
Therefore, in this chapter, we will go through how to dynamically display text in the frontend based on the backend.

Basic if function in all areas
-------------------------------

This chapter heavily relies on the if function, so we will first explain the basic structure of the if function.
In the init.py file, various frontend specifications can be made based on the input of the participating individuals using the if function.
The basic structure of the if function is as follows:

.. code-block:: console

    HTML in oTree using Django Example:
    {% if condition %}
        Text to be displayed.
    {% endif %}

    Python:
    if condition:
        # code to execute if condition is true

As an example, a participants answer to a survey item on their gender can be used to change text in several places based on that choice.

.. code-block:: console

    What is <strong> {% if player.Gender == "Female" %}she {% elif player.Gender == "Male" %}he {% else %}he {% endif %} </strong>doing?