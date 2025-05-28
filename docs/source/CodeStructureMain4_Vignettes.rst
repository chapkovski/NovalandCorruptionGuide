The vignette pages
======================
The vignette page is where the main experimental treatments take place: On this page, participants are presented with a vignette that describes a situation in which they try to access a service in Novaland.

The vignette page are displayed in the third, fourth, sixth, and seventh rounds of the main app and dynamically filled with the content of the current vignette scenario. The code for the vignette pages is implemented in the :code:`Vignette` page class. The :code:`is_displayed` method is used to determine whether the page should be displayed to the participant. This is done by checking whether the current round number is in the list of :code:`VIGNETTE_ROUNDS` object defined in the :code:`C` class.

The order in which the vignettes are displayed is randomized for each participant. The order is defined in the :code:`timeline` variable in the :code:`creating_session` function, which we discussed above. The vignettes are defined in the :code:`vignettes` variable in the :code:`C` class as a list of strings: :code:`vignettes = ['doctor', 'handyman', 'kindergarten', 'passport']`. The randomization takes place in the :code:`creating_session` function, where the order of the vignettes is randomized for each participant using the :code:`random.shuffle(vignettes)` function. The randomized order is then stored in the :code:`Participant` object as :code:`p.vars['vignette_order']`.

Now, let's have a more detailed look into the code of the vignette pages - how they are created, how they are randomized and how they are displayed to the participants.

The :code:`render_survey` method is used to render the YAML file that contains the questions for the vignette. The YAML file is read from the :code:`data/vignette_q.yaml` file, which is defined in the :code:`C` class. The method uses the :code:`Template` class from the Jinja2 library to render the YAML file with the variables defined in the :code:`variables` dictionary. The rendered YAML file is then converted to a JSON string and returned.


Implementation of the experimental treatments
----------------------------------------------------



The Outcome Page
---------------------------------
After the participants submitted the vignette page, they were redirected to the :code:`VignetteResults` page. This page was displayed after each smokescreen and vignette and was used to inform participants about the outcome of the vignette they just experienced and thereby to increase participants' immersion and feeling of self-efficacy. As this page is the same one used for the outcomes of the smokescreens, see this :ref:`Outcome Page Section <outcome_pages>` for more detail.