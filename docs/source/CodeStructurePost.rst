Part 4: The Post-Experimental Questionnaire
=============================================================
After participants ended their stay in the fictional country of Novaland, they were asked to complete a post-experimental questionnaire. This questionnaire included a variety of questions on participants' socio-demographic characteristics, their real-life attitudes, and their participation in the experiment. Additionally, participants were asked if they wanted to receive a digital certificate of their stay as a citizen of Novaland.

Pages of the Post-Experimental Questionnaire
-------------------------------------------------------------------------
The post-experimental questionnaire consists of only three pages:
1. **PostSurvey**: This page contains the entire questionnaire. Here, the questions were dynamically rendered using a SurveyJS survey model.
2. **ReceiveCert**: On this page, participants could download a digital certificate of their stay in Novaland. The certificate was generated as a PDF file.
3. **LastPage**: On this last page, we thanked the participants for their participation and provided them with a link to be redirected to the sample provider website.


The PostSurvey Page
^^^^^^^^^^^^^^^^^^^^^^^^^^
The PostSurvey page is the main page of the post-experimental questionnaire. It contains a dynamically rendered survey model that includes various questions about the participants' socio-demographic characteristics, their attitudes, and their experience in the experiment. The content of the survey is defined in a separate JavaScript file, which is loaded into the page.

In the __init__.py file of the :code:`post` app, we define the **PostSurvey** class, which mainly consists of four methods:

1. The :code:`live_method` method is used to handle the real-time data submission from the survey. This method is required as the survey is rendered dynamically and the data is sent to the server in real-time. If we used the standard procedure of submitting the survey data only after participants answered all questions on this page, the survey would not be able to handle the real-time data submission but rather save the data only after participants answered all questions on this page. Thus, using the :code:`live_method` method is a more efficient and safer way to handle the survey data submission.

.. dropdown:: PostSurvey live_method
   :icon: terminal

   .. code-block:: python

    @staticmethod
    def live_method(player, data):
        # let's do this: store the data also in participant vars as a dict.
        # before assying (process_survey_data) let's check if all keys are there. if not all, let's set missing keys in
        # players' level to '' (empty string) silently (via try/except)

        print(f'data received: {data}')
        old_data = player.participant.vars.get('post_survey', {})
        existing_keys = old_data.keys()
        new_keys = data.keys()
        copy_data = data.copy()
        print(data.keys())
        print(old_data.keys())
        print('***')
        for k in old_data.keys():
            if k not in new_keys:
                copy_data[k] = ''

        old_data.update(copy_data)
        player.participant.vars['post_survey'] = copy_data

        PostSurvey.process_survey_data(player, copy_data)

2. The :code:`process_survey_data` method processes the survey data collected by the :code:`live_method`. It is called at the end of the :code:`live_method` method to convert the data into the appropriate format and assign it to the corresponding fields in the Player model. It ensures that the data is correctly stored in the database. For this, it checks the type of each field in the Player model and converts the data accordingly. The values are then assigned to the Player model instance, e.g., with :code:`converted_value = int(value)` for integer fields, :code:`converted_value = str(value)` for string fields, and so on. This method is crucial for ensuring that the data is correctly stored. Additionally, there is a logging mechanism in place to log any errors that occur during the data processing. This helps in debugging and ensuring data integrity.

