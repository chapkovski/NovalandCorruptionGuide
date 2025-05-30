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

4. The election ballot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The layout of the page is structured like so: As always, the header of the page is defined in the :code:`h1` element, which contains the current day and the day count in Novaland. The main text below explains that participants can now vote for one of the two parties. The information about which of the two parties is the incumbent party is also displayed here, using :code:`{{ incumbent_party }}` again.
Below this explanatory text, a column is created that contains the party image and the party name. Participants could click on either parties' image or name below the image to vote for that party. On the bottom of the page there is a button that participants can click to continue to the next page.

This is what the ballot page looks like:

.. image:: /_static/ElectionScreenshot.jpeg
   :width: 75%
   :align: center
   :alt: Vignette Screenshot with repeated ending



This is how the **party_1** page is created:
At the top of the HTML template, some styling is applied to the buttons. Also, the style settings of the table formatting of the page are defined.



At the bottom of the HTML page, the following functions are defined. They are called when the participants click on the buttons or on the images of the parties:

.. code-block:: HTML
   :linenos:

   {% block script %}
   <script>
       function SHOWCPN() {
           document.getElementById("wahlen1").checked = true
           document.getElementById("weiter-btn").style.display = "inline-block";
           document.getElementById("weiter-btn").value = "Konservative Partei Novaland";
       }

       function SHOWSPN() {
           document.getElementById("wahlen3").checked = true
           document.getElementById("weiter-btn").style.display = "inline-block";
           document.getElementById("weiter-btn").value = "Soziale Partei Novaland";
       }
   </script>
   {% endblock %}

These functions are used to display the buttons for the respective parties when participants click on the party images. The :code:`SHOWCPN()` function is called when participants click on the image of the **Konservative Partei Novaland**, while the :code:`SHOWSPN()` function is called when they click on the image of the **Soziale Partei Novaland**. The functions set the value of the :code:`weiter-btn` button to the name of the respective party and make it visible.


5. The referendum
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


6. The debrief from Novaland
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


