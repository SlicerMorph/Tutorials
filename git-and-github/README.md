## Setting up your github account for ssh access.
To be able to push (upload) changes to the GitHub repositories you have created or have write-access to, you need to setup an authentication method with github. This is most easily done using a secure key encryption. Much like ssh, once you set an ssh-key, you can push changes to the repositories (that you have access to) without having to enter password, or use another method of authentication. Instructions here cover Unix like operating systems (like Linux or MacOS), but they are almost identical to Windows. You can find [more detailed instructions from the relevant github page: Connecting github with ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys)

Let's get started:

1. In a UNIX systems `~` (tilda) sign is short for path to your home directory. For example if your home is /home/murat, then `cd /home/murat` and `cd ~` are equivalent.
2. Open a terminal windows and type `cd ~/.ssh`. **NOTE:** If you get an error message, it means there is no .ssh folder under your account yet. First you have to create it with these commands:
```
mkdir ~/.ssh
chmod 700 ~/.ssh
```

3. To create a private and public key pair to use on github, type:
`
ssh-keygen -t ed25519 -C "your_email@example.com" -f myGitHubKey
`
(replace your_email@example.com with the email you registered with github)

You can optionally create a passphrase for additional protection. Do not create a passphrase for this exercise. 

4. Add the private key to the ssh agent to use by giving the command: 
`ssh-add ~/.ssh/myGitHubKey
`
**NOTE:** for this command to work, the ssh agent should be running in the background. If you get an error, it means it is not running. In that case, type the command below, and then retry:
```
eval `ssh-agent -s`
```

5. Now you have two files created: **myGitHubKey** is your private key with your encryption signature that you will not share with anyone, and **myGitHubKey.pub** is your public key. You need to upload your public key to github. For that please follow their instructions:  https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account

6. To test whether everything has worked, type this command:
`ssh -T git@github.com`

you should get something that looks like this:

<img src="./gh1.png">

If you got this, you are set. If there is an error, check your steps and re-read the detailed instructions on GH page carefully.

**NOTE:** if you have another computer (e.g., office desktop) that you would like to be able to push changes to github, all you have to do is to copy the your **private** key to the appropriate folder (e.g., if this is a Windows computer, and your username is Murat, it would be `C:\Users\Murat\.ssh\myGitHubKey`). 

## Using Git and Github

**Couple clarifications:** Git and github are not identical things. GIT is an open protocol for distributed version tracking. Github is a commercial (Microsoft) service built onto git protocol and is not open source.  You do not need GITHUB to use git. There are other git protocol service providers (e.g., gitlabs) or you can even create your own git service on the cloud (not advised).  

Git is a distributed (and decentralized) version tracking software. Meaning you do not need to be connected to the internet to use git. You can make all the changes on your local repository and when you have connection (or ready to go) update the main (used to be called master) branch. Some git lingo that is important to understand are: 

**Local repository:** A repository that sits on your own computer. 

**Remote repository:** A repository that is somewhere else than your own computer (usually on the cloud, e.g., in a git service provider like github or gitlabs). 

**Clone:** obtaining a copy of a repository locally. This will create the entire development history in your local computer with all of its branches. By default it's the main branch that is active. (You can switch to a different branch using the `git checkout` command). 

**Pull:** Making the local repository contents up to date with the remote repository. 

**Push:** Making the contents of the remote repository up to date with the local one. (this is only possible if you are the owner of the remote repository, or have been granted write-access as a collaborator by the owner of the repo). 

**Commit:** a change made to the repository. Every commit has a short description of the what the change is and an associated unique commit number. Commits are what is stored in the project timeline. You can rollback commits. You can of course create a commit for every single file changed, but for best operational practices, commits should be logically linked (e.g., in SlicerMorph changes related to ALPACA should be committed separately of the changes to the GPA module. Because these are two separate modules with different functionalities. However, if the change in GPA is required by a change in ALPACA, it makes sense to commit them together). This requires some thinking about what and when you want to commit. For example, if you committed a change for every single file modified or added to a project and you find that your project doesn’t work after these commits, you would have to rollback all these commits one by one until you can bring your project back to a working state. This can be very annoying, if you have lots of files (hence lots of commits). Conversely, in the case of committing unrelated GPA and ALPACA changes in the same commit, it would be impossible to unroll ALPACA commit but keep the GPA one (let’s assume changes broke ALPACA, but GPA is fine). 

