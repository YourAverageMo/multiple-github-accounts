# Setting Up Multiple GitHub Accounts In VScode

- [Setting Up Multiple GitHub Accounts In VScode](#setting-up-multiple-github-accounts-in-vscode)
    - [The Goal:](#the-goal)
    - [How It Works:](#how-it-works)
    - [1. Setting Up SSH](#1-setting-up-ssh)
    - [2. SSH Agent](#2-ssh-agent)
    - [3. Creating ~/.ssh Config file](#3-creating-ssh-config-file)


### The Goal:

I wanted a way to push and pull to separate github accounts, all while maintaining VScodes source control functionality, without worrying about using the wrong account for commits. Most importantly no need to swap accounts in VScode. With this method, all ssh identification and verification are handled automatically.

### How It Works:

Create directories for each account somewhere on your PC and each folder acts as a "vault" for that GitHub account. So, for example, in my case i made 2 folders [Personal, Professional] and any time i clone a repo, say 'personal-repo', into them from its associated GitHub (using ssh ofcourse) the proper ssh key, username, and email address is used. If i move 'personal-repo' out of the 'Personal' folder credentials will not be supplied and will fail to push.

Its important to note that your current .gitconfig may already have a username and email that will be used by default on anything outside of the folders we create here in this guide. I dont think you need to remove those entries, but i recommend it. If you choose not to, at least put the changes we're going to make *below* the default values that way we override them. I cover this more in [2. SSH Agent](#2-ssh-agent)

<!-- TODO: update the above link when its written -->

### 1. Setting Up SSH

Open up file explorer (windows key + E for short). Goto `C:\Users\<your username>\.ssh`. if .ssh doesn't exist first enable hidden files and folders. if you still dont see it create a hidden folder named ".ssh". Once in .ssh, shift + right click and click on 'open powershell window here'

I'm going to pretend you don't have any ssh keys setup so lets go ahead and make a few. enter this into powershell:
```
ssh-keygen -t ed25519 -C "<comment>" -f <key-name>
```
  - replace `<comment>` with a comment name for your key. and `<key-name>` with the name of the file you want the key to be.
- you dont have to enter a passphrase.
- make as many keys as you have accounts. I had 2 so i made 2 keys 

### 2. SSH Agent

Now that we got our keys generated we need to add them to our SSH agent. Lets start our SSH agent in the same powershell window with this command
```
eval `ssh-agent -s`
```
Next add our generated SSH keys to the agent using the below command.
```
ssh-add ~/.ssh/<key-name>
```
- Replace `<key-name>` with the name of the ssh key file you created in step 1
- Dont forget DO THIS FOR EACH KEY YOU CREATED.

Great now that you got that all setup lets add the corresponding SSH keys to their rightful GitHub accounts. This is straight forward enough so i'll give you the short version.

Log in to each GitHub account and go to `https://github.com/settings/ssh/new`, give it a fancy shmacy title, leave it at authentication key, and paste in your .pub file for the corresponding key that we just generated in the .ssh folder.

So in my case i copy the text from `C:\Users\Your Average Mo\.ssh\PersonalSSH.pub` into my personal GitHub account.

### 3. Creating ~/.ssh Config file
  
We need to make a config file inside the same .ssh folder where we made the SSH keys. If you already have a config file in the .ssh folder you can open that one up and just comment out the previous code. If not, inside the same powershell window input these line by line:

```
touch config
code config
```

The `code config` should just open the file we just made in vscode. Now in that file copy paste this:
```
Host *
  IgnoreUnknown AddKeysToAgent,UseKeychain
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/<key-name>
```

- This 'IdentityFile' ssh key is the default key that will be used  
- But this time only put one ssh key in the `<key-name>`. In my case i choose my personal key.


i used [this blog](https://javascript.plainenglish.io/how-to-manage-multiple-github-accounts-in-vscode-using-ssh-keys-7f1a3adef58a) as a guide when setting up mine