# Setting up with WSL

WSL can be installed from the windows store - try: 
![image](https://github.com/user-attachments/assets/2fc77a25-10a0-4a1c-8a44-5efb3658940e)


If you open it will bring up a command prompt and you enter credentials - the passowrd that you will use with sudo does not need to be particularly secure, it is not your main password.  

Pin the prompt you get to your taskbar.  You may need to right-click and select ubuntu to open the correct shell.

## VSCode
VSCode will install install itself. I suggest everythong goes into a dev directory, so:
```
cd ~
code . #vscode will insteall itself
mkdir dev
cd dev
code .
```

## Conda
[Instructions mminiconda](https://www.anaconda.com/docs/getting-started/miniconda/install#macos-linux-installation)
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh  
bash Miniconda3-latest-Linux-x86_64.sh
```
## GitLab - internal pr external

1. An ssh key for gitlab

Create the ssh key on both your local computer and HPC with the following keygen command:
```
ssh-keygen -t ecdsa -C comment
chmod 600 ~/.ssh/id_ecdsa
```
This will create a key file `~/.ssh/id_ecdsa.pub`. Copy the content of this file to gitlab (via web interface) in User/Preference/SSH Keys > Add new key.

2. ssh config
Next, in your ~/.ssh/config file you will need to add the following (please ensure to replace <your_username> with your username):
```
Host <your_usernames>.git.icr.ac.uk
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_ecdsa
```

