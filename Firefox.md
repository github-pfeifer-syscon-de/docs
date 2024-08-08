# Firefox

With Version 57 the tree-style-tab wont hide the main tab bar so:

add /chrome/ directory to user profile
add userChrome.css file in the directory 
add:

```
#tabbrowser-tabs { visibility: collapse !important; }
```

[tree-style-tab](https://www.ghacks.net/2017/09/27/tree-style-tab-is-a-webextension-now/)
