---
layout: post
title: "Containing Python Dependencies using Virtual Environment"
description: "An explanation on what virtual environments are, why everyone should use them and how to use them in Python projects."
date: 2019-01-19
tags: [python, virtual-environment]
comments: true
share: true
---

I'm constantly building my skills and knowledge in Machine Learning. It usually means spinning up new project directories and installing different packages for each of these projects. When you are working on multiple projects in parallel, managing the dependencies for the projects becomes a hassle.

All the dependencies that we download are installed into the system's Python environment. That means, if a new version of a package is downloaded then it updates the previous older version. The problem with that is packages may not be backward compatible and projects that depend on the older version stop working. Fortunately, there is a workaround for this problem - Virtual Environment.

# Introduction

Virtual environment is the concept of creating isolated Python project environments. These environments create and use their own Python executable file and Python packages. We can easily create a virtual environment for a Python project and install the required project packages inside the environment. Hence, not affecting any other project dependencies.

Virtual environments are a very handy tool for Python programmers. I have been using them for all my projects since I learned about it and I love it.

In this short tutorial, you can learn to use virtual environment for your own Python projects.

*Note: I'm using Python3.6 and Ubuntu 18.04 LTS. For Windows equivalent code check [Python doc.](https://docs.python.org/3/tutorial/venv.html){:target="_blank"}*

### Preliminary Steps

Install **virtualenv** package by running the command `pip install virtualenv`

*Note: Python 3 has **venv** module installed in the standard library.*

Before working in a virtual environment, let's create a project directory and navigate to inside the directory.

```bash
$ mkdir my_project
$ cd my_project/ 
```

Now we will create and use virtual environment for **my_project** with the following step-by-step guide.

### Creating a Virtual Environment

```bash
# For Python 2
$ virtualenv env

# For Python 3
$ python3 -m venv env
```

Running the above command will create our virtual environment.

`env` is the name of the virtual environment that we created. The name can be any arbitrary name. If we check our project directory, a new directory will be created with the same name as our virtual environment. It will have the following directory structure.

    ├── bin
    │   ├── activate
    │   ├── activate.csh
    │   ├── activate.fish
    │   ├── easy_install
    │   ├── easy_install-3.6
    │   ├── pip
    │   ├── pip3
    │   ├── pip3.6
    │   ├── python
    │   └── python3
    ├── include
    ├── lib
    │   └── python3.6
    │       └── site-packages
    └── pyvenv.cfg

As we can see, the **env** directory consists of Python executable file **python3** which will be used to run Python files in our virtual environment. The directory also consists of `lib/python3.6/site-packages`. All the packages that we will install, will be stored inside this directory instead of the system Python packages directory. Hence, the virtual environment.

The **bin** directory consists of `activate` script which we will use to activate the virtual environment.

### Activating the Virtual Environment

```bash
my_project$ source env/bin/activate
(env) $ 
```

The source command together with the path to `activate` script is used to activate the virtual environment. Upon successful activation, the name of our virtual environment will appear as a prefix in the terminal. 

You can run the `which python` command to check the Python source file. It should be pointing to the Python executable inside the **env** directory.

```bash
(env) $ which python
/home/awalesushil/my_project/env/bin/python
```

When you are not using the virtual environment, it will point to the system's Python executable.

```bash
$ which python
/home/awalesushil/anaconda3/bin/python
```

You can also check the Python package list. Only the base packages will show.

```bash
(env) $ pip list
pip (9.0.3)
setuptools (39.0.1)
```

### Installing Dependencies

```bash
# Install requests package
(env) $ pip install requests
```

You can install any Python packages inside the virtual environment. The packages will not overlap with the system's Python packages; containing the Python packages to your project environment.

You can check the `env\lib\python3.6\site-packages` folder. It should contain the newly downloaded package's resources.

### Extracting the Dependency List

```bash
(env) $ pip freeze > requirements.txt
```

You can also create a text file containing the list of all the dependencies used in your project (inside virtual environment). You can push this file to GitHub, instead of pushing the virtual environment directory and any other user can use the file to install all the dependencies in their own virtual environment.

### Installing Dependencies from text file

```bash
$ pip install -r requirements.txt
```

### Deactivating the Virtual Environment

```bash
(env) $ deactivate
$ 
```

You can deactivate the virtual environment by simply writing `deactivate` command in the terminal.

As you can see, virtual environment creates an isolated project environment. You can add, remove and update the project dependencies without affecting other projects in the system. It is a must have tool set for any Python programmer.

# References

1. [Virtual Environments and Packages - Python Docs](https://docs.python.org/3/tutorial/venv.html){:target="_blank"}
2. [Python Virtual Environments: A Primer - Real Python](https://realpython.com/python-virtual-environments-a-primer/){:target="_blank"}
3. [Taming Python Dependencies with Virtualenv - Jody LeCompte](https://medium.com/@jodylecompte/taming-python-dependencies-with-virtualenv-44bbafc57a48){:target="_blank"}