.. dropdown:: PostSurvey process_survey_data
   :icon: terminal

   .. code-block:: python

    @staticmethod
    def process_survey_data(player, survey_results):

        # Get the SQLAlchemy mapper for the Player model
        mapper = inspect(player.__class__)

        # Iterate over each key-value pair in the survey results
        for key, value in survey_results.items():
            logger.info(f'Processing {key}: {value}')
            try:
                # Check if the column exists in the model
                if key not in mapper.columns:
                    logger.warning(f'No such field: {key}')
                    continue

                # Get the column object
                column = mapper.columns[key]
                column_type = column.type

                # Determine the column type and convert the value accordingly
                if isinstance(column_type, (Integer, BigInteger, SmallInteger)):
                    # Handle integer fields
                    if isinstance(value, int):
                        converted_value = value
                    elif isinstance(value, str) and value.isdigit():
                        converted_value = int(value)
                    else:
                        # Attempt to convert to integer
                        converted_value = int(value)
                elif isinstance(column_type, (String, Text)):
                    # For string/text fields, ensure the value is a string
                    converted_value = str(value)
                elif isinstance(column_type, Boolean):
                    # Convert to boolean
                    if isinstance(value, bool):
                        converted_value = value
                    elif isinstance(value, str):
                        converted_value = value.lower() in ['true', '1', 'yes']
                    else:
                        converted_value = bool(value)
                elif isinstance(column_type, (Float, Numeric)):
                    # Handle float and numeric fields
                    converted_value = float(value)

                else:
                    # For other types, assign as-is or handle accordingly
                    converted_value = value

                # Assign the converted value to the model instance
                setattr(player, key, converted_value)
                logger.info(f'Successfully set {key} to {converted_value}')

            except ValueError as ve:
                logger.error(f'Value error for field "{key}": {value} - {ve}')
            except Exception as e:
                logger.error(f'Error setting field "{key}": {e}')

3. The :code:`get_form_fields` method returns a list of form fields that are required for the survey. This method is used to dynamically generate the form fields based on the survey definition. It is only used if the participant is not a browser bot (i.e., if the participant is a real user and not a bot). The method returns a list of field names that are required for the survey. This allows for flexibility in the survey design, as the fields can be dynamically generated based on the survey definition.

.. dropdown:: PostSurvey get_form_fields
   :icon: terminal

   .. code-block:: python

    def get_form_fields(player):
    if player.participant.is_browser_bot:
        r = ['nova_certificate_bin', 'nova_certificate_open', 'nova_dec', 'bribery_exp_pers', 'bribery_exp',
             'bribery_exp_smone', 'att_state_inequality', 'soc_trust', 'helpful', 'att_taxes_welfare',
             'att_incr_welfare', 'att_immigration', 'att_voting_intention', 'att_voting_intention_open',
             'soc_gender', 'soc_birthyear', 'soc_marital_status', 'soc_hhsize', 'soc_children', 'soc_employment',
             'soc_employment_open', 'soc_pers_income', 'soc_education', 'soc_job_education', 'att_leftright',
             'soc_postalcode', 'soc_citizenship', 'soc_born_germany', 'soc_parents_born_germany',
             'svx_participation_location', 'svx_participation_device', 'svx_interest', 'svx_difficulty',
             'svx_privacy', 'svx_technical_problems_bin', 'svx_technical_problems_open', 'svx_gaming',
             'svx_purpose', 'svx_final_comments']

        return r

4. The :code:`post` method is called when the survey is submitted. It processes the survey data and calls the :code:`process_survey_data` method to handle the data submission. This method is responsible for handling the survey submission and ensuring that the data is correctly processed and stored in the database. It also handles any errors that may occur during the submission process, such as invalid JSON data or unexpected errors.

.. dropdown:: PostSurvey post
   :icon: terminal

   .. code-block:: python

    def post(self):
        if self.participant.is_browser_bot:
            return super().post()

        # Assuming self._form_data contains 'surveyResults'
        try:
            # Parse the JSON data from the survey
            survey_results = json.loads(self._form_data.get('surveyResults'))
            pprint(survey_results)  # For debugging purposes
            PostSurvey.process_survey_data(self.player, survey_results)

        except json.JSONDecodeError as e:
            logger.error(f'Invalid JSON data: {e}')
        except Exception as e:
            logger.error(f'Unexpected error: {e}')

            # Proceed with the superclass's post method
        return super().post()

5. As always in this oTree project, the :code:`form_model` attribute is set to :code:`player`, indicating that the form fields are associated with the Player model.

Now that the PostSurvey page is defined, we will have a look at the **HTML template** that is used to render the survey. The template includes the necessary JavaScript and CSS files to render the survey dynamically. It also includes a hidden input field to store the survey results, which will be submitted when the survey is completed.

