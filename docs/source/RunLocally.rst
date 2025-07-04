
.. _runlocally:

Running the Novaland Project Locally
====================================

This guide explains how to set up and run the Novaland oTree project on your local machine for development and testing purposes.

Requirements
------------

Before proceeding, ensure that you have the following installed:

- Python 3.8 or newer
- Git
- pip (Python package installer)
- Virtual environment support (optional but recommended)

Step-by-Step Setup
------------------

1. **Clone the repository**

   First, clone the Novaland project repository to your local machine:

   .. code-block:: bash

      git clone https://github.com/YOUR_USERNAME/novaland.git
      cd novaland

2. **Create a virtual environment**

   It is recommended to use a virtual environment to manage dependencies:

   .. code-block:: bash

      python -m venv venv
      source venv/bin/activate  # On Windows: venv\Scripts\activate

3. **Install dependencies**

   All required Python packages are listed in the `requirements.txt` file:

   .. code-block:: bash

      pip install -r requirements.txt

4. **Run the oTree development server**

   Once everything is installed, start the local server:

   .. code-block:: bash

      otree devserver

   This will launch a local server at `http://localhost:8000`, which you can open in your browser.

Optional Configuration
----------------------

- If you want to run a specific session configuration, you can append its name:

  .. code-block:: bash

     otree devserver SESSION_CONFIG_NAME=novaland_experiment

- To reset the database (removes all previous data):

  .. code-block:: bash

     otree resetdb

Useful Files and Folders
------------------------

- `settings.py` — main configuration file for session settings, installed apps, and environment variables
- `main/` — contains the core logic for the Novaland experiment, including HTML pages, Python logic, and templates
- `data/` — contains YAML files such as `vignette_q.yaml` and `corrupt_endings.yaml` for question templates and storyline branching

Troubleshooting
---------------

- If you encounter module not found errors, make sure your virtual environment is activated.
- If port 8000 is busy, you can specify another port:

  .. code-block:: bash

     otree devserver --port 8001

- For full documentation on oTree: https://otree.readthedocs.io

