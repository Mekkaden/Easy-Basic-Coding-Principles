
📦 1. git add .

    This means: "Track all the changes in the current directory (and subdirectories)."

So here:

    You probably had new files (git, hello.js, and test.js) that weren't tracked by Git before.

    When you ran git add ., you basically told Git:
    👉 “Hey Git, I want to include these files in the next commit.”

    ⚠️ But this alone does NOT save anything permanently.
    Think of this like selecting files to be saved. You're prepping.

💾 2. git commit -m 'Add hello and test file'

    This means:
    “Now that I’ve selected stuff (with add), save that state as a commit with a message.”

So now Git:

    Took all the things you added using git add .

    Bundled them into a snapshot

    Saved that snapshot with the message 'Add hello and test file'

You can imagine this commit like a save point in a game with a title.
📝 What do you see in the output?

[master aad3a59] Add hello and test file
 3 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 git
 create mode 100644 hello.js
 create mode 100644 test.js

Here's what this means:

    ✅ Commit happened on the master branch

    🆔 The commit's ID is aad3a59 (every commit has a unique hash)

    📂 It added 3 files (but no lines of code were added yet, just empty files maybe)

    create mode 100644 — means regular file, default permissions

🧠 TL;DR
Command	What it does
git add .	Selects files for committing
git commit -m '...'	Saves the snapshot + message

You need both together to actually commit.
add selects, commit saves.






----------------------------------------------------------------------


#To see history of git use  command : git log
.So imagine we first added the readme file right now that file got some wha
 buggy so we dont need that file and need to go to older 
 just readme.md file so what we do is we go to git status 
 htne copy the hash id of the commit we need then use type q to quit the status page
 and type git checkout hashid.

 #  Git doesnt actually remove them ..see theres something called Head in git , which is a pointer pointing to
  the latest commit, so what here happens is the head pointer moves to the commit just before the discarded 
  commit u just discarded

   #how to go back to original state with the real files type git checkout master or main depending on ur version



 # NOW WE CAN TALK ABOUT GITHUB
 # GIT IS LIKE A TECHNOLOGY AND GITHUB IS WHAT WE USE FOR PUSH , PULL ,LEGS ETC.
 #THINK LIKE GIT IS GCC AND GITHUB IS VS code
 #To connect ur repositery to github first u need to link ur repositery to a remote;
 repository - 2 types - local - stuff in ur local machine- pc,phone ahtever
 so when u do gitinit u create a reposi in ur local machine and only when u push etc they go public
. Remote repo - the content in server = Like the sharing stuff 
.git branch -M main   = its a command made to change the name to Main.
.Now to access ur remote repository to local machine use code = git remote add origin <ur repositerylink> hit enter
.So to push ur local content to remote repo use command - git push -u origin main
.origin is like the remote repo name and stuff ...like an alias


. NOW WE KNOW TO CREATE A REPO ,PUSH SOME CODE AND YEAH BASIC STUFF DONE
------------------------------------------------------------------------------\



#Now lets go to branching and mergiing


.You’re working on your project in main, everything is stable.

Now you think:

    “Hmm… what if I try a dark mode feature?”

    You create a new branch:

git checkout -b dark-mode

➤ This is like copying the whole codebase and saying, "Let me play around here, without messing up main."

You change some stuff, test it, and everything looks sexy.
Now you’re like “Yup, this is worth keeping.”

You switch back to main:

git checkout main

And then merge your feature:

    git merge dark-mode

🎯 Result:

Your main now has the working dark mode — clean and without breaking anything else.
🔄 If things went bad in the branch?

No problem. Just delete the branch, or ignore it. Your main stays untouched and chillin’.



 - if u wanna create a new branch  - git branch new
 - if u wanna switch to that branch  - git checkout <branchname here:> new

 -IMP  - WARNING - If u r on a main branch and u create a new branch while u were on main branch then u will create a copy of whats in main in ur new branch 
 ..So lets say i experimented somehting new in the new branch, the copy we just made now to add that to remote repo to original content 
 we need to use command ,, aaah before that first we need to commit the the stuff so 
 git commit -m 'Modify readmen' //Bcs the new feature i did here is just modify readme haha
 then git push --set-upstream origin <ur new branch name>feature-branch  OR ESY is git push -u origin feature-branch


