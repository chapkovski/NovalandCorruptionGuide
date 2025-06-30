.. _addvignettes:

Adding New Vignettes and Smokescreens
=====================================

To add new vignettes or smokescreens:

1. **Create a new app** using:
   .. code-block:: bash

      otree startapp new_vignette

2. **Add the new app** to the `session_configs` in `settings.py`.

3. **Add it to the sequence** in the treatment session config:
   .. code-block:: python

      'app_sequence': ['intro', 'new_vignette', 'election'],

4. **Define the logic and layout** in the app's `models.py` and `pages.py`.

5. For smokescreens, adjust `vars_for_template` or HTML files to show/hide decoy elements.