On the HTML template, the PostSurvey page uses the SurveyJS library to render the survey dynamically. The survey definition is loaded from a separate JavaScript file, which contains the survey questions and their properties. The template also includes a progress bar that shows the progress of the survey as participants answer the questions. The progress bar is updated in real-time as participants navigate through the survey pages. The template is more complex than the previous pages, as it includes JavaScript code to handle the survey rendering, data submission, and progress bar updates. The JavaScript code is responsible for initializing the SurveyJS model, loading the survey data from the local storage of participants' browsers, and handling the survey completion event.
Overall, the template handles the rendering of the data. The content of the survey is defined in the :code:`survey_definition.js` file, which is loaded into the template. The survey definition includes the questions, their types, and other properties. Thus, it is not necessary to understand the details of HTML and Javascript code in order to understand how the PostSurvey page works. Thus, the code following code from the :code:` PostSurvey` template will not be explained in detail. We annotated the functionality of the most important parts of the code below.

.. dropdown:: PostSurvey HTML Template
   :icon: terminal

   .. code-block:: html

        // The first section loads the necessary JavaScript libraries and stylesheets for SurveyJS
        <script src="https://cdnjs.cloudflare.com/ajax/libs/showdown/1.6.4/showdown.min.js"></script>
        <link rel="stylesheet" href="{{static 'surveyjs/defaultV2.min.css'}}"/>
        <link rel="stylesheet" href="{{static 'Vignette.css'}}"/>

        <script src="{{static 'surveyjs/survey.jquery.min.js'}}"></script>

        <script src="{{static 'surveyjs/index.min.js'}}"></script>
        <script src="{% static 'surveyjs/survey_definition.js' %}"></script>

        // The second section defines the SurveyJS model and handles the survey rendering and data submission
        <input type="hidden" id="surveyResults" name="surveyResults">
        <div class="content-box">

            <div id="surveyElement" name="surveyData"></div>

            <script>
                // Define a unique key for the survey, incorporating the participant's code
                const SURVEY_STORAGE_KEY = `surveyData{{participant.code}}`; // Ensure {{participant.code}} is correctly rendered server-side

                // Flag to ensure restoration happens only once
                let isRestored = false;

                // Function to check if localStorage is available (ensuring that the browser supports the dynamic questionnaire)
                function isLocalStorageAvailable() {
                    try {
                        const testKey = '__test__';
                        localStorage.setItem(testKey, testKey);
                        localStorage.removeItem(testKey);
                        return true;
                    } catch (e) {
                        return false;
                    }
                }

                // Function to save survey data and current page to localStorage (participants' browsers)
                function saveSurveyData(surveyData, currentPageName) {
                    if (!isLocalStorageAvailable()) {
                        console.warn("LocalStorage is not available. Data will not be saved.");
                        return;
                    }

                    try {
                        const combinedData = {
                            data: surveyData,
                            currentPage: currentPageName
                        };
                        localStorage.setItem(SURVEY_STORAGE_KEY, JSON.stringify(combinedData));
                        console.log("Survey data and current page saved to localStorage.");
                    } catch (error) {
                        console.error("Error saving survey data to localStorage:", error);
                    }
                }

                // Function to load survey data and current page from localStorage
                function loadSurveyData() {
                    if (!isLocalStorageAvailable()) {
                        console.warn("LocalStorage is not available. Cannot load saved data.");
                        return {
                            data: {},
                            currentPage: null
                        };
                    }

                    const savedData = localStorage.getItem(SURVEY_STORAGE_KEY);
                    if (savedData) {
                        try {
                            const parsedData = JSON.parse(savedData);
                            return {
                                data: parsedData.data || {},
                                currentPage: parsedData.currentPage || null
                            };
                        } catch (error) {
                            console.error("Error parsing survey data from localStorage:", error);
                            return {
                                data: {},
                                currentPage: null
                            };
                        }
                    }
                    return {
                        data: {},
                        currentPage: null
                    };
                }

                // Function to restore the current page of the survey from localStorage
                function restoreCurrentPage(savedPageName) {
                    if (savedPageName) {
                        const page = survey.getPageByName(savedPageName);
                        if (page) {
                            survey.currentPage = page;
                            console.log(`Survey restored to page: ${savedPageName}`);
                        } else {
                            console.warn(`Saved page name "${savedPageName}" does not exist. Starting from the first page.`);
                            survey.currentPageNo = 0; // Reset to the first page if the saved page doesn't exist
                        }
                    }
                }

                // Instantiate Showdown for Markdown processing for SurveyJS
                const converter = new showdown.Converter();

                // Initialize SurveyJS with loaded data
                const survey = new Survey.Model(surveyJSON); // Ensure surveyJSON is correctly defined
                survey.locale = "de"; // Set the locale to German
                survey.applyTheme(SurveyTheme.BorderlessLight); // Apply a theme to the survey

                // Load existing survey data and current page
                const savedSurvey = loadSurveyData();
                survey.data = savedSurvey.data;

                // Make all questions required when they are added
                survey.onQuestionAdded.add((sender, options) => {
                    options.question.isRequired = true;
                });

                // Listen to value changes and save data
                survey.onValueChanged.add((sender, options) => {
                    liveSend(sender.data);
                    saveSurveyData(sender.data, survey.currentPage.name);
                });

                // Initialize variables to store initial progress and increment
                let initialPercentage = null;
                let perPageIncrement = null;

                // Listen to page changes and save the current page name
                survey.onCurrentPageChanged.add((sender, options) => {
                    const newCurrentPage = sender.currentPage;
                    const newIndex = sender.pages.indexOf(newCurrentPage);

                    console.debug("PAGE CHANGED to index", newIndex);

                    // Save the survey data along with the new page name
                    saveSurveyData(sender.data, newCurrentPage.name);

                    // Update the progress bar based on the current page index
                    // If this is the first time we're updating the progress bar, capture the initial percentage and calculate the per-page increment
                    if (initialPercentage === null) {
                        // Capture the initial progress bar percentage
                        initialPercentage = parseInt($('.progress-bar').attr('aria-valuenow'), 10) || 0;

                        // Total number of pages in the survey
                        const totalPages = sender.pages.length+window.maxpages;

                        // Calculate per-page increment
                        if (totalPages > 1) {
                            perPageIncrement = (100 - initialPercentage) / (totalPages - 1);
                        } else {
                            perPageIncrement = 0; // If there's only one page, no increment needed
                        }

                        console.debug(`Initial Percentage: ${initialPercentage}%`);
                        console.debug(`Per Page Increment: ${perPageIncrement}%`);
                    }

                    // Calculate the new percentage based on the current page index
                    let newPercentage;
                    if (sender.pages.length === 1) {
                        newPercentage = 100;
                    } else {
                        newPercentage = Math.round(initialPercentage + perPageIncrement * newIndex);
                        // Ensure the percentage does not exceed 100%
                        newPercentage = Math.min(newPercentage, 100);
                    }

                    // Update the progress bar's width, aria-valuenow, and text
                    $('.progress-bar')
                        .css('width', newPercentage + '%')
                        .attr('aria-valuenow', newPercentage)
                            .find('b').text(newPercentage + '%');

                    console.debug(`Progress updated to ${newPercentage}%`);
                });

                // Handle survey completion
                // Convert survey data to JSON and store it in a hidden input field
                survey.onComplete.add((sender, options) => {
                    const surveyResultsString = JSON.stringify(sender.data);

                    // Find the hidden input field by its ID
                    const hiddenInput = document.getElementById('surveyResults');
                    hiddenInput.value = surveyResultsString;

                    // Clear the stored data from localStorage
                    localStorage.removeItem(SURVEY_STORAGE_KEY);
                    console.log("Survey data cleared from localStorage.");

                    // Submit the form. This will trigger the post method in the PostSurvey class
                    $('#form').submit();
                });

                // Handle Markdown conversion. Required for rendering Markdown in survey questions
                survey.onTextMarkdown.add(function (survey, options) {
                    // Convert Markdown to HTML
                    let str = converter.makeHtml(options.text);
                    // Remove root paragraphs <p></p>
                    str = str.substring(3);
                    str = str.substring(0, str.length - 4);
                    // Set HTML markup to render
                    options.html = str;
                });
                survey.currentPage = survey.getPageByName(savedSurvey.currentPage);

                // Render the survey and restore the current page if available. Required to ensure that the survey is displayed correctly.
                $("#surveyElement").Survey({
                    model: survey,

                });
            </script>

        </div>


        {{ endblock }}

