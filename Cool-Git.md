## **Git Branching, Merging, & Workflows: A Practical Workbook**

Welcome to this hands-on workbook. The goal isn't just to watch, but to *do*. You will follow the same steps as the tutorial to build a real-world understanding of how professional developers use Git.

**Learning Objectives:**

  * **Module 1:** Use **branching** to work on separate tasks (a feature and a bug fix) at the same time.
  * **Module 2:** Use **merging** to combine your completed work back into a main branch.
  * **Module 3:** Identify, create, and **resolve merge conflicts** locally.
  * **Module 4:** Apply the **Feature Branch Workflow** using GitHub and Pull Requests.
  * **Module 5:** Resolve a **professional-level merge conflict** that happens on a remote repository.

**Prerequisites:**

  * Git installed on your computer.
  * A code editor (like VS Code).
  * A GitHub account.
  * Basic Git knowledge (`git init`, `git add`, `git commit`).

-----

### **Module 1: The Basics of Branching**

**Objective:** Understand *why* we branch and how to create and switch between branches. This module follows the video from to.

#### **Conceptual Questions**

1.  The video describes a manager asking for a bug fix while you're working on a feature. Why is it a bad idea to make the bug fix commit on top of your "work in progress" feature commits?
2.  What does the `HEAD` pointer represent in Git?

#### **Practical Exercise: The Feature vs. The Bug**

Let's simulate the exact scenario from the video.

1.  **Setup Your Project:**

      * Create a new folder named `git-workbook`.
      * Open your command line/terminal, `cd` into that folder, and initialize Git:
        ```bash
        mkdir git-workbook
        cd git-workbook
        git init
        ```

2.  **Create Your Initial History:**

      * Create a file named `hello.txt` and add the text "version 1".
      * Add and commit this file.
        ```bash
        # (Create hello.txt and add "version 1")
        git add .
        git commit -m "version 1"
        ```
      * Now, change the text to "version 2", then add and commit.
      * Finally, change the text to "version 3", then add and commit.
      * **Checkpoint:** Check your history. You should see three commits.
        ```bash
        git log --all --graph
        ```

3.  **Start Your New Feature:**

      * Your manager asks for a new feature. Let's create a branch for it.
        ```bash
        git branch feature-a
        ```
      * Switch to your new branch to start working on it.
        ```bash
        git checkout feature-a
        ```

4.  **Work on the Feature:**

      * Create a new file named `feature.txt` with the text "feature commit 1".
      * Add and commit this new file.
        ```bash
        # (Create feature.txt)
        git add .
        git commit -m "feature commit 1"
        ```
      * Now, update `feature.txt` to say "feature commit 2".
      * Add and commit this change.
        ```bash
        # (Update feature.txt)
        git add .
        git commit -m "feature commit 2"
        ```

5.  **The Urgent Bug Fix:**

      * Oh no\! Your manager just found a bug on the live site. You need to fix it, but you can't include your unfinished feature.
      * **Switch back to safety:** Go back to your main branch (it might be named `master` or `main`).
        ```bash
        git checkout master
        ```
      * **Checkpoint:** Look at your files. What happened to `feature.txt`? It's gone\! This is because it only exists on the `feature-a` branch.

6.  **Make the Bug Fix:**

      * Create a new file named `bugfix.txt` with the text "This is the bug fix".
      * Add and commit this fix *directly to master*.
        ```bash
        # (Create bugfix.txt)
        git add .
        git commit -m "bug fix"
        ```

7.  **See the Divergence:**

      * Now, look at your project history. This is the "Aha\!" moment of branching.
        ```bash
        git log --all --graph
        ```
      * **Question:** Describe what you see. You should see your "version 3" commit, with two *different* paths branching off it: one for `feature-a` and one for `master`.

-----

### **Module 2: Merging Branches**

**Objective:** Combine your completed work from `feature-a` back into `master`. This follows the video from to.

#### **Conceptual Questions**

1.  When you merge two branches, where does the final merge commit go? (Hint: What does it depend on?)
2.  Why is it the best practice to merge *into* `master` rather than merging `master` *into* your feature branch?

#### **Practical Exercise: Combining Your Work**

Let's finish the feature and merge it.

1.  **Finish Your Feature:**

      * Go back to your feature branch.
        ```bash
        git checkout feature-a
        ```
      * Make one final change. Edit `feature.txt` to say "feature complete".
      * Add and commit this final change.
        ```bash
        # (Update feature.txt)
        git add .
        git commit -m "feature complete"
        ```

