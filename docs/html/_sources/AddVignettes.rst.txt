
.. _addvignettes:

Adding New Vignettes and Smokescreens
=====================================

This guide explains how to add new **vignettes** or **smokescreens** to the Novaland experiment.

Folder structure
----------------

The HTML files that define the visual layout of vignettes and smokescreens are stored in the following locations:

- `main/vignettes/` — contains the HTML files for each vignette (e.g., `doctor.html`, `passport.html`)
- `main/smokescreens/` — contains HTML files for smokescreen pages

Each vignette or smokescreen corresponds to a real-life public service scenario.

Steps to add a new vignette
---------------------------

1. **Create the HTML file**

   Create a new HTML file and save it in `main/vignettes/`, for example:

   ```
   main/vignettes/hospital.html
   ```

2. **Update constants in `main/__init__.py`**

   Add the name of your new vignette to the `vignettes` list and decide in which round(s) it should appear.

   Example:

   ```python
   class C(BaseConstants):
       ...
       VIGNETTE_ROUNDS = [3, 4, 6, 7]
       vignettes = ['doctor', 'handyman', 'kindergarten', 'passport', 'hospital']
   ```

3. **Edit `vignette_q.yaml`**

   This YAML file, located in `data/vignette_q.yaml`, defines the survey questions shown after the vignette page.

   Example excerpt from `vignette_q.yaml`:

   ```yaml
   - name: satisfaction_page
     elements:
       - type: radiogroup
         name: satisfaction_service
         title: Bitte bewerten Sie die Qualität der Dienstleistung.
         choices:
           - value: 1
             text: "1 Sehr gut"
           ...
   ```

   Conditional logic can be used to include or skip certain blocks depending on the context (e.g., if the vignette is corrupt).

4. **Edit `corrupt_endings.yaml`**

   If your vignette has a corrupt version with an alternative ending, add the corresponding text to `corrupt_endings.yaml` located in the same `data/` folder.

   Example structure:

   ```yaml
   hospital: >
     Der Arzt verweigert die Behandlung, bis Sie eine informelle Zahlung leisten.
   ```

5. **Optional: Add smokescreen pages**

   If you want to include a smokescreen (a filler or distractor vignette), follow the same steps as for vignettes, but place the HTML file in `main/smokescreens/` and include the round number in `SMOKESCREEN_ROUNDS`.

   Example:

   ```python
   SMOKESCREEN_ROUNDS = [1, 5]
   ```

Checklist
---------

- [ ] HTML file added to the correct folder
- [ ] Name added to `C.vignettes`
- [ ] Round(s) added to `VIGNETTE_ROUNDS` or `SMOKESCREEN_ROUNDS`
- [ ] YAML files updated with relevant questions and endings
