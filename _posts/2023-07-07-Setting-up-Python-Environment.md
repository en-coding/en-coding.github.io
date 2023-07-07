# Setting Up Python Environment

### Eijy Nagai

2023-07-07

Step-by-step guide to setting up Python environment on Ubuntu Server.



Dealing with machine learning algorithms often involves working with Python - a powerful, flexible, and popular programming language. Whether you're doing your computations on your local system or on an old workstation you've converted into a server, setting up a robust Python environment is key to productive work. Instead of letting the valuable resource of an old machine gather dust, why not turn it into your own development server?

Here, we'll walk through the process of setting up Python on an Ubuntu server. We'll also look at setting up a virtual environment, which allows you to create an isolated space for your Python projects, each with its own set of dependencies.


## 1 Installing Python
First, we need to make sure that Python is installed on your server. Open your terminal and type the following command:

`python3 --version`

If Python is installed, this command should return its version. If not, install Python by typing:

`sudo apt install python3`


## 2 Installing Pip
Pip is a package manager for Python. It allows you to install and manage additional packages that are not part of the Python standard library. To install pip, type:

`sudo apt install python3-pip`


## 3 Setting Up Virtual Environment
Next, we need to set up a virtual environment for our Python projects. This allows you to isolate your projects and avoid package conflicts. Install the venv module by typing:

`sudo apt install python3-venv`

To create a new virtual environment, navigate to your project directory and type:

`python3 -m venv env`

This command creates a new directory called env where the virtual environment files are stored.


## 4 Activating Virtual Environment
To activate the virtual environment, type:

`source env/bin/activate`

Your terminal prompt should now start with (env), indicating that you're inside the virtual environment.


## 5 Installing Packages
Once inside the virtual environment, you can install packages using pip. For example, to install the NumPy package, type:

`pip install numpy`

This installs the NumPy package inside your virtual environment.


## 6 Exiting Virtual Environment
When you're done working in your virtual environment, you can deactivate it by typing:

`deactivate`

That's all! You've successfully set up a Python environment on your Ubuntu server.



## Useful commands

`python3 --version` # Check Python version

`sudo apt install python3` # Install Python3

`sudo apt install python3-pip` # Install pip

`sudo apt install python3-venv` # Install venv

`python3 -m venv env` # Create virtual environment

`source env/bin/activate` # Activate virtual environment

`pip install numpy` # Install package inside virtual environment

`deactivate` # Deactivate virtual environment
