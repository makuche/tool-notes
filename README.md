## About this repository

This is a collection of personal notes for software usage, e.g. for version control
systems, virtual environment usage, etc.

## Virtual environments

### Creating environment from scratch


```bash
virtualenv test_env                         # Creates a folder, containing a virtual environment
```
```bash
source test_env/bin/activate                # Activates that environment
```
```bash
which python3                               # Prints location of Python executable (can also test which pip3)
```
```bash
pip3 list                                   # List installed packages
```
```bash
pip3 install <PACKAGE-NAME>                 # Install package
```
```bash
pip3 freeze --local > requirements.txt      # Writes all installed packages to .txt file. With that, environment can be easily recreated
```
```bash
deactivate                                  # Deactivate environment
```

### Create environment from package list
```bash
virtualenv -p /usr/bin/python3 test_env     # Optional: Use certain python version
```
```bash
pip3 install -r requirements.txt            #  # Uses requirements.txt inside environment folder to install packages
```
```bash
deactivate
```

### Misc
- If one uses environments, it's recommended to keep project files and the environment
folder separately. With that, the environment can be deleted and easily recreated
from the requirements.txt file.
- ```pip3 list``` lists all globally installed packages.

## Version control