The ReceiveCert page
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Those participants who have chosen to receive a certificate will be redirected to the :code:`ReceiveCert` page, where they can download their certificate. The certificate is generated as a PDF file, which is created using the ReportLab library. The name of the participant is merged into a pre-defined PDF template which contains the layout of the certificate, and the final PDF is saved in the static files directory. The participant can then download the certificate by clicking on a link that is connected to the generated PDF file. The certificate is only generated if the participant has chosen to receive it, and the file is deleted after the participant has downloaded it to ensure that no unnecessary files are left on the server and that the participant's data is not stored longer than necessary.

To understand how the :code:`ReceiveCert` page works, we will first look at the app init file, which contains the logic for displaying the page, generating the PDF certificate, and cleaning up after the participant has downloaded the certificate. The logic is implemented in several methods, including :code:`is_displayed`, :code:`vars_for_template`, and :code:`before_next_page`. These methods are used to determine whether the page should be displayed, to prepare the data for the template, and to clean up after the participant has downloaded the certificate.
The :is_displayed method checks whether the participant has chosen to receive a certificate by checking the :code:`nova_certificate_bin` field. If the participant has chosen to receive a certificate, the page is displayed - if not, the page is not displayed and participants are redirected to the :code:`LastPage` page. The :code:`vars_for_template` method prepares the data for the template, including function used to generate the PDF certificate and the path to the generated PDF file for download. The :code:`before_next_page` method is called before the participant is redirected to the next page, and it is used to clean up after the participant has downloaded the certificate. It deletes the generated PDF file and clears the :code:`nova_certificate_open` variable, which contains the name of the participant that was used to generate the certificate.

