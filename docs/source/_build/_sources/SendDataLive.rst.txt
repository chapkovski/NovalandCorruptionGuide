================================
Send Data with a live Method
================================
The live_method feature allows for real-time updates to be sent from the server to the participant's browser without requiring a full page refresh. To use live_method, data must first be sent using livesend.
The data that is sent via livesend can be received by the live_method function, which can then trigger a function in the backend that can have an impact on the participant.

livesend
__________________________
livesend is an oTree method that allows you to send data from the server to the client side in real time.
It can be used, for example, to update the display of a page or to trigger a JavaScript function on the client side.
The livesend method takes two arguments: a string that identifies the type of data being sent, and the actual data to be sent.
The data can be in any format, such as a string, integer, list, or dictionary.

.. code-block:: console

    liveSend({"Variable": "Value"})


We are creating a scenario in which a query is made on an HTML page and it is transmitted live to the init.py page.
However, the init.py page immediately sends a signal back so that the page can continue, for example.

HTML:

.. code-block:: console

    <input type="radio" onclick="functionNameForYes()" class="radio"> Yes </input>
    <input type="radio" onclick="functionNameForNo()" class="radio"> No</input>


The 'type' attribute is set to "radio" to indicate that they are radio buttons.
The 'onclick' attribute is used to call JavaScript functions when the radio button is clicked.

The first radio button has the label "Yes" and the onclick attribute is set to call a JavaScript function called functionNameForYes().
The second radio button has the label "No" and the onclick attribute is set to call a different JavaScript function called functionNameForNo().

Both radio buttons have a class attribute with a value of "radio", which can be used for styling or selecting these elements using JavaScript or CSS.

To trigger the functions, they must be included as JavaScript code between two script tags.
It's important that the name of the JavaScript function matches the name of the function we determined to trigger at the top.

.. code-block:: console

    <script>
    function functionNameForYes(){
        liveSend({"QuestionAnswer": "Yes"})
    }

    function functionNameForNo(){
        liveSend({"QuestionAnswer": "No"})
    }
    </script>

Now we have written a JavaScript function that sends data to init.py when a button is pressed on the HTML page.

live_method
__________________________
he live_method feature allows for real-time updates to be sent from the server to the participant's browser without requiring a full page refresh.
To use live_method, data must first be sent using livesend.
The data that is sent via livesend can be received by the live_method function, which can then trigger a function in the backend that can have an impact on the participant.

.. code-block:: console

    class pageName(Page):
        @staticmethod
            def live_method(player: Player, data):
                if "QuestionAnswer" in data:
                    player.Question = data["QuestionAnswer"]


The method takes two arguments: 'player' and 'data'.
The 'player' argument is an instance of the 'Player' class in oTree and represents the participant whose browser is sending the update.
The data argument is a dictionary containing the data sent from the participant's browser.

The method checks if the "QuestionAnswer" key is in the data dictionary.
If it is, then the method sets the 'Question' attribute of the 'player' instance to the value associated with the "QuestionAnswer" key in the data dictionary.

LiveRecv
_________________________________
To send information back to the live participant page, you can create a LiveRecv function in the HTML page that can receive the data from the backend or live_method.

Here's an example of a LiveRecv function:

.. code-block:: console


    function liveRecv(data) {
            if (data['type'] == 'NextPage') {
                document.getElementById("form").submit();
            }
        }

This function takes a single parameter called data, which is expected to be an object that contains information sent from the server to the client browser.
In this example, the function checks if the type key in the data object is equal to the string 'NextPage' using an if statement.
If the condition is true, the function calls the submit() method of the HTML form element with the ID "form".
This causes the browser to submit the form and move to the next page of the experiment.

To define the data that needs to be sent, it is already defined in the live_method in the init.py file.
If we want to send data based on the example above, we can write the information in the live_method in the init.py file.

Here's an example:

.. code-block:: console

    class pageName(Page):
            @staticmethod
            def live_method(player: Player, data):
                return {0: dict(type='NextPageData')}

In this example, we've given the data value on the participant page the variable 'NextPageData' in the live_method.
This sends the page in the example above to the next page.

So in summary, livesend is used to send data from the server to the client side in real time, live_method is used to receive this data and trigger a function in the backend, and LiveRecv is used to receive data back from the backend and define how it should be used on the client side.