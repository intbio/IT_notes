 # Configuring working Python environment using Sublime Text 2, Anaconda and Anaconda.
+ Install Sublime Text editor https://www.sublimetext.com
+ Here is a cheat-sheet https://gist.github.com/paulovera/4486672
+ Command-Shift-P - is the access to command prompt and all cool functionality.
+ Control + \` - access command line
+ https://packagecontrol.io - is the repo of all cool packages for sublime.
+ Here is how to install package controll and use it https://www.granneman.com/webdev/editors/sublime-text/packages/how-to-install-and-use-package-control/
+ To turn Sublime into Python IDE you need to install Anaconda (do not confuse with Continuum Conda!) http://damnwidget.github.io/anaconda/
+ You need to be able to launch Sublime from the command line, so it will inherit at least the working directory.   
Linux: sudo ln -s /opt/sublime/sublime_text /usr/bin/subl      
OS X: ln -sv "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/subl
+ Now the tricky part:
 1. Tools -> Build or Command+B will execute your Python script, but how to specify what Python Env it uses?
