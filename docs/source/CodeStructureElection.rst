Part 3: The Election in Novaland
=======================================

After participants completed the fourth vignette, the part of the Novaland stay that revolves around the national election started. The respective pages are part of the `election` app in the oTree project. In this election, participants could vote for onw of two parties, the **Soziale Partei Novaland** or the **Konservative Partei Novaland**. The election was designed to test the effects of public service quality and corruption on electoral behavior and incumbent punishment.

The election app is structured as follows:
    #.  The first page introduces the election and the political system of Novaland.
    #.  The second pages presents one of the two parties.
    #.  The third page presents the other party.
    #.  The fourth page is the ballot page, where participants can cast their vote.
    #.  The fifth page contains a referendum, where participants can vote to change the income tax in Novaland.
    #.   On the sixth page, participants were informed that their stay in Novaland ends and were debriefed.

Randomization of the election app
-------------------------------------------------
The information that was provided to participants in the context of the election was randomized in two instances. The first randomization was the order of the party pages. Participants were randomly assigned to one of two groups: one group saw the **Soziale Partei Novaland** page first, while the other group saw the **Konservative Partei Novaland** page first. This was done by using the :code:`random.choice()` function in the :code:`creating_session` function to randomly assign participants to one of the two groups and then storing the information of which party page they saw first in the :code:`Player` object as :code:`first_party`.

The second randomization was the incumbency of the parties. Participants were randomly assigned to one of two groups: one group saw the **Soziale Partei Novaland** as the incumbent party, while the other group saw the **Konservative Partei Novaland** as the incumbent party. This was also done by using the :code:`random.choice()` function in the :code:`creating_session` function to randomly assign participants to one of the two groups and then storing the information of which party was the incumbent in the :code:`Player` object as :code:`incumbent_party`.


The pages of the election app
-----------------------------

1. The introduction of the election
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The first page of the election app is the **election_intro** page. Here, participants were told that they will now take part in the national election of Novaland. They were informed that the head of government will be elected from the parliament and needs the majority of the votes in the parliament to be elected. The page also mentions which of the two parties is the incumbent party, depending on the randomization of the incumbency by calling the :code:`incumbent_party` variable on the HTML page using :code:`{{ incumbent_party }}`.

2. & 3. The introduction of the parties
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The second and third pages are the **party_1** and **party_2** pages, respectively. On these pages, participants were presented with the information about the two parties. The order of the pages was randomized, so that participants either saw the **Soziale Partei Novaland** first or the **Konservative Partei Novaland**. This is done by setting conditional statements in the HTML templates which call the values of the :code:`first_party` variable.

The second and third pages are the **party_1** and **party_2** pages, respectively. On these pages, participants were presented with the information about the two parties. The order of the pages was randomized, so that participants either saw the **Soziale Partei Novaland** first or the **Konservative Partei Novaland**. This is done by setting conditional statements in the HTML templates which call the values of the :code:`first_party` variable. This is how the **party_1** page is created:

