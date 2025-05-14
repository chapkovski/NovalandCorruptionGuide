.. _election-page:

======================
Election
======================
Elections in Novaland play a significant role as they have an impact on the income of participants.
In Novaland, parties are introduced and each participating person can choose a party to vote for.
In this chapter, we will discuss all the elements that deal with the elections.

Introduce Parties
==========================
To introduce the parties, we show a text on the party with the corresponding party logo to the participants.
Columns are used to ensure that the text is displayed next to the image.
These columns can be modified in terms of style using CSS code on the page or in the stylesheet.
The following CSS code can be used to style the columns:

.. code-block:: console

    {% block style %}
    {% load staticfiles %}
    <link href="{% static 'NovalandStyle.css' %}" rel="stylesheet" type="text/css" />
        <style>
        .column1{
            float: left;        # Determines which side the column should be aligned to
            width: 70%;         # Percentage of the screen width
            padding: 10px;      # The amount of space to be left around the object
            height: 200px;      # The height of the column
    }
        }
       .column2{
            float: right;
            width: 30%;
            padding: 10px;
            height: 200px;
        }
        </style>
    {% endblock %}

With this CSS code, the text can be placed in one column and the image in the other column.

.. code-block:: console

    <h2> Social Party Novaland </h2>
    <hr>
    <br>
    <div class="row">
        <div class="column1">
            <p class="p"> The SPN (Social Party Novaland) wants <strong>more social benefits, even if that means higher taxes and levies.</strong>
                <br>
    The incomes of people with <strong>high incomes should decrease by about 5%, while the incomes of people with <strong>low incomes should increase by about 10%</strong>.</strong>.

                <br>
                The SPN wants to <strong>restrict the opportunities for foreigners to immigrate to Novaland.</strong></p>
                <br><br>
            <button class='button' type='button' onclick='Next()'> Next </button>
        </div>
        <div class="column2">
            <img src="{% static 'phase_3/SPN.png' %}" width="80%">
        </div>
    </div>

    This code displays the Social Party Novaland (SPN) and their policies.
    The text provides an overview of the party's political stance and their plans for the country.
    The image shows the party's logo or a representative image of the party.
    By using columns and CSS code, the page layout is structured, and the text and images are presented in a visually appealing way.

Basic code for selecting a political party.
===========================================
To select a political party as a participant, once all the parties have been introduced, we provide an overview of all the parties using rows, which display the parties in a 2 x 2 grid. If you require more or fewer parties, this can be customized accordingly.
To create a user-friendly interface, we add a radio button to each image. When you click on the image, the corresponding radio button is selected.
The following code creates an HTML page with four images of party logos and radio buttons for selecting a party. The images are arranged in two rows with two columns each, with each column containing a logo and an associated radio button.

Election HTML Code:

.. code-block:: console

    {% block content %}
    # First Row
    <div class="row">
        <div class="column3">       # First Party
            <img src="{% static 'Party1PNG' %}" width="30%" class="center" onclick="Party1()">      # Party Image 'Party1PNG' from the static folder + 'onclick' function 'Party1()'
            <p class="p" style="text-align: center"> <input type="radio" id="election1" name="Party" value="Party 1"> Party 1 </p>      # Radio button with the id 'election1', the name 'Party and the value 'Party 1'
        </div>

        <div class="column3">
            <img src="{% static 'Party2PNG.png' %}" width="30%" class="center" onclick="Party2()">
           <p class="p" style="text-align: center"> <input type="radio" id="election2" name="Party" value="Party 2">  Party 2 </p>
        </div>
    </div>
        <br>
    # Second Row
    <div class="row">
        <div class="column3">
            <img src="{% static 'Party3PNG.png' %}" width="30%" class="center" onclick="Party3()">
            <p class="p" style="text-align: center"> <input type="radio" id="wahlen3" name="Party" value="Party 3">  Party 3 </p>
        </div>

        <div class="column3">
            <img src="{% static 'Party4PNG.png' %}" width="30%" class="center" onclick="Party4()">
            <p class="p" style="text-align: center"> <input type="radio" id="wahlen4" name="Party" value="Party 4">  Party 4 </p>
        </div>
    </div>
    {% endblock %}

    {% block script %}
    <script>    # Javascript are to define the functions
        function Party1(){
            document.getElementById("Election1").checked = true
        }
        function Party2(){
            document.getElementById("Election2").checked = true
        }
        function Party3(){
            document.getElementById("Election3").checked = true
        }
        function Party4(){
            document.getElementById("Election4").checked = true
        }

    </script>
    {% endblock %}


