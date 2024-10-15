# README

Commit 1: Test nak cancel PR dan delete branch @ github after accidental git push (new branch only)

Undo commit Commit 1 on local

Commit 1.1: Berjaya delete git github branch so agaknya clean, tetapi masa undo commit kat local lain sikit simbol 'M' (Modified) dalam source control, ttp cuba try.

Commit 1.2: Instead of Publish Branch, it has Sync Changes; this mean my local still thinks remote has the branch. The synch also fails (because i delete the branch in remote) and I receive git-error-x:
    > git pull --tags origin 241015-Commit1
    fatal: couldn't find remote ref 241015-Commit1
It's better to find another way:
1- Delete branch local and remake it (Untested but highly possible)
2- [ChatGPT] (It works when follow Commit 1.2.1)
    If you deleted the branch on GitHub but your local Visual Studio still thinks the branch exists, you need to synchronize your local repository with the remote to reflect the deletion. Here's how to fix this:

    ### 1. **Fetch and Prune Deleted Remote Branches**
    You need to remove references to the deleted remote branch from your local repository. This can be done by fetching and pruning.

    #### Steps:
    1. **Open Visual Studio's Terminal** or your system's terminal.
    
    2. **Run the following commands**:
    - Fetch from the remote to update your local references:
        ```bash
        git fetch --prune
        ```
        This will remove references to branches that no longer exist on the remote (GitHub), including the one you deleted.

    ### 2. **Check Branch Status**
    - After running the fetch command, check whether the branch still exists locally:
        ```bash
        git branch
        ```
    - If the branch still exists locally, you'll see it in the list. If you want to delete it locally as well, follow the next step.

    ### 3. **Delete the Local Branch (Optional)**
    - If you no longer need the local branch, you can delete it:
        ```bash
        git branch -d branch_name
        ```
    - If the branch has unmerged changes and you still want to delete it, use:
        ```bash
        git branch -D branch_name
        ```

    ### 4. **Sync Changes in Visual Studio**
    - After pruning, go back to Visual Studio and you should see the **"Publish Branch"** button again if you've checked out another local branch that isn't on the remote anymore.

    This will synchronize your local branch state with the remote repository, reflecting that the branch was deleted on GitHub.

Commit 1.2.1: Follow Commit 1.2 until Step 2. After that:
[CLI]
     git fetch --prune
    From github.com:nadiamysara/helloWorld
    - [deleted]         (none)     -> origin/241015-Commit1
     git branch
     git checkout main
    Switched to branch 'main'
    Your branch is up to date with 'origin/main'.
     git fetch --prune
     git checkout 241015-Commit1
    Switched to branch '241015-Commit1'
    Your branch is based on 'origin/241015-Commit1', but the upstream is gone.
    (use "git branch --unset-upstream" to fixup)

But instead I went to ChatGPT and asked the message above:
[ChatGPT] (I choose Option 2)
    The message you're seeing indicates that your local branch `241015-Commit1` is still linked to the remote branch `origin/241015-Commit1`, but since the remote branch was deleted on GitHub, it no longer has an upstream branch to track.

    To fix this, you can either:

    ### 1. **Unlink the Upstream (Remove the Remote Tracking)**
    This will keep your local branch without any tracking to the deleted remote branch:

    ```bash
    git branch --unset-upstream
    ```

    After running this, your local branch will no longer track the deleted remote branch, and you'll be able to continue working on it locally without any issues.

    ### 2. **Recreate the Branch on GitHub**
    If you deleted the remote branch but still need it, you can recreate it by pushing the local branch back to the remote:

    ```bash
    git push -u origin 241015-Commit1
    ```

    This will create a new remote branch on GitHub named `241015-Commit1` and link it to your local branch.

    ### Option 1 or 2?
    - Use **Option 1** if you don't need the remote branch anymore.
    - Use **Option 2** if you want to restore the branch on GitHub while continuing your work.

[CLI]
     git push -u origin 241015-Commit1
    Enumerating objects: 8, done.
    Counting objects: 100% (8/8), done.
    Delta compression using up to 8 threads
    Compressing objects: 100% (6/6), done.
    Writing objects: 100% (6/6), 1.54 KiB | 1.54 MiB/s, done.
    Total 6 (delta 3), reused 0 (delta 0), pack-reused 0 (from 0)
    remote: Resolving deltas: 100% (3/3), completed with 1 local object.
    remote:
    remote: Create a pull request for '241015-Commit1' on GitHub by visiting:
    remote:      https://github.com/nadiamysara/helloWorld/pull/new/241015-Commit1
    remote:
    To github.com:nadiamysara/helloWorld.git
    * [new branch]      241015-Commit1 -> 241015-Commit1
    branch '241015-Commit1' set up to track 'origin/241015-Commit1'.

Success: I'm able to "Publish Branch" on local, and on GitHub I can "Compare & pull request" and the branch I pushed is created like normal.

But, I decided to reset (delete branch on remote and undo commit in local) and try Option 1.

Commit 1.2.2: Follow Commit 1.2.1, except I choose Option 1
[CLI] (After commit because i git checkout a working branch, but later undo commit before publish branch)
     main  git fetch --prune
    Enter passphrase for key '/Users/securepay/.ssh/id_ed25519':
    From github.com:nadiamysara/helloWorld
    - [deleted]         (none)     -> origin/241015-Commit1
     main  git checkout
    241015-Commit1
    Switched to branch '241015-Commit1'
    Your branch is based on 'origin/241015-Commit1', but the upstream is gone.
    (use "git branch --unset-upstream" to fixup)
     241015-Commit1  git branch --unset-upstream
I probably missed it but, there was no Publish Branch button and still Synch Changes; so i checkout and come back again
     241015-Commit1  git checkout main
    Switched to branch 'main'
    Your branch is up to date with 'origin/main'.
     main  git checkout 241015-Commit1
    Switched to branch '241015-Commit1'

Partial success: I'm able to see "Publish Branch" on local