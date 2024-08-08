# Git

## Create repository

Netbeans:

Team -> Init repository c:\Users\rpf\srcs

CmdLine:
```
git init
```


```
git config --global user.name "Roland Pfeifer"
git config --global user.email "git@nowhere.com"
```

## Working

See whats goin on:
```
git status 
```

Add a file:
```
git add some.txt
```

Do a comit:
```
git commit -a -m "Descr...."
```
-a is optional 

View history:
```
git log
```


Checkout a specific revision:
```
git checkout c8616db
```

## Branches

Create branch:
```
git branch my-feature-branch
```


View actual branch:
```
git branch
```


Switch a branch
```
git checkout my-feature-branch
```

Merge:
```
git merge my-feature-branch
```


## Remote

Link with (Svn)  from c:/Users/rpf
```
git svn clone https://localhost/svn/java/srcs -r <rev> --stdlayout --prefix svn/
```
Use -rREV:HEAD to choose a specific revision

[http://stackoverflow.com/questions/747075/how-to-git-svn-clone-the-last-n-revisions-from-a-subversion-repository stov](http://stackoverflow.com/questions/747075/how-to-git-svn-clone-the-last-n-revisions-from-a-subversion-repository stov.md)

### Remote commit

Um commits zu mergen change the lines in interactve edit but the fist to squash
```
git rebase -i HEAD~5
```

There @{u}

```
git svn rebase
git svn dcommit
```

### Push

As done by netbeans:

```
git branch
git submodule status
git push file:///run/media/rpf/MEDIA/srcs/ refs/heads/master:refs/heads/master
```

To strore credentials (unsafe):

```
 git config credential.helper store
```

Alternative (timeout 1week):

```
 git config --global credential.helper cache
 git config --global credential.helper "cache --timeout=604800"
```

## Links

[http://juristr.com/blog/2013/04/git-explained/ Juristr](http://juristr.com/blog/2013/04/git-explained/ Juristr.md)

[http://viget.com/extend/effectively-using-git-with-subversion Viget](http://viget.com/extend/effectively-using-git-with-subversion Viget.md)
