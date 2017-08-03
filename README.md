# Git Basics Tutorial 0.1.0-alpha.1

## Setup

Before anything else, we must tell Git who we are:

    git config --global user.name "John Doe"
    git config --global user.email "johndoe@example.com"

## My first repository

Let's create a shopping list, and track it via git.

### 1. Create directory for our project

    mkdir shopping-list
    cd shopping-list

### 2. Initialize `shopping-list` as a new git repository

    git init

### 3. Create a file

Create a file called `shopping-list.md` with the following content:

    # My Shopping List

    - Milk
    - Coffee
    - Sugar
    - Steak
    - Coke
    - Shower Soap

### 4. Check the repository status

    git status

Which should the following, indicating that `shopping-list.md` is a untracked
file, meaning git has not been told what it should do with the file yet.

    On branch master

    Initial commit

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            shopping-list.md

    nothing added to commit but untracked files present (use "git add" to track)

### 5. Add `shopping-list.md` to the git staging area

Before a file can be committed, it must be added to staging area. The staging
area is effectively a list of things that are ready to be committed.

    git add shopping-list.md

### 6. Check repository status again

    git status

Output should now look like this indicating that `shopping-list.md` is marked as
a new file that can be committed.

    On branch master

    Initial commit

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)

            new file:   shopping-list.md

### 7. Commit the changes

    git commit -m "Initial commit"

Which should output something like this:

    [master (root-commit) 3e6b8d7] Initial commit
     1 file changed, 8 insertions(+)
     create mode 100644 shopping-list.md

### 8. Check repository status once again

    git status

This time, there's no changes, and nothing to commit:

    On branch master
    nothing to commit, working directory clean

### 9. Look at the commit log

    git log

Prints a list of commits, should look something like:

    commit 3e6b8d77ac551c55621a88506e1d9e22e72e6a12
    Author: John Doe <johndoe@example.com>
    Date:   Wed Aug 2 23:53:26 2017 +0000

        Initial commit

## Branches

One of git's best features is how cheap and fast it is to create branches. Let's
take a look at this by creating a new branch for adding things to our shopping
list that we'll need to prepare a smorgasbord party tomorrow.

But we don't know for certain if the smorgasbord party is happening, so for now
we'll add the required items to a separate branch, as to not disturb the main
list on the default `master` branch.

### 1. List all branches

    git branch -a

Will print:

    * master

This means we only have the default branch named `master` right now.

### 2. Create and switch to a new branch

    git checkout -b smorgasbord-party

You should see a message indicating that you've switched to a new branch:

    Switched to a new branch 'smorgasbord-party'

The `checkout` command is used to switch between branches. Because the
`smorgasbord-party` branch didn't exist, we needed to also add the `-b` option
which instructs git to create the branch before switching to it. If you're
switching to a branch that already exists, you don't need to use the `-b`
option.

### 3. Modify `shopping-list.md` with the new items we need

Add the following to the end of `shopping-list.md`:

    - Bread
    - Butter
    - Smoked Ham
    - Sliced Cheese
    - Caviar

### 4. Check repository status

    git status

The `shopping-list.md` will be indicated as modified:

    On branch smorgasbord-party
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

            modified:   shopping-list.md

    no changes added to commit (use "git add" and/or "git commit -a")

### 5. Take a look at what has been changed

One way to have a look at what's been modified is using the `diff` command:

    git diff

This will print a diff of what's changed since the last commit:

```diff
diff --git a/shopping-list.md b/shopping-list.md
index 47d5491..2f07a06 100644
--- a/shopping-list.md
+++ b/shopping-list.md
@@ -6,3 +6,8 @@
 - Steak
 - Coke
 - Shower Soap
+- Bread
+- Butter
+- Smoked Ham
+- Sliced Cheese
+- Caviar
```

### 6. Stage the changes

Instead of just running `git add` like when we originally staged
`shopping-list.md`, this time let's interactively add the changes:

    git add -i

You will be preseted with a list of choices:

               staged     unstaged path
      1:    unchanged        +5/-0 shopping-list.md

    *** Commands ***
      1: status       2: update       3: revert       4: add untracked
      5: patch        6: diff         7: quit         8: help
    What now>

We want to choose "patch" to have a look at what has changed inside existing
files. Type either `5` or `p` into the prompt and press return to trigger patch
mode.

Next you're presented with a list of files which have changes in them:

               staged     unstaged path
      1:    unchanged        +5/-0 shopping-list.md
    Patch update>>

Type `1` and enter to mark `shopping-list.md`:

               staged     unstaged path
    * 1:    unchanged        +5/-0 shopping-list.md
    Patch update>>

Then press return again to start the patch process where you choose what to do
with each specific change within all marked files:

