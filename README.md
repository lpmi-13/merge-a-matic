# MERGE-A-MATIC 

One of the difficult parts of collaboration is dealing with merge
conflicts. This can happen, for example, when Person A alters
a file (eg, README.md), and a second person also alters the same
file.

When one of them pushes back to the repository, Person B's
push will be rejected, and when Person B pulls, they will see
a merge conflict.

Never fear, though! In this repository, we'll walk through how
to resolve one such conflict, and you will get immediate
feedback on whether you successfully did it.

## setup

clone the project and run 'npm install'

This will automatically run a script to attempt to merge the 
'develop' branch into the 'master' branch. Since there are 
conflicts, these will have to be resolved manually.

## you have a conflict!

you probably see something like this:
```
>>>>>>>>HEAD
this is added line
=======
this is original line
<<<<<<<<7f7rh4nrf
```

Don't worry though, we'll walk through resolving it step by step.

## check your work

run 'npm run check' to see how you did!
