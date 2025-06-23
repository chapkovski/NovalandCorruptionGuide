The introduction to the Novaland stay
=========================================
The introductory part of the main app consists of four **NovalandIntro** pages, on which participants are informed about their stay in Novaland as citizens of the fictional country and about some characteristics of the country itself. The text that was included on these pages can be found in the :code:`main/instructions/` folder. There is one file for each of these four information pages.

On the fourth NovalandIntro page, the first experimental treatment takes place. Participants were randomly assigned to two groups: the first group is informed about the social norm of bribing in Novaland (6 out of 10 citizens of Novaland are willing to pay a bribe for a service). Participants in the second group do not receive any information. The variable that determines if participants received this information is called :code:`corruption_info` and is stored in the :code:`Participant` object. The variable is set to 1 if participants received the information and 0 if they did not. This variable is used to determine which text is displayed on the page and is passed to the HTML template (:code:`main/instructions/instructions_4.html`). There, the conditional statement of showing the social norm information works with this variable: :code:`{{ if participant.vars.corruption_info == 1 }}`.

The next page is the **Comprehension** page. On this page, participants had to answer three comprehension questions correctly in order to continue with the questionnaire. The questions are about the information that was provided on the previous pages. The questions are defined in the player models :code:`comprehension_citizen`, :code:`comprehension_income`, and :code:`comprehension_financing`. These three questions are defined as :code:`form_fields` in the :code:`Comprehension` page class and called by the page's HTML template using :code:`{{formfields}}`.

We gave participants the possibility to receive the respective pieces of information again using an "instructions" button that was embedded on the page. For this, we used the :code:`live_method` in the init file and the following code in the HTML template to dynamically animate the window with the text from the :code:`main/instructions/` HTML files that were also used to display the information on the previous pages. The code runs as soon as participants click on the instructions button:

.. code-block:: html
    :linenos:

    <script>
        $(document).ready(function () {
            $('#instruction_button').click(function () {
                console.debug('button clicked');
                liveSend('button_clicked');
            });
        });
    </script>

Participants had two attempts to answer all comprehension questions correctly. If they tried to submit the page with incorrect answers, the :code:`error_counter` increases and an error message appears. The page is then reloaded. They are then informed that they have one attempt left. If they submit the page again with incorrect answers for the second time, they will be filtered out. We also set a timeout counter on this page to prevent participants from spending too much time on it. If they exceed the time limit, they were also filtered out. The timeout counter is set to five minutes, which is more than enough time to answer the questions.

If participants failed to answer the comprehension questions correctly two times, or if they exceeded the time limit, they were next redirected to the **TerminationPage**. This page is not shown to participants, but is only used for background functionality. Only participants who fulfill one of these two criteria are redirected to this page, the other ones skip it, as defined in

.. code-block:: python
    :linenos:

    @staticmethod
    def is_displayed(player: Player):
        return player.error_counter > 1 or player.comprehension_timeouted

As soon as participants reached this page, they were directly redirected using a link provided by the panel provider, defined in :code:`TERMINATION_LINK`. Participants were then informed that they failed to comply with the requirements of the study and were thus filtered out.

Those participants who answered all three comprehension questions correctly, reached the last page of the introduction to Novaland. On the **IntroVignette** page, participants were informed that they will encounter several situations on the next pages, that they are a citizen of Novaland in this context and that they should answer the questions as themselves.