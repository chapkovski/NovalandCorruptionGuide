Structure of the Novaland stay pages
======================================
After receiving the information and passing the comprehension checks in the introductory part, participants began their stay in Novaland. For an overview of the fictional stay, please refer to the :ref:`flowchart of the Novaland stay <flowchart>`.

Before we discuss how the experience on each page was created, we first have to take a step back and talk about the underlying logic of the main app. While participants experience their fictional stay as one continuous trip, the app does not run only once. Instead, oTree runs the :code:`main` app seven rounds. The flow of the Novaland stay is created by defining for each page in which round it is displayed.
The different rounds of the main app contain the following pages:

#. Round: NovalandIntro Pages, Comprehension Page, IntroVignette Page, First Smokescreen Page
#. Round: Quiz Page, Quiz Results Page
#. Round: First Vignette Page, First Vignette Results Page
#. Round: Second Vignette Page, Second Vignette Results Page
#. Round: Second Smokescreen Page
#. Round: Third Vignette Page, Third Vignette Results Page
#. Round: Fourth Vignette Page, Fourth Vignette Results Page, Pollster Pages, Pollster Results Page


**Order of the smokescreens and vignettes**

This order is defined in the :code:`timeline` variable in :code:`creating_session` function. By using the list objects for the order of the smokescreens and vignettes, we can define the order in which they are displayed to the participants. Each entry of the list contains a string that identifies the corresponding vignette or smokescreen scenario. This part of the :code:`creating_session` is executed only once at the beginning of the first round. We will discuss the functionality of these pages in more detail below.

.. code-block:: python
    :linenos:

    p.vars['timeline'] = (
            [p.vars['smokescreen_order'][0]] +  # note that the quiz page comes after the first smokescreen
            [p.vars['quiz_page'][0]] +  # quiz page
            p.vars['vignette_order'][0:2] +  # the first two vignettes
            [p.vars['smokescreen_order'][1]] +  # the second smokescreen
            p.vars['vignette_order'][2:4]  # the last two vignettes
    )

The second part of the :code:`creating_session` function is executed at the beginning of each round. Here, we access the order of vignettes, smokescreens, and dependent variables for each player. This is done by accessing the :code:`Participant` object and storing the name of the current scenario in the :code:`current_scenario` variable. This variable is then used to determine which page should be displayed to the participant in the current round. This process allows us to only have one page for the smokescreens and one for the vignettes, which are then dynamically filled with the content of the current scenario. This is done by using the :code:`current_scenario` variable in the :code:`vars_for_template` method of the respective page class.