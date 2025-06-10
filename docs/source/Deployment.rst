Deploying the oTree project
=====================================

Deploying locally
----------------------------------

ö

Deploying on Heroku
----------------------------------

Heroku is a Platform-as-a-Service (PaaS) solution that allows developers to host, manage, and scale applications
in the cloud.
Heroku supports a variety of programming languages and provides an easy, fast, and reliable way to deploy applications
without having to worry about infrastructure.

Open Heroku:

    1. Open your browser and go to `Heroku <https://www.heroku.com>`_
    2. Create an account or login to your account.
    3. Follow the steps to zip your Novaland project -> data-collection-page
    4. Select your Novaland Project zip.

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