Mini git tutorial 1 - getting the Campbell Lab started
================
Amy Bauer
last edited “2024-01-24”

------------------------------------------------------------------------

*This mini workflow assumes you already have*

**UPDATE 1/24/2024** *for Windows 11, install wsl as described in 1.,
then install the WSL2 app from the windows app store*

*1. wsl installed on your Windows computer. You can install wsl by
opening PowerShell and running `wsl --install`. For more information,
see <https://docs.microsoft.com/en-us/windows/wsl/setup/environment>*

*2. git installed on your computer - for more information, see
<https://git-scm.com/downloads>*

*3. a chosen directory that you want to work inside (e.g., a ‘GitHub’
folder)*

*If this is not the case, do this before getting started here.*

------------------------------------------------------------------------

# Getting ready on a Windows Computer

### Open wsl and some basics

We start by opening the Windows Subsystem for Linux (wsl) on our
computer. Wsl allows us to run a Linux environment directly on our
Windows system, without having to use a traditional virtual machine or
dualboot setup.

We can access wsl by using the **search** tool, entering **wsl**.

To access your Windows PC’s files, wsl mounts drives or other removable
disks to the system (think of it as inserting a USB drive). We can
change mounts easily by using `/mnt/`. After running wsl, it mounts to
the Windows system by default, but you can mount any drive that your
Windows PC recognizes. We then change the mounted location to the GitHub
folder/repository that we want to work in.

    cd /mnt/[filepath]                                # cd : change directory
    cd /mnt/c/Users/amely/OneDrive/Documents/GitHub   # example path 

    mkdir [directoryname]                             # make a new directory within your current working directory

    # to move between folders:
    cd [foldername]                                   # move downstream into a directory 
    cd ..                                             # navigate up one directory level
    cd -                                              # navigate to previous directory (go back)

### Create and add a SSH key

Next, we want to create a SSH key and add it to our GitHub account. The
Secure Shell Protocol (SSH) provides a secure channel over an unsecured
network and allows us to connect and authenticate to a remote server.
The SSH key validates your, or rather your computer’s identity, and
enables us easily push and pull changes to and from the remote
repository later on as it allows us to bypass entering our GitHub
credentials every time we want to make a change.

We first generate a new SSH key on our computer using `ssh-keygen -o`.
If desired, we can then navigate to the file in which to save the key,
but let’s just go ahead and hit enter. We are then prompted to enter a
passphrase if desired, and by hitting enter twice, we continue without a
passphrase. We then print the public key using `cat` and the path to the
public key.

    ssh-keygen -o               # create a new SSH key
    cat [path to public key]    # print the public key

We can now copy the SSH key (it starts with *ssh-rsa*) and use our
browser to head to GitHub, where we navigate to our profile settings.
Here, we klick on the **SSH and GPG keys** tab, enter our new SSH key,
and save it.

------------------------------------------------------------------------

# Get started with git

### Setting up git

Let’s configure git within our system before we continue. This step
ensures that your future commit messages have your name and email
embedded and thus can easily be identified as yours. To do so, we can
use the `git config` command. You only need to configure git once, but
if you forgot your configurations or want to check them, you can easily
do so by listing the git configurations.

    git config --global user.name "[Your Name]"
    git config --global user.email "[youremail@domain.com]"

    git config --list          # we can check our configurations any time by using this

### Creating and cloning

Now that we navigated to our chosen folder, we can go ahead and
initialize a git repository within this directory or make a local copy
of a online git repository to our computer by cloning it from github. In
our browser, we navigate to the repository we wish to clone. Here, we
click on the green code button to retrieve the link. Since we previously
added a SSH key to our profile, we will clone using the **SSH** path.

<img src="./github/git_clone.png" alt="find clone link on github" width="533" height="240">

    git init                   # create a new local repository

    git clone <url>            # make a local copy of a repository from the web
    git clone git@github.com:Campbell-Lab-FMEL/test.git   # this is the SSH path

We can also clone using the **https** url. However, we would then have
to enter our github login and personal token every single time we want
to push or pull something from the remote repository.

Now that we downloaded a copy of the remote repository to our directory,
we can check if the cloning was successful by listing all folders inside
our current directory using the `ls` command. We will then navigate into
the newly created directory.

    ls                          # lists all elements within the directory

    cd [directoryname]/         # navigate into the the named directory
    cd test/                    # in this example, into the new folder "test"

