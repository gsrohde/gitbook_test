# GitBook Bugs

1. The backtic quotes should allow unescaped angle brackets to be displayed as such.  But:

```bash
cd /usr/local/ebi # or to the Rails root of the copy you are upgrading

# Check the git status to be sure the copy is "clean".
# Generally there shouldn't be any modified files and you should be 
# on the master branch.
#
git status

# Get the latest code from Github.
#
git pull

# Usually we deploy the code at the HEAD of the master branch.  But if there has 
# been code checked in to the master branch since the version to be released, 
# you will have to back up to it.
# Note that we tag releases AFTER deploying them, so we can't use the release tag
# as the git reference.
#
# [If necessary:]
# git checkout <git reference to version being deployed>

# Check the gem bundle.
#
bundle check
```
