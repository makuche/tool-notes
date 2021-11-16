## About this repository

This is a collection of personal notes for software usage, e.g. for version control
systems, virtual environment usage, etc.

## Virtual environments

### Creating environment from scratch


```bash
virtualenv test_env                 # Creates a folder, containing a virtual environment
```
```bash
source test_env/bin/activate         # Activates that environment
```
```bash
which python3
```
```bash
pip3 list
```
```bash
pip3 install <PACKAGE-NAME>
```
```bash
pip3 freeze --local > requirements.txt
```
```bash
deactivate
```

### Create environment from package list
```bash
virtualenv -p /usr/bin/python3 test_env
```
```bash
pip3 install -r requirements.txt
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
TODO
