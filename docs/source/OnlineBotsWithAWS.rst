=========================================
Online Bots with Amazon Web Services
=========================================

Another method to unleash online bots on your server is by running the program on an Amazon Web Server.
This offers the advantage of harnessing significantly greater computational power from an external server, while also stress-testing and pushing the limits of your Heroku server's stability.
Moreover, this scenario closely simulates a real study environment.

Amazon Web Services
=================================

Amazon Web Services (AWS) is a cloud computing platform that provides a wide range of cloud-based services and resources.
One of its key services is Amazon Elastic Compute Cloud (EC2).
EC2 allows you to run virtual servers, known as instances, in the cloud.

With EC2, you can deploy various types of applications, including Python programs, on virtual machines without the need to invest in physical hardware.
EC2 instances can be configured with specific hardware, storage, and networking capabilities to suit your application's requirements.

Login or Create AWS Account
______________________________
Log in to your AWS account using your web browser. If you don't have an account yet, you can create one.
You'll receive an account number that you'll need for logging in.
Keep in mind that access to all AWS features is granted only after providing your credit card information.

Creating a virtual machine
================================
For our purpose, we require an Amazon Elastic Compute Cloud, also known as EC2.
This is where we can run our virtual machine.

So, navigate to the "Create a Solution" dashboard and click on "Launch a Virtual Machine."
You will now be on a page where you specify the basic performance and properties that the virtual machine should have.

.. image:: docs/source/_static/Doku1.png
  :width: 800

Give the instance a name or a tag.

Creating an Application and Operating System Image (Amazon Machine Image)
_____________________________________________________________________________
Our objective is to launch the instance on a Linux server.
Therefore, choose the Amazon Linux Server as your option.
For the AMI, select the available application that is free of charge.

Selecting AMI:

.. image:: docs/source/_static/Doku2.png
  :width: 800

Choosing Instance Type:
Similarly, opt for the no-cost instance type for this purpose.

.. image:: docs/source/_static/Doku3.png
  :width: 800

Creating Key Pair:

If you don't already have one, generate a new key pair.
This is essential for later use as a superuser on the server.
Being a superuser grants you all necessary rights to execute the Novaland bots there.

.. image:: docs/source/_static/Doku5.png
  :width: 800


A document will be downloaded, which you should save on your computer in a location where you can easily retrieve it.
Without it, you'll lose access to your key pair.


Security Group Rule:

Leave all values at default.

Connecting to instance
_______________________________
You can access the "EC2 Dashboard" by navigating to the "EC2" section under the "Console Home" dashboard.
Here, you'll find all the information about your existing instances.

.. image:: docs/source/_static/Doku13.png
  :width: 800

To access your already existing instances, click on the "Instances (Running)" option under Resources.
Select your desired instance and then click on the "Connect" button.

.. image:: docs/source/_static/Doku14.png
  :width: 800

Linus Terminal Surface
============================
Now, a Linux programming window from the Amazon server will open, providing us with the environment to work on and write our programs.

.. image:: docs/source/_static/Doku8.png
  :width: 800

Elevate access through user transition
____________________________________________
The command "sudo su" in the Linux terminal, including on AWS, opens a new shell session with root privileges to execute commands with elevated privileges.

.. code-block:: console

    sudo su

Update all packages
_____________________
Before we begin, let's update all packages or check if they are already updated using this command:

.. code-block:: console

    yum update

Install Python on the server
_____________________________________________
To check if Python has already been installed, simply enter:

.. code-block:: console

    python3

If that's not the case, install it using:

.. code-block:: console

    yum install python3

To install packages using Python, we also need the package management tool called pip.

.. code-block:: console

    yum install python3-pip


Installing our repository on the server
===============================================
To get our 'Novaland' repository onto the server, we need to start by installing Git.

Install Git
__________________
Enter the following command in the terminal to install Git:

.. code-block:: console

    yum install git

Confirm by typing 'y' to proceed with downloading all the necessary content.

Clone Repository
_________________________
You need to copy the link of the repository from GitHub.
This repository must be public in order to copy its files to the server.

Use the following command to clone the GitHub repository to your AWS server:

.. code-block:: console

    git clone https://github.com/User/YourRepository.git

You can use the 'ls' command to verify if the installation was successful.

