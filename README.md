# Git Guide
#### A comprehensive and simple beginner guide to using git as a version control system.

## Part 1: What is Git and why use it?

Don't worry about all of the details right away; everything will be explained later throughout the guide. The below image is a basic representation of a git tree (read from top to bottom). Each dot represents a *commit* which is sort of like saving your work in its current state. You can always revert to a previous commit if you make a mistake and lose your progress.

This commit is done to a *branch*. Think of branches as a slightly different version of the project. For example, you might want to have a `stable` branch that gets updated infrequently in order to have a stable version of the code. Then a `testing` branch can be used to test and implement new features.

The commits in the image contain messages informing the team about the purpose of that commit. While commit messages are not required, it is considered bad practice to leave them out.

![Example Graph](/images/example_graph.png)

Now that you have a basic understanding of what Git is, the next parts will help guide you through the setup process.

## Part 2: The Terminal

### NOTICE: If you are using a Windows computer you will need to configure WSL (version 2) to follow the rest of the guide from here on: https://learn.microsoft.com/en-us/windows/wsl/install. You should just be able to run `wsl --install` from powershell as an administrator.

### Basic Syntax

Git can be used through a *command line interface*. This means that you will be utilizing git through a *terminal emulator* on your computer. This is an interactive prompt that allows you to type commmands in the *bash* language to execute. An example of a terminal command that commits to the current working branch looks like this:

```
$ git commit --message 'Added methods for telemetry'
```

The `$` indicates the start of the line for a command to be entered by the user. It is not typed by the user but will exist on its own line in the terminal emulator by default. If the line is blank, then your terminal emulator is already executing a task and not ready for user input.

The command to be executed is then entered: `git`. Since the `git` command takes one required argument, it is then followed by the phrase: `commit`. In other words, you are telling the shell that you would like to make a new git commit.

Next are the optional arguments or *options*. You can choose a specific option by starting with a `--` and then one or more words. In this case, `--message` tells the `git commit` to include a message. The message content is then typed after the message option. To reiterate you are specifying an option that takes one argument: the content of your message. If there is an option with multiple words it will probably look something like this: `--option-with-multiple-words`.

Since arguments are seperated by a space in bash, the message argument must be in quotes or only the first word will be taken as the message. ie:

```
$ git commit -m Added methods for telemetry
$ git commit -m 'Added methods for telemetry'
```

The commit message of the first command is "Added", while the commit message of the second command is "Added methods for telemetry". The messsage option will only take one argument.

### Navigation

Feel free to follow along in your own computer's terminal emulator during this section.

The following paragraphs explain how to access a *bash* shell from your operating system. This is an interactive prompt that allows the user to enter commands in the bash language.