**Fork:** Making an identical copy (of then current state) of somebody else’s repository under your account. When you are changing contents of the “forked” copy under your account, these changes are not **pushed** to the original repository, but only existed in your **fork**. (a crude biological analogy would be a cladogenetic -two lineages separating- event, like speciation.)  The most basic purpose and motivation of forking is to enable permissionless (you do not need permission to make changes to a repository under your account) and distributed contribution (see Pull Request).  

**Branch:**  In some ways you can think of creating an internal fork of the project without having to create a new repository. In multi-developer settings, branches are a must. Imagine we have two developers working on ALPACA and GPA module at the same time, and independent of each other. They start with the same code base at the same time. To avoid potential conflicts, both of them will create separate branches and continue their development and commit these changes independently of each other. Even, in a single developer situation, you want to use branches when you are ready to experiment on the code, but do not want to mess up the existing working code base. You create new branch, make your changes in there, and if they work you can merge that branch with the MAIN branch. Or discard it. 

**Pull request (PR):** Is the change request by the non-priviledged user to the remote repository. (i.e., when you want to contribute to someone else’s project with a some piece of code, first you fork their repository. Then you commit your changes and provide a description. Push the commits to your own (forked) repository, and then notify the other group that you have changes you want to contribute back to their project. They will review your changes and if they agree they will MERGE your PR.  


Now, let's play with git and github:



1. Create a repository called DEMO using the Github web interface. (Go to github, login to your account and hit + to add a new repo. Choose to initialize with a readme file)
2. In the terminal window of your computer, first check the git is configured correctly with the right user name and email address registered with the github. To do so, type this command:
`
git config -l |grep user
`
If the output doesn’t display the correct user name and/or email, you can set them via:
```
git config --global user.name 'your github username’
git config --global user.email 'email address you registered with github'
```
3. Clone the freshly created repository to your local drive.  
`
git clone github.com/your_GH_username/demo
`

4. Go to that folder and edit the README.md file contents. Just type anything (except for whitespace).
 
5. Committing files that have changed is a two-step process. First you have to tell git to track changed file(s) via the add command. 
`
git add README.md
`

If there are more changed files, you can keep typing their names (after the README.md). When you are ready to commit the changes to git, issue the commit command (shown below), which will ask you a brief description of the changes. 
`
git commit
`

Optionally, you can merge the add and commit into one command with this: 
`
git commit -a 
`
However this will indiscriminately add all the files that have modified and created in  the directory. This may include unwanted files such as temporary files. So I suggest using this combination carefully or simply adding files and committing changes as separate steps as shown above.

 
6. So far the changes are in the local copy of your git repository. To update the remote server with these changes we will use the git command `push`. Push syntax with SSH keys is slightly different. You replace the https:// with the git@github.com and then instead of / you type “:” and enter your username. So the full syntax looks like this:  
```
git push git@github.com:your_GH_username/demo.git
````

you can also use the same syntax for cloning repositories:
```
git clone git@github.com:your_GH_username/demo.git
```


7. Go to the repository's page on github, and confirm that the README.md file now reflects your recent changes, and shows your brief commit description. 
Congratulations, you have now mastered the basics git :fireworks: 


8. Now, go to the github repo, create a new branch called development. Switch to this branch add some files (eg. I added a blank file called only_in_development) from the web browser.

At this point, your local repo on your computer is no longer up-to-date with the remote repo. To obtain the changes in the remote locally, we will use the `pull` command. Make sure to execute the following command inside the clone repo folder (in this case DEMO) 
`
git pull origin 
`

Unless you have commits that are not pushed to remote, you should see something like this:
<img src="GH2.png">

But if you type `dir` or use the file browser to see the contents of the folder, you will not see the new file(s) you have created in the development branch. That’s because by default the main branch is active. You can change the branch to development by giving the checkout command:
`
git checkout development
`
After this if you type `dir` or use the file browser, you will see the new file (in my case **only_in_development**). 

9. Ready for more in-depth knowledge? Software carpenteries have an excellent tutorial called [**Version Control with git (and github)**](https://swcarpentry.github.io/git-novice/) that covers much more than this brief intro.