We now can take a look at the branches of this repository using the
`git branch` command and, if necessary, create a new branch from the
current commit to work in.

    git branch                  # shows all local branches
    git branch -a               # shows all local AND remote branches

    git branch [branch name]    # creates new branch from current commit
    git branch amy              # creates new branch from current commit named amy

For us, it makes sense to have individual ‘developing’ branches to work
in, and to gather ready projects in the main branch.

Now we can switch to the branch we want to work in using `git checkout`.
We can also combine creating a new branch and switching into it in one
command.

    git checkout [branch name]  # switch into specified branch
    git checkout amy            # switch into branch amy

    git checkout -b amy         # switch into new branch amy

We can also combine any changes that have been made in another branch
into our current working branch using the `git merge` command. For
example, we might want to merge new changes that were made in branch
*amy* into our *main* branch.

    git merge [other branch]    # merge changes from other branch into current branch
    git merge amy               # merge changes from amy into e.g., main

### Saving changes in git - create a new “snapshot” of your work

Does this look familiar?

<img src="./github/git_savedfiles.png" alt="Screenshot of a timeline based name saving system for word documents" width="535" height="90">

Git essentially creates something very similar and thus serves as a
timeline management utility. With above word documents serving as
analogy, each new commit of a staged change is comparable to a new
document created with the save as function. Each commit is a snapshot
along the timeline of the git project that captures the state of the
project at the point in time the commit is created. Additionally, git
will never change commits (unless explicitly told to), and they can be
used to backtrack when, by whom, and what kind of changes were made, and
serve as backups.

First, we need to add a change in the working directory to the staging
area using the `git add` command. While this command in itself does not
visibly affect the repository, it is necessary to subsequently take the
snapshot of the staged file(s) using `git commit`. Don’t forget to
include a **short**, descriptive message with your commit.

    git add [file]              # Adds a file to the staging area
    git add thebestloopever.r   # the loop is perfect and should totally be shared with everyone

    git add [directory]         # Adds all the files in a specified directory to the staging area
    git add myfolder/           

    git add .                   # Adds all the files in the working directory to the staging area

    git commit -m "[message]"   # Creates a new commit from changes added to the staging area 

### Check changes

Let’s take a look at changes in our working directory. To do this, we
can use the `git status` command to check the status of the changed
files in our working directory, and `git diff` to show any uncommitted
changes since the last commit.

    git status                  # Shows status of changed files in the working directory

    git diff                    # Shows differences in changed files that are not staged

    git diff [file]             # shows differences in a specific file
    git diff thebestloopever.r  

    git diff --staged           # Shows differences in changed files that are staged but not committed

    git diff [branch name]      # Shows differences between current branch and [branch name]

In addition, we can also temporarily stash changes we have made. This is
useful when we need to switch into another branch to work on something
else, but we are midway through a change that we are not ready to commit
yet. Admittedly, this function may (?) be more useful to, e.g.,
programmers but let’s include it here for completeness.

    git stash                   # Puts your current changes into a temporary stash

    git stash pop               # Applies stashed changes to current wd and removes them from stash
    git stash apply             # Applies stashed changes to  current wd AND keeps them in the stash

    git stash drop              # Deletes all changes stored in the stash

### Inspect history

As discussed above, each commit represents a snapshot of a given project
in time. As we continue working developing our project and commit our
changes, git starts building a history. Git allows us to review and
revisit any commit, using the `git log` command. Each commit has a
unique ID that we can then use to visit, and even restore our project
to, that specific commit.

    git log                     # Shows history of the currently selected branch

    git log --branches=[name]   # Shows history of the 

We can then go and visit any previous commit using `git checkout`. This
will temporarily match our working directory to the exact state of the
project at the time of the commit of interest. We can then look at it,
run it, and even make changes without having to worry about loosing the
current state the project is in, as none of our actions will be saved to
our repository.

    git checkout [commit ID]     # Loads exact state of the project at time of the commit 

    git checkout [branch name]   # Returns to the current state of the project in the named branch

### Undo changes

*As of May 2022, this is not something I have dealt with in detail yet -
please proceed with caution*

Suppose we change our mind and do not like our changes anymore, maybe
because our absolutely perfect and amazing loop code totally broke after
Petrusilius Zwackelmann walked around on Amy’s keyboard while she went
to grab a snack. Unbeknownst of Petrie’s sabotage, she then went and
staged (or even committed) the changed .r file.

<img src="./github/git_serotonin_boost.jpg" alt="Screenshot of wsl pushing a project to remote main branch" width="535" length="266">

