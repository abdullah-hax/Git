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

#### 🧾 Final Summary:

* ✅ Remote এ conflict থাকলে push হয় না
* ✅ Pull এর আগে stash করা হয়েছে
* ✅ Pull + rebase সফল হয়েছে
* ✅ Stash pop দিয়ে পরিবর্তনগুলো ফেরত এসেছে
* ✅ Push শেষে সফলভাবে GitHub এ update হয়েছে

এখন GitHub এ গিয়ে Commit history দেখলে সব update দেখে ফেলতে পারো 😄

---

### ✅ Commit Explanation

#### Terminal Output:

```bash
$ git commit -m "new & modified"
[main f2bd938] new & modified
 8 files changed, 316 insertions(+), 18 deletions(-)
 create mode 100644 problem_solving/codeforces/A_Blackboard_Game.c
```

#### 🔍 Explanation:

* তুমি একটি নতুন commit করেছো
* এই commit এর SHA ID = `f2bd938`
* ৮টি ফাইলে মোট 316 লাইন যোগ ও 18 লাইন মুছে ফেলা হয়েছে
* ১টি নতুন ফাইল যোগ হয়েছে → `A_Blackboard_Game.c`

🧠 Git এই commit-কে `.git/objects`-এ একটি commit object হিসেবে store করেছে
আর তোমার `main` branch এখন `f2bd938` commit কে point করছে ✅


---
### 📤 Git Push with Repository Moved Warning

#### ✅ Terminal Output:

```bash
$ git push
Enumerating objects: 24, done.
Counting objects: 100% (24/24), done.
Delta compression using up to 8 threads
Compressing objects: 100% (13/13), done.
Writing objects: 100% (13/13), 4.95 KiB | 563.00 KiB/s, done.
Total 13 (delta 8), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (8/8), completed with 8 local objects.
remote: This repository moved. Please use the new location:
remote:   https://github.com/abdullah-core/C-programming.git
To https://github.com/abdullah-backops/C-programming.git
   db62e2b..f2bd938  main -> main
```

---

#### 🔍 Output ব্যাখ্যা (Step-by-Step):

#### 🔸 1. **Git Local Changes গুলো গুনে ও Compress করে**

```bash
Enumerating objects: 24, done.
Counting objects: 100% (24/24), done.
Delta compression using up to 8 threads
Compressing objects: 100% (13/13), done.
```

**মানে:**

* Git তোমার পরিবর্তিত commit গুলোকে **object** হিসেবে ধরলো (24টা total)
* এর মধ্যে 13টি object (blob, tree, commit) compress করা হলো efficiently
* Git compression করে server-এ পাঠানোর জন্য প্রস্তুত করলো

#### 🔸 2. **GitHub-side এ object পাঠানো হলো এবং process করা হলো**

```bash
Writing objects: 100% (13/13), 4.95 KiB | 563.00 KiB/s, done.
Total 13 (delta 8), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (8/8), completed with 8 local objects.
```

**মানে:**

* GitHub-এ সফলভাবে 13টি object পাঠানো হয়েছে
* এর মধ্যে 8টি ছিল **delta** (মানে diff-based optimized object)
* GitHub সেগুলো resolve করতে পেরেছে

#### ⚠️ 3. **GitHub তোমাকে একটা জরুরি Message দেয়**:

```bash
remote: This repository moved. Please use the new location:
remote:   https://github.com/abdullah-core/C-programming.git
```

🎯 **এখানে আসল চমক!**

> GitHub বলছে:  তোমার যেই repo আগে `abdullah-backops` GitHub account এ ছিল, সেটা **move করে এখন ********************************`abdullah-core`******************************** account এ রাখা হয়েছে**।

📌 তাই ভবিষ্যতে GitHub-এ কিছু push/pull করতে হলে — **এই নতুন URL ব্যবহার করতে হবে।**

#### 🔸 4. **Push সফলভাবে শেষ হয়েছে**

```bash
To https://github.com/abdullah-backops/C-programming.git
   db62e2b..f2bd938  main -> main
```

**মানে:**

* Local commit `db62e2b` → GitHub এর main branch এ পৌঁছেছে
* এখন GitHub এ `f2bd938` হল latest commit

✅ তোমার push **পুরোপুরি সফল** হয়েছে।

---

#### ✅ এখন নতুন Github account তোমার PC এর Git এ এড কর:

```bash
git remote set-url origin https://github.com/abdullah-core/C-programming.git
```

এতে future push/pull সব নতুন URL দিয়েই হবে।

---
