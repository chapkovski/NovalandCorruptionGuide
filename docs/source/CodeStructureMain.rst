The Stay in Novaland
=====================================
The central part of this oTree project is the :code:`main` app. This app contains the part of the project which revolve around participants' stay in Novaland. It begins with some pages that provide information about Novaland and then the virtual stay in this fictional country. Thus, the main app contains the vignettes and experimental treatments that are presented in the paper. Please note that we also simulated the national election of Novaland, in which participants could vote for one of two parties. This election is not included in this app, but is instead implemented in the :code:`election` app.

Basic functionality
--------------------


Global settings
^^^^^^^^^^^^^^^^^^^^^
The global settings of the project are defined in the :code:`C` class, which contains constant variables that define the fundamental functionality and variables of the app.


.. dropdown:: Main App Global Settings
   :icon: terminal

   .. code-block:: python

    class C(BaseConstants):
        # read the file containing the vignette questionnaire
        file_path = 'data/vignette_q.yaml'
        with open(file_path, 'r', encoding="utf-8") as yaml_file:
            yaml_template = yaml_file.read()
        # read the file containing the text for the corrupt endings
        with open('data/corrupt_endings.yaml', 'r', encoding="utf-8") as file:
            CORRUPT_ENDINGS = yaml.safe_load(file)

        NAME_IN_URL = 'main' # displayed name for participants in URL
        PLAYERS_PER_GROUP = None # no interactions between participants
        AVERAGE_NOVA_INCOME = 3000 # average income in Novaland
        SMOKESCREEN_ROUNDS = [1, 5, ] # app rounds that contain smokescreens
        VIGNETTE_ROUNDS = [3, 4, 6, 7] # app rounds that contain vignettes
        BRIBE_THRESHOLD = 300 # not used in this study

        vignettes = ['doctor', 'handyman', 'kindergarten', 'passport']  # names of the vignettes
        POS = "pos" # Represents a positive outcome
        NEG = "neg" # Represents a negative outcome
        CORR = "corr"  # Represents a corrupt outcome

        # Define the outcomes combinations to which participants were randomly assigned
        outcomes_combinations_collection = [
            [POS, POS, POS, POS],
            [POS, POS, POS, NEG],
            [POS, POS, POS, CORR],
            [POS, NEG, NEG, NEG],
            [POS, CORR, CORR, CORR],
            [POS, POS, NEG, CORR],
            [POS, POS, NEG, CORR]
        ]

        smokescreens = ['pets', 'restaurant']  # names of the smokescreens

        # Error handling, used in development
        assert len(smokescreens) == len(SMOKESCREEN_ROUNDS), "Number of smokescreens and smokescreen rounds must be equal"
        assert len(vignettes) == len(outcomes_combinations_collection[0]), "Number of vignettes and outcomes must be equal"
        assert len(vignettes) == len(VIGNETTE_ROUNDS), "Number of vignettes and vignette rounds must be equal"

        # Define the service levels for the vignettes
        service_levels = ['pos', 'corr', 'neg']

        # Define the number of rounds for later referencing
        NUM_ROUNDS = len(vignettes) + len(smokescreens) + 1

        # Define the days for the web page headers
        NUM_DAYS = NUM_ROUNDS + 2
        scenario_headers = ["Samstag", "Sonntag", "Montag", "Dienstag", "Mittwoch", "Donnerstag", "Freitag"]

        ENDOWMENT = cu(1000) # not used in this study

Random assignment to treatments and order of vignettes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The logic that the main app follows is a bit more complex than the other apps. Most importantly, the main app consists of numerous rounds. The issue here is that in oTree, the variables stored in the :code:`Player` object are reset at the beginning of each round.

This means that if we want to define how the app behaves across the entirety of this app, we need to store them in the :code:`Participant` object. This is done in the :code:`creating_session` function, which is called once for each participant when they first enter the app (after completing the intro questionnaire). In this function, we create a random order of vignettes, outcomes of the vignettes, and smokescreens for each participant. This is done by creating a list for each possible orders of those, and then randomly assigning one entry from that list to each participant.

XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX


.. dropdown:: Randomization in the main app
   :icon: terminal

   .. code-block:: python


    def creating_session(subsession: Subsession):
    # create random order of vignettes, smokescreens, and dependent variables once at the beginning of the session
    if subsession.round_number == 1:
        # calculate execution time
        start_time = time.time()
        cycle_collection = cycle(C.outcomes_combinations_collection)
        for p in subsession.session.get_participants():
            # randomize the order of vignettes
            vignettes = C.vignettes.copy()
            random.shuffle(vignettes)
            p.vars['vignette_order'] = vignettes

            # choose outcomes combination
            # outcomes_collection = C.outcomes_combinations_collection.copy()
            outcomes_collection = next(cycle_collection)

            # randomize the order of outcomes
            outcomes = outcomes_collection.copy()
            random.shuffle(outcomes)

            # create outcomes variable for uniform outcomes treatment sessions
            treatment = subsession.session.config.get('treatment')
            if subsession.session.config.get('treatment'):
                outcomes = [treatment] * len(outcomes)

            # create participant variables for vignettes and outcomes
            p.vars['outcomes_collection'] = outcomes_collection
            p.vars['vignette_outcomes'] = outcomes
            p.vars['vignettes_with_outcomes'] = OrderedDict(zip(vignettes, outcomes))

            # randomize the order of smokescreens
            smokescreens = C.smokescreens.copy()
            random.shuffle(smokescreens)
            p.vars['smokescreen_order'] = smokescreens

            # create variable for quiz page
            p.vars['quiz_page'] = ['quiz_page']

            # create timeline over the 9 days in Novaland at participant level
            p.vars['timeline'] = (
                    [p.vars['smokescreen_order'][0]] +  # note that the quiz page comes after the first smokescreen
                    [p.vars['quiz_page'][0]] +  # quiz page
                    p.vars['vignette_order'][0:2] +  # the first two vignettes
                    [p.vars['smokescreen_order'][1]] +  # the second smokescreen
                    p.vars['vignette_order'][2:4]  # the last two vignettes
            )

            # create variable on corruption info treatment
            corruption_info = random.choice([0, 1])
            p.vars['corruption_info'] = corruption_info

            # create variable for randomized order of pollster questions
            pollster_questions = ["pollster_question_gov_trust", "pollster_question_taxes_welfare",
                                  "pollster_question_perc_corruption"]
            pollster_order = random.sample(pollster_questions, 3)
            p.vars['pollster_order'] = pollster_order
        end_time = time.time()
        print(f'PARTIICPANT BLOCK Execution time: {end_time - start_time}')

    start_time = time.time()
    # access the order of vignettes, smokescreens, and dependent variables for each player (per round)
    for p in subsession.get_players():
        participant = p.participant
        p.dump_collection = json.dumps(participant.vars['outcomes_collection'])
        p.scenario_order = json.dumps(participant.vars['timeline'])
        p.current_scenario = participant.vars['timeline'][p.round_number - 1]
        if p.current_scenario in C.vignettes:
            p.vignette_order = json.dumps(participant.vars['vignette_order'])
            p.service_level = participant.vars['vignettes_with_outcomes'][p.current_scenario]
            # p.num_pos_vign_before = [p.field_maybe_none('service_level') for p in p.in_previous_rounds()].count('pos')
            # p.num_neg_vign_before = [p.field_maybe_none('service_level') for p in p.in_previous_rounds()].count('neg')
            # p.num_corr_vign_before = [p.field_maybe_none('service_level') for p in p.in_previous_rounds()].count('corr')
        p.pollster_order = json.dumps(participant.vars['pollster_order'])
    end_time = time.time()
    print(f'PLAYER BLOCK Execution time: {end_time - start_time}')



The pages of the main app
-------------------------------------
Here, the pages of the main questionnaire are described in the order in which they appear in the questionnaire. Participants lived through nine days in Novaland. The main app contains eight of those, while the last day is the voting day, which is implemented in the :code:`election` app.

The main app is structured in **two distinct parts**: First participants received information about Novaland and are asked some questions about this. The then began their actual stay in the country, where they were presented with various situations and are asked to answer questions about them.

1. The introduction to Novaland
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The introductory part of the main app consists of four **NovalandIntro** pages, on which participants are informed about their stay in Novaland as citizens of the fictional country and about some characteristics of the country itself. The text that was included on these pages can be found in the :code:`main/instructions/` folder. There is one file for each of these four information pages.

On the fourth NovalandIntro page, the first experimental treatment takes place. Participants were randomly assigned to two groups: the first group is informed about the social norm of bribing in Novaland (6 out of 10 citizens of Novaland are willing to pay a bribe for a service). Participants in the second group do not receive any information. The variable that determines if participants received this information is called :code:`corruption_info` and is stored in the :code:`Participant` object. The variable is set to 1 if participants received the information and 0 if they did not. This variable is used to determine which text is displayed on the page and is passed to the HTML template (:code:`main/instructions/instructions_4.html`). There, the conditional statement of showing the social norm information works with this variable: :code:`{{ if participant.vars.corruption_info == 1 }}`.

The next page is the **Comprehension** page. On this page, participants had to answer three comprehension questions correctly in order to continue with the questionnaire. The questions are about the information that was provided on the previous pages. The questions are defined in the player models :code:`comprehension_citizen`, :code:`comprehension_income`, and :code:`comprehension_financing`. These three questions are defined as :code:`form_fields` in the :code:`Comprehension` page class and called by the page's HTML template using :code:`{{formfields}}`.