Luckily, we can undo changes using the `git reset` command.

    git reset                   # Removes everything from the staging area (but not the working directory)

    git reset [file]            # Remove a specific file from the staging area 

    git reset --hard            # Removes everything from the staging area AND resets the working directory to the most recent commit

    git reset --hard [commit]   # Sets branch to the specified [commit]. Discards all local changes
    git reset --hard fd5b0f3e5f3589238    # Petrie will totally get house arrest if this ever becomes necessary

We could also have committed a project prematurely and some changes to
our code are still missing, which we noticed soon after. Instead of
removing or resetting **the last commit**, we can go and amend it. For
this, we stage the desired ‘final’ state of the project and use
`git commit --amend` to commit it. We are then able to modify the last
commit message, and the new changes will be added to the previous
commit.

    git add                     # stage the updated version of the last commit
    git commit --amend          # amends the last commit 

------------------------------------------------------------------------

# Collaborating with git - let’s meet on GitHub

### git remote

While we now have been committing our project, these commits have been
made locally only so far. To collaborate, we now want to be able to
share our work with our lab mates. The `git remote` command allows us to
create, view, and delete connections with other repositories, by saving
connections to a url as convenient names in our `./.git/config` file.

    git remote                      # list all remote connections to other repositories
    git remote -v                   # list all remote connections and their underlying urls

We can also take a look at all remote branches we currently have access
to using the `git branch` command.

    git branch -r                   # lists all remote branches

To create, edit, or remove a connection, we then use the `git remote`
command.

    git remote add [name] <url>     # create a new connection to a remote repository and save it under the specified name
    git remote add [name] <url> -f  # create a new connection and immediately `git fetch` 

    git remote rename [oldname] [newname]

    git remote get-url [name]       # Shows the underlying url

    git remote rm [name]            # remove the connection to the named repository

Whenever we clone a repository using `git clone`, it will automatically
create a connection called **origin** pointing towards the cloned
repository. Good to know: **origin** is a commonly used name for a
project’s central repository, and you’ll come across this name often in
tutorials.

### Fetching and pulling from a remote

Suppose that for a new analysis Yasmin wants to expand a code Amy wrote
to format data. Since Amy had pushed the code file to the lab’s repo on
github, Yasmin can easily access the file even when Amy is currently not
available to share it.

We can now use `git fetch`to review content by showing changes that have
been made at the origin repository without integrating it into our local
repository yet, which allows for a more cautious approach. We can also
use `git pull`, which executes `git fetch` immediately followed by
`git merge`, creating a merge commit for the new content.

    git fetch [remote]              # checks changes that have been made on the remote repository but does not apply them
    git merge [remote]              # merges changes from remote into your local repository 

    git pull [remote]               # get all changes from remote and merge into local repository

We can also fetch or pull a specified branch instead of the entire
repository.

    git fetch [remote] [branch]     # checks changes that have been made in the specified branch of the remote repository

    git pull [remote] [branch]      # fetches and merges changes from a specific branch of the remote repository

    git pull origin amy             # allows Yasmin to pull changes from Amy's branch 

### Push to a remote - ready for the world to see!

We are now ready to share our recent project with the world, or at least
with our collaborators! We are happy, but before we head out to get some
frozen orange juice and stroll along the beach, we want to actually make
it accessible by pushing it to our remote repository. We can do this
using the `git push` command.

    git push [remote] [branch]    # push local changes from current branch to the specified remote branch

    git push origin yasmin        # pushes the local changes to yasmin's branch on the remote repository

### Publishing a project in the main branch

Once we are ready to move a project (e.g., a code file) from a
developing branch to the our main branch, we then go through a number of
steps: We pull both the developing branch and the main branch from the
remote repository to ensure we are up to date with all changes that have
been made to both branches, before we merge them locally. We then push
the merged local repository main branch to the remote repository main
branch.

    git checkout [developing branch]        # switch into the local developing branch
    git pull origin [developing branch]     # pull the remote developing branch into the local branch

    git checkout main                       # switch into the local main branch
    git pull origin main                    # pull the remote main branch into the local branch

    git merge [developing branch]           # merges the local developing branch into the (currently active) local main branch

    git push origin main                    # pushes changes from the local main to the remote main branch

<img src="./github/git_push_to_main.png" alt="Screenshot of wsl pushing a project to remote main branch" width="539" length="394">

------------------------------------------------------------------------

**Acknowledgements**

The Campbell lab would like to thank Mitch Brooks for his time, help,
and patience, all of which proved invaluable to conquering these first
steps of learning git.