```diff
diff --git a/shopping-list.md b/shopping-list.md
index 47d5491..2f07a06 100644
--- a/shopping-list.md
+++ b/shopping-list.md
@@ -6,3 +6,8 @@
 - Steak
 - Coke
 - Shower Soap
+- Bread
+- Butter
+- Smoked Ham
+- Sliced Cheese
+- Caviar
Stage this hunk [y,n,q,a,d,/,e,?]?
```

If you want to know what all the options are, type `?` and enter. To continue
and stage this chunk, type `y` and enter. You will then be dropped back into the
main interactive add menu. Type `1` or `s` and enter to see the current status:

              staged     unstaged path
      1:        +5/-0      nothing shopping-list.md

Type `7` or `q` to quit interactive add.

### 7. Check repository status

For good measure, check repo status:

    git status

Now you can see that `shopping-list.md` is marked as "modified":

    On branch smorgasbord-party
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

            modified:   shopping-list.md

### 8. Commit changes

    git commit

Without the `-m` option, git will launch an editor for you to write the commit
message. In this case use "`Add items for smorgasbord party`" as the commit
message. Output will give a quick summary of what was commited:

    [smorgasbord-party 3503549] Add items for smorgasbord party
     1 file changed, 5 insertions(+)

### 9. Look at git log

    git log

The log will now include both the original commit, and the new one that added
our smorgasbord items:

    commit 35035498ed638b6a331f437a10b5fad6725f3391
    Author: John Doe <johndoe@example.com>
    Date:   Thu Aug 3 00:36:59 2017 +0000

        Add items for smorgasbord party

    commit 3e6b8d77ac551c55621a88506e1d9e22e72e6a12
    Author: John Doe <johndoe@example.com>
    Date:   Wed Aug 2 23:53:26 2017 +0000

        Initial commit

## Go back to master and update shopping list

We still don't know if the smorgasbord party is happening, but we've realized we
need to add and change a few items unrelated to the potential party.

To do this, we'll switch back to master and make the changes as required.

### 1. Switch back to master branch

    git checkout master

Prints confirmation you've switched:

    Switched to branch 'master'

### 2. Modify `shopping-list.md` as needed

- Low-fat milk tastes bad, so we want to ensure we remember to buy
  full-fat milk, so change the `Milk` entry to read `Milk (full-fat)`
- We don't just want any steak, but a Rib-Eye Steak, so change the
  `Steak` entry to read `Rib-Eye Steak`.
- It turns out we're out of shampoo, so add `Shampoo` to the end of the list.

When you're done, `shopping-list.md` should look like this:

    # My Shopping List

    - Milk (full-fat)
    - Coffee
    - Sugar
    - Rib-Eye Steak
    - Coke
    - Shower Soap
    - Shampoo

### 3. Check status and diff

    git status

The `shopping-list.md` file will be modified:

    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

            modified:   shopping-list.md

    no changes added to commit (use "git add" and/or "git commit -a")

Then check the diff if you're curious:

    git diff

Will show what we've changed:

```diff
diff --git a/shopping-list.md b/shopping-list.md
index 47d5491..378f04c 100644
--- a/shopping-list.md
+++ b/shopping-list.md
@@ -1,8 +1,9 @@
 # My Shopping List

-- Milk
+- Milk (full-fat)
 - Coffee
 - Sugar
-- Steak
+- Rib-Eye Steak
 - Coke
 - Shower Soap
+- Shampoo
```

### 4. Add changes to staging area

    git add -i

As we did previously, use the patch functionality of git's interactive add mode
to stage the changes.

### 5. Commit changes

    git commit -m "Update Milk and Steak, and add Shampoo"

Again, it'll print a summary of the commit:

    [master b685a76] Update Milk and Steak, and add Shampoo
     1 file changed, 3 insertions(+), 2 deletions(-)

### 6. Have a look at the log

    git log

Note how the log only contains the commits on the master branch, and not the
commit on the smorgasbord-party branch:

    commit b685a76e3a860cd22a59a37800c367fe00f6cd94
    Author: John Doe <johndoe@example.com>
    Date:   Thu Aug 3 00:57:47 2017 +0000

        Update Milk and Steak, and add Shampoo

    commit 3e6b8d77ac551c55621a88506e1d9e22e72e6a12
    Author: John Doe <johndoe@example.com>
    Date:   Wed Aug 2 23:53:26 2017 +0000

        Initial commit

## Merge Branches

Now we have a `master` branch with a few basic necessities, and a
`smorgasbord-party` branch which has a few additional items needed for
tomorrow's party.

Previously we weren't sure if the party was happening, so we added the items
needed for it to the shopping list in a separate branch, to keep it separate
from what was happening to the list in master.

We now have confirmation the party is happening, so we don't need to keep a
separate branch. Let's merge the `smorgasbord-party` branch back into the
`master` branch.

### 1. Ensure we are on the `master` branch

    git checkout master

### 2. Start the merge

    git merge smorgasbord-party

