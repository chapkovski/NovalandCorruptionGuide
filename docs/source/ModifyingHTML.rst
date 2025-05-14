==================================
Modifying HTML with JavaScript
==================================

The section of an HTML document that needs to be modified can be specified using the <div> attribute.
The code that requires modification should be enclosed within the <div> tags and assigned a unique ID.

For instance:

.. code-block:: console

    <div id='uniqueID'>
        # your code
    </div>

To change the content of this area, we can use a JavaScript function based on the example given below:

.. code-block:: console

    document.getElementById('uniqueID').innerHTML = 'Your change';

This code will select the element with the ID 'uniqueID' and change its content to 'Your change'.

As an example, a button can be defined which changes the text on the page when clicked:

.. code-block:: console

    {% block content %}
    <div id='uniqueID'>
        Button is not pressed
    </div>

    <br>
    <button class='button' type='button' onclick='changeFunction()'> Change Text </button>
    {% endblock %}

    {% block script %}
    function changeFunction() {
        document.getElementById("uniqueID").innerHTML = 'Button is pressed'
    }
    {% endblock %}

The above code defines a button that, when clicked, changes the text within the <div> tag with the ID 'uniqueID' from "Button is not pressed" to "Button is pressed" by invoking the JavaScript function 'changeFunction()'.

Optional Novaland Example
____________________________
Based on the above example, we want to show another example of the Novaland project to explain the usefulness of this function in more detail.

This example is much more detailed and represents a scenario in which the participant is asked to choose a type of meal.
Based on their account balance and their choice, the displayed text for the participant changes.
The code provided includes HTML and JavaScript for displaying the options and updating the account balance based on the selected choice.

.. code-block:: console

    {% block content %}
    <p class='p'>
    You have different options for meals to choose from:
    </p>
    <div id="Korrektur">
        <input type="radio" name="Question_4_MealChoice" onclick="Restaurant()" class="radio" value="Restaurants"> You regularly eat in restaurants (400 Novas) </input>
            <br>
        <input type="radio" name="Question_4_MealChoice" onclick="OrderFood()" class="radio" value="Essen bestellen"> You frequently order food to your home (300 Novas) </input>
            <br>
        <input type="radio" name="Question_4_MealChoice" onclick="CookAtHome()" class="radio" value="Selber kochen"> You buy groceries cheaply and cook at home (200 Novas) </input>
            <br>
    </div>
    <div id="BankAccount">
        <p class="p">
            <br>
    Your account balance is currently: <strong>{% NewValue2 %} Novas</strong>.
        </p>
    </div>
    {% endblock %}

    {% block script %}
    <script>
    let NetIncome = js_vars.NewValue;
    let MealCosts = 0;
    let Account = 0;

        function Restaurant(){
            MealCosts = 400;
            Account = NetIncome - MealCosts;
            if (Account >= 150){
                document.getElementById("BankAccount").innerHTML = "<p class='p'>" + "<br>" + "Your account balance is still: " + "<strong>" + Account + " Novas" + "</strong>" + "</p>" + "<br>" + "<button class='button' type='button' onclick='Next()'> Next </button>";
            }
            if (Account < 150){
                document.getElementById("BankAccount").innerHTML = "<p class='p'>" + "<br>" + "Your account balance is still: " + "<strong>" + Account + " Novas" + "</strong>" + "<br>"
                    + "Since you have other expenses, you cannot afford this type of meal, unfortunately." + "</p>";
                }
            }

        function OrderFood() {
            MealCosts = 300;
            Account = NetIncome - MealCosts;
            if (Account >= 150) {
                document.getElementById("BankAccount").innerHTML = "<p class='p'>" + "<br>" + "Your account balance is still: " + "<strong>" + Account + " Novas" + "</strong>" + "<br>" + "</p>" + "<button class='button' type='button' onclick='Next()'> Next </button>";
            }
            if (Account < 150) {
                document.getElementById("BankAccount").innerHTML = "<p class='p'>" + "<br>" + "Your account balance is still: " + "<strong>" + Account + " Novas" + "</strong>" + "<br>"
                    + "Since you have other expenses, you cannot afford this type of meal, unfortunately." + "</p>";
            }
        }

         function CookAtHome(){
             MealCosts = 200;
             Account = NetIncome - MealCosts;
            if (Account >= 150) {
                document.getElementById("BankAccount").innerHTML = "<p class='p'>" + "<br>" + "Your account balance is still: " + "<strong>" + Account + " Novas" + "</strong>"  + "<br>" + "</p>" + "<button class='button' type='button' onclick='Next()'> Next </button>";
            }
            if (Account < 150) {
                document.getElementById("BankAccount").innerHTML = "<p class='p'>" + "<br>" + "Your account balance is still: " + "<strong>" + Account + " Novas" + "</strong>" + "<br>"
                    + "Since you have other expenses, you cannot afford this type of meal, unfortunately." + "</p>";
            }
        }

        function Next(){
            document.getElementById("form").submit();
        }
    </script>
    {% endblock %}