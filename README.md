# Git

### ❌ Git Push Error: Remote Contains Work You Don't Have

#### ❗ Terminal Output:

```
$ git push
To https://github.com/abdullah-backops/C-programming.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/abdullah-backops/C-programming.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

---

#### 🔍 Explanation:

* তোমার local repo এবং GitHub repo এর মধ্যে পার্থক্য ছিল
* হয়তো অন্য কেউ GitHub এ কিছু push করেছে যা তোমার local repo তে নেই
* Git এই জন্য push reject করেছে, যাতে history mismatch না হয়

✅ সমাধান:

```
git pull origin main --rebase
# বা conflict থাকলে: git pull --no-rebase
# তারপর:
git push
```

---

### ⚠️ Git Pull Error: Unstaged Changes

#### ❗ Terminal Output:

```
$ git pull origin main --rebase
error: cannot pull with rebase: You have unstaged changes.
error: Please commit or stash them.
```

---

#### 🔍 Explanation:

* তোমার working directory-তে কিছু পরিবর্তন ছিল যেগুলো commit বা stage করা হয়নি (unstaged)
* Rebase শুরু করার আগে Git clean state চায়, তাই pull করতে দেয়নি

✅ সমাধান:

* হয় `git add` দিয়ে stage করো এবং `git commit` করো
* না হয় `git stash` করে আলাদা করে রাখো, তারপর pull

---

### 📥 Git Pull (with rebase) + Stash Example

#### ✅ Terminal Output:

```
$ git stash
$ git pull origin main --rebase
$ git stash pop
Saved working directory and index state WIP on main: c8dfc7a git pull done
From https://github.com/abdullah-backops/C-programming
 * branch            main       -> FETCH_HEAD
Successfully rebased and updated refs/heads/main.
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   problem_solving/codeforces/A_Bear_and_Big_Brother.c
        modified:   problem_solving/codeforces/A_Beautiful_Matrix.c

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (286f9c10e5c9c6947dde23ebd34a9966bf1f0e01)
```

---

#### 🔍 Explanation:

* `git stash` ➤ তোমার unstaged changes safe করে রাখে এবং working directory clean করে
* `git pull origin main --rebase` ➤ GitHub থেকে আপডেট নিয়ে আসে এবং তোমার local commit গুলোর নিচে বসিয়ে দেয় (clean history)
* `git stash pop` ➤ আগের stash করা changes গুলো ফেরত দেয় এবং সেই stash entry মুছে দেয়

📌 Final status:

* ✅ Local branch updated with GitHub (rebased)
* ✅ Unstaged modified files ফিরে এসেছে
* ✅ ১টি commit ahead of origin/main → push করা দরকার

---

### 📤 Git Push Summary & Explanation

#### ✅ Terminal Output:

```
$ git push
Enumerating objects: 21, done.
Counting objects: 100% (21/21), done.
Delta compression using up to 8 threads
Compressing objects: 100% (17/17), done.
Writing objects: 100% (18/18), 4.39 KiB | 280.00 KiB/s, done.
Total 18 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/abdullah-backops/C-programming.git
   6213b1d..a0bcf6a  main -> main
```

---

#### 🔍 Line-by-Line Explanation:

* `Enumerating objects: 21, done.`
  ➤ Git খুঁজে বের করলো total ২১টা object (commit, blob, tree) পাঠাতে হবে।

* `Counting objects: 100% (21/21), done.`
  ➤ এই ২১টি object-এর সংখ্যা ঠিকমতো গুনে দেখা হলো।

* `Delta compression using up to 8 threads`
  ➤ Git একসাথে অনেক ফাইল efficiently compress করতে ৮টি thread ব্যবহার করছে।

* `Compressing objects: 100% (17/17), done.`
  ➤ ১৭টি object compress করা হয়েছে (বাকি ৪টি আগেই হয়ত compressed ছিল)।

* `Writing objects: 100% (18/18), 4.39 KiB | 280.00 KiB/s, done.`
  ➤ ১৮টি object GitHub-এ পাঠানো হয়েছে, মোট ৪.৩৯ KB ডেটা।

* `Total 18 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)`
  ➤ এই ১৮টি object-এর মধ্যে ২টি ছিল delta (মানে diff)।

* `remote: Resolving deltas: 100% (2/2), completed with 2 local objects.`
  ➤ GitHub তার পাশে পাওয়া diff (delta) object গুলো মেলাতে সক্ষম হয়েছে।

* `To https://github.com/abdullah-backops/C-programming.git`
  ➤ তোমার remote repo-এর URL যেখানে push করা হয়েছে।

* `6213b1d..a0bcf6a  main -> main`
  ➤ আগে GitHub main branch ছিল commit `6213b1d`
  ➤ এখন তোমার local main branch এর commit `a0bcf6a` দিয়ে update হয়েছে
  ➤ "main -> main" : Local main → Remote main এ সফলভাবে push হয়েছে ✅

---

### 🧾 Final Summary:

* ✅ Remote এ conflict থাকলে push হয় না
* ✅ Pull এর আগে stash করা হয়েছে
* ✅ Pull + rebase সফল হয়েছে
* ✅ Stash pop দিয়ে পরিবর্তনগুলো ফেরত এসেছে
* ✅ Push শেষে সফলভাবে GitHub এ update হয়েছে

এখন GitHub এ গিয়ে Commit history দেখলে সব update দেখে ফেলতে পারো 😄
