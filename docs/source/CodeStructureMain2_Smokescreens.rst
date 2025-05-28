The smokescreen pages
=============================
The smokescreen pages are designed to immerse participants more into the virtual country and to reduce demand effects by distracting participants from the main purpose of the questionnaire,. They are presented as everyday situations that participants might encounter in Novaland. The smokescreens are displayed in the first and fifth rounds of the main app. The first smokescreen is displayed after the introduction to Novaland, while the second smokescreen is displayed after the second vignette.

The scenarios of the smokescreens contain arbitrary situations that are not directly related to the main purpose of the questionnaire. One smokescreen is about adopting a pet, while the other smokescreen is about visiting a restaurant. The order of these two scenarios was randomized for each participant.

Let's have a detailed look into the code of the smokescreen pages - how they are created, how they are randomized and how they are displayed to the participants.

In the :code:`C` section, the rounds in which the smokescreens are displayed are defined in :code:`SMOKESCREEN_ROUNDS = [1, 5]`.
The smokescreens are then defined in the :code:`smokescreens` variable as a list of strings: :code:`smokescreens = ['pets', 'restaurant']`. The first smokescreen is called "pets" and the second smokescreen is called "restaurant".
In the :code:`creating_session` function, the order of the smokescreens is randomized for each participant. This is done by using the :code:`smokescreens` object and then shuffling it using the :code:`random.shuffle()` function. The randomized order is then stored in the :code:`Participant` object as :code:`p.vars['smokescreen_order']`.

The code for the smokescreen pages is implemented in the :code:`Smokescreen` page class:

.. code-block:: python
    :linenos:

    class Smokescreen(Page):
        form_model = 'player'

        @staticmethod
        def is_displayed(player):
            return player.round_number in C.SMOKESCREEN_ROUNDS

        def get_form_fields(player):
            return [player.current_scenario]

        @staticmethod
        def vars_for_template(player: Player):
            return {
                'smokescreen': f'main/smokescreens/{player.current_scenario}.html',
                'smokescreen_header': C.scenario_headers[player.round_number - 1]

The :code:`is_displayed` method is used to determine whether the page should be displayed to the participant. This is done by checking whether the current round number is in the list of smokescreen rounds defined in the :code:`C` class.

The :code:`get_form_fields` method is used to define the form fields that are displayed on the page. In this case, it returns the current scenario of the participant, which is stored in the :code:`current_scenario` variable. This variable is set in the :code:`creating_session` function and contains the name of the current smokescreen scenario for the participant. This dynamic variable function is used to ensure that the correct smokescreen scenario is displayed to the participant. It is required because there is only one HTML template for both smokescreens, which is dynamically filled with the content of the current smokescreen scenario.

The :code:`vars_for_template` method is used to pass variables to the HTML template. In this case, it passes the path to the HTML template of the current smokescreen scenario and the header for the smokescreen page. This is used because the HTML template for the smokescreen pages is the same for both smokescreens and the content is dynamically filled with the current smokescreen scenario.

The path to the HTML template is constructed using the :code:`main/smokescreens/` folder and the name of the current scenario, as this folder contains the two HTML files (:code:`pets.html` and :code:`restaurant.html`) that define the text of the smokescreen pages.
The header of the page is defined in the :code:`scenario_headers` variable in the :code:`C` class and contains the name of the day on which the smokescreen is displayed. This dynamic variable function is used to ensure that the correct header is displayed on the page, depending on the round number in which the smokescreen is displayed. It contains the number of the day in Novaland and the current day of the week.

The participants were asked to answer a question about the smokescreen scenario - either to adopt a pet or to choose the restaurant they want to visit. Again, we used a dynamic variable function to ensure that the correct question is displayed to the participant. The question object were defined in the :code:`Player` model as :code:`pets` and :code:`restaurant`, respectively. The question is then displayed in the HTML template using the :code:`{{ formfields }}` variable, which is defined in the :code:`get_form_fields` method of the :code:`Smokescreen` page class.

With this setup, we ensure that the smokescreen pages are displayed correctly to the participants - at the correct time, and with the correct content according to the randomized order of the smokescreens.


.. _outcome_pages:
The Outcome Page
-----------------------------

After the participants submitted the smokescreen page, they were redirected to the :code:`EndOfDay` page. This page is used to inform participants about the outcome of the smokescreen scenario they just experienced. They are dynamically filled with the content of the current scenario and adjusted to the decision that participants made in that scenario. They were not only used after the smokescreen pages, but also after the vignette pages. They were implemented to increase participants' immersion and feeling of self-efficacy.

The code for the :code:`EndOfDay` page is implemented in the :code:`EndOfDay` page class:

XXXXXXXXXXXXXXXXXXXXXXXXXXXX