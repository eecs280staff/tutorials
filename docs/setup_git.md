---
layout: spec
title: Version Control
sitemapOrder: 30
---

Version Control Tutorial
========================
{: .primer-spec-toc-ignore }

This tutorial will walk you through setting up a Git repository, linking it to GitHub, and using it to collaborate with a partner.  This is the EECS 280 adaptation of the [EECS 485 Version Control Tutorial](https://eecs485staff.github.io/p2-cv/setup_git.html).

<div class="primer-spec-callout danger" markdown="1">
**WARNING:** Do not post any code in a publicly accessible repo!
</div>

<div class="primer-spec-callout info" markdown="1">
If you've used version control before on your computer, skip to the [Create a local repository](#create-a-local-repository) section.
</div>

## Prerequisites
We're assuming that you already have a folder, e.g.,  `p2-cv/` or `p1-stats/`. Your folder name may be different. It's OK if there aren't any files in it yet.
```console
$ pwd
/Users/awdeorio/Developer/eecs280/p2-cv
```

You have installed `git`.  Your version might be different.
```console
$ git --version
git version 2.37.2
```

You have configured your name and email address.
```console
$ git config --global user.name
Andrew DeOrio
$ git config --global user.email
awdeorio@umich.edu
```

If you need to set your name and email, here's how.
```console
$ git config --global user.name "YOUR NAME HERE"
$ git config --global user.email "YOUR EMAIL HERE"
```

## Restarting this tutorial
If you tried using this tutorial in the past and want to start over, here's how to delete the files created by `git` and how to remove your repository from GitHub.

First, remove your GitHub repository.  Browse to your repository's project page from [https://github.com/](https://github.com/).  Select "Settings", scroll to the bottom and select "Delete this repository" under the "Danger Zone" heading.

<img src="images/github001.png" width="768px" />

...

<img src="images/github002.png" width="768px" />

Next, remove the hidden files created by `git`.  Remember, hidden files start with a dot (`.`).
```console
$ pwd
/Users/awdeorio/Developer/eecs280/p2-cv
$ rm -rf .git/ .gitignore
$ ls -A
```

Confirm that this is no longer a git repository.
```console
$ git status
fatal: Not a git repository (or any of the parent directories): .git
```

## GitHub Authentication
There are two ways to connect to GitHub: SSH Keys and GitHub Personal Access Tokens Keys.  An SSH Key is a special file that you can use to connect to remote terminals.  A Personal Access Token works like a separate password used just for GitHub.

**We recommend SSH Keys.**

### SSH Keys
Check for existing SSH keys.  You already have an SSH key if you see one of these files.  If you get an error that `~/.ssh` does not exist, you can create it in the next step.
- `id_rsa.pub`
- `id_ecdsa.pub`
- `id_ed25519.pub`
```console
$ ls ~/.ssh
id_ed25519
id_ed25519.pub
...
```

If you don't have an SSH key, generate one.  
```console
$ ssh-keygen -t ed25519 -C "your_email@example.com"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/awdeorio/.ssh/id_ed25519): 
```
Press Enter to save the file to the default location.

We recommend entering a passphrase in the following prompts.
```console
Enter passphrase (empty for no passphrase): <put your passphrase here!>
Enter same passphrase again: <put your passphrase here!>
...
Your public key has been saved in /Users/awdeorio/.ssh/id_ed25519.pub
```

Copy your SSH public key (the file ending with `.pub`).  Select and copy the output.  **Your output will be different.**
```console
$ cat ~/.ssh/id_ed25519.pub
ssh-ed25519 KLBJDjlkaksfadhinoueliwekljhfasdlkjhfdss/asdnfkjlnaksjdfdfnkljdafslF awdeorio@umich.edu.com
```
<div class="primer-spec-callout danger" markdown="1">
**WARNING:** Do not share your private key with anyone!  It's the file that looks like `id_ed25519`.
</div>

Navigate to Profile > Settings > Access > SSH and GPG Keys.  Here's a [quick link](https://github.com/settings/keys).  Click New SSH key or Add SSH key.

<img src="images/github_new_ssh_key.png" width="768px" />

Paste your SSH public key.  **Your paste will be different.**  ([GitHub docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?platform=linux#adding-a-new-ssh-key-to-your-account)).

<img src="images/github_paste_ssh_key.png" width="768px" />

Test your connection.
```console
$ ssh -T git@github.com
Hi awdeorio! You've successfully authenticated, but GitHub does not provide shell access.
```

This section is based on the [GitHub SSH docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

### Personal Access Token
<div class="primer-spec-callout info" markdown="1">
Skip this section if you are using SSH Keys.
</div>

A Personal Access Token is an alternative to SSH Keys.

Login to GitHub.  Navigate to Profile > Settings > Developer Settings > Tokens (classic).  Here's a [direct link](https://github.com/settings/tokens).

Click on Generate new token and from the drop down menu, click on Generate new token (classic).

<img src="images/github_new_token.png" width="768px" />

Give your token a name and an expiration.  We recommend setting the expiration date for one year.

<img src="images/github_token_view.png" width="768px" />

Check only the `repo` box.

<img src="images/github_scopes.png" width="768px" />

Scroll down and click generate token.

<img src="images/github_gen_token.png" width="768px" />

#### Copy token
{: .primer-spec-toc-ignore }

Copy your token.  You'll need it later when you do `git push`.  **Do not close the browser tab. You won't be able to see the token again!**

<img src="images/github_copy_token.png" width="768px" />

Avoid entering your access token every time by configuring the local Git client to save the GitHub personal access token. 
```console
$ git config --global credential.helper store
```

## Create a local repository
Only one team member creates the first local repository.

Navigate to your project directory.  Your directory might be different.
```console
$ pwd
/Users/awdeorio/Developer/eecs485/p2-cv
```

### Add a `.gitignore` file
Add a `.gitignore` file.  This will prevent Git from controlling files like compiled binaries or locally installed Python libraries.  It's a hidden file (starts with a dot `.`), so you'll need to use `ls -A` to see it.

#### C/C++ Projects
This sample is pre-configured to work with most C++ projects, so use the same sample file for all EECS 280/281 projects.
```console
$ pwd
/Users/awdeorio/src/eecs280/p2-cv
$ wget https://eecs280staff.github.io/tutorials/dot_gitignore_sample -O .gitignore
$ ls -A
.gitignore     ...
```

### Initialize repo
Initialize the repo.  You may have more untracked files.
```console
$ git init
Initialized empty Git repository in /Users/awdeorio/Developer/eecs280/p2-cv/.git/
$ git status
On branch main

Untracked files:

	.gitignore
    ...

nothing added to commit but untracked files present (use "git add" to track)
```

### Add existing files to version control
First, double-check that you have a `.gitignore` file.  Your gitignore might be different.
```console
$ head .gitignore
This is a sample .gitignore file that's useful for C++ projects.
... OR ...
This is a sample .gitignore file that's useful for EECS 485 projects.
```

Add your `.gitignore` file and any additional starter files.  See that your existing files are ready to be committed.  You may have more files.
```console
$ git add .
$ git status
On branch main

Changes to be committed:

	new file:   .gitignore
    ...
```

Commit the added files to the local repo with the commit message "Initial commit".
```console
$ git commit -m "Initial commit"
[main (root-commit) cefd222] Initial commit
 1 file changed, 71 insertions(+)
 create mode 100644 .gitignore
```

View the commit log and see our first commit.
```console
$ git log
commit cefd2227510fa5e16e357198be19832b952d314e (HEAD -> main)
Author: Andrew DeOrio <awdeorio@umich.edu>
Date:   Tue Aug 30 19:29:52 2022 -0400

    Initial commit
```

See that the status is clean.
```console
$ git status
On branch main
nothing to commit, working tree clean
```

<div class="primer-spec-callout warning icon-warning" markdown="1">
Verify you didn't accidentally commit automatically generated or binary files.  Binaries that work on your machine may not work on another machine (like a group member's or CAEN Linux).
```console
$ git ls-files
...
main.exe
main.exe.dSYM/Contents/Info.plist
main.exe.dSYM/Contents/Resources/DWARF/main.exe
...
```

First, double-check your `.gitignore`.  Go [back](#add-a-gitignore-file) if necessary and re-download it.
```console
$ head .gitignore
# This is a sample .gitignore file that's useful for C++ projects.
...
```

Then, remove the offending binary files and folders and commit.
```console
$ git rm -f main.exe
$ git rm -rf main.exe.dSYM
$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	deleted:    main.exe
	deleted:    main.exe.dSYM/Contents/Info.plist
	deleted:    main.exe.dSYM/Contents/Resources/DWARF/main.exe
$ git commit -m "remove binary files"
$ git status
On branch main
nothing to commit, working tree clean
```
</div>

## Create a remote repository
Only one team member creates the remote repository.

First, log in to [https://github.com/](https://github.com/login).

Create a new project.

<img src="images/github003.png" width="384px" />

Call the new project `p2-cv`, mark it as "private". Click "Create repository".

<div class="primer-spec-callout danger" markdown="1">
**Double check that you have selected "private".** Making your project code or test cases publicly available online is a violation of the honor code.
</div>

<img src="images/github004.png" width="768px" />

Triple-check that your new repository is private.  If you made a mistake, you can change it to private in "Settings".

<img src="images/github005a.png" width="768px" />

You now have a project page for your remote repo.  In this example, awdeorio's project page is [https://github.com/awdeorio/p2-cv](https://github.com/awdeorio/p2-cv). You won't have access to it because it's private, but your page should look similar.

<img src="images/github005b.png" width="768px" />

#### Connect local repo to remote repo
Browse to your repository's project page from [https://github.com/](https://github.com//) and copy the repo URL.

| SSH Keys | Personal Access Tokens |
| - | - |
| Copy the URL from the `SSH` tab. [SSH Keys](#ssh-keys-recommended) | Copy the URL from the `HTTPS` tab. [Personal Access Tokens](#personal-access-token) |
| <img src="images/github_clone_ssh.png" width="384px" /> | <img src="images/github_clone_https.png" width="384px" /> |

Connect your local repo to your remote repo.
```console
$ pwd
/Users/awdeorio/Developer/eecs485/p2-cv
$ git remote add origin https://github.com/awdeorio/p2-cv.git  # use your URL
```

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Double-check that your local repo is connected to your remote repo.

If you're using [SSH Keys](#ssh-keys-recommended), you'll see `git@github.com`.
```console
$ git remote -v
origin	git@github.com:awdeorio/p2-cv.git (fetch)
origin	git@github.com:awdeorio/p2-cv.git (push)
```

If you're using GitHub [Personal Access Tokens](#personal-access-token), you'll see `https://`.
```console
$ git remote -v
origin	https://github.com/awdeorio/p2-cv.git (fetch)
origin	https://github.com/awdeorio/p2-cv.git (push)
```
</div>

Push commits already committed on the local repo to the remote repo. If you are using Personal Access Tokens, the password is the token you [copied earlier](#copy-token). You will not see a prompt if you use SSH as it does not require a password.
```console
$ git push -u origin main
Username for 'https://github.com': awdeorio
Password for 'https://awdeorio@github.com':
Counting objects: 14, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 6.97 KiB | 3.48 MiB/s, done.
Total 14 (delta 0), reused 0 (delta 0)
To https://github.com/awdeorio/p2-cv.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

<div class="primer-spec-callout warning" markdown="1">
**MacOS Pitfall:** If you are not prompted for your username and password when using a Personal Access Token, your credentials may be cached on your computer. [Clear your credentials from Keychain Access](https://docs.github.com/en/enterprise-server@3.4/get-started/getting-started-with-git/updating-credentials-from-the-macos-keychain#updating-your-credentials-via-keychain-access).
</div>

<div class="primer-spec-callout warning" markdown="1">
**Pitfall:** Your local `git` may use `master` as the name for the initial branch, whereas GitHub expects it to be named main.

```console
$ git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'github.com:awdeorio/p2-cv.git'
```

You can rename your local branch to `main` to resolve this.

```console
$ git branch
* master
$ git branch -M main
$ git branch
* main
```

</div>

Verify that your local repo is up to date with the remote the repo (AKA `origin/main`).
```console
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Verify the commit log.
```console
$ git log
commit cefd2227510fa5e16e357198be19832b952d314e (HEAD -> main)
Author: Andrew DeOrio <awdeorio@umich.edu>
Date:   Tue Aug 30 19:29:52 2022 -0400

    Initial commit
```

See the commit log on your GitHub project page.  Click "1 commit".  (You may need to refresh the page to see this.)

<img src="images/github007.png" width="768px" />

You should see your first commit show up.

<img src="images/github008.png" width="768px" />

## Daily work flow with version control
This section describes the work flow for a coding session with `git`.

Start with clean files.
```console
$ git status
On branch main
Your branch is up-to-date with 'origin/main'.

nothing to commit, working tree clean
```

Retrieve any changes from the server.  For example, you might have pushed changes while working on another computer, like CAEN Linux, or your partners may have made changes, if applicable.
```console
$ git fetch
$ git rebase
```

Make changes to files.  Add and commit when you're ready.  You can do this several times.
```console
$ git add SOME_FILE
$ git commit -m "Short description goes here"
```

Push changes to GitHub server.
```console
$ git push
```

If you get a `rejected` error, see [Fixing rejected pushes](#fixing-rejected-pushes), below.

### Add a new file to version control
Let's practice by adding a README, which will show up on the front page of our GitHub repository web page.

Navigate to the directory containing your source code using the [`cd`](cli.html#cd) command.

We have no modified files and our code is in sync with the code stored on GitHub.  In other words, we haven't changed anything since the last `git commit`, and no changes have happened on the GitHub server.
```console
$ git fetch
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Add an empty file called `README.md` using the command line.
```console
$ touch README.md
$ ls
README.md
```

Now, using the text editor or IDE of your choosing, edit `README.md` to look like the following example.  Be sure to change the name :)
```markdown
EECS 485 Project 1 Solution
===========================
Templated Static Site Generator

By Andrew DeOrio <jmapple@umich.edu>
```
{: data-title="README.md" }

You can see that the new file shows up as untracked.
```console
$ git status
On branch main
Your branch is up-to-date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README.md
```

We want to track `README.md`, so `git add` it.
```console
$ git add README.md
$ git status
On branch main
Your branch is up-to-date with 'origin/main'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   README.md
```

Commit, and you'll see that the repo has a clean status again.
```console
$ git commit -m "Added README"
[main e016bfa] Added README
 1 file changed, 5 insertions(+)
 create mode 100644 README.md
$ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)
```

From the previous `git status` command, we see that we have added code on our local computer that hasn't yet been pushed to the remote GitHub server (`Your branch is ahead of 'origin/main' by 1 commit.`).  Let's push it now.

<div class="primer-spec-callout info icon-info" markdown="1">
**NOTE:** if you get a `rejected` error, see [Fixing rejected pushes](#fixing-rejected-pushes), below.
</div>

```console
$ git push
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 361 bytes | 361.00 KiB/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To https://github.com/awdeorio/p2-cv.git
   350f38e..7fa8093  main -> main
$ git status
On branch main
Your branch is up-to-date with 'origin/main'.

nothing to commit, working tree clean
```

Browse to your repo's project page on GitHub and you'll see a fancy formatted version of your README.

<img src="images/github009.png" width="768px" />

### Modify a version controlled file
Let's practice modifying an existing file.  We'll add to the README.

Navigate to the directory containing your source code using the [`cd`](cli.html#cd) command.

Check that our version controlled files are clean and up to date with the remote repo.
```console
$ git fetch
$ git status
On branch main
Your branch is up-to-date with 'origin/main'.

nothing to commit, working tree clean
```

Edit `README.md` to add quick start instructions.  We're using [Markdown](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) formatting.  It should look this when you're done:
````markdown
EECS 485 Project 1 Solution
===========================
Templated Static Site Generator

By Andrew DeOrio <jmapple@umich.edu>

## Quick start
```console
$ python3 -m venv env/
$ source env/bin/activate
```
````
{: data-title="README.md" data-highlight="7-11" }

We can see that our files are no longer clean.
```console
$ git status
On branch main
Your branch is up-to-date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

See what's different.  You can see lines added (`+`) compared to the last clean committed version of the file.
```console
$ git diff README.md
diff --git a/README.md b/README.md
index 4641b12..b1dde51 100644
--- a/README.md
+++ b/README.md
@@ -3,3 +3,9 @@ EECS 485 Project 1 Solution
 Templated Static Site Generator
 
 By Andrew DeOrio <jmapple@umich.edu>
+
+## Quick start
+```console
+$ python3 -m venv env/
+$ source env/bin/activate
+```
```
{: data-highlight="10-15" }

<div class="primer-spec-callout info icon-info" markdown="1">
**NOTE:** On Windows, you may see lines that end with the `^M` character. This is normal; Windows uses a different convention for line endings than other operating systems.
</div>

Add, commit, and push.

<div class="primer-spec-callout info icon-info" markdown="1">
**NOTE:** if get a `rejected` error, see [Fixing rejected pushes](#fixing-rejected-pushes), below.
</div>

```console
$ git add README.md
$ git commit -m "Added quick start to README"
[main 2fac8d1] Added quick start to README
 1 file changed, 6 insertions(+)
$ git push
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 348 bytes | 348.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
To https://github.com/awdeorio/p2-cv.git
   a80db0b..2fac8d1  main -> main
```

View the commit history from the command line.
```console
$ git log
commit 6fce59386afd9c85b6de8953b1bd5e3fdbe9fcd3 (HEAD -> main)
Author: Andrew DeOrio <awdeorio@umich.edu>
Date:   Tue Aug 30 19:40:40 2022 -0400

    Added quick start to README

commit bfc9dc8341ca3999b7aad2775b625b6ddce63caf (origin/main)
Author: Andrew DeOrio <awdeorio@umich.edu>
Date:   Tue Aug 30 19:35:21 2022 -0400

    Added README

commit cefd2227510fa5e16e357198be19832b952d314e
Author: Andrew DeOrio <awdeorio@umich.edu>
Date:   Tue Aug 30 19:29:52 2022 -0400

    Initial commit
```

View the commit history on GitHub.  Click a commit message to see a graphical diff.

<img src="images/github010.png" width="768px" />

View the diff on GitHub.

<img src="images/github011.png" width="768px" />

## Version control for a team

### Add collaborators
Add your partner as a "Collaborator" on the remote repo by inviting them in the "Collaborators" tab.

   <img src="images/github012.png" width="768px" />

   <img src="images/github013.png" width="768px" />

   <img src="images/github014.png" width="768px" />

GitHub sends a confirmation email to your partner.  Your partner clicks accept.

Your partner creates an SSH key or a GitHub Personal Access Token using the [GitHub Authentication](#github-authentication) instructions.

Your partner `clone`s the remote repo on their own local machine using the same remote URL that you do.  Notice that `awdeorio` uses a link that has `awdeorio` in it, that's because `awdeorio` is a member of the repo that `awdeorio` created.
```console
$ whoami
awdeorio
$ pwd
/Users/awdeorio/src/eecs485/
$ git clone https://github.com/awdeorio/p2-cv.git
Cloning into 'p2-cv...
```

Remember, other team members don't need to download the starter files again because those files were already added by the first team member.

### Resolving conflicts
The following text is copied from a [helpful GitHub article](https://help.github.com/articles/resolving-merge-conflicts-after-a-git-rebase/).

When you perform a `git rebase` operation, you're typically moving commits around. Because of this, you might get into a situation where a merge conflict is introduced. That means that two of your commits modified the same line in the same file, and Git doesn't know which change to apply.

After you reorder and manipulate commits using `git rebase`, should a merge conflict occur, Git will tell you so with the following message printed to the terminal:

```console
error: could not apply fa39187... something to add to patch A

When you have resolved this problem, run "git rebase --continue".
If you prefer to skip this patch, run "git rebase --skip" instead.
To check out the original branch and stop rebasing, run "git rebase --abort".
Could not apply fa39187f3c3dfd2ab5faa38ac01cf3de7ce2e841... Change fake file
```

Here, Git is telling you which commit is causing the conflict (`fa39187`).

If you did this by mistake, you can undo the `git rebase` with `git rebase --abort`.

To fix the conflict, follow [this how-to]( https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/).

### Fixing `rejected` pushes
If you get a `rejected` error when using `git push`, this probably means you (or your partner) changed your repo using another computer (or the GitHub web interface).
```console
$ git push
To https://github.com/awdeorio/p2-cv.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/awdeorio/p2-cv.git'
```

Fetch changes from the remote repo.
```console
$ git fetch
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 2), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From https://github.com/awdeorio/p2-cv
   4d375b4..2b9bea7  main     -> origin/main
```

See that the local and remote repos have diverged.
```console
$ git status
On branch main
Your branch and 'origin/main' have diverged,
and have 1 and 1 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

nothing to commit, working tree clean
```

Reapply your local commits on top of your remote commits, combining them into one sequence of commits.
```console
$ git rebase
First, rewinding head to replay your work on top of it...
Applying: Local update to README.
```

Divergence between local and remote is fixed.  You're good to push.
```console
$ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
$ git push
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 318 bytes | 318.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0)
To https://github.com/awdeorio/p2-cv.git
   2b9bea7..63a5d0b  main -> main
$ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

## Acknowledgments
Original document written by Andrew DeOrio <awdeorio@umich.edu>. Updates made to use GitHub by Andrew DeOrio <jmapple@umich.edu>.

This document is licensed under a [Creative Commons Attribution-NonCommercial 4.0 License](https://creativecommons.org/licenses/by-nc/4.0/). You're free to copy and share this document, but not to sell it.  You may not share source code provided with this document.
