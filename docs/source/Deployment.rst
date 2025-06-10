Deploying the oTree project
=====================================

Deploying locally for testing
----------------------------------

For testing purposes, you can deploy the oTree project locally on your computer. This allows you to run the project in a controlled environment and test its functionality before deploying it to a production server.
To deploy the oTree project locally, follow these steps:

1. Install oTree on your local machine by following the instructions in the `oTree documentation <https://otree.readthedocs.io/en/latest/install.html>`_.

2. Clone the Novaland project repository from GitHub to your local machine.

3. Navigate to the project directory in your terminal.

4. Run the command :code:`otree devserver` to start the local server.

5. Open your web browser and go to :code:`http://localhost:8000` to access the oTree project.

6. You can now test the functionality of the project and make any necessary changes before deploying it to a production server.

Deploying the oTree project to a production server is essential for making it accessible to participants and collecting data.
To deploy the oTree project to a production server, you can use various hosting platforms such as Heroku, AWS, or DigitalOcean. In this documentation, we will focus on deploying the project on Heroku, which is a popular Platform-as-a-Service (PaaS) solution that simplifies the deployment process.

Deploying on Heroku
----------------------------------

Heroku is a Platform-as-a-Service (PaaS) solution that allows developers to host, manage, and scale applications
in the cloud.
Heroku supports a variety of programming languages and provides an easy, fast, and reliable way to deploy applications
without having to worry about infrastructure.

To deploy the oTree project on Heroku, follow these steps:

1. **Create a Heroku account**: If you don't have a Heroku account, sign up for one at :code:`https://www.heroku.com/`.

2. **Install the Heroku CLI**: Download and install the Heroku Command Line Interface (CLI) from :code:`https://devcenter.heroku.com/articles/heroku-cli`.

3. **Login to Heroku**: Open your terminal and run the command :code:`heroku login` to log in to your Heroku account.

4. **Create a new Heroku app**: Run the command :code:`heroku create <your-app-name>` to create a new Heroku app. Replace :code:`<your-app-name>` with a unique name for your app.

5. **Add the Heroku Postgres add-on**: Run the command :code:`heroku addons:create heroku-postgresql:hobby-dev` to add a free Postgres database to your app.

6. **Configure environment variables**: Set the necessary environment variables for your app by running the following commands:
    .. code-block:: text
        :linenos:

        heroku config:set ADMIN_USERNAME=<your-admin-username>
        heroku config:set ADMIN_PASSWORD=<your-admin-password>
        heroku config:set SECRET_KEY=<your-secret-key>
        heroku config:set OTREE_PRODUCTION=True

   Replace :code:`<your-admin-username>`, :code:`<your-admin-password>`, and :code:`<your-secret-key>` with your desired values.

7. **Push your code to Heroku**: Make sure you have a Git repository set up for your oTree project. Then, run the following commands to push your code to Heroku:
    .. code-block:: text
        :linenos:

        git add .
        git commit -m "Deploy oTree project to Heroku"
        git push heroku main

8. **Migrate the database**: Run the command :code:`heroku run otree migrate` to apply any database migrations required by your oTree project.

9. **Open your app**: After the deployment is complete, you can open your app by running the command :code:`heroku open` or by visiting :code:`https://<your-app-name>.herokuapp.com` in your web browser.

10. **Access the oTree admin interface**: You can access the oTree admin interface by visiting :code:`https://<your-app-name>.herokuapp.com/admin` in your web browser. Use the admin username and password you set earlier to log in.

After completing these steps, your oTree project will be deployed on Heroku and accessible to participants.

Your Project on Heroku
----------------------

Overview of your App:
^^^^^^^^^^^^^^^^^^^^^^
The overview of a Heroku app shows the following information:

    - App name and its unique URL
    - Application status (running or crashed)
    - Dynos (number of web or worker processes running)
    - Add-ons (list of any installed add-ons)
    - Collaborators (list of people who have access to the app)
    - Last release (when the app was last updated and the status of the update)
    - Metrics (resource usage of the app, including memory, CPU, and bandwidth usage)

Open your project
^^^^^^^^^^^^^^^^^^^^^^^^
Just click on the 'Open app' Button at the top right of the screen.


