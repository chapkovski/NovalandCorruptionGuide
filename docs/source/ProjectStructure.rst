Structure of the oTree project
==============================

The project is structured into several folders and files, each serving a specific purpose. The main components of the project are explained below.

Visualisation of the project file structure
-------------------------------------------
The following filetree provides an overview of the project's file structure. Each of the elements will be explained in detail in the following sections. The oTree project itself is named 'novaland2'. This is because this is the oTree project for the second study that utilizes the virtual state 'Novaland'.

.. code-block:: text
    :linenos:

      novaland2/
      ├── settings.py
      ├── requirements.txt
      ├── intro/
      │   ├── __init__.py
      │   ├── tests.py
      │   └── XXXXX.html
      ├── main/
      │   ├── instructions/
      │   │   └── XXXXX.html
      │   ├── outcomes/
      │   │   └── XXXXX.html
      │   ├── smokescreens/
      │   │   └── XXXXX.html
      │   ├── vignettes/
      │   │   └── XXXXX.html
      │   ├── __init__.py
      │   ├── tests.py
      │   └── XXXXX.html
      ├── election/
      │   ├── __init__.py
      │   ├── tests.py
      │   └── XXXXX.html
      ├── post/
      │   ├── __init__.py
      │   ├── tests.py
      │   └── XXXXX.html
      ├── data/
      │   └── XXXXX.yaml
      ├── templates/
      │   ├── global/
      │   │   ├── XXXXX.html
      └── _static/
          ├── certificates/
          │   └── XXXXX.pdf
          ├── global/
          │   └── XXXXX.css
          ├── images/
          │   ├── smokeescreens/
          │   │   └── XXXXX.jpg
          │   └── vignettes/
          │       └── XXXXX.jpg
          ├── surveyjs/
          │   ├── XXXXX.js
          │   └── XXXXX.css
          ├── XXXXX.css
          └── XXXXX.png

**Note:** 'XXXXX' is a placeholder for the name of one or more files with the corresponding extension in the respective folder. The actual file names are not relevant for understanding the project's structure and functionality, so they are not listed here. The functionality of these files is explained below.


Explanation of the project file structure
-----------------------------------------
In this section, the different folders and files in the project are explained in detail. The explanation is structured according to the file structure shown above.

Files to define project-wide settings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    #. **settings.py**: This file contains the settings and configuration for the oTree project. It defines the project's name, version, and other global settings.
    #. **requirements.txt**: This file lists the dependencies and packages required to run the oTree project. It is used to install the necessary libraries and modules.

Folders for creating the questionnaire and experiment (apps): intro, main, election, post
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    #. The **intro** folder contains the files for the introduction pages of the project. The following pages are produces by this app:
        #. one introduction page that participants see when they first enter the project and that contains information about their participation
        #. one page for the informed consent
        #. one page with information about the upcoming comprehension questions and the filtering of participants
        #. four pages with questions on participants' socio-demographic characteristics
        #. one page with information about the layout of the study

    #. The **main** folder contains the files for the main part of the study. This is also where our experimental treatments take place. The main app specifically produces
        #. several pages that introduce participants to Novaland
        #. one page with comprehension questions based on this information
        #. two smokescreen pages
        #. one newspaper quiz page and the corresponding result page
        #. four vignette pages
        #. four vignette result pages
        #. three pages with questions from a virtual pollster and the pollster result page

    #. The **election** folder contains the files for the election app. This app produces
        #. one page with information about the election
        #. two pages for introducing the two parties
        #. one page with the election ballot
        #. one page with the referendum vote
        #. one page with the debrief from Novaland

    #. The **post** folder contains the files for the post-experimental questionnaire. This app produces
        #. one page with the post-experimental questionnaire
        #. one page on which participants receive their certificate of participation if they requested one
        #. one outro-page of the study

    * File types in the app folders:
        #. **__init__.py**: This file is the heart of every oTree app: Here, the variables used to handle data and the pages of the questionnaire are defined. Also, this is where the randomization of the treatments takes place. The __init__.py file is the first file that is executed when an app is loaded.
        #. **tests.py**: This file is used to define test cases for the project. It can be used to test the functionality and behavior of the project using bots. These files are not relevant for the main data collection and will not be explained in detail here.
        #. **XXXXX.html**: These HTML files define the appearance and layout of the web pages used in the project. They define the content and structure of the pages that participants will see during the experiment.