Now if someone made a change in your or ur teams remote rep then u need to pull taht to ur local repo for dping ur own thing 
so for that we use --  git pull ---So go to github , go to pull requests then go then specify from where and to there  then click pull requests 
then u or the team lead will click on merge request then confirm request and lets go!!

#Now if u see something like left behind or behind one that typically means: tahts branch is pretty much not needed as its already been inside the mian branch so u can just delte it if u need.
#Now still u need to understand thta this have happenend in the remotre repo not on the local repo...so for that to be DONE

use command git pull 

-----------------------------------------------------------------------------------

Now heres the typical workflow u should follow

1,clone the repo
2,create a new branch from the main or another branch
3,make ur changes
4,push the branch to the remote repo 
5,open a pull request
6,merge the changes
7,pull the merged changes into the local main branch




##MERGE CONFLICTS

The merge conflicts happens when 2 people A and B  make changes to the same file -specifically to the same line in the same file, So the reviewer accepted the A's branch , it got committed to main branch and then person B tries to do the same thing as already person A have done it , person B cant Merge.

-------------------------------------------------------------------------
HOW RO RESOLVE MERGE CONFLICT
#MERGE THE MAIN BRANCH TO UR SMALL BRANCH - git merge main // first checkout to ur subbranch and then merge main

#Next we got something called git reset. Imagine U ahve say 10 commits , u have to delete everthing from 4 including 4 to 10 but need to  keep the changes u made in thise 4 - 10 

⚙️ Git Reset — The Real Deal

Now let’s say you’ve committed something, but you regret it. That's where git reset comes in.
1. ✅ --soft Reset

Moves commit pointer back, but keeps all your files staged (ready for another commit).

You use it when:

    "Oops, I committed too early. I want to redo the commit message or add something else."

git reset --soft HEAD^

👉 Now your changes are still in staging (git status will say "changes to be committed").
2. ⚠️ --mixed Reset (Default)

Moves commit pointer back, unstages files, but keeps your changes.

You use it when:

    "I committed something by mistake. I want to start over but keep my edits."

git reset --mixed HEAD^
# or just
git reset HEAD^

👉 Your files are still changed, but now you have to git add again.
3. 🔥 --hard Reset

DESTROYS EVERYTHING after that commit. Commit is gone, staging is gone, edits are gone.

You use it when:

    "Bro I just wanna go back. Like nothing happened."

git reset --hard HEAD^

👉 Files are deleted. No mercy. Can’t undo easily (unless you’re lucky).
📦 So what’s the “staging area” again?

Imagine you're packing a box to send to your future self.
You don’t want to send every single thing in your room — just the good stuff.

You run:

git add filename

You’re saying:

    "Yo Git, include this in the next snapshot."

You can also add everything:

git add .

    "Take everything I've changed and prepare it for saving."

Then:

git commit -m "A nice caption"

Boom. Saved

#🧪 git revert — Full Guide:

Let’s say your last 3 commits are:

* Commit C (bug introduced here)
* Commit B
* Commit A

Now suppose Commit C broke something. You want to undo its effect, but still keep your commit history clean (for safety, tracking, etc.).
✅ Step-by-step to git revert:

git log

    Get the commit hash of the one you want to revert (say a1b2c3d)

Then:

git revert a1b2c3d

    This creates a new commit that reverses the changes in a1b2c3d.

If you want to revert multiple commits in one shot:

git revert HEAD~2..HEAD

This reverts the last 3 commits (because HEAD~2..HEAD includes 2 commits + HEAD itself)
⚠️ Difference Recap:
Feature	git revert	git reset
Destroys history?	❌ No	✅ Yes (depends on type)
Safe for shared repos?	✅ Yes	❌ No (can cause chaos on shared repos)
Creates new commit?	✅ Yes	❌ No (just moves branch)
Reverts a commit’s effect?	✅ Yes	✅ Yes, but with risks
💡 Real World Example:

Let’s say:

    You committed console.log("secret password")

    It's pushed to GitHub

    You want to remove it safely

