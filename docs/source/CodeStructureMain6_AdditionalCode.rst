Additional code for extended background functionality
========================================================
In addition to the code described above, there are some additional code snippets that are used to implement the extended background functionality of the questionnaire. These snippets are not directly related to the main functionality of the questionnaire, but they are used to enhance the user experience and provide additional features.

The progress bar
-----------------

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


Animating the status bar
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

XXXXXXXXXXXXXXXXXXXXXXXX


The dynamic instructions button
---------------------------------

XXXXXXXXXXXXXXXXXXXXXXXXXXXX