The output will let us know what happened. Uh-oh, there's a merge conflict!

    Auto-merging shopping-list.md
    CONFLICT (content): Merge conflict in shopping-list.md
    Automatic merge failed; fix conflicts and then commit the result.

### 3. Resolve merge conflicts

Let's take a look at what happened, open `shopping-list.md`, it will now look
like this:

    # My Shopping List

    - Milk (full-fat)
    - Coffee
    - Sugar
    - Rib-Eye Steak
    - Coke
    - Shower Soap
    <<<<<<< HEAD
    - Shampoo
    =======
    - Bread
    - Butter
    - Smoked Ham
    - Sliced Cheese
    - Caviar
    >>>>>>> smorgasbord-party

What's happened is that it tried to apply the change from `smorgasbord-party` on
top of the `master` branch, The changes from `smorgasbord-party` were just
adding new content after `Shower Soap`. In the mean-time in the `master` branch
we added `Shampoo` directly after `Shower Soap`, and now git doesn't know which
of the two additions after `Shower Soap` it should keep, and hence we need to
now manually fix the conflict.

In this specific case, we actually want to keep both sets of additions, so we
simply remove the conflict markers from the file, leaving it as such:

    # My Shopping List

    - Milk (full-fat)
    - Coffee
    - Sugar
    - Rib-Eye Steak
    - Coke
    - Shower Soap
    - Shampoo
    - Bread
    - Butter
    - Smoked Ham
    - Sliced Cheese
    - Caviar

### 4. Check repository status

    git status

Due to the merge conflict, the merge operation has been halted half-way through,
hence the changes to `shopping-list.md` are already added to the staging area:

    On branch master
    All conflicts fixed but you are still merging.
      (use "git commit" to conclude merge)

    Changes to be committed:

            modified:   shopping-list.md

### 5. Commit changes

    git commit

Because we're finishing off a merge, git has already prepared a commit message
for us, hence we do not want to use the `-m` option and override that
message. Instead let `git commit` launch an editor for you so you can review the
pre-made commit message.

The editor should be pre-populated with something like this:

    Merge branch 'smorgasbord-party'

    # Conflicts:
    #       shopping-list.md
    #
    # It looks like you may be committing a merge.
    # If this is not correct, please remove the file
    #       .git/MERGE_HEAD
    # and try again.


    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    # On branch master
    # All conflicts fixed but you are still merging.
    #
    # Changes to be committed:
    #       modified:   shopping-list.md
    #

Save the file as is, and exit the editor. A commti summary will be printed:

    [master 6c402a2] Merge branch 'smorgasbord-party'

### 6. Look at git log

    git log

You will now have four commits in total, the initial commit, the two commits
that added and changed stuff on the two branches, and the merge commit:

    commit 6c402a28889d334b3cebb24425580ee0172ad04a
    Merge: b685a76 3503549
    Author: John Doe <johndoe@example.com>
    Date:   Thu Aug 3 01:25:11 2017 +0000

        Merge branch 'smorgasbord-party'

    commit b685a76e3a860cd22a59a37800c367fe00f6cd94
    Author: John Doe <johndoe@example.com>
    Date:   Thu Aug 3 00:57:47 2017 +0000

        Update Milk and Steak, and add Shampoo

    commit 35035498ed638b6a331f437a10b5fad6725f3391
    Author: John Doe <johndoe@example.com>
    Date:   Thu Aug 3 00:36:59 2017 +0000

        Add items for smorgasbord party

    commit 3e6b8d77ac551c55621a88506e1d9e22e72e6a12
    Author: John Doe <johndoe@example.com>
    Date:   Wed Aug 2 23:53:26 2017 +0000

        Initial commit

To see a tree-like view of the log history, you can run this:

    git log --all --graph --decorate --oneline

Which gives you this pretty tree:

    *   6c402a2 (HEAD -> master) Merge branch 'smorgasbord-party'
    |\
    | * 3503549 (smorgasbord-party) Add items for smorgasbord party
    * | b685a76 Update Milk and Steak, and add Shampoo
    |/
    * 3e6b8d7 Initial commit

### 7. Delete the `smorgasbord-party` branch

We no longer need the `smorgasbord-party branch, so let's delete it:

    git branch -d smorgasbord-party

Confirmation is printed:

    Deleted branch smorgasbord-party (was 3503549).

Run `git branch -a` to see the branch list again:

    * master

And run `git log --all --graph --decorate --oneline` again to see how the
`smorgasbord-party` is no longer listed on the tree:

    *   6c402a2 (HEAD -> master) Merge branch 'smorgasbord-party'
    |\
    | * 3503549 Add items for smorgasbord party
    * | b685a76 Update Milk and Steak, and add Shampoo
    |/
    * 3e6b8d7 Initial commit

## License

[CC0 1.0 Universal](http://creativecommons.org/publicdomain/zero/1.0/)
