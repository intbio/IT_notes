## Logging in
Replace USERNAME with your user name
### Via ssh 
`ssh USERNAME@newton.bioeng.ru`

### Via X2GO
* Install x2go [client](https://wiki.x2go.org/doku.php/download:start)
* Create new session

| Setting | Value |
|---------|-------|
| Session name | newton |
|host     |newton.bioeng.ru |
| login   | USERNAME  |
| session type| XFCE |
* Double click created session
* **On a first login wait until question about default panel settings arizes. Select default panels.**

#### Important X2GO Info:
You can run multiple sessions detach and resume them later. 
* To detach from a session just close X2GO window.
* To resume a session, choose it during login
* To finish session, click your user name (top right panel) and choose "Log out"

#### Troubleshooting
* If there are no desktop panels and icons open terminal (right click on a desktop, 'Open Terminal Here') and run this line:
`xfce4-panel --quit ; pkill xfconfd ; rm -rf ~/.config/xfce4/panel ~/.config/xfce4/xfconf/xfce-perchannel-xml/xfce4-panel.xml ; xfce4-panel`

Then select default panel settings.

### Via JupyterHub
Open [https://newton.bioeng.ru/jupyter](https://newton.bioeng.ru/jupyter) and log in.

### JupyterHub and conda
Firstly see [info about conda environments](conda.md) and [conda cheatcheet](https://conda.io/docs/_downloads/conda-cheatsheet.pdf).

In jupyter you can select different kernels. There are a couple of preinstalled kernels that contain different conda environments (see 'conda' tab in jupyter), those environments are located at /opt/miniconda3 and can not be chaneged by user.
To add a new kernel for your project you have to install `ipykernel` package to the environment:

0. Create or clone conda environment (skip it, if you already done that): conda create -n YOUR_ENV_NAME 
1. source activate YOUR_ENV_NAME
2. conda install ipykernel

### JupyterHub and JupyterLab
Currently jupyter notebooks are lunched by default.

To open JupyterLab change the last word in the link from /tree to /lab