Code Explanation
________________________
We will now discuss individual elements from the code to ensure better understanding.
If you have understood everything in the above code, you can skip this subsection.

Radio Buttons
___________________________
To allow participants to choose between the parties, we create radio buttons that can be clicked by the participants.
These buttons contain the names of the parties to choose from.

.. code-block:: console

    <p class="p" style="text-align: center"> <input type="radio" id="election1" name="Party" value="Party 1"> Party Name 1 </p>
    <p class="p" style="text-align: center"> <input type="radio" id="election2" name="Party" value="Party 2"> Party Name 2 </p>

The input field is set to type 'radio' so that it gets the design and functionality of a radio button.
To link the radio buttons together so that only one option can be selected, the 'name' attribute must be the same.
Additionally, the 'name' attribute links the selection to the backend form field, which must have the same name.
The 'value' is the content that will be transferred to the 'Party' form field.

Clickable pictures
______________________
To link the selection function to the images, we give the images an 'onclick' attribute to trigger a JavaScript function.

.. code-block:: console

    <img src="{% static 'Party1PNG' %}" width="30%" class="center" onclick="Party1()">

This JavaScript function, triggered by the onclick function, triggers the corresponding radio button.

.. code-block:: console

    If the radio button looks like this:
        <p class="p" style="text-align: center"> <input type="radio" id="election1" name="Party" value="Party 1"> Party Name 1 </p>

    Then this is the desired function for it:
        function Party1(){
                document.getElementById("election1").checked = true
            }


Save the election value in the backend
=========================================
To save the selection of participants, we simply use a form field with the term 'Party' in the backend, in the init.py file.

init.py:

.. code-block:: console

    class PageName(Page):
        form_model = 'player'
        form_fields = ['Party']

In the HTML Election code, we selected a value named 'Party' to trigger exactly this variable in the backend and store the information there.
It is important that the name of the input field is exactly the same as the name in the 'form_field'.

Get the values of all participants
______________________________________________________

After all participants have voted, these values must be stored in the SESSION FIELDS.
This is to make sure that all elected parties are in one place so that they can be counted.

.. code-block:: console

    class Phase_4_Page_10(Page):
        @staticmethod
        def vars_for_template(player: Player):
            try:
                player.session.PARTY = player.session.PARTY + player.Party
            except KeyError:
                player.session.PARTY = player.Party


To begin with, we take the session field file containing the party votes and count the number of votes for each party within the session field.
From this, we can calculate the percentage of each party, which is important to display the results visually for the participants.

Next, we create a list that includes the order of the participants' votes.

.. code-block:: console

    AllVotes = player.session.PARTEI

    Party1Votes = AllVotes.count("Party1")
    Party2Votes = AllVotes.count("Party2")
    Party3Votes = AllVotes.count("Party3")
    Party4Votes = AllVotes.count("Party4")

    Party1Percent = (Party1Votes / AllVotes) * 100
    Party2Percent = (Party2Votes / AllVotes) * 100
    Party3Percent = (Party3Votes / AllVotes) * 100
    Party4Percent = (Party4Votes / AllVotes) * 100

    ElectionList = sorted([Party1Percent, Party2Percent, Party3Percent, Party4Percent])

These values are the basis for the visual representation and for the further course of the study since these values have an influence on the formation of government in Novaland.
These variables can now be saved.

Visual representation
=========================
In order to present the results we have decided to use a diagram and dynamic text.

Dynamic Text
____________________________

Using the election results, various dynamic texts can be displayed based only on the final outcome.
To achieve this, if functions can be easily used.

.. code-block:: console

    HTML Example:
    {% if Party1Percent > 10 %}
    Party 1 has reached more than 10 percent
    {% endif %}

    Python Example:
    if Party1Votes + Party2Votes > Party3Votes + Party4Votes:
        Party1andParty2 = "Party1 and Party2 have more votes"

Diagram
_________________

This diagram shows participants the result of the election in Novaland.

