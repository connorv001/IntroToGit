## 🏁 1. Start Your Repo (Project)

This is step one\! You're telling Git to start tracking this folder.

### Create an empty repo

This command turns the current folder into a Git repository.

```bash
git init
```

### Tell Git who you are

Git needs a name and email to "sign" your commits. This is super important\!

```bash
# Set this just for this one repo
git config user.email "alice@example.com"
git config user.name "Alice Zakas"
```

💡 **Pro-Tip:** Use the `--global` flag to set your name and email for *all* repos on your computer. (You only need to do this once\!)

```bash
git config --global user.email "your_email@example.com"
git config --global user.name "Your Name"
```

### See your settings

Want to double-check your setup? This lists all your configs and where they're set.

```bash
git config --list --show-origin
```

-----

## ➕ 2. Add a New File

This is the core "save" workflow: create, stage, and commit.

**Step 1:** Create a new file.

```bash
echo "git is awesom" > message.txt
```

**Step 2:** Add the file to the **staging area**. (Think of this as "on deck" or "waiting to be saved").

```bash
git add message.txt
```

**Step 3:** See what's in the staging area.

```bash
git diff --cached
```

**Step 4:** **Commit** your change. This takes the snapshot and saves it to your repo's history forever.

```bash
git commit -m "add message"
```

-----

## ✏️ 3. Edit a File

Now let's change a file that's already in the repo.

**Step 1:** Edit the file.

```bash
echo "git is awesome" > message.txt
```

**Step 2:** Check your **local changes**. This shows what's different between your working folder and your last save.

```bash
git diff
```

**Step 3: The Shortcut\!**
You can **add** *and* **commit** all changes to files Git already knows about in one command.

```bash
git commit -am "edit message"
```

⚠️ **Heads-up:** The `-a` flag only works for files Git *already* tracks. It won't add brand-new, untracked files.

-----

## 🏷️ 4. Rename a File

Use Git's own "move" command so it can track the rename.

**Step 1:** Rename the file.

```bash
git mv message.txt praise.txt
```

**Step 2:** Check the staging area. (The `git mv` command automatically staged this for you\!)

```bash
git diff --cached
```

**Step 3:** Commit the rename.

```bash
git commit -m "rename message.txt"
```

-----

## 🗑️ 5. Delete a File

Like renaming, use Git's "remove" command.

**Step 1:** Delete the file.

```bash
git rm message.txt
```

**Step 2:** Check the staging area. (Yep, `git rm` also stages the change for you.)

```bash
git diff --cached
```

**Step 3:** Commit the deletion.

```bash
git commit -m "delete message.txt"
```

-----

## 🧐 6. Check Your Status

This is your **best friend** in Git. It tells you exactly what's going on.

Let's make a small mess to see how it works.

**Action 1:** Edit a file and stage it.

```bash
echo "git is awesome" > message.txt
git add message.txt
```

**Action 2:** Create a new file but *don't* stage it.

```bash
echo "git is great" > praise.txt
```

**Now, ask Git for the status:**

```bash
git status
```

It will clearly show you:

  * `message.txt` is in "Changes to be committed" (it's staged).
  * `praise.txt` is in "Untracked files" (it's new, and Git is ignoring it for now).

-----

## 📜 7. Read Your History (The Log)

See the history of all your commits.

**The default view** (can be a lot of text):

```bash
git log
```

**The tidy view** (my favorite\!):

```bash
git log --oneline
```

**The artsy graph view** (great for seeing branches):

```bash
git log --graph
```

**The ultimate combo:**

```bash
git log --oneline --graph
```

-----

## 🔎 8. Inspect a Specific Commit

"What did I *actually* save in that commit?"

**See the very last commit:**
`HEAD` is just a pointer to "your current location."

```bash
git show HEAD
```

**See the commit *before* the last one:**
`~1` means "one step back."

```bash
git show HEAD~1
```

(You can use `HEAD~2` for two steps back, `HEAD~3`... you get it\!)

-----

## 🕵️‍♀️ 9. Search Your Repo

Find text inside your files, even in old commits\!

**Search your *current* files:**
This searches your working tree right now.

```bash
git grep "debate"
```

**Search the *past* (this is like magic):**
This searches the project as it looked at the `HEAD~1` commit.

```bash
git grep "great" HEAD~1
```

(You can also use a specific commit hash instead of `HEAD~1`\!)