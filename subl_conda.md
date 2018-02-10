 # Configuring working Python environment using Sublime Text 2, Anaconda and Conda.
+ Install Sublime Text editor https://www.sublimetext.com
+ Here is a cheat-sheet https://gist.github.com/paulovera/4486672
+ Command-Shift-P - is the access to command prompt and all cool functionality.
+ Control + \` - access command line - this is a Python terminal.
+ https://packagecontrol.io - is the repo of all cool packages for sublime.
+ Here is how to install package controll and use it https://www.granneman.com/webdev/editors/sublime-text/packages/how-to-install-and-use-package-control/
+ To turn Sublime into Python IDE you need to install Anaconda (do not confuse with Continuum Conda!) http://damnwidget.github.io/anaconda/
+ You need to be able to launch Sublime from the command line, so it will inherit at least the working directory.   
Linux: sudo ln -s /opt/sublime/sublime_text /usr/bin/subl      
OS X: ln -sv "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/subl     
Then `subl .` will do what we want.

+ As a python environment manager let's use miniconda, install it from here https://conda.io/miniconda.html (usually you'd what Python 2.7)
   - Here is how to manage conda environments https://conda.io/docs/user-guide/tasks/manage-environments.html
   - `conda env list`
   - `conda create --name myenv`
   - `source activate myenv`
   - `conda install ...`
   - `source deactivate`
+ Now the tricky part:
   - Tools -> Build or Command+B will execute your Python script, but how to specify what Python Env it uses?
   - By default, Sublime will use the python interpreter that is in your PATH (see `which python`)
   - Even if run from command line, Sublime does not inherit the PATH variables - it reads it directly from .bash_rc (?)
   - Here is a discussion https://stackoverflow.com/questions/44006122/launch-sublime-text-3-from-terminal-and-build-with-activated-conda-environment
   - So one way is to:
      - Preferences->Package Settings->Anaconda->Settings-User and add `{"python_interpreter": "/usr/bin/python"}` choosing the desired path to python
      - In Tools-> Build System choose Anaconda Python Builder
      - But this is global
   - Another way is per project
      - Project -> Edit Project and add the settings section with correct path. shell_cmd might also be needed (?)
      ~~~
      {
    "build_systems":
    [
        {
            "name": "Anaconda Python Builder",
            "selector": "source.python",
            "shell_cmd": "/home/damnwidget/.virtualenvs/anaconda/bin/python -u \"$file\""
        }
    ],
    "folders":
    [
        {
            "follow_symlinks": true,
            "path": "."
        }
    ],
    "settings":
    {
        "python_interpreter": "/home/damnwidget/.virtualenvs/anaconda/bin/python"
        ...
    }
}
      ~~~
