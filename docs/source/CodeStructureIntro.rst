Part 1: The Introduction App
=============================
This section explains the intro app, which contains the introductory questionnaire. Here, participants were welcomed, asked for their consent to participate, informed about the procedure of the study, and asked basic socio-demographic questions.

Basic functionality
--------------------
The first part of the questionnaire is also the simplest one code-wise.
In the __init__.py file, the variables used to store participants' data in are defined, along with the content and data processing of the pages that appear in the app.

Global settings
^^^^^^^^^^^^^^^^^^^^^
The global settings of the project are defined in the class called :code:`C`, which contains constants that define some fundamental characteristics of the app and the variables used to store participants' data. The :code:`C` class contains the following variables:

.. code-block:: python
    :linenos:

    class C(BaseConstants):
        NAME_IN_URL = 'introduction' #displayed name for participants
        PLAYERS_PER_GROUP = None # no interactions between participants
        NUM_ROUNDS = 1 # only one round, no repeated functionality

        ENDOWMENT = cu(1000) # not relevant for this study


Variables for data storage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The variables used to store participants' data are defined in the class called :code:`Player`. It contains the variables that are used to store the data collected from the participants during the questionnaire. The variables are defined as fields of the class, and each field has a specific type and properties. There are three sets of variables defined here: First, some variables that collect data on the devices that participants used to complete the questionnaire. These variables are defined in the :code:`user_agent_XXX` block. The second block contains the variables that are used to collect the participants' consent to participate in the study and their agreement on the comprehension questions with their filter function. The third block contains the socio-demographic variables that were asked for in the questionnaire.

.. dropdown:: Intro App Variables
   :icon: terminal

   .. code-block:: python


    class Player(BasePlayer):
        # Set of variables that collect data on the devices that participants used to complete the questionnaire
        user_agent_browser = models.StringField()
        user_agent_browser_version = models.StringField()
        user_agent_os = models.StringField()
        user_agent_os_version = models.StringField()
        user_agent_device = models.StringField()
        user_agent_is_bot = models.BooleanField()
        user_agent_is_mobile = models.BooleanField()
        user_agent_is_tablet = models.BooleanField()
        user_agent_is_pc = models.BooleanField()


        # Set of variables that collect the participants' consent to participate in the study and with the comprehension questions
        data_consent = models.IntegerField(choices=[[1, "Ich bin damit einverstanden, an der aktuellen Studie "
                                                        "teilzunehmen. Mir ist bewusst, dass ich meine Zustimmung zur "
                                                        "Teilnahme jederzeit widerrufen kann und dass ich mit meiner "
                                                        "Zustimmung nicht auf meine gesetzlichen Rechte verzichte."]],
                                           widget=widgets.RadioSelect,
                                           label="")
        comprehension_consent = models.IntegerField(choices=[[1, "Ich habe verstanden, dass ich die Studie nur "
                                                                 "abschließen kann, wenn ich die Verständnisfragen "
                                                                 "korrekt beantworte."]],
                                                    widget=widgets.RadioSelect,
                                                    label="")

        # Set of socio-demographic variables
        age = models.IntegerField(label="In welchem Jahr sind Sie geboren (z. B. 1987)?<br>"
                                       "Geben Sie bitte Ihr Geburtsjahr an:",
                                    min=1924, max=2024)
        edu = models.IntegerField(label="Wie viele Jahre haben Sie insgesamt eine Schule besucht, "
                                        "inklusive des etwaigen Besuchs einer Berufsschule oder Hochschule? "
                                        "Berücksichtigen Sie bitte alle Voll- und Teilzeitausbildungen, "
                                        "und rechnen Sie die Gesamtdauer Ihrer Schul- bzw. Ausbildungszeit "
                                        "in ganze Jahre um.",
                                  min=0, max=99)
        gndr = models.IntegerField(label="Sie sind...",
                                    choices=[[1, "Männlich"],
                                             [2, "Weiblich"],
                                             [3, "Divers"]],
                                    widget=widgets.RadioSelect)




HTML files
^^^^^^^^^^^^
In the HTML files, the layout and design of each page is defined. For a more detailed documentation on the structure of the HTML files, please refer to the :ref:`html` section. Here, only the specific content of the HTML files is described. Each page of the questionnaire has its own HTML file, which is used to define the layout and design of that specific page. The HTML files are named according to the pages they represent, and they are included in the app's code using the :code:`page_sequence` variable at the very bottom of the intro app's init file.

The pages of the intro questionnaire
-------------------------------------
Here, the pages of the intro questionnaire are described in the order in which they appear in the questionnaire.

Welcome page
^^^^^^^^^^^^^^
The welcome page consists of text only and has no special functionality to participants other than being informed about the general purpose of the study and the contact information of the researchers.
In the background, several variables are processed to identify participants' browser, operating system, and device type. This information is used to ensure that the questionnaire is displayed correctly on the participants' devices and to collect data on the devices used by the participants. The data is stored in the :code:`user_agent_XXX` variables defined in the :code:`Player` class.

.. dropdown:: Welcome Page
   :icon: terminal

   .. code-block:: python


    class Welcome(Page):
        def get(self, *args, **kwargs):
            user_agent_string = self.request.headers.get('User-Agent')
            user_agent = parse(user_agent_string)

            res = {
                'browser': user_agent.browser.family,
                'browser_version': user_agent.browser.version_string,
                'os': user_agent.os.family,
                'os_version': user_agent.os.version_string,
                'device': user_agent.device.family,
                'is_mobile': user_agent.is_mobile,
                'is_tablet': user_agent.is_tablet,
                'is_pc': user_agent.is_pc,
                'is_bot': user_agent.is_bot
            }
            for k,v in res.items():
                try:
                    self.player.__setattr__(f'user_agent_{k}', v)
                except AttributeError:
                    print(f"{f'user_agent_{k}'} not found in player model")
            return super().get(*args, **kwargs)

Data protection page
^^^^^^^^^^^^^^^^^^^^
Participants are informed about the data protection regulations and their rights as participants in the study. This page is also used to collect the participants' consent to participate in the study and to process the data. The consent variable is stored in the :code:`data_consent` variable.

Comprehension info page
^^^^^^^^^^^^^^^^^^^^^^^
Participants are informed about the comprehension questions that will be asked within the questionnaire. They are also informed that they need to answer these questions correctly in order to complete the questionnaire. This page is also used to collect the participants' agreement on the comprehension questions. The comprehension variable is stored in the :code:`comprehension_consent` variable.

Socio-demographic questions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
On the pages 'Age', 'Gender', and 'Edu', participants are asked some socio-demographic questions. The answers to these questions are stored in the :code:`age`, :code:`gndr`, and :code:`edu` variables.

Study Layout page
^^^^^^^^^^^^^^^^^
This page is used to inform participants about the layout of the study. No data was processed here.
