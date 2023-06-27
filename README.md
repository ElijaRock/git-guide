# Git Guide
#### A comprehensive and simple beginner's guide to using git as a version control system.

-------------------------------------------

### NOTICE: If at any point reading this guide you want to skip a section that you are already familiar with, click on the hamburger menu at the top left of this file (above the title and to the left of the filename).

## Part 1: What is Git and why use it?

Git allows different people in different locations to contribute to a project in a centralized location. The project is usually stored on a website, and can be modified through a tool known as `git`. This tool also preserves project history, so every version of the project can be accessed at any time.

These project "versions" are known as *commits*. You can always revert to a previous commit if you make a mistake and lose your progress.

This commit is done to a *branch*. Think of branches as a slightly different version of the project. For example, you might want to have a `stable` branch that gets updated infrequently in order to have a stable version of the code. Then a `testing` branch can be used to test and implement new features.

The commits in the image contain messages informing the team about the purpose of that commit. While commit messages are not required, it is considered bad practice to leave them out.

![Example Graph](/images/example_graph.png)

Now that you have a basic understanding of what Git is, the following parts will help guide you through the setup process.	

## Part 2: The Terminal

### NOTICE: If you are using a Windows computer you will need to configure WSL (version 2) to follow the rest of the guide from here on. You should be able to run `wsl --install` from powershell as an administrator (click on: https://learn.microsoft.com/en-us/windows/wsl/install).

### Basic Syntax

Git can be used through a *command line interface*. This means that you will be utilizing git through a *terminal emulator* on your computer (terminal for short). This is an interactive prompt that allows you to type commmands in the *bash* language to execute. An example of a terminal command that commits to the current working branch looks like this:

```
$ git commit --message 'Added methods for telemetry'
```

The `$` indicates the start of the line for a command to be entered by the user. It is not typed by the user but will exist on its own line in the terminal emulator by default. If the line is blank, then your terminal emulator is already executing a task and not ready for user input.

The command to be executed is then entered: `git`. Since the `git` command takes one required argument, it is then followed by the phrase: `commit`. In other words, you are telling the terminal that you would like to make a new git commit. Now you can finish the command by hitting the enter key and you have made a git commit. However, as I said earlier, it is good practice to include an optional message describing the purpose of the commit.

Next are the optional arguments or *options*. You can choose a specific option by starting with a `--` followed by one or more words. In this case, `--message` tells the `git commit` to include a message. If there is an option with multiple words it will probably look something like this: `--option-with-multiple-words`.

Since arguments are seperated by a space in bash, the message argument must start and end with a single quote mark or only the first word will be taken as the message. ie:

`$ git commit -m Added methods for telemetry` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Incorrect**\
`$ git commit -m 'Added methods for telemetry'` &nbsp; **Correct**

### Navigation

Feel free to follow along in your own computer's terminal emulator during this section.

The following paragraphs explain how to access a bash *shell* from your operating system. A shell runs inside of your terminal emulator and is what you interface with when typing out commands.

