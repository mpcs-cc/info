---
layout: page
title: "Working with Python in MPCS 51083"
---

## Working with Python

In this class we will work in Python for all homework assignments and our capstone project. While it is not required, I recommend that you use one of the many available Python tools (such as [pipenv](https://pipenv.pypa.io/en/latest/) or [virtualenv](https://virtualenv.pypa.io/en/latest/) and [virtualenvwrapper](https://pypi.org/project/virtualenvwrapper/)) to set up Python virtual environments. We’ll be experimenting a lot and the isolation provided by virtual environments helps minimize configuration conflicts/issues. In order to be most productive in class, please take time before the course starts to set up a working Python environment on your laptop. Coursework must be completed using Python 3.7 or later.

_Note: If you use Anaconda as your Python development environment, I strongly suggest you set it aside for this class and use Python directly._

Below is a walkthrough of installing and using Python virtual environments on macOS (10.15 or later). If you’re not using macOS, please check out your respective Hitchhiker’s guide:

Windows: http://docs.python-guide.org/en/latest/starting/install3/win/<br />
Linux: http://docs.python-guide.org/en/latest/starting/install3/linux/

### Using Python on Amazon EC2 instances

You can choose to work on assignments on Amazon EC2 virtual machine instances instead of on your local machine (this will increasingly be the case later in the course). All our instances have pre-configured Python environments that can be activated by running: `workon mpcs`.

## macOS Setup using virtualenv

IMPORTANT NOTE: The process below should work in most scenarios, but there is a good chance that you’ll run into issues if your current macOS configuration is different from the default. If that happens, please try to resolve it by consulting online resources before asking on Ed; it’s very difficult to troubleshoot these issues without poking around on your machine :-), so we will likely ask you to visit us during office hours ...and even then we may not be able to get things working without a lot of effort, leaving you with less time to work on your assignments.

We first install the `pip` package manager and then use that to install other required packages. I use [homebrew](https://brew.sh/) to bootstrap the process. If you don’t already have it, install homebrew thus:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```
Then check that we’re ready to brew:

`$ brew doctor && brew update`

I prefer to override Apple’s default python and install a fresh copy:

$ brew install python3

Depending on your operating system version, running python on the macOS command line may, by default, start a Python 2.x shell (the default for older MacOS versions), and pip will run the Python2 version of pip. You can ensure you’re running Python3 and the associated pip3, by forcing the system path to look for python in its new home.

Add the following to your ~/.zshrc file and source it (or open a new shell): 

      export PATH=/usr/local/opt/python/libexec/bin:$PATH

Note that there are some differences in how the homebrew installer works if your Mac has Apple silicon, i.e., you’re running macOS 11 or later. In this case you may need to set the path as follows (and optionally, alias the python executable):

      export PATH=/opt/homebrew/bin:/opt/homebrew/sbin:$PATH
      alias python="python3"  # optional

Now when you run python or pip you will be using version 3 by default.

The brew command above also conveniently installed pip, so now we can install virtualenv and virtualenvwrapper thus:

$ pip install --upgrade pip
$ pip install --upgrade virtualenv
$ pip install --upgrade virtualenvwrapper
$ export WORKON_HOME=~/.virtualenvs
$ source /usr/local/bin/virtualenvwrapper.sh

Note: The WORKON_HOME environment variable tells virtualenvwrapper to store your environments in .virtualenvs/ under your home directory; change this location as you see fit.

Again, for later macOS versions you may need slightly different commands above:

$ export WORKON_HOME=~/.virtualenvs
$ export VIRTUALENVWRAPPER_PYTHON=/opt/homebrew/bin/python3
$ source /opt/homebrew/bin/virtualenvwrapper.sh

Let’s test that everything is ready. Source a new shell and create a virtualenv called ‘test’:

$ mkvirtualenv test
created virtual environment CPython3.9.12.final.0-64 in 216ms
  creator CPython3Posix(dest=/Users/...
...
(test) $ _

Once the virtualenv is created it will be automatically activated, and your shell prompt will show the name of the virtualenv in parentheses. Now, install the Python requests package as a test:

(test) $ pip install --upgrade requests

Check the packages installed in the current virtualenv; You should see the requests package listed:

(test) $ lssitepackages
__pycache__                        pip-22.2.2.virtualenv
_distutils_hack                    pkg_resources
_virtualenv.pth                    requests
_virtualenv.py                     requests-2.28.1.dist-info
...

Start an interactive python shell and check that you can import the requests package:

(test) $ python
Python 3.9.10 (main, Jan 15 2022, 11:40:53)
...
>>> import requests
>>> 

If all that works, you should be good to go. You can now deactivate and remove the test virtualenv thus:

(test) $ deactivate
$ rmvirtualenv test
Removing test...
$ _

Note: If you have multiple virtualenvs, you can activate the desired virtualenv using the workon command, e.g. workon mpcs would activate the “mpcs” virtualenv.



To ensure new shells have the right environment add these commands to ~/.zshrc:

export WORKON_HOME=~/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh

Or for later macOS versions:

export WORKON_HOME=~/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/opt/homebrew/bin/python3
source /opt/homebrew/bin/virtualenvwrapper.sh
Kickin’ it up a Notch!
Using the direnv package, you can configure your system to automatically activate the appropriate virtualenv when you cd into a directory, and deactivate it when you change to another directory. First, install the package:

$ brew install direnv

Then add the following to ~/.zshrc:

setopt PROMPT_SUBST
show_virtual_env() {
  if [[ -n "$VIRTUAL_ENV" && -n "$DIRENV_DIR" ]]; then
    echo "($(basename $VIRTUAL_ENV)) "
  fi
}
PS1='$(show_virtual_env)'$PS1
eval "$(direnv hook zsh)"

Create a file called .envrc in your source code directory, and add: 

source <path_to_env>/bin/activate
unset PS1

For example, if your virtualenvs are stored in ~/.virtualenv and you have an environment called “mpcs” your would add:

source ~/.virtualenvs/mpcs/bin/activate
unset PS1

Finally, in your source code directory run:

$ direnv allow
Now, when you cd into the directory with the .envrc file, it will automatically activate the virtualenv.

Of course, you can also still activate a virtualenv manually from anywhere in the filesystem by running workon <virtualenv_name>, for example:

$ workon mpcs #activate the mpcs virtual environment
(mpcs) $ _
Note: All of the above commands assume you’re using zsh, which is the default shell under Mac OS 10.15 (Catalina) and later. If you’re on a Mac OS version earlier than 10.15, the default shell is likely bash. The commands should work exactly the same way within a bash shell, but direnv will require a slightly different configuration. BUT, you really should upgrade to a more recent version since 10.14 is now past end-of-life, and hence potentially vulnerable.
Stylin’ with Python
If you’re new to Python, please spend a little time looking at the generally accepted best practice when it comes to coding style: https://www.python.org/dev/peps/pep-0008/

We may penalize you for sloppy code, and sloppy code will make us less inclined to give you an “O” ...reading other people’s clean code is hard enough ;-)

If you learn one Python thing, and one Python thing only from this class, let it be this: 

                                    Use spaces instead of hard tabs in your code.

You’re welcome! Seriously though, even if your homework code is correct you will be penalized for using tabs (or, even worse, mixing tabs and spaces) in your code. Most decent text editors have an option to convert tabs to spaces - set that option! And please indent by only 4 spaces. Yes, this may be nitpicking but clean source files are a very important aspect of professional software development so get used to that now. Again, failure to follow these simple requirements will result in an unsatisfactory grade. Don’t be a victim of styling laziness!

And one more thing… beware of copy-pasting weirdness. Many of the issues you encounter will be the result of copying and pasting code/commands from one of these Google documents or a PDF file. These document formats embed a lot of hidden characters for formatting and often transform punctuation into non-standard characters, e.g., “smart” quotes like these! If you see weird errors and are sure that your code/command is correct, check again by typing it manually instead of copy-pasting.

Virtual Environment Setup for Anaconda Users (NOT recommended)
For Anaconda users, this is a set of instructions created by a past student. It may be helpful if you decide you want to stick with conda instead of plain ol’ Python. Instead of using virtualenv, you can use the virtual environment that ships with conda. 

$ conda update -n base conda  # updates the base version of conda
$ sudo conda update --all  # updates all packages (can take a while) 

To create a new environment:

$ conda update --name mpcs
 
To activate the environment:

$ source activate mpcs 

To deactivate the environment:

$ source deactivate 

To install specific packages in that environment:

$ conda install --name mpcs <package_name>
 
Note the use of conda (instead of pip above) to install the required package. Where possible, if you are using anaconda, stick to using conda as your package manager. However, there are some packages that can only be installed via pip. In order to run pip inside the conda-created environment run the following:

$ conda install -n mpcs pip # once inside the env, use pip normally

Remember to upgrade pip once inside your environment with:
 
$ pip install --upgrade pip

To list the packages in a particular environment (use when outside environment):

$ conda list -n mpcs


To see packages installed in an environment, when inside that environment:

$ conda list

To install a package using conda-forge in an environment:

$ sudo conda install --name mpcs -c conda-forge <package_name>

Note the use of sudo in the above line of code. Sometimes, when installing packages, either locally or in an environment from conda or conda-forge, you will get permission denied errors; you can use sudo to overcome this.
 
To view all environments on your system:

$ conda info --envs

Alternatively, you may use:

$ conda env list

To remove an environment and all associated packages inside that environment:

$ conda remove --name mpcs --all

You will need to install a couple of packages for homework assignments; some of these cannot be installed using conda, so do the following (within your environment):

$ pip install --upgrade jmespath-terminal
$ sudo pip install awscli 

To install Boto3 (when outside your environment):

$ conda install -name mpcs boto3

