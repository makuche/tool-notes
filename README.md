## About this repository

This is a collection of personal notes for software usage, e.g. for version control
systems, virtual environment usage, etc.

## Virtual environments

### Creating environment from scratch
```bash
virtualenv test_env                         # Creates a folder, containing a virtual environment
source test_env/bin/activate                # Activates that environment
which python3                               # Prints location of Python executable (can also test which pip3)
pip3 list                                   # List installed packages
pip3 install <PACKAGE-NAME>                 # Install package
pip3 freeze --local > requirements.txt      # Writes all installed packages to .txt file. With that, environment can be easily recreated
deactivate                                  # Deactivate environment
```

### Create environment from package list
```bash
virtualenv -p /usr/bin/python3 test_env     # Optional: Use certain python version
pip3 install -r requirements.txt            #  # Uses requirements.txt inside environment folder to install packages
deactivate
```

### Misc
- If one uses environments, it's recommended to keep project files and the environment
folder separately. With that, the environment can be deleted and easily recreated
from the requirements.txt file.
- ```pip3 list``` lists all globally installed packages.
- To install an external package, either ```python3 setup.py install``` or
```pip3 install packagename.tar.gz``` can be used. Installation with pip is
recommended, as this is more beneficial:
  - Pip automatically installs dependencies for a package, with ```setup.py``` this
  has to be done manually
  - Pip keeps track of metadata, with which commands like ```pip3 uninstall <PACKAGE>```
  ```pip3 install --upgrade <PACKAGE>``` can be used, with ```setup.py``` this has
  to be done manually
  - Files don't have to be downloaded, Pip searches the Python Package Index (PyPi)
  to see if package exists there and then downloads, extracts and installs from there.
  - It becomes easy to install [wheels](https://pythonwheels.com/).
  - Can be integrated well with ```virtualenv```, so multiple projects with
  conflicting library requirements and/or python versions can be used on computer.

## Version control

