git-all
=======

Git utility to execute commands over multiple repositories.


Examples
========

	$ cd
	$ mkdir projects
	$ cd projects
	$ export GIT_REPOS="$(readlink -f .)"
	$ git init prj1
	$ git init prj2
	$ git init prj3

	$ echo "Readme file" | tee prj{1,2,3}/README.txt
	$ git all status
	prj1: # On branch master
	prj1: #
	prj1: # Initial commit
	prj1: #
	prj1: # Untracked files:
	prj1: #   (use "git add <file>..." to include in what will be committed)
	prj1: #
	prj1: #	README.txt
	prj1: nothing added to commit but untracked files present (use "git add" to track)

	prj3: # On branch master
	prj3: #
	prj3: # Initial commit
	prj3: #
	prj3: # Untracked files:
	prj3: #   (use "git add <file>..." to include in what will be committed)
	prj3: #
	prj3: #	README.txt
	prj3: nothing added to commit but untracked files present (use "git add" to track)

	prj2: # On branch master
	prj2: #
	prj2: # Initial commit
	prj2: #
	prj2: # Untracked files:
	prj2: #   (use "git add <file>..." to include in what will be committed)
	prj2: #
	prj2: #	README.txt
	prj2: nothing added to commit but untracked files present (use "git add" to track)

	$ git all add README.txt
	$ git all commit -m "Adding readme."
	prj1: [master (root-commit) ea1927c] Adding readme.
	prj1:  1 file changed, 1 insertion(+)
	prj1:  create mode 100644 README.txt

	prj3: [master (root-commit) ea1927c] Adding readme.
	prj3:  1 file changed, 1 insertion(+)
	prj3:  create mode 100644 README.txt

	prj2: [master (root-commit) ea1927c] Adding readme.
	prj2:  1 file changed, 1 insertion(+)
	prj2:  create mode 100644 README.txt
	$ git all prj1 prj2 branch mytest
	$ git all branch
	prj1: * master
	prj1:   mytest

	prj3: * master
	prj2: * master
	prj2:   mytest

	$ git all prj1 prj2 checkout mytest
	Switched to branch 'mytest'
	Switched to branch 'mytest'
	$ echo "More stuff" | tee -a prj{1,2}/README.txt
	More stuff
	$ git all prj1 prj2 commit -m "Adding more to readme."
	prj1: # On branch mytest
	prj1: # Changes not staged for commit:
	prj1: #   (use "git add <file>..." to update what will be committed)
	prj1: #   (use "git checkout -- <file>..." to discard changes in working directory)
	prj1: #
	prj1: #	modified:   README.txt
	prj1: #
	prj1: no changes added to commit (use "git add" and/or "git commit -a")

	prj2: # On branch mytest
	prj2: # Changes not staged for commit:
	prj2: #   (use "git add <file>..." to update what will be committed)
	prj2: #   (use "git checkout -- <file>..." to discard changes in working directory)
	prj2: #
	prj2: #	modified:   README.txt
	prj2: #
	prj2: no changes added to commit (use "git add" and/or "git commit -a")

	$ git all prj1 prj2 commit -am "Adding more to readme."
	prj1: [mytest 19c6fc6] Adding more to readme.
	prj1:  1 file changed, 1 insertion(+)

	prj2: [mytest 19c6fc6] Adding more to readme.
	prj2:  1 file changed, 1 insertion(+)

	$ git all prj1 prj2 checkout master
	Switched to branch 'master'
	Switched to branch 'master'
	$ git all prj1 prj2 merge mytest
	prj1: Updating ea1927c..19c6fc6
	prj1: Fast-forward
	prj1:  README.txt |    1 +
	prj1:  1 file changed, 1 insertion(+)

	prj2: Updating ea1927c..19c6fc6
	prj2: Fast-forward
	prj2:  README.txt |    1 +
	prj2:  1 file changed, 1 insertion(+)

	$ git all log --oneline
	prj1: 19c6fc6 (HEAD, mytest, master) Adding more to readme.
	prj1: ea1927c Adding readme.

	prj3: ea1927c (HEAD, master) Adding readme.
	prj2: 19c6fc6 (HEAD, mytest, master) Adding more to readme.
	prj2: ea1927c Adding readme.
