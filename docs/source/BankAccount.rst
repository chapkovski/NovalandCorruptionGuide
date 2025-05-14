======================
Bank Account
======================
In Novaland, the bank account is not stored in the usual way in oTree due to the fact that the various values from each phase or app are needed repeatedly to make other calculations.
To enable the bank account to be used in different phases, participants fields are used.

Dynamic income
------------------------------
The bank account is based on dynamic income, which is assigned to the participant at the beginning of the game, but may change depending on events in Novaland.
Based on this, decisions about lifestyle and other things can be made.
In each phase, a new variable for the account balance is created, allowing access to previous account balances at a later stage.

.. code-block:: console

    @staticmethod
    def vars_for_template(player: Player):
        if player.participant.CSVIncome == 0:
            player.Brutto_Einkommen = 2000
        elif player.participant.CSVIncome == 1:
            player.Brutto_Einkommen = 2850
        elif player.participant.CSVIncome == 2:
            player.Brutto_Einkommen = 5000

The net income is derived from the gross income:

.. code-block:: console

    player.Netto_Einkommen = player.Brutto_Einkommen - ((player.Brutto_Einkommen / 100) * 30)
        if player.Netto_Einkommen == 1995:
            player.Netto_Einkommen = 2000
        return {
            "Brutto_Einkommen": player.Brutto_Einkommen,
            "Netto_Einkommen": player.Netto_Einkommen
        }

Dynamic decisions
__________________________________
As a result of the dynamic decisions that can be made within Novaland, the balance changes.
Therefore, all information is consolidated at the end of the last page of an app, resulting in a new variable that can be used in the next app.

.. code-block:: console

    player.participant.NewBankBalance = player.Netto_einkommen - decision1 - decision2 - ...
