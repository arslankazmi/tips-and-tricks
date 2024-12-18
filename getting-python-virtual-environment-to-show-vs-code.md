# Getting VSCode "Select Kernel" to show New Python Virtual Environments

## Why

Sometimes when you create a new python virtual environment using the ```python -m venv``` command, it fails to show up in VS Code's "Select Kernel" option in the Jupyter Notebook interface.

## How

You can use a combination of commands to register the new environment.

> All commands will need to be run using the command palette in VSCode, accessible using ```CTRL/CMD + SHIFT + P```

1. Python: Select Interpreter : Navigate to your virtual environment folder and select your python interpreter at /bin/python


2. Developer: Reload Window :  Refreshing the window does not close your open tabs, but allows the jupyter ui to refresh and check for new kernels.


Now got to "Select Kernel" and your new environment should be showing in the list.
