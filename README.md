This repo explains the usage of Git interaction with GitHub for uploading and maintaining repo's.
----
Date: 23 Sep 2022  
Git enables version control of your projects. There are three main steps 'stage, commit and push'

You can skip to 'Starting a project' if you've already installed the programs, added your SSH key to GitHub and added your details onto git.


First time setup
----
### Install programs... N.B. This is assuming you have a windows OS
If you're only using the HPC, then you don't really need to install git. And can simply ssh onto the HPC. Then load the git module 'module load git'
1. Make sure you have a GitHub account [www.github.com](www.github.com)
2. Install git [https://git-scm.com/downloads](https://git-scm.com/downloads)
   Most of the default options are already ideal for new and experienced coders, as suggested [https://www.maketecheasier.com/install-git-bash-on-windows/](https://www.maketecheasier.com/install-git-bash-on-windows/)
   Definitely recommend the deault option:
   install git bash 
   'OpenSSL library' for 'choosing HTTPS transport backend'
   'Check out windows-style, commit Unix-style line endings' for 'Configuring the line ending conversions'. (for windows OS users)
3. Install [notepad++](https://notepad-plus-plus.org/downloads/) and/or [atom](https://atom.io/)
   These enable to you more cleanly write and modify the files used. These natively support coding types used on GitHub (atom was built with GitHub) and make viewing and editing files/projects much more user friendly. Especially the latter for projects with multiple files and subdirectories.
   
### Link SSH local passcode/key with GitHub
This is to enable you to 'push' your local repo to the GitHub site. To do this, we're going to create a 'key' that's physically stored on your local computer and also save the details on your GitHub account. Essentially, this key acts as a digital password (or house key) to enable access between your local computer and GitHub web server via the command line.  
GitHub no longer support old keys or https, so you probably need to create a new one... Good news - it's easy!  
Consider if you're using the HPC/research server or your local computer. You'll need to repeat these steps if you intend to use both.  
See these webpages for more details on [adding a new SSH key to your GitHub page](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) which links to [checking if you have old SSH keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys) and [creating a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#adding-your-ssh-key-to-the-ssh-agent)

1. Open Git Bash (login to remote server via ssh if necessary).
2. Paste the text below, substituting in your GitHub email address, to create your new SSH key.
	```
	ssh-keygen -t ed25519 -C "your_email@example.com"
	```
	I used my no reply GitHub email, which is typically <user.name>@users.noreply.github.com  
	This can also be seen from github.com account (Settings/Emails - then under Primary email address it should show to no reply option).

3. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.
	```
	> Enter a file in which to save the key (/c/Users/you/.ssh/id_algorithm):[Press enter]
	```
4. At the prompt, type a secure passphrase. For more information, see ["Working with SSH key passphrases."](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases)
	```
	> Enter passphrase (empty for no passphrase): [Type a passphrase]
	> Enter same passphrase again: [Type passphrase again]
	```
	You don't have to enter a passphrase. But, only do this if you 1) have strong confidence in the security of computer/server from malicious agents (hackers) and 2) the key your generating will have limited use for your servers. It's only a problem if someone can gain physical access to your 'keys' i.e. If the key is only used to get access GitHub and no other important files/documents, would it matter or compromise anything of high value? 

5. Now it's time tell GitHub about the new key! Copy the contents of the new key by: 
	Right click and open the new file in a text editor, and copy all the contents (ctrl-A and ctrl-C).
	If you can see the folder below, you probably need to enable viewing of 'hidden files', as it should be found:
	~/.ssh/id_ed25519.pub
	
6. Sign into github and click on settings (drop down menu from the upper right icon).
7. SSH and GPG keys
8. Click New SSH Key or Add SSH key
9. Add relevant title i.e. 'HPC'
10. Paste the the key details in the 'key' field.
11. Click Add SSH key, and done!

Add your git details
----
This is a really simple step to add your own credentials to git. You're just assigning your name and email address to the git programme. So that every change that is pushed can attributed to who you are. :)
1. Open git bash.
   If you're using the HPC, ssh onto it, and load the git module
   module load git
2. Check if you already have any details (it should return as empty if you haven't done this yet).
	```
	git config --list
	```
3. Add your username to git
	```
	git config --global user.name "SBurnard"
	```
3. Add your email
	```
	git config --global user.email "email_address@something.com"
	```
	The email address can be your no return GitHub address <your.user.name@users.noreply.github.com>
4. Confirm the settings have been added
	```
	git config --list
	```


Starting a project
----
This outlines the steps to initialise and connect a local repo on GitHub. This assumes you've already done the above steps 'install programs', 'Link SSH local passcode/key with GitHub' and 'Initial git l (ADD LINKS)
1. Create Repo on GitHub
   - Click '+' button.
   - Change owner if making it on an organisation page
   - Add repo name (this will show up on the weblink so make it simple and memorable with no spaces).
   - 'Description' is added onto the side of the repo. You could add the full name here if the repo name is an acronym.
   - Don't click 'add Readme file', or .gitignore. We create these locally.
   - For now, don't click 'add licence' we will create this after the first push! But, it is important to add one later (and consider which type is most relevant). I will create a separate comment later about this, but most likely 'MIT' is most applicable. In short, this makes it more likely and attractive for people to use and interactive with you work! Without it, technically, no one is really allowed to use your code... Which really defeats the point of GitHub and your hardwork... Although, this isn't important if you're not going to make your repo public. ^^
   - Create repository.

2. Add .gitignore to your project folder on your computer/project folder (important if you have a lot of big or intermediate files you don't want tracked and uploaded). Open notepad++ or atom and save as '.gitignore' with the files on separate lines as described below.

	Essentially, this is a text-like file with one line per rule of file to ignore relative to the top level of the repo i.e.
	```
	results/intermediateImage1.png
	results/intermediateImage*
	results/intermediate_files/*
	annotation_file.gtf
	```
	

3. Open git bash and cd to project directory (if it's on canepi ssh across).
4. Time to initialise your git repo! This starts git tracking changes to your local directory (nothing to do with github yet....) The most appropriate option below depends on if you've installed the latest version of git or using the HPC git (which is an old version)  
	*Newest git version*:
	```
	git init 
	```
	*HPC module loaded git*:
	```
	module load git
	git init && git symbolic-ref HEAD refs/heads/main
	# If you're using the HPC (git/2.24.0) or an older version of git locally(unlikely if you've just downloaded it)  make sure to name the top level with: git init -b main
	```
	Previously the head node were named with 'master' but most dev platforms are moving removing this legacy style name (due to wide usage of files/ programmes named as 'master' and 'slave'....)
	
	
5. Confirm if .gitignore is working/recognised
	```
	git status
	# Only files not listed in the '.gitignore' should be shown here. N.B. But this also only shows the top directory too, so ignored files specified within directories isn't indicated here. Unfortunately. Thus, can only confirm with files at the top level....
	git status -uall
	# This worked for showing all files (including subdirectories).
	```
6. Stage/Add all files ready to be commited
	```
	git add .
	# Adds the files in the local repository and stages them for commit. To unstage a file, use 'git reset HEAD YOUR-FILE'.
	```
	
7. Commit the files that were just staged.
	```
	git commit -m "First commit"
	# Commits the tracked changes and prepares them to be pushed to a remote repository. To remove this commit and modify the file, use 'git reset --soft HEAD~1' and commit and add the file again.
	```

8. Now it's time link your GitHub repo to your local git repo. Get/Copy link of your repository 'SSH' on GitHub.com's Quick Setup page, click  to copy the remote repository URL.
	Click green 'code'; button and take ssh to copy, this is basically your git@github.com:<user.name>/<repo.name> e.g.	git@github.com:canepi/scTEM-seq.git
	```
	git@github.com:<user.name>/<repo.name>
	```

9. Add the new remote origin (using the above name)
	```
	git remote add origin git@github.com:<user.name>/<repo.name>
	git remote -v # Confirms it linked correctly
	# If you need to change it from https to ssh for example (I had previously tried to use https: 
	# git remote set-url origin git@github.com:canepi/scTEM-seq.git
	# git remote -v
	```

10. Almost there! Push the changes with 'git push'
	```
	git push -u origin main 
	OR 
	git push -u -f origin main # 
	# But with force flag (-f) will remove anything in the remote repo including the license file. So maybe not the best option.... 
	#-u (https://www.educative.io/answers/what-is-the-git-push--u-remote-branch-name-command) Pushes the changes in your local repository up to the remote repository you specified as the origin
	```
	
Quick summary:
- git init
- git add -A
- git commit -m 'Added my project'
- git remote add origin git@github.com:sammy/my-new-project.git
- git push -u -f origin main
 
Congratulations! You've pushed your local repo onto the remote repo on GitHub. Go have a look! :) But... If you're going to be making this public you should consider adding the license now before you forget! 

#### Add license 
11. On your GitHub repo, click 'add file'
   - field name = LICENSE or LICENSE.md (with all caps).
   - On the right of field name, 'choose a license template' should have appeared - click it!
   - At this point you can see the description of the various options. You can review the various links below for some brief information and suggestions about the most appropriate options. Typically it's best to choose either **MIT** (Most permisive license - people are essentially free to do whatever they like with you work). or **GNU** (people can still freely use your work, but are enforced to use the same license type/make their code free if it includes parts of yours).
12. Click review and submit.
   - Feel to add a brief message about the commit/license addition (maybe reasoning for license type chosen).
   - Commit to main branch (so it's instantly added).
   - Add your desired email address. If you've enabled privacy already then your no.reply github email address should be defaulted already.
   
13. Pull your added license to your local repo! But first, check it can detect your new license file...
	```
	git fetch # This just checks if there were changes. This should signify
	```
	Now. Let's actually pull the file.
	```
	git pull
	```

Yahooooo (remeber that old search site!?) - You've created and uploaded a github repo, added a license and updated your

### Ongoing work
1. git fetch
2. git pull
3. git add .
4. git commit -m 'Meaningful message'
5. git push

- Commit regularly and with meaningful messages, this enables you to roll back to previous version if something breaks. Then push when ready with key updates.