2.  **Prepare to Merge:**

      * Your feature is done and ready to be combined with the `master` branch (which already has the bug fix).
      * First, you must be on the branch you want to merge *into*.
        ```bash
        git checkout master
        ```

3.  **Execute the Merge:**

      * Now, tell Git to merge the `feature-a` branch *into* your current branch (`master`).
        ```bash
        git merge feature-a -m "Merge feature-a into master"
        ```

4.  **Checkpoint:**

      * Look at your files. What do you see? You should now see `hello.txt`, `bugfix.txt`, *and* `feature.txt` all together\!
      * Look at your history. You will see how the branches have joined.
        ```bash
        git log --all --graph
        ```

-----

### **Module 3: Handling Merge Conflicts**

**Objective:** Learn to create and resolve the "merge conflict" that Git can't handle automatically. This follows the video from to.

#### **Conceptual Questions**

1.  In your own words, when does a merge conflict happen?
2.  What do the `<<<<<<< HEAD`, `=======`, and `>>>>>>>` markers in a file mean?
3.  To resolve a conflict, do you *have* to pick one of the two versions? Or can you write something completely new?

#### **Practical Exercise: The Inevitable Conflict**

Let's intentionally cause a conflict.

1.  **Create a New Branch for Conflict:**

      * Make sure you are on `master`.
        ```bash
        git checkout master
        ```
      * Create a new branch.
        ```bash
        git branch conflict-branch
        ```

2.  **Make Change 1:**

      * Switch to the new branch.
        ```bash
        git checkout conflict-branch
        ```
      * Open `hello.txt` and change the text on line 1 to "Change from conflict branch".
      * Add and commit this change.
        ```bash
        # (Edit hello.txt)
        git add .
        git commit -m "conflict branch change"
        ```

3.  **Make Change 2 (The Conflict):**

      * Switch back to `master`.
        ```bash
        git checkout master
        ```
      * Open `hello.txt` and change the *exact same text on line 1* to "Change from master branch".
      * Add and commit this change.
        ```bash
        # (Edit hello.txt)
        git add .
        git commit -m "master branch change"
        ```

4.  **Attempt the Merge:**

      * Now, try to merge `conflict-branch` into `master`.
        ```bash
        git merge conflict-branch
        ```
      * **STOP\!** Read the output. Git will tell you: `Automatic merge failed; fix conflicts and then commit the result.`

5.  **Resolve the Conflict:**

      * Open `hello.txt`. You will see the conflict markers.
        ```
        <<<<<<< HEAD
        Change from master branch
        =======
        Change from conflict branch
        >>>>>>> conflict-branch
        ```
      * **Your job:** You are the developer. Git is asking you, "What should the final code be?"
      * Manually edit the file. Delete *all* the markers (`<<<`, `===`, `>>>`) and decide on the final text. For this exercise, make the line say: "Final resolved code".
      * Save the file.

6.  **Finalize the Merge:**

      * You've resolved the conflict. Now you must tell Git.
      * First, `add` the file you fixed.
        ```bash
        git add .
        ```
      * Second, `commit` to finalize the merge.
        ```bash
        git commit -m "Resolved conflict between master and conflict-branch"
        ```
      * **Checkpoint:** Run `git log --all --graph`. You'll see the merge is now complete.

-----

### **Module 4: The Professional Feature Branch Workflow**

**Objective:** Use the full professional workflow with GitHub and Pull Requests. This follows the video from to.

#### **Conceptual Questions**

1.  What is the main purpose of uploading a feature branch to GitHub *before* merging it?
2.  What is a "Pull Request" (PR)?
3.  After the PR is merged on GitHub, what is the *last* command you must run locally to stay in sync?

#### **Practical Exercise: Your First Pull Request**

1.  **Setup GitHub:**

      * Go to GitHub.com and create a **new, blank repository**.
      * Call it `git-workbook-remote`. **Do not** add a README or .gitignore.
      * GitHub will give you a URL. Copy it.

2.  **Link Your Local Repo:**

      * In your terminal, link your local `git-workbook` folder to the GitHub repo.
        ```bash
        git remote add origin <YOUR_GITHUB_REPO_URL>
        ```