Use git revert:

git revert a1b2c3d   # commit where you added that line

It’ll auto-generate a commit like:

    Revert "added secret log"


-------------------------------------------------------------------------
🚀 WHAT IS git stash?

You're working on something — editing files, writing some code — and suddenly:

    💥 “Oh crap, I need to switch to another branch... but I haven’t finished my work!”

But Git won’t let you switch branches if you have unsaved changes.
You don’t want to commit half-done work either, right?

👉 So, git stash temporarily saves your uncommitted changes, makes your working directory clean, and lets you switch branches or do whatever.

    Think of it like throwing your changes into a backpack. You can pull them out later when you’re ready.

🛠️ WHAT EXACTLY DOES IT STASH?

It stashes:

    Modified tracked files (git knows them)

    Staged files

    Optionally, untracked or ignored files

📦 BASIC USAGE
1. Save (stash) your changes

git stash

This saves all changes and reverts your working directory to the last commit (clean state).
2. See the stash list

git stash list

Shows all your stashed changes like:

stash@{0}: WIP on main: abc123 Some message

3. Apply your changes back

git stash apply

This applies the most recent sta h but does NOT remove it from the list.
4. Apply and remove the stash

git stash pop

This applies the stash and deletes it from the stash list (like pulling it out of the backpack and tossing it).
5. Remove stash without applying

git stash drop stash@{0}

6. Clear all stashes

git stash clear

Be careful: this deletes ALL stashed changes permanently.
🔍 COMMON EXAMPLES
🔄 Stash and switch branch:

git stash
git checkout feature-branch

📁 Want to stash even untracked files?

git stash -u
# or
git stash --include-untracked








s🧠 What are SSH keys, in simple terms?

Think of SSH keys as a super secure digital ID for your computer.

    It's like your laptop has a secret handshake with GitHub.

    Instead of typing your GitHub username & password every time you push code, you let the keys do the talking.

    It’s more secure than a password because the private key never leaves your device.

⚙️ Why use SSH keys with GitHub?
🔐 1. Secure Authentication

    GitHub trusts your computer because it has the correct private key.

    You don’t have to type your password or personal access token again and again.

🛠️ 2. Easy Push & Pull

    Once set up, you can do:

    git push origin main
    git pull origin main

    Without entering credentials.

🧪 3. Recommended by GitHub

    Since August 2021, GitHub removed password-based authentication for git commands.

    Now it's either SSH or personal access tokens. SSH is easier once set up.

🪛 How SSH keys work in GitHub (in simple story)

    You generate a key pair:

        Private key → stays in your laptop

        Public key → you upload this to your GitHub account.

    When you do git push, GitHub checks:

        "Does this request come from a computer I trust?" 🔍

        If your private key matches the public one on GitHub — it lets you in.

📦 How to Set It Up (Step-by-step guide for beginners)
🔧 1. Generate SSH key

In terminal (Linux/macOS) or Git Bash (Windows):

ssh-keygen -t ed25519 -C "your_email@example.com"

    It’ll ask for file location — just press Enter to accept default.

    You can also set a passphrase (optional, adds more security).

This generates two files:

    ~/.ssh/id_ed25519 → your private key

    ~/.ssh/id_ed25519.pub → your public key

📋 2. Add public key to GitHub

Copy your public key:

cat ~/.ssh/id_ed25519.pub

Then:

    Go to GitHub → Settings → SSH and GPG keys

    Click "New SSH key"

    Paste the public key, give it a title (e.g. "My Laptop") → Save.

✅ 3. Test the connection

Run:

ssh -T git@github.com

If it works, you’ll see:

Hi your_username! You've successfully authenticated...

🔄 4. Use SSH URLs in Git commands

Instead of using:

https://github.com/yourname/yourrepo.git

Use:

git@github.com:yourname/yourrepo.git

    So when you clone, push, or pull, GitHub uses the SSH handshake.

🤖 Bonus: What if I use GitHub Desktop?

GitHub Desktop automatically sets up SSH if you choose that during install. But if you’re using terminal Git, it’s good to set it up yourself.s

 