On a Windows computer you can search for "powershell" (don't run as administrator this time) and type `wsl` followed by the enter key. If the command hangs or is not found, that means you haven't set up WSL correctly and you must go back to the previous step to try again.

On MacOS search for the "terminal" app.

On Ubuntu search for "terminal".

When you first open your terminal emulator, there might be a message at the top. Below that message you should see somthing like:

```
eli@laptop$
```

Depending on what operating system you are using, this might look a bit different than mine (eg: a `%` symbol for macos). You may start typing your commands after the `$`, `%`, `>` or whatever other symbol is present. This is the indicator that lets you know the shell has finished executing its previous process and is ready for more input. If there is no symbol present then the terminal is currently executing a previous command and is not ready for you to type input.

I will now be using the `$` delimiter to mark the beginning of a line of input for the rest of this guide. You do not have to type it yourself. The first command I would like you to type is `pwd`:

```
$ pwd
/home/eli
```

The second line of this example is not prefixed with a `$` symbol as it is the output of the previous command and is not typed out by the user.

This command **prints** the current **working** **directory**. You are basically telling your bash shell to print folder on your computer that you are running commands inside of. 

If you would like to see the folders and files that are inside of your current folder (kind of like a file manager), you can **list** them with `ls`.

```
$ ls
Desktop   Documents   Downloads   Git-Guide.txt   Music   Pictures   Public   Videos
```

Note that this command lists everything in the current directory, including files. Your terminal might show the directories in bold, or they might be a different color to separate them from the files.

Now that you are aware of the folders inside (I'll call them subdirectories from now on) use `cd` to **change** **directories** (hence "cd"). This command takes a single argument unlike `ls`; you need to type the name of the directory to change to (case matters).

```
$ cd Downloads
$ pwd
/home/eli/Downloads
$ ls
File.txt   Minecraft.jar   Movie.mp4   Song.mp3   Virus.sh
```

You now know how to move around in the terminal.

*If you would like to exit the terminal, type:* `exit`.

## Part 3: SSH

If you have taken a Computer Science class prior to reading this guide, then you may remember asymmetric encryption.

If you don't, I will try to provide a simple example. I start by generating a keypair: two keys that are mathematically linked. One key is public and is known by everyone. The other is private and must not be distributed.

If my friend wants to send me a message, they can encrypt it with my public key (which everyone has). Now, the message is garbled gibberish and makes no sense to anyone (kind of like how an enigma machine works). This means if someone with mal-intent were to intercept this message, it would be of no use to them.

The encrypted message can then be decrypted with my own private key. Therefore, the message can only be understood by me, and nobody else can interpret it. Anything that is encrypted with the public key can only be decrypted with the private key and vice-versa.

You may ask what the use is of encrypting something with my private key, if everyone can just decrypt it with my public key. You are correct in that it doesn't work to obfuscate the message; however, it can be used as a digital signature.

If everyone can decrypt the message with my public key, that means it must have been encrypted with my own private key (the one that nobody else has). In other words, no one will be able to falsely claim that they are me because they do not have my private key and therefore cannot sign their messages under my name.

In other words you will need SSH for two reasons:

1. You are going to be signing your commits so we know that ***you*** are the one who made them.

2. If you are working on a private *repository*, meaning your code can only be seen by authorized users, you will need an SSH keypair to do this (SSH is a protocol that uses asymmetric encryption to verify identity among other things).

#### DISCLAIMER: The private key is private and STAYS ON YOUR COMPUTER. DO NOT SHARE YOUR PRIVATE KEY ***EVER***, not even between devices that you own. If you need to work on a repository with another device, you ***MUST*** generate a new keypair.

A repository is a cloud location that stores your projects and tracks their history. We will be working with repositories using git as an interface to contribute and collaborate.

Open the terminal and type `cd ~`, (`~` is an alias for "home" and in my case really translates to "/home/eli"), although you proabably are already in the home directory (you will know if you run `pwd`).

Now that you are home you should type `ls --all` which will list all files and folders (hence `--all`). Once again `--all` is an option for the `ls` command that takes 0 arguments. Once again, most command-line options also have a shorter, one letter counterpart. They are usually preceeded by a single `-` in their one letter form: `ls -a`. These options are functionally equiavalent. If you would like to see more options for a specific command, such as `ls`, you can succeed it with a `--help` or `-h` option. If you already see a directory named `.ssh`, skip the next step.

To create a new directory, enter the command `mkdir .ssh`. This **makes** a **directory** under your current working directory called `.ssh`. Now you should run `ls -a` and see a directory named `.ssh` in the list of files and folders.

You will need to generate an ssh keypair. To do this you will need to enter the following command:

```
ssh-keygen -a 100 -t ed25519 -f ~/.ssh/<filename> -C <your_email_address>
```

Replace "\<filename\>" with whatever you want to call your keypair and the last argument with your email address.

`-a` specifies the number of rounds of KDF (key derivation function) to be used. A higher number of rounds means more secure; 100 is plenty. `-t` allows you to pick the type of function used to generate the keypair. We are using ed25519 because it is secure, convenient, and fast. `-f` lets you pick where to store the keypair and `-C` allows you to type out the email address that you will be identified with.

If you are prompted to enter a password you can, or just leave it blank (it doesn't really matter which one you choose). However, if you are using a shared device I ***HIGHLY*** suggest you use a strong password.

Now you have files in the .ssh directory named `<filename>` and `<filename>.pub`. The key ending in `.pub` is your public key, the other (no extension) is your private and STAYS ON YOUR COMPUTER.

We have to then tell Git to use your keypair. You should now create a file in `.ssh` directory and name it `config`. Not `config.txt`. If you want to know how to do this from the terminal just do this:

```
$ cd ~/.ssh
$ pwd
/home/eli/.ssh
$ ls
<filename>   <filename>.pub
$ touch config
$ ls
config   <filename>   <filename>.pub
```

Of course you don't have to type `ls` but it's nice to know that you have successfully followed the guide thus far. Now you can edit the `config` file in your favorite text editor of choice, a good option is VSCode if you are not sure or haven't used one before.

The following config file is an example that you should follow. You may not have a Github account yet so you can leave that part blank for now, but make sure that you come back to the config file to input your username where stated in the example.

```
# ~/.ssh/config
# Primary Github account
Host github-<Github Account Username>
	HostName github.com
	IdentityFile ~/.ssh/<filename>
	IdentitiesOnly yes
```

The lines preceeded with a `#` are comments to help you remember the purpose of this file and not required. Basically this tells Github to use our `<filename>` and `<filename>.pub` keypair when connecting to github.com. Just make sure to replace all instances of "github.com" with "github-\<Github Account Username\>" when using a url with the former in it.

For example, instead of "git@**github.com**:ElijaRock/git-guide.git", I would change the url to "git@**github-ElijaRock**:ElijaRock/git-guide.git". You will need to do this for your username that you setup in the config file in the future. Don't worry about the semantics yet as they will be explained in the next parts of this guide.

## Part 4: Creating a GitHub Account

### What is Github?

Throughout this guide I have mentioned `git` many times and now I am talking about Github. What's the difference? Well, `git` is the tool that you use to interface with repositories on the Github website, which is a cloud service that allows you to store many repositories.

TLDR: `git` is the tool you work with; the results of using `git` are seen on the Github website. There is no "Git" website, and there is no `github` command in the terminal, it's the other way around.

### Account Creation

Visit [https://github.com/](https://github.com/) and create an account with your preferred email address, etc. Choose your preferred username, eg: "first-lastname", and you are good to go.

### Adding SSH key to account

Click on your profile picture on Github and press settings. 

![settings](/images/settings_edit.png)

Then, on the left, find `SSH and GPG Keys`, click on it.

![ssh_and_gpg_keys](/images/ssh_and_gpg_keys_edit.png)

Press `New SSH Key` and title it whatever you want.

![new_ssh_key](/images/new_ssh_key_edit.png)

 Key type: `Authentication Key`. Paste in the file contents of your public key: `<filename>.pub`. Repeat these steps but this time choose Signing Key.

![auth_key](/images/auth_key.png)

![signing_key](/images/signing_key_edit.png)

In total you will have added two keys. They both contain your `<filename>.pub` key but one has been setup as an authentication key and the other as signing. Now you can test your keypair like so (replacing \<Github Account Username\> with your actual Github account username):

```
$ ssh -T git@github-<Github Account Username>
Hi <Github Account Username>! You've successfully authenticated, but GitHub does not provide shell access.
```

Success! If you have not yet changed your `~/.ssh/config` file to include your actual github username, please do so now. You may get a prompt about a fingerprint and it will ask you to input yes or no, say yes.

## Part 6: Git

### Installing Git

Depending on which operating system you use, there may or may not be a *package manager* installed by default. A package manager does all the heavy work for you, preventing the need to search for applications on the internet. I will now show you how it works to seamlessly and efficiently install apps.

On Ubuntu and WSL (Windows) systems, a package manager `apt` is included by default. On MacOS, you will need to install `brew` by following the steps on the [brew homepage](https://brew.sh/).

Once you have successfully installed a package manager, you can then install `git` with the following command (depending on your operating system):

```
user@ubuntu/windows$ sudo apt install git
user@mac$ brew install git
```

Aptitude (apt) requires that you have elevated permissions when installing packages, which requires you to preceed it with the `sudo` command ("Superuser Do") and enter your password to confirm. You may need access to an administrator account on your computer to execute some of these commands.

### Using Git

It's time to clone your first repository. Cloning allows you to have a local copy of the code in a repo on your machine.

Open terminal and cd into a directory of your choice. Then, go to the top of the page on a Github repo and click on the `<> Code` green button, then click ssh and copy the link. Now you have to change the link like so:

###### TODO: Add example image

```
Before: git@github.com:<Example>/<Repository>.git
After: git@github-<Github Account Username>:<Example>/<Repository>.git
```

Now you can type this into your terminal (replacing my Github username with yours):

```
$ git@github-<Github Account Username>:<Example>/<Repository>.git
$ ls
<Repository>
$ cd <Repository>
$ pwd
/home/<user>/<Repository>
```

By default you have cloned and are working on the `main` branch. ***NEVER*** commit to or push to the main branch directly. You must create your own branch to work on a repo. Watch closely, but do not follow these steps yet:

```
$ git branch new-feature
$ git checkout new-feature
```

I have created a new branch called `new-feature`. You should create a branch with whatever name you want (don't use spaces in the name). Then I use `git checkout` to switch to working on that branch. You can then use `git branch` with no arguments to see which branch you are working on.

```
$ git branch
main
* new-feature
```

The star next to `new-feature` branch shows that I am currently working on it.

Open a file on your computer, correct mistakes, add features, or make a new file. If you are using a file ending in `.md` and not sure how the syntax works, look up: "markdown syntax" (this is a markdown file, hence the .md file extension).

If you want to see the changes you have made thus far you may type: `git diff` and it will highlight the differences between the original code and modified version.

There is one more configuration step before you can commit (you only have to do this once). Make sure your current working directory is inside of the git repo. Then, enter these commands:

```
$ git config user.name '<your name>'
$ git config user.email '<your email address>'
$ git config gpg.format ssh
$ git config user.signingkey </path/to/ssh/key>.pub
```

Replace the last argument with the actual file path of your public key (mine would be `~/.ssh/<filename>.pub`).

Now you can stage and commit the changes you have made to a repo. If you have added any files, you must type `git add -A` to add all files in the repo to be tracked. This command also stages the changes you have made to all files (kind of like a two in one). If you didn't run the previous command, you need to run `git add -u` to track the changes you made in only the currently tracked files (files you added won't be committed).

After you have staged your changes you can compare your branch and the upstream one before committing by doing the following (change `new-feature` to the name of your branch).

```
git diff new-feature origin/new-feature
```

You can now commit to the repo:

```
$ git commit -S -m 'This is a short message describing the changes I have made' -m 'This is a longer more thourough description of the changes made'
```

You can ommit the second `-m` if you have only made small, incremental changes. the `-S` option allows you to sign the commit, in other words we can verify that **YOU** made these changes (in the case of identity theft). That is why it's so important to not give your private key to anyone ever.

Okay now the last step is to push the commit to the branch you are working on. This will show the commit on github.com website and everyone else will be able to see it and fetch it. Type:

```
$ git push origin new-feature
```

This pushes it to my new branch that I made and now I will be able to see the changes on the website. The `origin` part is shorthand for the github url, so it basically means go to this link and find the new-feature branch then push to it.

Now visit the repository on github.com, go to the top left where is says you are on the main branch, and switch to `new-feature` branch. Then you can click on the top right it should say `x commits` below the `code` button you found earlier, click on the commits. You should now be able to see your commit with the message. If you have set up your ssh key correctly there will be a green `verified` bubble on the right side of your commit.

###### TODO: add example image

### Submit a Pull Request

Now that you have finished working on the feature in your branch and extensively tested it, you are ready to merge it back into the main branch.

Go to the `Pull Requests` tab at the top, and choose `New Pull Request`. At the top it will allow you to select the `new-feature` branch to be merged into `main`. Then provide a description and submit the request. Some repositories require that at least one other person (usually a specialized contributor) review the code in your request before giving the OK. Then, they can accept the request so your branch can be merged into main. Congrats!

###### TODO: add example image

You may have also noticed an `Issues` tab on the repo. You should open any problems or inaccuracies that you find in this guide as an issue on the repo. Don't worry about it now, but in your pull request you can choose to close issues that the PR fixed.

### Fetch the Latest Changes and Merge

Suppose someone on your team worked on your branch while you were gone to help implement the feature. Before you start making changes again, you need to fetch theirs. Do this by running the following command:

```
$ git fetch origin new-feature
```

Now you have their changes tracked, and need to review them before following through. (This basically works like reloading a website to see what's new).

If you want to see the difference between your `new-feature` branch and the repos up to date `new-feature` branch, run this command:

```
$ git diff new-feature origin/new-feature
```

You are now looking at the changes your team member made to your branch compared to the one you had from before. If you like these changes, you can merge them into your current directory. Actually you can use merge or rebase, if you would like to see the difference, take a look at [this link](https://www.atlassian.com/git/tutorials/merging-vs-rebasing). In this case I will rebase the changes they made onto the directory I am working in.

```
$ git rebase origin/new-feature
```

You may get an error that you need to stash your changes before a merge or rebase. This happens if you made changes and forgot to commit them. Git allows you to keep your changes in a special safe place called "stash" while having the latest version of the branch to work on.

```
$ git stash
```

You may then run `rebase` again and you will be on the most up to date version of `new-feature` branch. If you would then like to reapply your changes to this latest version, run the following command:

```
$ git stash apply
```

Now I am on the latest version of the branch `new-feature` and can track and commit my changes as usual. Git forces you to do this because it would be impossible to have your commit and your peer's commit to be "next to each other" at the same time on the same branch. Essentially what you just did is moved your changes from "next to" the peer's commit to after their commit, creating a linear branch history. If this is confusing at first, do not worry, the more you use Git the more you will understand over time.

I should also probably mention that you only need to clone the repo once; always use fetch to grab the newest changes. Also, git has many many more features than specified here and can be used to do even more powerful things.

## Part 7: Best Practices

Now that you have the knowledge, I will let you know of some best practices that I learned in the time that I have used git.

1. If ever in doubt run `git status`. This will give you valuable information on the current state of the git repo on your computer, changes you have made, etc.

2. Do not make changes to the main branch. The code on this branch is stable, tested, and reliable.

3. Do not force push other people's code off the repo. As annoying as it may be to deal with merge conflicts (you'll experience these at some point), do not force push your own code. Make sure to fetch and merge to resolve the conflicts instead.

4. Detailed commit messages. Make it obvious what your commit does to change the code (don't add a meaningless message).

5. Use the builtin issues feature. Get in the habit of submitting proper bug reports as an issue and document what you did and what didn't work. You can also use a `feature-request` tag on your issue to request a new feature, which can later be closed in the pull request that fufills the issue.

6. Read the docs. I have gathered almost all of my information from the Github documentation. If you are ever unsure, try reading the official documentation first.

###### TODO: add links for each section of the git proess and submit issues to this repo.