On a Windows computer you can search for "powershell" (don't run as administrator this time) and type `wsl` followed by the enter key. If the command hangs or is not found, that means you haven't set up WSL correctly and you must go back to the previous step to try again.

On MacOS search for the "terminal" app.

On Ubuntu search for "terminal".

When you first open your terminal emulator, there might be a message at the top. Below that message you should see your computer's username followed by a special symbol, usually a dollar sign. In my case, it looks like this:

```
eli@laptop$
```

Depending on what operating system you are using, this might look a bit different than mine (eg: a `%` symbol for macos). You may start typing your commands after the `$`, `%`, `>` or whatever other symbol is present. This is the indicator that lets you know the shell has finished executing its previous process and is ready for more input. If there is no symbol present then the terminal is currently executing a previous command and is not ready for you to type input.

I will now be using the `$` delimiter to mark the beginning of a line of input for the rest of this guide. It should appear automatically. You will see a `$` after your username and this is where you are meant to start typing your command. If you do not see a `$`, then that line is the output of the command. Don't get confused when you see a slightly different output. Ie: mine will say "eli" because that is my username. The first command I would like you to type is `pwd`:

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

Now that you are aware of the folders inside (I'll call them subdirectories from now on) use `cd` to **change** **directories**. This command takes a single argument unlike `ls`; you need to type the name of the directory to change to (case sensitive).

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

If you don't, I will provide a simple example. I start by generating a keypair: two keys that are mathematically linked. A key consists of randomly generated symbols and letters that would be difficult to guess. One key is public and is known by everyone. The other is private and must not be distributed.

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

Now that you are home you should type `ls --all` which will list all files and folders (hence `--all`). When you use the option `--all`, 0 arguments will follow. Once again, most command-line options also have a shorter, one letter counterpart. They are usually preceeded by a single `-` in their one letter form: `ls -a`. These options are functionally equiavalent. If you would like to see more options for a specific command, such as `ls`, you can follow it with a `--help` or `-h` option. If you already see a directory named `.ssh` (after executing `ls --all`), skip the next step.

If you do not see a `.ssh` directory, enter the command `mkdir .ssh`. This **makes** a **directory** under your current working directory called `.ssh`. Now you should run `ls -a` and see a directory named `.ssh` in the list of files and folders.

Now that everyone has an empty `.ssh` directory, you will need to generate an ssh keypair. To do this you will need to enter the following command, replacing the text in "\<filename\>" with a name of your choice without spaces and \<your_email_address\> with your actual email address:

```
ssh-keygen -a 100 -t ed25519 -f ~/.ssh/<filename> -C <your_email_address>
```

`-a` is an option that specifies the number of rounds of KDF (key derivation function) to be used. A higher number of rounds means the random strings are more secure; 100 is plenty. `-t` allows you to pick the type of function used to generate the keypair. We are using ed25519 because it is secure, convenient, and fast. `-f` lets you pick where to store the keypair and `-C` allows you to type out the email address that you will be identified with.

If you are prompted to enter a password, you can, or just leave it blank (it doesn't really matter which one you choose). However, if you are using a shared device I ***HIGHLY*** suggest you use a strong password.

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

The lines preceeded with a `#` are comments to help you remember the purpose of this file and not required. Basically this tells Git to use our `<filename>` and `<filename>.pub` keypair when connecting to github.com. Just make sure to replace all instances of "github.com" with "github-\<Github Account Username\>" when using a url with the former in it.

For example, instead of "git@**github.com**:ElijaRock/git-guide.git", I would change the url to "git@**github-ElijaRock**:ElijaRock/git-guide.git". You will need to do this for your username that you setup in the config file in the future. Don't worry about the semantics yet as they will be explained in the next parts of this guide.

## Part 4: Creating a GitHub Account

### What is Github?

Throughout this guide I have mentioned `git` many times and now I am talking about Github. What's the difference? Well, `git` is the tool that you use to interface with repositories on the Github website, which is a cloud service that allows you to store many repositories.

TLDR: `git` is the tool you work with; the results of using `git` are seen on the Github website.

### Account Creation

Visit [https://github.com/](https://github.com/) and create an account with your preferred email address, etc. Choose your preferred username, eg: "first-lastname", and you are good to go.

### Adding SSH key to account

Click on your profile picture on Github and press settings. 

![settings](/images/settings_edit.png)

Then, on the left, find `SSH and GPG Keys`, click on it.

![ssh_and_gpg_keys](/images/ssh_and_gpg_keys_edit.png)

Press `New SSH Key` and title it whatever you want.

![new_ssh_key](/images/new_ssh_key_edit.png)

 Key type: `Authentication Key`. Paste in the file contents of your public key (located in your `.ssh` folder): `<filename>.pub`. After doing this, we will repeat these steps but instead choose Signing Key.

![auth_key](/images/auth_key.png)

![signing_key](/images/signing_key_edit.png)

In total you will have added two keys. An authentication key is used if you are working on a private repository. This means that the people who created the repository only want specific trusted people to be able to view it, meaning you will need this key to gain access to the repository.

The signing key is used to sign your commits. This means that everyone knows you are the person who made the commit and not somebody posing as you. Now you can test your keypair like so (replacing \<Github Account Username\> with your actual Github account username):

```
$ ssh -T git@github-<Github Account Username>
Hi <Github Account Username>! You've successfully authenticated, but GitHub does not provide shell access.
```

Success! If you have not yet changed your `~/.ssh/config` file to include your actual github username, please do so now. You may get a prompt about a fingerprint and it will ask you to input yes or no, say yes.

## Part 5: Git

### Installing Git

Depending on which operating system you use, there may or may not be a *package manager* installed by default. A package manager does all the heavy work for you, preventing the need to search for applications on the internet.

On Ubuntu and WSL (Windows) systems, a package manager `apt` is included by default. On MacOS, you will need to install `brew` by following the steps on the [brew homepage](https://brew.sh/).

Once you have a package manager, you can then install `git` with the following command (depending on your operating system):

For Ubuntu and Windows users: `$ sudo apt install git`

For MacOS Users: `$ brew install git`

Aptitude (apt) requires that you have elevated permissions when installing packages, which requires you to preceed it with the `sudo` command ("Superuser Do") and enter your password to confirm. You may need access to an administrator account on your computer to execute some of these commands.

### Using Git to Clone a Repository

It's time to *clone* your first repository (repo). Cloning allows you to have a local copy of the code from a repository on your machine. Essentially you are "downloading" the code.

#### NOTE: You only need to clone a repository ONCE.

Open terminal and `cd` into a directory of your choice. Then, go to the top of the page on a Github repo that you would like to work on (if you don't have one you can create it yourself) and click on the `<> Code` green button, then click ssh and copy the link.

![code](/images/code.png)

Now you have to change the link. In my case, I am working on the "git-guide" repo and my username is "ElijaRock", therefore it looks like this: `git@github.com:ElijaRock/git-guide.git`. You should change it to look like the following: `git@github-ElijaRock:ElijaRock/git-guide.git`. I have changed the part that says "github.com" to "github-ElijaRock". This allows us to use our authentication key to gain access to the (private) repository. This is not necessary for a public repository but is still good practice.

Now you can type this into your terminal (replacing my Github username with yours):

```
$ git clone git@github-ElijaRock:ElijaRock/git-guide.git
$ ls
git-guide
$ cd git-guide
```

Now I am in the "git-guide" directory.

You must now tell git some information so it can properly identify you (you need to be inside of the cloned repo to do this):

```
$ git config user.name '<your name>'
$ git config user.email '<your email address>'
$ git config gpg.format ssh
$ git config user.signingkey ~/.ssh/<filename>.pub
```

Replace "\<your name\>" with your name, "\<your email address\>" with your email address, and "\<filename\>" with the name of your keypair. Make sure you are using the key ending in ".pub" (your public key).

You are now working in your cloned repo, on the `main` branch. ***NEVER*** commit to or push to the main branch directly. We want to protect the stability of the main branch.

You can see which branch you are working on using `git status`. It should say something like "On branch main" at the top, along with other information I will explain later.

You should create another branch if there is only branch main.

The first line creates a new branch called `new-feature`. You can call your new branch whatever you like, but there cannot be spaces (use the `-` instead). The second line switches from the `main` branch to the `new-feature` branch so you can start working on it without changing `main`.

```
$ git branch new-feature
$ git checkout new-feature
```
If you run `git status` again, this time it should say "On branch new-feature" since in the use case above, that's what I called my new branch. If you call your new branch something else, that name should appear.

If you would like to see a list of the existing branches at any time you can run `git branch`.

You can start working/coding inside of your branch.

### Tracking and staging your changes

If you want to see the changes you have made thus far type: `git diff` and it will highlight the differences between the original code and modified version.

Now you must *track* and/or *stage*, then commit the changes you have made to a repo. Files need to be tracked so that Git will see them. You track new files, but if you modify a file that is already tracked, what you are doing is called staging. Although these are two different names, you only have to run one command for either tracking and/or staging.

By default, any new files you create in the branch are untracked. That means that if you make a commit they will not show up and only stay on your machine. To track and stage a new file, you can type `git add <filename>`, followed by the actual file name. This can get tedious if you are adding a lot of new files, so you can also type `git add --all` which will track and stage the new files you have added.

There may also be some files on your computer that are already tracked. These need to be staged if you have made any modifications to them. In addition to tracking the newly created files, `git add <filename>` or `git add --all` commands can be used to stage the existing, modified ones. 

After you have tracked and staged your changes, you might be interested to see what the repo will look like before you commit. You can compare the branch on your computer with the one on the Github website by doing the following (change `new-feature` to the name of your branch).

```
git diff new-feature origin/new-feature
```

In this command, "origin" is shorthand for the url of the Github repo. In other words you are telling `git` to look at what is on the website and then compare it with what is on your computer. If it shows differences, that means you have properly staged your changes.

### Committing to the Repo

You can now commit to the repo. The `-S` option for the commit will sign it with your signing key. Then, type `-m` option to create a title for the commit. You should describe what you have changed in a few words here. If needed, you should use another `-m` option to provide a longer description of what you have done.

As noted in the example below, messages need to be enclosed in single quotes:

```
$ git commit -S -m 'This is a short message describing the changes I have made' -m 'This is a longer, more thorough description of the changes made'
```

You can ommit the second `-m` or "message" option if you have only made small, incremental changes. The `-S` option allows you to sign the commit with your signing key, in other words we can verify that **YOU** made these changes (in the case of identity theft). That is why it's so important to not give your private key to anyone ever.

The last step is to *push* the commit to the branch you are working on. Now that the commit is on your computer and everything is as you like it, you can update it to the repository online through a `git push` command. This will show the commit on github.com website and everyone else will be able to see it. Type (remember to replace `new-feature` with your branch name):

```
$ git push origin new-feature
```

This pushes it to the branch that you specified and now we all will be able to see the changes on the website.

Now visit the repository on github.com, go to the top left where is says you are on the main branch, and switch to `new-feature` branch. The checkmark will move from `main` to `new-feature`.

The website will refresh automatically and you should click on the `x commits`. x=the number of commits that have been made. It will be located below the `code` button you found earlier.

![x_commits](/images/x_commits.png)

You should now be able to see a list of commits including your own. If you have set up your ssh key correctly there will be a green `Verified` bubble on the right side of your commit. At the top left of the image, you can see that I am on the main branch; however, you should be on your new branch.

![commits_list](/images/commits_list.png)

![verified](/images/verified.png)

### Submit a Pull Request

If you feel that your branch has been extensively tested and is stable, you are ready to merge it back into the `main` branch.

Go to the `Pull Requests` tab at the top bar of the repository page, and choose `New Pull Request`.

![pull_req](/images/pull_req.png)

![new_pr](/images/new_pr.png)

At the top it will allow you to select the `new-feature` branch to be merged into `main` (see below). In my case, there is only one branch: `main` but yours will say "compare: new-feature to base: main" if your branch name is `new-feature`.

![merge_br](/images/merge_br.png)

Then provide a description of the changes you have made and submit the request. Some repositories require that at least one other person (usually a specialized contributor) review the code in your request before giving the OK. Then, they can accept the request so your branch can be merged into main. Congrats!

-------------------------------

You may have also noticed an `Issues` tab on the repo page to the left of "pull requests". You should open an issue for any problems or inaccuracies that you find in this guide.

So essentially the workflow is that there can be an `issue` opened for a new feature or a bug. Then, someone creates a new branch to work on the issue. Once that branch has been tested and is ready, a pull request can be submitted to merge that branch back into `main`. In the pull request, you will specify the issues that it fixes in order to close them.

### Fetch the Latest Changes and Merge

Suppose someone on your team worked on your branch while you were gone to help implement the feature. Before you start making changes on your computer again, you need to fetch their changes so you have the modified files. Do this by running the following command:

```
$ git fetch origin new-feature
```

Now you have their changes to compare to the branch on your computer, and need to review them before replacing the files on your computer with the new ones from the repo online.

If you want to see the difference between the `new-feature` branch on your computer and the repository's up-to-date `new-feature` branch, run this command:

```
$ git diff new-feature origin/new-feature
```

You are now looking at the changes your team member made to your branch compared to the one you had from before. If you like these changes, you can replace the current copy of the branch on your computer with the new version. This is called "rebasing". You can do the following:

```
$ git rebase origin/new-feature
```

You may get an error that you need to stash your changes before a rebase. This happens if you made changes and forgot to commit them. Git allows you to keep your changes in a special safe place called "stash" while having the latest version of the branch to work on.

```
$ git stash
```

You may then run `rebase` again and you will be on the most up to date version of `new-feature` branch. But your uncommitted changes will be missing (temporarily). If you would then like to reapply your changes to this latest version of the branch, run the following command:

```
$ git stash apply
```

Now I am on the latest version of the branch `new-feature` and can track and commit my changes as usual. Git forces you to do this because it would be impossible to have your commit and your peer's commit to be "next to each other" at the same time on the same branch. Essentially what you just did is moved your changes from "next to" the peer's commit to after their commit, creating a linear branch history (think of the image at the beginning of this guide). If this is confusing at first, do not worry, the more you use `git` the more you will understand over time.

I will remind you again that you only need to clone the repo once; always use fetch to grab the newest changes. Also, `git` has many many more features than specified here and can be used to do even more powerful things.

## Part 6: Best Practices

Now that you have the background knowledge, I will let you know of some best practices that I learned in the time that I have used git.

1. If ever in doubt run `git status`. This will give you valuable information on the current state of the git repo on your computer, changes you have made, etc.

2. Do not make changes to the main branch. The code on this branch is stable, tested, and reliable.

3. Do not force push other people's code off the repo. As annoying as it may be to deal with merge conflicts (you'll experience these at some point), do not force push your own code. Make sure to fetch and rebase to resolve the conflicts instead.

4. Write detailed commit messages. Make it obvious what your commit does to change the code (don't add a meaningless message).

5. Use the built-in issues feature. Get in the habit of submitting proper bug reports as an issue and document what you did and what didn't work. You can also use a `feature-request` tag on your issue to request a new feature, which can later be closed in the pull request that fixes the issue.

6. Read the docs. I have gathered almost all of my information from the [official Github documentation](https://docs.github.com/en). If you are ever unsure or would like to know about more powerful features, try reading the documentation first.