We gave participants the possibility to receive the respective pieces of information again using an "instructions" button that was embedded on the page. For this, we used the :code:`live_method` in the init file and the following code in the HTML template to dynamically animate the window with the text from the :code:`main/instructions/` HTML files that were also used to display the information on the previous pages. The code runs as soon as participants click on the instructions button:

.. code-block:: html
    :linenos:

    <script>
        $(document).ready(function () {
            $('#instruction_button').click(function () {
                console.debug('button clicked');
                liveSend('button_clicked');
            });
        });
    </script>

Participants had two attempts to answer all comprehension questions correctly. If they tried to submit the page with incorrect answers, the :code:`error_counter` increases and an error message appears. The page is then reloaded. They are then informed that they have one attempt left. If they submit the page again with incorrect answers for the second time, they will be filtered out. We also set a timeout counter on this page to prevent participants from spending too much time on it. If they exceed the time limit, they were also filtered out. The timeout counter is set to five minutes, which is more than enough time to answer the questions.

If participants failed to answer the comprehension questions correctly two times, or if they exceeded the time limit, they were next redirected to the **TerminationPage**. This page is not shown to participants, but is only used for background functionality. Only participants who fulfill one of these two criteria are redirected to this page, the other ones skip it, as defined in

.. code-block:: python
    :linenos:

    @staticmethod
    def is_displayed(player: Player):
        return player.error_counter > 1 or player.comprehension_timeouted

As soon as participants reached this page, they were directly redirected using a link provided by the panel provider, defined in :code:`TERMINATION_LINK`. Participants were then informed that they failed to comply with the requirements of the study and were thus filtered out.

Those participants who answered all three comprehension questions correctly, reached the last page of the introduction to Novaland. On the **IntroVignette** page, participants were informed that they will encounter several situations on the next pages, that they are a citizen of Novaland in this context and that they should answer the questions as themselves.

2. Nine days in Novaland
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
After receiving all this information and passing the comprehension checks, participants began their stay in Novaland. For an overview of the fictional stay, please refer to the :ref:`flowchart of the novaland stay <flowchart>`.

Before we discuss how the experience on each page was created, we first have to take a step back and talk about the underlying logic of the main app. While participants experience their fictional stay as one continuous trip, the app does not run only once. Instead, it runs seven rounds. The flow of the Novaland stay is created by defining for each page in which round it is displayed. One special XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX


SMOKESCREEN_ROUNDS = [1, 5, ]
VIGNETTE_ROUNDS = [3, 4, 6, 7]

**Order of the smokescreens and vignettes**

.. code-block:: python
    :linenos:

    p.vars['timeline'] = (
            [p.vars['smokescreen_order'][0]] +  # note that the quiz page comes after the first smokescreen
            [p.vars['quiz_page'][0]] +  # quiz page
            p.vars['vignette_order'][0:2] +  # the first two vignettes
            [p.vars['smokescreen_order'][1]] +  # the second smokescreen
            p.vars['vignette_order'][2:4]  # the last two vignettes
    )

Here is what the timeline tells us:


Additional code for extended background functionality
--------------------------------------------------------
In addition to the code described above, there are some additional code snippets that are used to implement the extended background functionality of the questionnaire. These snippets are not directly related to the main functionality of the questionnaire, but they are used to enhance the user experience and provide additional features.

**Progress bar**

At the top of the main app's __init__.py file, the :code:`Page` class is defined. This is the parent class for all pages in the app. This means that the code that is defined within it is executed by all survey pages across the app.
This class is used to automatically retrieve the current page number and maximum page number of the questionnaire using the :code:`get_context_data` function. This function then calculates the progress percentage. The progress percentage is then passed to the HTML template, where it is displayed as a progress bar.


.. dropdown:: Progress bar calculations
   :icon: terminal

   .. code-block:: python


    class Page(oTreePage):
        instructions = False

        def get_context_data(self, **context):
            NUM_SURVEY_PAGES = 36
            app_name = self.__module__.split('.')[0]
            page_name = self.__class__.__name__
            if page_name != 'PostSurvey' and app_name == 'post':
                index_in_pages = self._index_in_pages + NUM_SURVEY_PAGES
            else:
                index_in_pages = self._index_in_pages
            r = super().get_context_data(**context)

            if 'post' in self.session.config.get('app_sequence'):
                max_pages = NUM_SURVEY_PAGES + self.participant._max_page_index
            else:
                max_pages = self.participant._max_page_index

            r['maxpages'] = max_pages
            r['page_index'] = self._index_in_pages
            r['progress'] = f'{int(index_in_pages / max_pages * 100):d}'

            r['instructions'] = self.instructions
            return r


**Animating the status bar** XXXXXXXXXXXXXXXXXXXXXXXX