.. code-block:: HTML
   :linenos:

    <!-- Builds on the global Page.html template -->
    {{ extends 'global/Page.html' }}

    <!-- Style settings for embedding the parties' images -->
    {% block style %}
    <style>
        .text-image-container {
            display: flex;
            flex-direction: column;
        }
    </style>
    {% endblock %}

    <!-- Title of the page (not defined here)-->
    {% block title %}

    {% endblock %}

    <!-- Content of the page, depending on "first_party" -->
    {% block content %}

    <! Checks if the first party is the "Soziale Partei Novaland" -->
    {% if first_party == "Soziale Partei Novaland" %}
    <div class="content-box">
        <!-- Title of the page, adjusted to the Soziale Partei Novaland -->
        <h1 class="h1">Soziale Partei Novaland. Tag 9 von 9</h1>
        <!-- Define area for the text and images -->
        <div class="text-image-container">
            <div class="text-content">
                <!-- Create table for containing the text and the image in one row -->
                <div class="row">
                    <! Column for the text -->
                    <div class="col col-sm-9 col-12">
                        <!-- Text about the Soziale Partei Novaland -->
                        Die SPN (Soziale Partei Novaland) will mehr sozialstaatliche Leistungen, auch wenn das mehr
                        Steuern und Abgaben bedeutet. Sie will mehr staatliche Lösungen für die Probleme der Menschen
                        organisieren und finanzieren. <br><br>

                        Für Menschen in Novaland mit Ihrer Einkommenshöhe würde das verfügbare Einkommen sinken,
                        allerdings hätten Sie möglicherweise Zugang zu mehr Dienstleistungen oder Sozialleistungen
                        des Staates.
                    </div>
                    <! Column for the image -->
                    <div class="col col-sm-3 col-12">
                        <!-- Image of the Soziale Partei Novaland -->
                        <img src="{% static 'SPN.png' %}" width="80%">
                    </div>
                </div>
            </div>
        <!-- Button to continue to the next page -->
        </div>

        <button>Weiter</button>

    </div>
    <!-- End of the if statement for the first party being Soziale Partei Novaland -->
    {% endif %}

    <! Checks if the first party is the "Konservative Partei Novaland" -->
    {% if first_party == "Konservative Partei Novaland" %}

    <!-- Content for the Konservative Partei Novaland (same logic as above with some replacements -->
    <div class="content-box">
        <!-- Title of the page, adjusted to the Konservative Partei Novaland -->
        <h1 class="h1"> Konservative Partei Novaland.<br>Tag 9 von 9</h1>
        <div class="text-image-container">
            <div class="text-content">


                <div class="row">
                    <div class="col col-sm-9 col-12">
                        <!-- Text about the Konservative Partei Novaland -->
                        <p class="p">Die KPN (Konservative Partei Novaland) will Steuern und Abgaben senken, auch wenn das
                            weniger
                            sozialstaatliche Leistungen bedeutet. Sie will Menschen in die Lage versetzen,
                            eigenverantwortlich Lösungen für ihre Probleme zu suchen. <br><br>

                            Für Menschen in Novaland mit Ihrer Einkommenshöhe würde das verfügbare Einkommen steigen,
                            allerdings hätten Sie möglicherweise Zugang zu weniger Dienstleistungen oder Sozialleistungen
                            des Staates.

                        </p>
                    </div>
                    <div class="col col-sm-3 col-12">
                        <!-- Image of the Konservative Partei Novaland -->
                        <img src="{% static 'CPN.png' %}" width="80%">
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col">
                    <button>Weiter</button>
                </div>
            </div>
        </div>

    </div>

    {% endif %}

    {% endblock %}

The same code is repeated for the **party_2** page, with the only difference being that the text and image of the parties are swapped, depending on which party was presented first. This means that if the value of the :code:`first_party` variable is **Soziale Partei Novaland**, the text and image of the **Konservative Partei Novaland** are displayed on the **party_2** page, and vice versa.
The images of the parties are stored in the :code:`static` folder of the oTree project, so that they can be accessed by the HTML template using the :code:`{% static %}` tag. The name of the image files are **SPN.png** for the **Soziale Partei Novaland** and **CPN.png** for the **Konservative Partei Novaland**.

4. The election ballot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
After participants were introduced to the two parties, they were directed to the **voting_page** page. On this page, they could cast their vote for one of the two parties. This page follows a similar logic as the party pages, also embedding randomized orders and conditional statements to display the correct party name and image. It is more complex, however, including clickable images and buttons that participants could use to cast their vote. And a table-like layout to display the ballot.

The layout of the page is structured like so: As always, the header of the page is defined in the :code:`h1` element, which contains the current day and the day count in Novaland. The main text below explains that participants can now vote for one of the two parties. The information about which of the two parties is the incumbent party is also displayed here, using :code:`{{ incumbent_party }}` again.
Below this explanatory text, a table with multiple columns is created that contains the party image and the party name. Participants could click on either parties' image or name below the image to vote for that party. On the bottom of the page there is a button that participants can click to continue to the next page.

This is what the ballot page looks like:

.. image:: /_static/ElectionScreenshot.jpeg
   :width: 75%
   :align: center
   :alt: Vignette Screenshot with repeated ending



This is how the **election_page** page is created:

In page class of the init file, the :code:`vars_for_template` method is used to pass the information about the incumbent party (:code:`incumbent_party`) and the identifier of the first party (:code:`first_party`) to the HTML template. The :code:`Partei` variable from the :code:`Player` object is used to store the party that the participant voted for.

In the HTML template, there is a style block at the top that define the appearance of the buttons and the table layout in which the parties' images and names are displayed. After that, the title of the page is defined in the :code:`h1` element, which contains the current day and the day count in Novaland. The main text below explains that participants can now vote for one of the two parties. The information about which of the two parties is the incumbent party is also displayed here, using :code:`{{ incumbent_party }}` again.

Below this paragraph, a table with multiple columns is created that contains the party images and the party namen. Participants could click on either parties' image or name below the image to vote for that party. The images of the parties are displayed using the :code:`img` element, which uses the :code:`{% static %}` tag to access the images stored in the :code:`static` folder of the oTree project, as it was done on the previous pages. The names of the parties are displayed in a paragraph element below the images. The order of the parties (which party is displayed in the left and which in the right column) is determined by the value of the :code:`first_party` variable. If the value is **Konservative Partei Novaland**, the **Konservative Partei Novaland** is displayed in the left column and the **Soziale Partei Novaland** in the right column, and vice versa.

The following code snippet shows how the parties are displayed in the HTML template. This code is repeated for both parties, with the only difference being the party name and image:

.. code-block:: HTML
   :linenos:

   <!-- Display the image of the Konservative Partei Novaland -->
   <img src="{% static 'CPN.png' %}" width="30%" class="center" onclick="SHOWCPN()">
    <!-- Display the name of the Konservative Partei Novaland -->
   <p class="p" style="text-align: center"><input type="radio" id="wahlen1" name="Partei"
                                                  required
                                                  onclick="SHOWCPN()"
                                                  value="Konservative Partei Novaland"> Konservative Partei Novaland </p>

Here, the :code:`onclick` attribute is used to call the :code:`SHOWCPN()` function when participants click on the image or the party name. This function is defined at the bottom of the HTML page and is used to make the images of the party logos clickable and to store participants' data as soon as they submit the page. The code for the **Soziale Partei Novaland** looks the same, only the file name of the image and the party name are changed to **SPN.png** and **Soziale Partei Novaland**, respectively and the :code:`onclick` attribute calls the :code:`SHOWSPN()` function instead of the :code:`SHOWCPN()` function.

The functions that are called as soon as a party's image or name is clicked are defined at the bottom of the HTML template. These functions are used to make the images of the party logos clickable and to store participants' data as soon as they submit the page:

.. code-block:: HTML
   :linenos:

   {% block script %}
   <script>
      <!-- Function to show the next button and set the vote to "Konservative Partei Novaland" -->
       function SHOWCPN() {
           <!-- Set the radio button for the Konservative Partei Novaland to checked -->
           document.getElementById("wahlen1").checked = true
           <!-- Show the submit button -->
           document.getElementById("weiter-btn").style.display = "inline-block";
           <!-- Set the value of the submit button to the name of the Konservative Partei Novaland -->
           <!-- Meaning that their vote is cast for this party as soon as they click on the next button -->
           document.getElementById("weiter-btn").value = "Konservative Partei Novaland";
       }
      <!-- Function to show the next button and set the vote to "Soziale Partei Novaland" -->
       function SHOWSPN() {
           <!-- Set the radio button for the Soziale Partei Novaland to checked -->
           document.getElementById("wahlen3").checked = true
           <!-- Show the submit button -->
           document.getElementById("weiter-btn").style.display = "inline-block";
           <!-- Set the value of the submit button to the name of the Soziale Partei Novaland -->
           <!-- Meaning that their vote is cast for this party as soon as they click on the next button -->
           document.getElementById("weiter-btn").value = "Soziale Partei Novaland";
       }
   </script>
   {% endblock %}

These functions are used to display the next button when participants click on the party images or names. The :code:`SHOWCPN()` function is called when participants click on the image or name of the **Konservative Partei Novaland**, while the :code:`SHOWSPN()` function is called when they click on the image or name of the **Soziale Partei Novaland**. The functions set the value of the :code:`weiter-btn` button to the name of the respective party and make it visible. This ensures a consistent user experience, as participants can click on either the party image or the party name to cast their vote.


5. The referendum
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


6. The debrief from Novaland
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