.. dropdown:: ReceiveCert app init code
   :icon: terminal

   .. code-block:: python

    @staticmethod
    def is_displayed(player: Player):
        if not player.field_maybe_none('nova_certificate_bin'):
            return False
        return player.nova_certificate_bin == 1

    @staticmethod
    def vars_for_template(player: Player):
        generate_pdf(player, player.nova_certificate_open)

        file_url = f'/static/certificates/{player.participant.code}_final_certificate.pdf'
        return {'file_url': file_url}

    @staticmethod
    def before_next_page(player: Player, timeout_happened):
        if not player.participant.is_browser_bot:
            # delete the final PDF file
            final_pdf_path = f'_static/certificates/{player.participant.code}_final_certificate.pdf'
            if os.path.exists(final_pdf_path):
                os.remove(final_pdf_path)
            # clear name variable
            player.nova_certificate_open = None

Let's have a look at the code that generates the PDF certificate. The :code:`generate_pdf` function is responsible for creating the PDF file with the participant's name. It uses the ReportLab library to create a new PDF file, sets the font and size, calculates the center position for the name, and draws the name on the PDF. After that, it merges the empty certificate PDF (that does not contain the name) with the newly created PDF that only contains the name, and saves the final PDF file. The function also deletes the temporary PDF file that was created with only the name after the final PDF has been created. As before, this ensures that no unnecessary files are left on the server and that the participant's data is not stored longer than necessary.