3.  **Push Your `master` Branch:**

      * Let's get your existing `master` branch up to GitHub.
        ```bash
        git push origin master
        ```
      * *(Note: You might need to use `git push -u origin master` or `git push -u origin main`)*.

4.  **Start a New Feature:**

      * Let's create a *new* feature branch, the "pro" way.
        ```bash
        git checkout -b my-new-feature
        ```
      * *(This single command creates the branch AND checks it out.)*

5.  **Make Your Change:**

      * Create a new file called `readme.txt` with the text "This is my new feature".
      * Add and commit it.
        ```bash
        # (Create readme.txt)
        git add .
        git commit -m "Add new feature readme"
        ```

6.  **Push Your Feature Branch:**

      * Instead of merging locally, push your *branch* to GitHub.
        ```bash
        git push origin my-new-feature
        ```

7.  **Create the Pull Request (on GitHub):**

      * Go to your repository on GitHub.
      * You will see a yellow bar: "my-new-feature had recent pushes..."
      * Click the **"Compare & pull request"** button.
      * Give it a title and click **"Create pull request"**.

8.  **Review and Merge (on GitHub):**

      * You are now on the Pull Request screen. This is where your teammates would review your code.
      * Click the **"Files changed"** tab to see your work.
      * Go back to the **"Conversation"** tab.
      * Since you are the reviewer too, click the **"Merge pull request"** button, and then **"Confirm merge"**.

9.  **Sync Locally:**

      * Your local machine doesn't know you just merged on GitHub\! You must update your local `master` branch.
      * Switch to `master` and `pull` the changes from the remote (`origin`).
        ```bash
        git checkout master
        git pull origin master
        ```
      * **Checkpoint:** Look at your local files. `readme.txt` has now appeared\! You have successfully completed the full professional loop.

-----

### **Module 5: Challenge - Resolving a Remote Conflict**

**Objective:** Resolve a merge conflict that appears on GitHub, using the preferred *local* method. This follows the video from to.

#### **Practical Exercise: The Two-Engineer Problem**

We will simulate two engineers (you and... also you) working on the same file.

1.  **Simulate Engineer 1:**

      * Make sure you are on `master`: `git checkout master`
      * Create a feature branch: `git checkout -b feature-one`
      * Edit `hello.txt`. Change line 1 to "Change from feature one".
      * `git add .` and `git commit -m "feat1"`
      * `git push origin feature-one`
      * **On GitHub:** Create a Pull Request for `feature-one` and **MERGE IT**.

2.  **Simulate Engineer 2 (You):**

      * **Crucially:** You are "unaware" Engineer 1 just merged.
      * Start from your *local* `master` branch (which is now out of date).
        ```bash
        git checkout master
        # DO NOT PULL!
        ```
      * Create your feature branch: `git checkout -b feature-two`
      * Edit `hello.txt`. Change line 1 to "Change from feature two".
      * `git add .` and `git commit -m "feat2"`
      * `git push origin feature-two`

3.  **Observe the Conflict:**

      * **On GitHub:** Create a Pull Request for `feature-two`.
      * GitHub will show a red error: **"Can't automatically merge"**. You have a remote merge conflict\! Do *not* use the web editor.

4.  **Resolve Locally (The Pro Way):**

      * The video shows the best way to fix this is on your computer.
      * **Step A: Update `master`**
        ```bash
        git checkout master
        git pull origin master
        ```
      * **Step B: Go to your branch**
        ```bash
        git checkout feature-two
        ```
      * **Step C: Merge the *new* `master` *into* your branch**
        ```bash
        git merge master
        ```
      * **BAM\!** The merge conflict now appears on your *local machine*.

5.  **Fix and Push:**

      * Open `hello.txt`. You'll see the conflict markers (showing "Change from feature two" vs "Change from feature one").
      * Resolve the conflict: Edit the file, delete the markers, and make the final text "Feature two is the final code".
      * `git add .`
      * `git commit -m "Resolved conflict with master"`
      * Now, push your *fixed* branch back up to GitHub.
        ```bash
        git push origin feature-two
        ```

6.  **Final Check:**

      * Go back to your Pull Request on GitHub.
      * **Like magic,** the red error message is gone, and the "Merge" button is green. You have resolved the conflict.
      * You can now safely merge your pull request.

**Congratulations\! You have completed the full Git workflow, from basic branching to resolving professional-level remote conflicts.**
