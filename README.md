## About this repository

This is a collection of personal notes for software usage, e.g. for version control
systems, virtual environment usage, etc.

## Contents
[Virtual environments](https://github.com/mankuch/tool-notes/#virtual-environments)
[Version control Git](https://github.com/mankuch/tool-notes/#version-control-git)

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

## Version control Git
Gir is a distributed version control system. Everybody developer has a local
repository (backup/copy) on their machine, a central repository is optional.
Possible scenarios are e.g., one wants to start tracking an existing project
on the machine or one wants to start developing on an existing remote project.

### Most important commands
```bash
git init                         # Initialize a git repository
git status                       # Check staged files
git add <file>                   # Add changed file to stage
git commit -m "<comment>"        # Add commit
git pull                         # Pull repository branch
git push                         # Push current staged files
```

### Configure git
```bash
git config --global user.name "First last"          # Set user name
git config --global user.email "email adress"       # Set user email
git config --list                                   # Lists configuration values
```

### Get help
```bash
git help config                   # Shows help for a command
git config --help                 # Same as above
```

### Basics (First example)
```bash
git init                # Run this from within the project folder to start tracking
ls -la                  # Shows additional .git directory, contains everything that is
                        # related to the git repo -> If one wants to stop tracking that
                        # project with git, only delete that repo with rm -rf .git
git status              # Shows status of tracking

touch .gitignore        # .gitignore file contains private file that shouldn't be tracked
                        # wildcards can be used, e.g. *.pyc, folders can also be added
```

In git, three states exist:
1. Working directory (contains untracked changed files, listed with git status)
2. Staging area (organizes files/folders that we plan to commit -> make multiple commits to organize commit better)
3. .git directory/repository

```bash
git add <file>                                # Adds file to staging area
git *                                         # Adds everything to the staging area
git add -A                                    # Adds everything to the staging area
git reset <file>                              # Removes file from staging area
git reset                                     # Removes everything from staging area
git commit -m "specify changes to code"       # Commit staged files with a message
git status                                    # Now there are no untracked files, therefore the working directory is clean
git log                                       # Shows commit with its unique hash, author and date

git clone <url> <path_to_clone>               # General cloning command
git clone ../remote_repo.git .                # Clones all files from remote_repo.git dicectory to .

git remote -v                                 # View information about repository
git branch -a                                 # Shows local and remote branches

git pull origin master                        # Always pull before push, people could have changed the code in the
                                              # meanwhile origin is just the name of the repository, master is the
                                              # branch we want to push to
git push origin master                        # Pushes local changes to repo, so other people have access to it

git branch <name>                             # creates branch with the name "name"
git branch                                    # lists branches (local?)
```

### Merging a branch
```bash
git checkout <master>                         # Switch to branch <master>
git pull origin master                        # Get changes
git merge <branch> master                     # Merges <branch> and master
git branch --merged                           # Shows merged branch
git push origin master
git branch -d <branch>                        # Branch can now be deleted
```
