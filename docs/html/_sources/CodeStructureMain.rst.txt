Part 2: The Stay in Novaland
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

This means that if we want to define how the app behaves across the entirety of this app, we need to store them in the :code:`Participant` object. This is done in the :code:`creating_session` function, a part of which is called once for each participant when they first enter the app (after completing the intro questionnaire). In this function, we create a random order of vignettes, outcomes of the vignettes, and smokescreens for each participant. This is done by creating a list for each possible orders of those, and then randomly assigning one entry from that list to each participant.


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

The main app is structured in **two distinct parts**: First participants received information about Novaland and are asked some questions about this. The then began their actual stay in the country, where they were presented with various situations and are asked to answer questions about them. The following sections describe these two parts in detail.


.. toctree::
    :maxdepth: 1
    :caption: Main App Pages

    CodeStructureMain1_Intro
    CodeStructureMain1.5_Background
    CodeStructureMain2_Smokescreens
    CodeStructureMain3_Quiz
    CodeStructureMain4_Vignettes
    CodeStructureMain5_Pollster
    CodeStructureMain6_AdditionalCode