Folders for storing data and templates
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

    #. **data/**: The questions on the vignette pages were dynamically animated on the pages. The vignette_q.yaml file in this folder contains these questions. Also, in the corrupt versions of the vignettes, the last paragraph of the vignette was shown to participants again before answering the question. The corrupt_ending.yaml file contains these paragraphs.

    #. **templates/**: This folder contains the Page.html file that defines the structure and layout of the web pages used in the project. It is used to store global templates that can be reused across different apps. The other HTML file was not used in the final version of the project.

    #. **_static/**: This folder contains all the static files used in the project, such as images, CSS files, and JavaScript files. These files are used to customize the appearance and behavior of the web pages. The _static folder is divided into several subfolders:

        #. **certificates/**: This folder contains the PDF files that is generated for participants who requested a certificate of participation. Participants' could enter their names on these certificates and the PDF files which were generated for them were automatically deleted upon their completion. Certificates stored in this folder derive from manual or bots testing.
        #. **global/**: This folder contains the CSS file that defines the styles and layout of the web pages used in the project. As custom CSS files are used in the project, this file was not used in the final version of the project.
        #. **images/**: This folder contains all images which were embedded into the web pages of the questionnaire. The images of the smokescreens and vignettes are stored in the respective subfolders.
        #. **surveyjs/**: The questions of the post-experimental questionnaire were all displayed one after another on the same page. This folder contains the JavaScript and CSS files used to create dynamic elements on the web pages, i.e. to show the next question as soon as participants had answered the previous one. The questionnaire was animated using the SurveyJS library. The JavaScript file contains the code for the SurveyJS library, which is used to create and manage surveys and questionnaires. The list of questions in the survey can be found in survey_definition.js. The CSS in this folder file defines the styles and layout of the survey elements.
        #. **XXXXX.css**: These CSS files were used to define the styles and layout of the web pages used in the project.
        #. **XXXXX.png**: These images are the logos of the fictional parties.
        #. **Certificate.pdf**: This is the raw version of the certificate without participants' name written on it. This PDF file is overlaid with a second PDF that only contains participants' name. This is done to ensure that the certificate is generated in a way that participants cannot manipulate it. The overlaying of the two PDF files is done using the PyPDF2 library in Python. The final PDF file is then saved in the '_static/certificates' folder.


**Note:** There are additional folders and files in the project to those listed in the filetree above. Some of those were used purely for the development and testing of the project. Others contribute to the functionality of oTree itself. As none of these are necessary to understand, modify or run the project, they will not be discussed here.

The file types used in the project
----------------------------------
Before going into detail about the project structure, it is important to understand the different file types used in the project.

#. **.py** files: These are Python files that contain the code for the oTree project. They define the underlying functionality of the project. A short summary of these init files is given in the section below.
#. **.html** files: These are HTML files that define the structure and layout of the web pages used in the project. They can include both static and dynamic content. Each html file is a template that can be rendered by oTree to display the corresponding page to the participants. For more information about how the HTML files are structured, please refer to the `official oTree documentation on HTML templates <https://otree.readthedocs.io/en/latest/templates.html>`_.
#. **.yaml** files: These are YAML files that contain configuration data for the project. They are used to define settings and parameters that can be easily modified without changing the code.
#. **.css** files: These are CSS files that define the styles and layout of the web pages used in the project. They are used to customize the appearance of the project.
#. **.js** files: These are JavaScript files that contain code for client-side functionality. They are used to add interactivity and dynamic behavior to the web pages.
#. **.png**, **.jpg** files: These are image files used in the project. They are used for displaying images or icons on the web pages.
#. **.txt** files: These are text files that contain plain text data. They are used for various purposes, such as storing configuration settings or documentation.
#. **.pdf** files: These PDF files are used to create participation certificates.


The __init__.py files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The __init__.py files are the main files of the apps. They contain the code that defines the structure and functionality of the app. The __init__.py files are used to define the pages, models, and other components of the app. The __init__.py files are divided into several sections, each of which is used to define a specific aspect of the app.

The **sections of the __init__.py files** are as follows:

#. **Imports**: At the beginning of the file, all necessary libraries and modules are imported. This includes the oTree libraries, as well as any other libraries that are used in the app.

#. **Models**: The models section defines the data structure of the app. This includes the variables that are used to store data. oTree defines a set of classes and functions for defining these data storage models.

#. **Pages**: In this section, the content and behavior of the pages are defined. Each page is defined as a class, which inherits from the oTree Page parent class. The page classes define the content and behavior of the pages, including the variables that are used to store data, the functions that are used to process data. Each page class corresponds to one HTML file that defines the layout and design of the page.

#. **Page sequence**: At the end of the file, the page sequence is defined. This is a list of the pages that will be displayed in the app, in the order in which they will be displayed.

* **Additional constants and functions**: In addition to these sections, the __init__.py files may also include additional constants and functions that are used in the app. An example for this is the :code:`create_session` function, which was used in this study to conduct the treatment randomization at the beginning of the main app.

For a more detailed documentation on the structure of the __init__.py files, please refer to the `official oTree documentation <https://otree.readthedocs.io/en/latest/index.html>`_.