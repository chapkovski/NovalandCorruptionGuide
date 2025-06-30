.. _runlocally:

Running Novaland Locally
=========================

To run Novaland on your computer, follow these steps:

1. Clone the repository:
   .. code-block:: bash

      git clone <your-repo-url>
      cd Novaland

2. Create a virtual environment and install dependencies:
   .. code-block:: bash

      python -m venv venv
      source venv/bin/activate  # or venv\Scripts\activate on Windows
      pip install -r requirements.txt

3. Run the oTree server:
   .. code-block:: bash

      otree devserver

4. Open the browser at http://localhost:8000