# Basic Git Command List

## First time Git setup

Tell the Git who you are

``` Shell
$ git config --global user.name "John Doe"
```

```Shell
$ git config --global user.email johndoe@example.com
```

## Clone your assignment

Git clonning means to store a local copy of the repository. We will be using a local copy to edit and make changes and push them for submission.

```Shell
$ git clone <assignment's https url>
```

## Pushing your assignment

Remember to clean all the binary files and the files before you proceed with this. Makefiles's `make clean` should do if you have set it up correctly.

**Step 1.** Stage all the tracked changes

```Shell
$ git add *
```

it might prompt you with the following if you have files or folders in your directory that are mentioned within the `.gitignore` file.

```Shell
$ git add *
The following paths are ignored by one of your .gitignore files:
<file_name>
<folder_name>
Use -f if you really want to add them.
```

> It's not an error message just a warning

Remove these file if you wish or just ignore and continue if this does not bother you.

**Step 2.** Commit these changes with a message explaining the reason for the submitting the assignment this could be basic fix for resubmission details.

```Shell
$ git commit -m "Version 0.1" -m "Reason for the commit"
```

>An alternative for `m` that will allow us to skip **Step 1.** is `-am`. Here the additional `a` stands for add.

**Step 3.** Push all these changes

```Shell
$ git push
```

> Note always go for `git pull` before staging changes when collaborating with multiple people on the same repo.

## For saving user credentials

Tired of entering your username and password each time you push your changes? Lets use the SSH encryption for saving and accessing your repository.

Lets begin for generating your customized SSH encryption key

```Shell
$ ssh-keygen -t rsa -C "johndoe@example.com"
```

For ideal results:

```Shell
$ ssh-keygen -t rsa -C "johndoe@example.com
Generating public/private rsa key pair.
Enter file in which to save the key (/home/<your_username>/.ssh/id_rsa): /<your_username>/.ssh/id_rsa

Enter passphrase (empty for no passphrase): <enter a passphrase with at least 8 characters>
```

Go to your GitHub account settings and select `SSH and GPG keys` tab and select `New SSH key` button. Give a title you wish and copy the content of the file `~/.ssh/id_rsa.pub` to the key section.

Moving back to your terminal, lets test the connection with

```Shell
$ ssh -T git@github.com
```

the above command will process, type `yes` when prompted and then it will ask for your **passphrase if you had setup one before**. Enter the **passphrase** and the execution should look like bellow:

```Shell
$ ssh -T git@github.com
The authenticity of host 'github.com (140.82.114.4)' can't be established.
RSA key fingerprint is SHA256:<your_key>.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,140.82.114.4' (RSA) to the list of known hosts.
Hi <git username>! You've successfully authenticated, but GitHub does not provide shell access.
```

Then, `cd` into your cloned repository and enter the command:

``` Shell
$ git remote set-url origin git@github.com:wmucs<classcode>/<assignment_repository_name>.git
```

Now you have completed the ssh setup for your repository, from now on your `git push` will not prompt form username and password.

**Note:** Your `$ git push` might execute as

```Shell
$ git push
Warning: Permanently added the RSA host key for IP address '140.82.113.4' to the list of known hosts.
Counting objects: 2, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 220 bytes | 220.00 KiB/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:wmucs3240/<repo-name>.git
   c9398d6..acd1536  master -> master
```

> This execution is as expected and works as expected.

## Saving user credentials 2

SSH too complicated for you??

Save your credentials locally, so that git remember who you are.

Before you say push enter the following command so that git locally store your username and password.

``` Shell
$ git config --global credential.helper store
```

## Other helpful commands

### For removing user credentials

`$ git config --global --unset credential.helper`

### Discard all changes since last commit

`$ git checkout -f`

### Reset the head to a previous commit

`$ git reset --hard <commit-hash>`
> Replace `<commit-hash>` with your actual commit hash numbers

**Notice:** Only first 8 digits/numbers(or more) should be sufficient to  uniquely identify that commit

### Find more about your previous commits

`$ git log`

> Must follow the steps ar the beginning for this to be relevant

`$ git reflog`

### For untracking a folder

Step 1. Add the folder path to your repo's root .gitignore file.

`path_to_your_folder/`

Step 2. Remove the folder from your local git tracking, but keep it on your disk.

`$ git rm -r --cached path_to_your_folder/`

Step 3. Push your changes to your git repo.

### Remove untacked files

1. First know what you will be deleting: `$ git clean -nfd`

1. Then delete it, if all looks good: `$ git clean -fd`

### Solving merge conflit

Anything locally is before the `=====` section and the version online is the one after before `<<<<<<<` section folowed by a hash.

Remove anything section that you want to do not want to commit and then re-add and merge.
