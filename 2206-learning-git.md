## Quick setup
### create a new repository on the command line
echo "# learning-git" >> README.md

git init

git add README.md

git commit -m "first commit"

git branch -M main

git remote add origin git@github.com:wehelpwe-outlook/learning-git

git push -u origin main

### or push an existing repository from the command line
git remote add origin git@github.com:wehelpwe-outlook/learning-git

git branch -M main

git push -u origin main

## ERROR: git push -u origin main

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (main)
    $ git push -u origin main
    To github.com:wehelpwe-outlook/learning-git.git
     ! [rejected]        main -> main (non-fast-forward)
    error: failed to push some refs to 'github.com:wehelpwe-outlook/learning-git.git'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. Integrate the remote changes (e.g.
    hint: 'git pull ...') before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

Reason: 

github repository is not empty
- create .gitignore

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (main)
    $ git pull origin main
    From github.com:wehelpwe-outlook/learning-git
     * branch            main       -> FETCH_HEAD
    fatal: refusing to merge unrelated histories

Solution 1:

Using git fetch first, then merge with local main

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (main)
    $ git fetch origin remote-gitignore
    fatal: couldn't find remote ref remote-gitignore

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (main)
    $ git fetch origin main:remote-gitignore
    From github.com:wehelpwe-outlook/learning-git
     * [new branch]      main       -> remote-gitignore

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (remote-gitignore)
    $   git checkout main
    Switched to branch 'main'

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (main)
    $ git merge remote-gitignore --allow-unrelated-histories
    Merge made by the 'ort' strategy.
     .gitignore | 104 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
     1 file changed, 104 insertions(+)
     create mode 100644 .gitignore

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (main)
    $ ls -a
    ./  ../  .git/  .gitignore  index.html  index.js  main.css  test/

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (main)
    $ git push -u origin main
    Enumerating objects: 13, done.
    Counting objects: 100% (13/13), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (8/8), done.
    Writing objects: 100% (12/12), 1.04 KiB | 213.00 KiB/s, done.
    Total 12 (delta 3), reused 0 (delta 0), pack-reused 0
    remote: Resolving deltas: 100% (3/3), done.
    To github.com:wehelpwe-outlook/learning-git.git
       ab8eae1..a5da652  main -> main
    branch 'main' set up to track 'origin/main'.

Solution 2: (add --allow-unrelated-histories)

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (main)
    $ git push -u origin main --allow-unrelated-histories


---
Check branch:

    $ git branch
    * main
      remote-gitignore

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (main)
    $ git branch -r
      origin/main

    hp@hp MINGW64 ~/Desktop/code/2206-learning-git (main)
    $ git branch -a
    * main
      remote-gitignore
      remotes/origin/main

