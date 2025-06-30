.. NovalandCorruptionGuide documentation master file, created by
   sphinx-quickstart on Thu May 15 10:55:42 2025.


Documentation of the oTree project for the Novaland corruption experiment
===========================================================================

This is the documentation of the technical setup that was used to conduct the data collection for the paper **How the Experience of Public-Service Quality and Corruption Shapes Political Solidarity and Trust: Experimental Evidence from a Novel Virtual-State Approach**.
The paper and this documentation was written by the team of Achim Goerres at the University of Duisburg-Essen as part of the ERC-funded project **POLITSOLID**. The project team members were: Prof. Dr. Achim Goerres, Dr. Philipp Chapkovski, Jakob Eicheler, and Philipp Kemper.

For this study, we used a novel virtual-state approach to conduct a large-scale online experiment. The virtual state was named **Novaland**. The experiment was designed to experimentally test the effects of different levels of public service quality and corruption on political solidarity and political trust.

The entire project was written and programmed using oTree.
oTree  provides a framework for building and running online experiments, allowing researchers to easily create custom games and surveys, manage participants, and collect and analyze data.
An oTree project consists of a set of components, such as Python scripts and HTML templates, which work together to define the structure and behavior of the experiment.

The goal of this documentation is to provide a comprehensive overview of the technical setup and implementation of the Novaland experiment, including the file structure, code functionality, and data collection process. Please note that long code snippets are hidden in dropdown boxes. Click on them to see the code:

.. dropdown:: Example dropdown code box
   :icon: terminal

   .. code-block:: python

      Hello there.

For information about the functionality of oTree beyond this project documentation, please refer to the `official oTree documentation <https://otree.readthedocs.io/en/latest/index.html>`_.

This documentation is structured as follows:

* :doc:`The first page <Novaland2Layout>` provides an overview of the layout of the Novaland experiment, including the flowchart of the Novaland stay and the randomization of the treatments
* :doc:`The second page <ProjectStructure>` provides an overview of the structure and functionality of the oTree project's folders and files
* The following pages, titled 'PartX ...' go into detail about which parts of the code produce certain parts of the online questionnaire and experimental treatments.
* The last page of this documentation step away from the oTree project's code and explains how the project was :doc:`hosted <Deployment>` on a server.

Contents
-----------------------------------------

.. toctree::
   :maxdepth: 1


   Novaland2Layout
   ProjectStructure
   RunLocally
   AddVignettes
   CodeStructureGlobalFiles
   CodeStructureIntro
   CodeStructureMain
   CodeStructureElection
   CodeStructurePost
   Deployment

