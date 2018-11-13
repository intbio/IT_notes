# Configure a new mac
- Install http://brew.sh
- `brew install bash-completion`
- `brew install wget`
- Miniconda

## Scripts
```
sudo ls
cd /usr/local/bin
sudo wget -N https://raw.githubusercontent.com/intbio/IT_notes/master/scripts/gacp
sudo wget -N https://raw.githubusercontent.com/intbio/IT_notes/master/scripts/ll
sudo wget -N https://raw.githubusercontent.com/intbio/IT_notes/master/scripts/upd_ssh_keys
sudo wget -N https://raw.githubusercontent.com/intbio/IT_notes/master/scripts/conda_git_init
sudo chmod 755 ll
sudo chmod 755 gacp
sudo chmod 755 upd_ssh_keys
sudo chmod 755 conda_git_init
sudo upd_ssh_keys
echo "alias cga='source activate \${PWD##*/}'" >>~/.bash_profile
echo "alias cgs='conda env export > env.yml'" >>~/.bash_profile
echo "alias jl='jupyter lab" >>~/.bash_profile
echo "alias jn='jupyter notebook" >>~/.bash_profile
```



## Soft
- Sublime Text
   - `sudo ln -s '/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl' /usr/local/bin/subl`
   ```
   Fix Home and End keys
   Preferences > Key Bindings - User

Adding the following to the array;

{ "keys": ["home"], "command": "move_to", "args": {"to": "bol"} },
{ "keys": ["end"], "command": "move_to", "args": {"to": "eol"} },
{ "keys": ["shift+end"], "command": "move_to", "args": {"to": "eol", "extend": true} },
{ "keys": ["shift+home"], "command": "move_to", "args": {"to": "bol", "extend": true } }
   ```
- MS Office Education
- Mendeley
- Slack