Install all Requirements
_________________________________
Once you have installed the Novaland project on the server, you'll find all the necessary packages that have been installed within the 'requirements.txt' file.
If it's not present, you should add it.

To do this, we first need to navigate to the directory where our 'requirements.txt' file is located.
You can achieve this using the following commands:

.. code-block:: console

    cd YourApp

In our example:

.. code-block:: console

    cd Novaland


Then, execute this command to install all the packages listed in the 'requirements.txt' file:

.. code-block:: console

    python3 -m pip install -r requirements.txt

If you encounter installation issues with the 'psycopg2' package, install it as follows:

.. code-block:: console

    python3 -m pip install psycopg2-binary


If, at a later stage, you intend to run the program and not all packages are installed, the terminal will provide guidance on which packages are missing.
To install these, simply use the "pip install" command. For example:

.. code-block:: console

    pip install packagename

If you encounter issues with the 'db.sqlite3' file or any other file that needs to be removed, you can do so with this command:

.. code-block:: console

    rm db.sqlite3

Of course, you need to be in the correct directory using the 'cd' command to ensure that the correct file is removed.

Install Google Chrome on the Web Server
==============================================
We need Google Chrome on the AWS server to enable the execution of browser bots. Let's install it:

Command:

.. code-block:: console

    wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
    sudo yum localinstall google-chrome-stable_current_x86_64.rpm

The first command downloads the Chrome RPM package, and the second command installs it.
Confirm by typing 'y' to proceed with the installation.

If the BROWSER_COMMAND has not been defined as 'google-chrome' in the Settings.py file, you can do so afterwards.
Navigate using 'cd' to the folder containing the settings.py file. In the Novaland repository, for example, it would be in the Novaland folder.
Once there, enter the following to edit the file:

.. code-block:: console

    nano settings.py


A window will open where you can edit the settings.py file.
Add this line:

.. code-block:: console

    BROWSER_COMMAND = 'google-chrome'

Install 'xvfb' that you can run Google Chrome in the headless mode.

.. code-block:: console

    sudo yum install xorg-x11-server-Xvfb

Run Browser Bots on AWS
===============================
Now all installations are complete.
The final preparations are now underway before we can start the browser bots.

Heroku
_____________________
Um die Browser Bots bei Heroku laufen lassen zu können, muss dort ein Secret Key auf dem Server eingebaut werden.
Mehr Infos dazu findest dazu hier: :ref:`otree-rest-key`


.. _starting-browser-bots:
Starting Browser Bots
______________________
If you are still logged in as the root user, as we did at the beginning of this tutorial to install packages, you need to revert this now.
You can do so by restarting the terminal.
From now on, you will be a regular user again, allowing you to successfully execute the following commands.

We need to check beforehand whether the browser bots are enabled in the settings.py of Novaland.

To do this, we navigate to our project folder using 'cd', which contains the settings.py file, and enter this command:

.. code-block:: console

    nano settings.py

Search there for 'use_browser_bots', and if it's set to 'False', we change this value to 'True'.

.. code-block:: console

    use_browser_bots = True

Navigate to the folder containing the settings.py file.
In our project, this can be done using:

.. code-block:: console

    cd Novaland


Here, enter your :ref:`otree-rest-key` as an environment variable using 'export':

.. code-block:: console

    export OTREE_REST_KEY="Your-Secret-Key"

Now the environment variable is set. We can write the 'run otree browser bots' command, which runs the browser bots.
However, this command needs to include information that Google Chrome should be started in headless mode, as our server does not support graphical interfaces.
Therefore, we add 'xvfb' to our code.
The resulting command looks like this:

.. code-block:: console

    xvfb-run -a otree browser_bots Pretest_Novaland --server-url=https://your-heroku-project-name.herokuapp.com/room/Novaland

If the console indicates missing other packages, install them.
For example:

.. code-block:: console

    pip install requests
    pip install ws4py
    pip install email_validator
    pip install otree

Run the browser bot command again.
Now, this command will be executed.
You can open your app on Heroku and under 'Sessions', you will see a new session has been started, with the bots currently in progress.

Possible Errors
_________________________________
If the connection isn't working, you can try these steps:

Check if packages are missing:
Simply execute the steps of the chapter: :ref:`starting-browser-bots` mentioned above as an administrator.

Now, all packages that are not yet installed will be displayed.
Install them using 'pip,' refresh the page, and repeat the section :ref:`starting-browser-bots`.