.. dropdown:: ReceiveCert PDF generation
   :icon: terminal

   .. code-block:: python

    def generate_pdf(player, name):
        # path to the certificate template PDF (without name)
        existing_pdf_path = './_static/Urkunde.pdf'
        # path to the new PDF with only the name
        new_pdf_path = f'./_static/certificates/{player.participant.code}_certificate.pdf'
        # path to the final PDF with the name merged into the template
        final_pdf_path = f'./_static/certificates/{player.participant.code}_final_certificate.pdf'

        # Create a new PDF with the name
        page_width, page_height = landscape((29.70 * cm, 21.01 * cm))
        c = canvas.Canvas(new_pdf_path, pagesize=(page_width, page_height))

        # Set the font and size
        font_size = 36
        c.setFont("Times-Roman", font_size)

        # Calculate the center position
        text = f"{name}"
        text_width = c.stringWidth(text, "Times-Roman", font_size)
        x = (page_width / 2) - (text_width / 2)
        y = (page_height / 2) + 10

        # Draw the text on the new PDF
        c.drawString(x, y, text)
        # Save the new PDF
        c.save()

        # Read the existing PDF
        existing_pdf = PdfReader(existing_pdf_path)
        new_pdf = PdfReader(new_pdf_path)
        output = PdfWriter()

        # Merge the new PDF with the existing one
        for page in range(len(existing_pdf.pages)):
            existing_page = existing_pdf.pages[page]
            if page == 0:
                new_page = new_pdf.pages[0]
                existing_page.merge_page(new_page)
            output.add_page(existing_page)

        # Write the final PDF to a file
        with open(final_pdf_path, 'wb') as outputStream:
            output.write(outputStream)

        # Delete the new PDF after the final PDF has been created
        if os.path.exists(new_pdf_path):
            os.remove(new_pdf_path)

        return final_pdf_path

The HTML template of the :code:`ReceiveCert` page is quite simple. It includes a link to download the generated PDF certificate, which is displayed as a clickable link. The template also includes a stylesheet for styling the page and a header that informs the participant about the purpose of the page. The link to download the certificate is dynamically generated based on the file URL that was passed from the app init file.

.. dropdown:: ReceiveCert HTML Template
   :icon: terminal

   .. code-block:: html

    <h1>Laden Sie Ihre Urkunde herunter</h1>
    <br>
    Hier können Sie Ihre Urkunde herunterladen. <br><br>
    <b>--> <a href= {{ file_url }} download><u>Klicken Sie hier, um Ihre Urkunde herunterzuladen</u></a> <--</b>
    <br><br>
    Klicken Sie auf "<b>Weiter</b>", um fortzufahren.

The LastPage page
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The last page of the survey is the :code:`LastPage` page, which is displayed to all participants after they have completed the survey. The purpose of this page is to redirect participants to the Prime Panels website, where they can receive their payment for participating in the survey. The page includes a button that participants can click to be redirected to Prime Panels. The link for redirecting is defined in the :code:`FINAL_PP_URL` variable, which is used to construct the URL for redirecting participants to Prime Panels. Behind this link, the participant's unique ID (label) is appended as a query parameter to ensure that the payment can be tracked and processed correctly. If the participant's label is not available, the page will simply redirect to the default end page of the survey. The page also includes a thank-you message from the research team for participating in the study.

.. dropdown:: LastPage app init code
   :icon: terminal

   .. code-block:: python

        class LastPage(Page):
                def post(self):
                FINAL_PP_URL = 'https://app.cloudresearch.com/Router/End?aid='
                self.player.is_redirected=True
                # if there is a participant.label then inject it to aid url query param and redirect to PrimePanels
                if self.participant.label:
                    return RedirectResponse(f'{FINAL_PP_URL}{self.participant.label}')
                else:
                    return super().post()

.. dropdown:: LastPage HTML Template
   :icon: terminal

   .. code-block:: html

        <link rel="stylesheet" href="{{static 'Vignette.css'}}"/>
        <style>
            .btn {display: block}
        </style>
            <h1>Ende der Umfrage</h1>
        <br>
         <button class="btn btn-lg btn-success">
             Klicken Sie hier, um Ihre Auszahlung zu erhalten
         </button>
        <br>
            Wir bedanken uns ganz herzlich für die Teilnahme an dieser Studie!<br>
            <br>
            Klicken Sie bitte auf "<b>Klicken Sie hier, um Ihre Auszahlung zu erhalten</b>", um zurück zu Prime Panels zu gelangen.
            <br><br>
            <b>Ihr Forschungsteam</b><br>
            Prof. Dr. Achim Goerres<br>
            Dr. Philipp Chapkovski<br>
            Jakob Eicheler<br>
            Philipp Kemper


This is the end of the part of the documentation that describes the code of the oTree project. The last page of this documentation explains how this project can be deployed and hosted with Heroku, a platform that allows developers to build, run, and operate applications entirely in the cloud. That page provides a step-by-step guide on how to deploy this oTree project on Heroku, including the necessary configurations and commands to get the application up and running.
