---
description: >-
  If you mistakenly add a secret to your GitHub repo, like a password, API key
  or private key a delete and commit won't be good enough. Hackers will still be
  able to search your commit history.
---

# Removing your GitHub commit history

This guide will show you how you can delete your github commit history to prevent any future damage.

#### First Method

Deleting the `.git` folder may cause problems in our git repository. If we want to delete all of our commits history, but keep the code in its current state, try this:

```text
# Check out to a temporary branch:
git checkout --orphan TEMP_BRANCH

# Add all the files:
git add -A

# Commit the changes:
git commit -am "Initial commit"

# Delete the old branch:
git branch -D master

# Rename the temporary branch to master:
git branch -m master

# Finally, force update to our repository:
git push -f origin master
```

This will not keep our old commits history around. **But sometimes this does not work if not, try the next method below.**

#### Second Method

```text
# Clone the project, e.g. `myproject` is my project repository:
git clone https://github/ankh2054/test-git

# Since all of the commits history are in the `.git` folder, we have to remove it:
cd test-git

# And delete the `.git` folder:
git rm -rf .git

# Now, re-initialize the repository:
git init
git remote add origin https://github/ankh2054/test-git
git remote -v

# Add all the files and commit the changes:
git add --all
git commit -am "Initial commit"

# Force push update to the master branch of our project repository:
git push -f origin master
```



