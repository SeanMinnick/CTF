
Level 7-8 opens up to an index.php page with two links, one for a home page and one for an "about" page. When you click on either of these links the url changes and adds ```

```
index.php?page=home
index.php?page=about
```

in the source code of the main page there is a comment that gives the path to get the password for lvl 8 which was etc/webpass/natas8

i placed "admin" in the page= part of the url to see if there were any hidden pages that wouldn't be linked in the main file. This fed me an error since page=admin didn't exist, but the error showed me that index.php was using an include function to find the page it needed to display. 

After doing some research on the include function I was able to insert my custom path after the page=

```
index.php?page=../../../../etc/webpass/natas8
```

this printed the password and moves you onto the next level