The code below defines a layout consisting of a box containing four bars representing the vote share of each party.
The four skills are Party1, Party2, Party 3, and Party4. Each bar has a name centered below it.

The box is relatively positioned and horizontally and vertically centered.
The skills are arranged using flexbox, so that they are evenly distributed across the available space.
In this context, "skills" refers to the four political parties that are being represented in the diagram.

The bar graphs are styled using the .graph selector.
Each bar graph has an absolute positioning command and a percentage value that specifies the height of the bar. The bars also have a background color and a foreground color created by a linear gradient.

The percentage values are displayed in a centered text element with a black font.
The name of the skill is also centered below the bar graph and has a black background and white text.

CSS definition:

.. code-block:: console

    # section defines a container that spans the entire width and height of its parent container.
    .section{
             width: 100%;
             height: 100%;
         }

    # .box defines a container for the four skills and their corresponding bar charts.
    # It is positioned relatively, so it is centered horizontally and vertically using the transform property.
    # It also has a background color, border, and a fixed height of 300 pixels.
         .box{
             position: relative;
             top: 50%;
             left: 50%;
             transform: translate(-50%, -50%);
             width: 100%;
             height: 300px;
             background: transparent;
             border-bottom: 1px solid #000;
             border-left: 1px solid #000;
             display: flex;
         }

    # .box .skill defines a container for each skill and its bar chart.
    # It is set to flex and has a flex: 1 property to distribute the available space evenly among the four skills.
         .box .skill {
             position: relative;
             flex: 1;
             text-align: center;

         }

    # box .skill .graph defines the container for the bar chart and its associated styles.
    # It has absolute positioning, with a width of 20% and a bottom position of 0 so it aligns with the bottom of the skill container.
    # It also has a background color and a linear gradient to give the bar chart a gradient effect.
         .box .skill .graph{
             position: absolute;
             width: 20%;
             bottom: 0;
             background: rgba(0,0,0,.1);
             left: 50%;
             transform: translateX(-50%);
         }

    # box .skill .graph:before and .box .skill .graph:after define the top and bottom layers of the linear gradient applied to the bar chart.
    # They are also positioned absolutely to fill the entire bar chart container.
         .box .skill .graph:before{
             content: '';
             position: absolute;
             top: 2px;
             left: 1px;
             right: 1px;
             bottom: 0;
             background: linear-gradient(0deg, #000000,#200000 );
         }

         .box .skill .graph:after{
             content: '';
             position: absolute;
             top: 2px;
             left: 1px;
             right: 1px;
             bottom: 0;
             background: linear-gradient(0deg, #000000,#200000 );
         }

    # box .skill .graph .percent defines the text element that displays the percentage value for the bar chart.
    # It is positioned absolutely above the bar chart, centered horizontally using the transform property, and styled with a black font color and bold font weight.
         .box .skill .graph .percent {
             position: absolute;
             top: -20px;
             left: 50%;
             transform: translateX(-50%);
             test-align: center;
             color: black;
             front-weight: bold;
         }

    # box .skill .name defines the text element that displays the skill name below the bar chart.
    # It is positioned absolutely below the bar chart, centered horizontally using the transform property, and styled with a black font color, white background color, and rounded border.
         .box .skill .name{
             position: absolute;
             bottom: -30%;
             left: 50%;
             transform: translateX(-50%);
             text-align: center;
             color: black;
             padding: 1px 3px;
             border-radius: 4px;
         }


Use the CSS in HTML Area to define the diagram:

.. code-block:: console

    # Here the different CSS classes are called, which rely on each other to display the diagram
    <section class="section"> #
        <div class="box">

            # Party 1
            <div class="skill"> # The individual skills are shown here as bars
                <div class="graph" style="height:{%Party1Percent%}%">   # This area defines the size of the bar based on the percentage
                    <div class="percent"> {% Party1Percent %}%</div>    # The visual representation of the percentage
                </div>
                <div class="name"> Party1 </div> # Shown name below the bar
            </div>

            # Party 2
            <div class="skill">
                <div class="graph"style="height:{%SPNProzent%}%">
                    <div class="percent"> {% SPNProzent %}%</div>
                </div>
                <div class="name"> SPN </div>
            </div>

            # Party 3 & 4 uses the same schema
            ...
    </section>