[//]: # (tags: git)

### git
to view changes:

`git status`

to add changes at a new stage:

```git add .```

to commit the changes:

```git commit -m "message of commit"```

to push the changes to master:

```git push -u origin master```

to create a new branch:

```git checkout -b BRANCHNAME```

to push the changes to another branch:

```git push -u origin BRANCHNAME```

to download from the new upstream:

```git pull origin master```


## to discard all and reset
to discard all local changes and reset to master:

```
git reset --hard origin/master
git pull origin master
```

## git diff patching
to generate a patch from the (unchanged) changes:

```
git diff > mypatch.patch
```
or
```
git diff --cached > mypatch.patch
```