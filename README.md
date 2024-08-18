# Setting Up Multiple GitHub Accounts In VScode

- [Setting Up Multiple GitHub Accounts In VScode](#setting-up-multiple-github-accounts-in-vscode)
    - [The Goal:](#the-goal)
    - [How It Works:](#how-it-works)
    - [1. Setting Up SSH](#1-setting-up-ssh)


### The Goal:

I wanted a way to push and pull to separate github accounts, all while maintaining VScodes source control functionality, without worrying about using the wrong account for commits. Most importantly no need to swap accounts in VScode. With this method, all ssh identification and verification are handled automatically.

### How It Works:

Create directories for each account somewhere on your PC and each folder acts as a "vault" for that GitHub account. So, for example, in my case i made 2 folders [Personal, Professional] and any time i clone a repo, say 'personal-repo', into them from its associated GitHub (using ssh ofcourse) the proper ssh key, username, email address is used. If i move 'personal-repo' out of the 'Personal' folder credentials will not be supplied and will fail to commit.

Its important to note that your current .gitconfig may already have a username and email that will be used by default on anything outside of the folders we create here in this guide. I dont think you need to remove those entries, but i recommend it. If you choose not to, at least put the changes we're going to make *below* the default values that way we override them.

### 1. Setting Up SSH

- open up file explorer (windows key + E for short)
- goto `C:\Users\<your username>\.ssh`
  - if .ssh doesn't exist first enable hidden files and folders. if you still dont see it create a hidden folder named ".ssh"
- once in .ssh, shift + right click and click on 'open powershell window here'

I'm going to pretend you don't have any ssh keys setup so lets go ahead and make a few

- enter this into powershell: `ssh-keygen -t ed25519 -C "<comment>" -f <key-name>`
  - replace `<comment>` with a comment name for your key. and `<key-name>` with the name of the file you want the key to be.
- you dont have to enter a passphrase.
- make as many keys as you have accounts. I had 2 so i made 2 keys 


  
  
  
  
i used [this blog](https://javascript.plainenglish.io/how-to-manage-multiple-github-accounts-in-vscode-using-ssh-keys-7f1a3adef58a) as a guide when setting up mine