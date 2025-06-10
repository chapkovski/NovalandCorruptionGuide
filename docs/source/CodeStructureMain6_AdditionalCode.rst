Additional code for extended background functionality
========================================================
In addition to the code described above, there are some additional code snippets that are used to implement the extended background functionality of the questionnaire. These snippets are not directly related to the main functionality of the questionnaire, but they are used to enhance the user experience and provide additional features.

The progress bar
-----------------

At the top of the main app's __init__.py file, the :code:`Page` class is defined. This is the parent class for all pages in the app. This means that the code that is defined within it is executed by all survey pages across the app.
This class is used to automatically retrieve the current page number and maximum page number of the questionnaire using the :code:`get_context_data` function. This function then calculates the progress percentage. The progress percentage is then passed to the HTML template, where it is displayed as a progress bar.


.. dropdown:: Progress bar calculations in the init file of the main app
   :icon: terminal

   .. code-block:: python


    class Page(oTreePage):
        instructions = False # variable to determine if the dynamic instructions button should be displayed,
                             # set to True in the page class if the button should be displayed

        # Index of the page in the app's page sequence, used to calculate the progress bar
        def get_context_data(self, **context):
            NUM_SURVEY_PAGES = 36 # number of survey pages in the main app, manually counted and defined
            app_name = self.__module__.split('.')[0]
            page_name = self.__class__.__name__
            if page_name != 'PostSurvey' and app_name == 'post':
                index_in_pages = self._index_in_pages + NUM_SURVEY_PAGES
            else:
                index_in_pages = self._index_in_pages
            r = super().get_context_data(**context)

            # calculate the current page index and maximum page index.
            # If the current session includes the post-survey, the maximum page index is adjusted accordingly.
            # The latter is necessary because on the PostSurvey page, multiple pages are displayed dynamically
            # in a single page, which is not counted in the total number of pages.
            if 'post' in self.session.config.get('app_sequence'):
                max_pages = NUM_SURVEY_PAGES + self.participant._max_page_index
            else:
                max_pages = self.participant._max_page_index

            r['maxpages'] = max_pages # number of pages in the questionnaire
            r['page_index'] = self._index_in_pages # current page index
            r['progress'] = f'{int(index_in_pages / max_pages * 100):d}' # calculate the progress percentage

            r['instructions'] = self.instructions # pass the instructions to the template for the dynamic instructions button if the page class contains the button
            return r


Animating the progress bar
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

As the progress bar is displayed on each page of the main app, it is defined in the :file:`_templates/global/Page.html` file. The progress bar is animated using CSS and JavaScript to provide a smooth transition effect as participants navigate through the questionnaire. The progress bar is updated dynamically based on the current page index and maximum page index, which are passed from the :code:`get_context_data` method in the :code:`Page` class. This allows the progress bar to reflect the current progress of the participant in real-time as they move through the pages of the questionnaire. The variable :code:`progress` contains the current progress percentage, and the variable :code:`maxpages` contains the maximum number of pages in the questionnaire. These variables are used to set the width of the progress bar and to display the current progress percentage.
Here is the code that is used to animate the progress bar in the HTML template:

.. dropdown:: Progress bar animation in the HTML template
   :icon: terminal

   .. code-block:: html

    <script>
        window.progress = {{progress}};
        window.maxpages = {{maxpages}};
    </script>

    <nav class="navbar navbar-light bg-light justify-content-between fixed-top">
        <div class="navbar-nav flex-grow-1 mx-1">
            <div class="progress " style="height: 30px;">
                <div class="progress-bar" role="progressbar" style="width: {{ progress }}%"
                     aria-valuenow="{{ progress }}"
                     aria-valuemin="0" aria-valuemax="100">
                    <b> {{ progress }}%</b></div>
            </div>
        </div>

    [...]

The dynamic instructions button
---------------------------------

The instructions button is a feature that allows participants to access the information provided on the NovalandIntro pages at any time during the questionnaire. This is particularly useful for participants who may need to refer back to the information provided earlier in the questionnaire. In our study, we used it to give participants the possibility to access to the information provided on Novaland on the comprehension check page in the intro app.The button is implemented using a live method that dynamically loads the content of the instructions when clicked. The code for animating the button and its content is embedded in the same HTML template as the progress bar, which is the :file:`_templates/global/Page.html` file. The button is displayed only if the :code:`instructions` variable is set to True in the page class. This allows us to control whether the button is displayed on a specific page or not. The button opens a modal window that displays the content of the instructions, which is loaded dynamically when the button is clicked.The modal window is implemented using Bootstrap's modal component, which provides a user-friendly way to display additional information without navigating away from the current page.


.. dropdown:: Dynamic instructions button in the HTML template
   :icon: terminal

   .. code-block:: html

    <nav class="navbar navbar-light bg-light justify-content-between fixed-top">

        [...]

        {% if instructions %}


        <div class="mx-1">
            <button class="btn   btn-success" id="instruction_button" data-bs-toggle="modal" data-bs-target="#instructionsModal"
            style="margin:0px">
                Informationen
            </button>
        </div>


        {% endif %}
    </nav>

The live method that is called when the button is clicked is defined in the :code:`Page` class in the :file:`__init__.py` file of the main app. This method is used to handle the click event and load the content of the instructions dynamically. The code for this live method is as follows:

.. dropdown:: Live method for the dynamic instructions button in the init file
   :icon: terminal

   .. code-block:: python

    class Page(oTreePage):

        # This variable constantly checks if the instructions button was clicked so that the HTML template can display the button
        @staticmethod
        def live_method(player, data):
            print(f'data received: {data}. apparently instuctions clicked')
            player.instructions_clicked += 1

        instructions = True # set to True for the button to be displayed on the page

The content of the instructions window is saved in the :file:`main/instructions/instructions_X.html` files, where 'X' stands for an integer between 1 and 4. This means that the content of the instructions is dynamically loaded from these files when the button is clicked. Each of these pages repeat the content of one of the four pages on which participants received the information on Novaland in the first place. In the HTML template of the :code:`Comprehension` page, the following code is used to load the content of the instructions dynamically:

.. dropdown:: Dynamic instructions content in the HTML template
   :icon: terminal

   .. code-block:: html

    <div class="modal fade" id="instructionsModal" tabindex="-1" aria-labelledby="exampleModalLabel"
     aria-hidden="true">
        <div class="modal-dialog modal-xl">
            <div class="modal-content">
                <div class="modal-header">
                    // The modal header contains the title and a close button
                    <h5 class="modal-title" id="exampleModalLabel">Informationen über Novaland</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                // The modal body contains the content of the instructions, which is loaded dynamically from the HTML files
                <div class="modal-body">
                    {{ include 'main/instructions/instructions_1.html' }}
                    {{ include 'main/instructions/instructions_2.html' }}
                    {{ include 'main/instructions/instructions_3.html' }}
                    {{ include 'main/instructions/instructions_4.html' }}

                </div>
                // The modal footer contains a close button to close the modal window
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Schließen</button>

                </div>
            </div>
        </div>
    </div>

    // The following code calls the live method from the Page class
    // when the button is clicked to handle the click event and show the modal window
    <script>
        $(document).ready(function () {
            $('#instruction_button').click(function () {
                console.debug('button clicked');
                liveSend('button_clicked');
            });
        });
    </script>