Monitoring
------------------------------
At Heroku, there are several options for monitoring applications.
The Heroku Dashboard allows real-time monitoring of application status, displaying error messages and warnings to quickly respond to issues.
Additionally, Heroku provides the ability to monitor various metrics such as CPU usage, memory consumption, and network activity.
Monitoring add-ons such as "papertrail" can be used to provide detailed performance data of applications.
This helps identify potential bottlenecks and problems early on to ensure smooth application operation.

Resources
----------------------

In the context of Heroku, resources refer to the various add-ons and services that you can use to extend the functionality of your application. Some of the common resources on Heroku include:

    1. Databases: Heroku provides several options for managed databases, including PostgreSQL, MySQL, and MongoDB.
    2. Analytics: You can use resources such as Heroku Metrics, Logs, and Monitoring to track the performance of your application and identify any issues.

By adding these resources to your Heroku app, you can enhance the functionality and performance of your application, making it easier to manage and scale.

Add a ressource to your app
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

    1. To add an add-on, go to the "Resources" tab in your Heroku app dashboard and search for the add-on you want to use.
    2. Then, simply click on the add-on to add it to your app.

Heroku Postgres
------------------
Postgres on Heroku is a managed database service provided by Heroku that enables you to run your PostgreSQL database in the cloud.
PostgreSQL is an open-source relational database management system, and the Postgres service on Heroku provides a fully managed, scalable, and secure database solution for applications hosted on the Heroku platform.

With Postgres on Heroku, you can manage and scale your database, ensuring that your application always has the necessary resources to handle high traffic and large amounts of data.
Additionally, Heroku provides several tools and services to monitor, backup, and recover your PostgreSQL database, making it a reliable and scalable option for your database needs.

Save the participant data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you have added Heroku Postgres as an add-on to your app, it will serve as the database for storing all the variables and functions associated with your oTree project.
The naming of these variables is determined within your PyCharm project or another development environment and cannot be altered in the Heroku Postgres database.

Download the participant data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
You can download a backup of your database directly from the Heroku Dashboard.

    1. Go to the "Resources" tab in your app dashboard
    2. Select the Heroku Postgres database
    3. click on the "Backups" section
    4. then, click on the "Download" button to download the latest backup


Reset the Database
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you need to reset your Heroku Postgres database, you can do so easily through the Heroku Dashboard.
Resetting the database will permanently erase all data stored within it.

To reset your Heroku Postgres database, follow these steps:

    1. Go to the "Resources" tab in your app dashboard.
    2. Select the Heroku Postgres database.
    3. Click on the "Settings" tab.
    4. Scroll down to the "Danger Zone" section and click on the "Reset Database" button.
    5. Confirm that you want to reset the database by typing the name of your Heroku app in the confirmation field.
    6. Click on the "Reset" button.

After you reset your Heroku Postgres database, it will be empty and ready for you to start adding data again.


Papertrail
----------------------------
Papertrail is a great tool that you can use to manage logs for your applications.
With Papertrail's centralized log management, you can easily find and fix issues without having to search through multiple log sources.
Using Papertrail, you can quickly and conveniently view all of your Heroku logs in one place.

Sentry
--------------------------
By integrating Sentry with Heroku, you can receive notifications about any errors or performance issues that occur within your Heroku-hosted applications.
This way, you can resolve any issues before they affect your users, ensuring that your applications run smoothly and efficiently.

Metrics
----------------------
Metrics of an app on Heroku can provide valuable insights into the performance and state of your application.
With this information, you can identify and resolve issues before they become serious problems for your users.

How to read and interpret the metrics of your Heroku app to optimize and improve your application.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Overview of Metrics:
Heroku offers a variety of metrics that you can access through the dashboard or the Command Line Interface (CLI).
Some of the key metrics include:

    1. Web and Worker Dyno Metrics: This metric indicates how many dynos are active for your web and worker tests.
    2. HTTP Requests: This metric indicates how many HTTP requests your application has processed.
    3. Memory Usage: This metric indicates how much memory your application has used.
    4. CPU Utilization: This metric indicates how much CPU power your application has used.

Understanding these metrics can help you to monitor and improve the performance of your Heroku app.
By regularly reviewing these metrics, you can identify and address any issues before they impact your users, ensuring that your application runs smoothly and efficiently.