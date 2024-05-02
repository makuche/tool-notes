# About this repository

This is a collection of personal notes for software usage, e.g. for version control
systems, virtual environment usage, etc.

# Contents
[Virtual environments](https://github.com/mankuch/tool-notes/#virtual-environments)

[Version control Git](https://github.com/mankuch/tool-notes/#version-control-git)

[Linux commands](https://github.com/mankuch/tool-notes/#linux-commands)

[Starting and accessing HTTP Server](https://github.com/mankuch/tool-notes/#starting-and-accessing-http-server)

[Visual Studio Code](https://github.com/mankuch/tool-notes/#visual-studio-code)

[Debugging and exploratory programming](https://github.com/mankuch/tool-notes/#debugging-and-exploratory-programming)

[Profiling in Python](https://github.com/mankuch/tool-notes/#profiling-in-python)

[HTMX](https://github.com/mankuch/tool-notes/#htmx)

# Virtual environments

### Creating environment from scratch
```bash
virtualenv test_env                         # Creates a folder, containing a virtual environment
source test_env/bin/activate                # Activates that environment
source test_env/bin/activate.fish           # Activates that environment for fish shell
which python3                               # Prints location of Python executable (can also test which pip3)
pip3 list                                   # List installed packages
pip3 install <PACKAGE-NAME>                 # Install package
pip3 freeze --local > requirements.txt      # Writes all installed packages to .txt file. With that, environment can be easily recreated
deactivate                                  # Deactivate environment

# Installs ipykernel, so that 'test' kernel jupyter notebook can use kernel 'test'
# With that, jupyter notebook uses the libraries from the activated environment (instead of the globally installed libraries)
python3 -m ipykernel install --user --name=test
```

### Create environment from package list
```bash
virtualenv -p /usr/bin/python3 test_env     # Optional: Use certain python version
pip3 install -r requirements.txt            #  # Uses requirements.txt inside environment folder to install packages
deactivate
```

### Installing a project package
(The following is taken from 'Good Research Code' by Patrick Mineault).
It is convenient to use functions, classes or other objects across the project folder. For that, one can install a 'pip-installable' package in the following way:
#### 1. Create a `setup.py` file
```
from setuptools import setup, find_packages

setup(
    name=`src`,
    packages=find_packages()
)
```
#### 2. Create a `__init__.py` file
Create an empty `__init__.py` file in `src/`, e.g. by `touch src/__init__.py`. With that, the function `find_packages` can find our project-specific package.

#### 3. `pip install` the package
Install the package by calling
```
pip install -e .
```
#### 4. Using the package
Consider the function `hello_world` is implemented
in the package `src.py`. Then the function can be
called (within the activated environment) by
```
import src.hello_world
```
#### 3. (Optional) Change name of the package
```
mv src new_name
pip install -e .
```
### Misc
- If one uses environments, it's recommended to keep project files and the environment
folder separately. With that, the environment can be deleted and easily recreated
from the requirements.txt file.
- ```pip3 list``` lists all globally installed packages.
- ```setup.py``` is a python file. It's presence indicates that the module/package
has been packaged and distributed with Disutils, a standard way for distributing
python modules. It allows to easily install python packages. Just call
```python3 install .```, pip will then use the setup file (avoid calling
```setup.py```).
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

### Using a makefile for Python projects:

```Makefile
.PHONY: requirements test

.venv:
    python3 -m venv .venv

requirements:
    source .venv/bin/activate && \
        python3 -m pip install -r requirements.txt && \
        python3 -m pip install pytest

test: .venv requirements dev-requirements
    source .venv/bin/activate && \
        pytest

```


# Version control Git
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

git pull origin master                        # Always pull before push, people could have changed the code in the
                                              # Origin is the name of repository, master is branch we want to push to
git push origin master                        # Pushes local changes to repo, so other people have access to it

git branch <name>                             # Creates branch with the name "name"
git branch <name> <root-branch>               # Creates branch, based on existing branch
git branch                                    # Shows local branches
git branch -a                                 # Shows local and remote branches
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

### Rebase branch
```bash
git stash 
git fetch origin main:main
git rebase main
# Force with lease prevents pushing changes since the fetch, if someone else has pushed since then
git push --force-with-lease origin feat/test-branch    
git stash pop
```

# Linux commands
```bash
ln -s {source-filename} {symbolic-filename}             # Soft link named symbolic-filename, refering to the symbolic filepath
# Note that source-filename can also be a directory, e.g. such as
ln -s /home/test/http/users/manuel/ /app/
# Soft links serve as a reference to another file or directory. Hard links refer to the actual location of physical data.
```


# Starting and accessing HTTP Server
```bash
# Creates HTTP server that can be accessed in the same network (by calling IP address and port in browser, e.g. 127.0.0.1:8000)
# With that, files between computers in the same network can be exchanged
python3 -m http.server

# Prints information on access to internet. 1 is 'something historical' (can be ignored),
# 2 is ethernet cable, 3 (wl) is wifi: inet xxx.xxx.xxx.xx shows the IP address,
# 4 is internet access for the docker container
ip address
```

# Visual Studio Code
Useful shortcuts:
- `STRG+SHIFT+`\` opens terminal. If VS Code has opened folder, shell path is set to this folder.
- `STRG+ALT+N` can be used to execute a Python file

# Debugging and exploratory programming

```Python
for a in range(20):
    print(a)
    if a == 10:
        import IPython; IPython.embed()
```
The code opens an IPython promt that can be used to explore stages in a code. This is very useful to [learn new APIs or debug code](https://lukeplant.me.uk/blog/posts/repl-python-programming-and-debugging-with-ipython/).

## Available exceptions in Python
```
BaseException
 ├── BaseExceptionGroup
 ├── GeneratorExit
 ├── KeyboardInterrupt
 ├── SystemExit
 └── Exception
      ├── ArithmeticError
      │    ├── FloatingPointError
      │    ├── OverflowError
      │    └── ZeroDivisionError
      ├── AssertionError
      ├── AttributeError
      ├── BufferError
      ├── EOFError
      ├── ExceptionGroup [BaseExceptionGroup]
      ├── ImportError
      │    └── ModuleNotFoundError
      ├── LookupError
      │    ├── IndexError
      │    └── KeyError
      ├── MemoryError
      ├── NameError
      │    └── UnboundLocalError
      ├── OSError
      │    ├── BlockingIOError
      │    ├── ChildProcessError
      │    ├── ConnectionError
      │    │    ├── BrokenPipeError
      │    │    ├── ConnectionAbortedError
      │    │    ├── ConnectionRefusedError
      │    │    └── ConnectionResetError
      │    ├── FileExistsError
      │    ├── FileNotFoundError
      │    ├── InterruptedError
      │    ├── IsADirectoryError
      │    ├── NotADirectoryError
      │    ├── PermissionError
      │    ├── ProcessLookupError
      │    └── TimeoutError
      ├── ReferenceError
      ├── RuntimeError
      │    ├── NotImplementedError
      │    └── RecursionError
      ├── StopAsyncIteration
      ├── StopIteration
      ├── SyntaxError
      │    └── IndentationError
      │         └── TabError
      ├── SystemError
      ├── TypeError
      ├── ValueError
      │    └── UnicodeError
      │         ├── UnicodeDecodeError
      │         ├── UnicodeEncodeError
      │         └── UnicodeTranslateError
      └── Warning
           ├── BytesWarning
           ├── DeprecationWarning
           ├── EncodingWarning
           ├── FutureWarning
           ├── ImportWarning
           ├── PendingDeprecationWarning
           ├── ResourceWarning
           ├── RuntimeWarning
           ├── SyntaxWarning
           ├── UnicodeWarning
           └── UserWarning
```


# Profiling in Python
- Profiling useful to detect bottlenecks in the implementation
- Can be applied by `python3 -m cProfile -o out.prof script.py`
- Visualization either via
    - `snakeviz out.prof    # Interactive tool (includes runtimes)`
    - `gprof2dot -f pstats out.prof | dot -Tpng -o output.png   # Creates directed graph (no runtimes, resources in %)`
In an API, one can use Profiling e.g. via
```Python
def main(args=None):
    """The main routine..."""
    
    import cProfile, pstats
    profiler = cProfile.Profile()
    profiler.enable()
    
    ## HERE COMES THE CODE
    
    profiler.disable()
    stats = pstats.Stats(profiler)
    stats.dump_stats('out.prof')
```



# HTMX
TLDR: Library to access modern browser capabilities from HTML instead of Javascript.
- Htmx is a dependency-free Javascript library
- HTTP requests are sent asynchronously
- Extends HTML: Any _element_ (not only anchors and forms) can issue HTTP requests
- Any _event_ (not only clicks and form submissions) can issue HTTP requests
- All HTTP verbs can be used
- Servers typically respond with HTML instead of JSON
- Htmx requests re

Example for Migration from React to HTMX:
[react to htmx](https://htmx.org/essays/a-real-world-react-to-htmx-port/)
Summary:
- Took about 2 months (21K LOC, mostly JavaScript)
- No reduction in the application’s user experience (UX)
- Reduced the code base size by 67% (21,500 LOC to 7200 LOC)
- Increased backend (py) code by 140% (500 LOC to 1200 LOC)
- Reduced their total JS dependencies by 96% (255 to 9)
- Reduced their web build time by 88% (40 seconds to 5)
- First load time-to-interactive was reduced by 50-60% (from 2 to 6 seconds to 1 to 2 seconds)
- Much larger data sets were possible when using htmx, because react simply couldn’t handle the data
- Web application memory usage was reduced by 46% (75MB to 45MB)
