The Stay in Novaland
=====================================
The central part of this oTree project is the :code:`main` app. This app contains the part of the project which revolve around participants' stay in Novaland. It begins with some pages that provide information about Novaland and then the virtual stay in this fictional country. Thus, the main app contains the vignettes and experimental treatments that are presented in the paper. Please note that we also simulated the national election of Novaland, in which participants could vote for one of two parties. This election is not included in this app, but is instead implemented in the :code:`election` app.

Basic functionality
--------------------


Global settings
^^^^^^^^^^^^^^^^^^^^^
The global settings of the project are defined in the class called :code:`C`, which contains some constant variables that define some fundamental characteristics of the app:


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

        # Define the outcomes combinations
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

        # Error handling
        assert len(smokescreens) == len(SMOKESCREEN_ROUNDS), "Number of smokescreens and smokescreen rounds must be equal"
        assert len(vignettes) == len(outcomes_combinations_collection[0]), "Number of vignettes and outcomes must be equal"
        assert len(vignettes) == len(VIGNETTE_ROUNDS), "Number of vignettes and vignette rounds must be equal"

        # Define the service levels
        service_levels = ['pos', 'corr', 'neg']  # service levels

        # Define the number of rounds for later referencing
        NUM_ROUNDS = len(vignettes) + len(smokescreens) + 1

        # Define the days for the web page headers
        NUM_DAYS = NUM_ROUNDS + 2
        scenario_headers = ["Samstag", "Sonntag", "Montag", "Dienstag", "Mittwoch", "Donnerstag", "Freitag"]

        ENDOWMENT = cu(1000) # not used in this study

Variables for data storage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Additional code for extended background functionality
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
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

HTML files
^^^^^^^^^^^^
In the HTML files, the layout and design of each page is defined. For a more detailed documentation on the structure of the HTML files, please refer to the :doc:`HTML Pages <HtmlPages>` section. Here, only the specific content of the HTML files is described. Each page of the questionnaire has its own HTML file, which is used to define the layout and design of that specific page. The HTML files are named according to the pages they represent, and they are included in the app's code using the :code:`page_sequence` variable at the very bottom of the intro app's init file.

The pages of the main app
-------------------------------------
Here, the pages of the intro questionnaire are described in the order in which they appear in the questionnaire.
