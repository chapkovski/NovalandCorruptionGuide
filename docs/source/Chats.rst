======================
Chat
======================
In Otree, it is possible to chat with other participants within an app or phase using Otree's built-in chat function.

To integrate the chat on an HTML page, simply place this line where you want the chat to appear:

.. code-block:: console

    {{ chat channel=1 }}

Using channels, you can also allow participants to join different chats.
For this, you only need to assign the correct number to the channel.


Activating at Session Start
==========================================
To ensure that the chat is not enabled in every run of the session, it can be activated or deactivated using a session variable.
To do this, we add the variable to the SESSION_CONFIGS:

.. code-block:: console

    SESSION_CONFIGS = [
        dict(
            ...
            activate_chat=False,
        )
    ]

Now, to activate the chat using this session variable, you need to insert this condition into the project:

.. code-block:: console

    {% if player.session.config.activate_chat %}
        {{ chat channel=1 }}
    {% endif %}

Opening and Closing Chat with a Button
==========================================
To allow participants to decide whether they want the chat open or closed, you can control the chat using a button.
Therefore, we add a function to a button that opens and closes the chat if it is already open.

The button code looks like this:

.. code-block:: console

    {% block content %}
    {% if player.session.config.activate_chat %}
        <div class="column8">
            <button class="button" type="button" id="chat" onclick="Chat()">Open Chat</button>
        </div>
    {% endif %}
    {% endblock %}

And the corresponding function is as follows:

.. code-block:: console

    {% block script %}
    <script>
        let chatopen = 0;
        var element = document.querySelector("#openChat");
        function Chat() {
            if (chatopen == 0) {
                document.getElementById("chat").textContent = "Close Chat";
                element.style.display = "block";
                chatopen = 1;
            } else {
                document.getElementById("chat").textContent = "Chat";
                element.style.display = "none";
                chatopen = 0;
            }
        }
    </script>
    {% endblock %}


Chat Template
=====================
For better organization, there is a template available for the chat that can be inserted.

.. code-block:: console

    {% extends '_templates/global/StandardChat.html' %}

If the path should be different for you, please make the necessary changes.

The template itself includes the essential functions and also the "Next" button to save space.

.. code-block:: console

    <div  style="alignment: center" id="openChat">
    {{ chat channel=1 }}
    </div>
    <div class="column-container">
        <div class="column7">
            <button class="button" type="submit" id="weiter-btn">Weiter</button>
        </div>
        {% if player.session.config.activate_chat %}
        <div class="column8">
            <button class="button" type="button" id="chat" onclick="Chat()">Open Chat</button>
        </div>
        {% endif %}
    </div>
    <h1 class="h1"></h1>