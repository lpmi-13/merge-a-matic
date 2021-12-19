# MERGE-A-MATIC 

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/lpmi-13/merge-a-matic)

One of the difficult parts of collaboration is dealing with merge
conflicts. This can happen, for example, when Person A alters
a file (eg, `server.js`) on Person A's computer, and Person B also
alters the same file on Person B's computer.

When one of them pushes back to the repository, Person B's
push will be rejected, and when Person B pulls, they will see
a merge conflict.

Never fear, though! In this repository, we'll walk through how
to resolve one such conflict, and you will get immediate
feedback on whether you successfully did it. Feel free to
keep this open in your browser while you work through the
exercise in your terminal.

## setup

clone the project and type `npm install` to make sure the
checking scripts (here, using husky hooks) are installed. These
are used to check your work after you've resolved the merge and
give you some feedback on how you did.

Then type `git merge origin/develop`.

(NOTE: You could also type `git checkout develop`, then
`git checkout main`, then `git merge develop` to merge
from a local copy of the `develop` branch that you set
up on your machine, but this isn't necessary.)

we're using `git merge origin/develop` instead of `git pull`,
but for this exercise it has the same result. The command means,
"merge the develop branch from the remote repository into
my current branch"

## you have a conflict!

you will see something like this:

```
Auto-merging server.js
CONFLICT (content): Merge conflict in server.js
Auto-merging extra.md
CONFLICT (content): Merge conflict in extra.md
Auto-merging README.md
Automatic merge failed; fix conflicts and then commit the result.
```

ah, looks like we have a conflict in `server.js` and `extra.md`...

...well let's start with `server.js`. First, just to confirm what 
git wants us to do, type `git status` and you should see output 
like this:

```
On branch main
Your branch is up to date with 'origin/main'.

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   extra.md
	both modified:   server.js

no changes added to commit (use "git add" and/or "git commit -a")
```

We can see here that if anything goes wrong, we can use 
`git merge --abort` to get back to before we started the merge.

## the easy example

so let's open the file `server.js`, and you should see the
following:

```
<<<<<<< HEAD
const http = require('http')
const port = 3001

const requestHandler = (request, response) => {
  console.log(request.url)
  response.end('this is the response')
}
=======
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))
>>>>>>> origin/develop

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

This is an easy example, since the two files are mostly completely
different, and so git thinks there's only a little overlap of
similar content right at the bottom to make things tricky.

We'll walk through resolving this conflict step by step.

The first thing to notice is the line right at the top:
`<<<<<<< HEAD`

This tells us where the conflict starts, and also the branch it
refers to. Here, `HEAD` refers to the place that our
current branch is at, so it's basically the same as our local
copy of the `main` branch.

The next important line is 
`=======`

This is telling us where the first part of the conflict ends. So
everything above it is in our current branch's file, and everything
below it is in the develop branch's file.

The final interesting line is right near the bottom:
`>>>>>>> origin/develop`

This tells us where the conflict ends. This is a simple example
since everything from the first marker line to the middle line is
the version in `main`, and everything from the middle line to the
final line is the version in `develop`.

So to resolve the conflict in this file, since the two files
are completely different and we probably only want one version or
the other, just keep the version from `origin/develop` using
express.

In our example here, you should delete everything between
`<<<<<<< HEAD` and `=======`, including those lines themselves.
Remember to also delete the final marker line `>>>>>>> origin/develop`.
(git has actually written those marker lines into our file, so we
need to delete them manually)

That file's conflict is now fixed, let's move on to `extra.md`...

## another still simple example

So when we open the file, we know exactly what to look for 
`<<<<<<< HEAD`, `=======` and `>>>>>>> origin/develop` lines.

Let's open up `extra.md` and look at the conflicts. The first one
looks like:

```
<<<<<<< HEAD
It uses the http library to set up a simple server.
=======
It uses the amazing express library to set up a simple server.
>>>>>>> origin/develop
```

Here are our three marker lines again, so we know where the
conflict is and what the content of it is.

In this case, since we want to use the awesomeness of express,
we can delete everything between the first and second marker
lines (as well as the three marker lines themselves).

So in this case we're deleting everything except the one line.

```
It uses the amazing express library to set up a simple server.
```

Easy, right?!?!

...now on to the second conflict in the file:

```
<<<<<<< HEAD
The usual port is 3000, but it could easily be 3001.
=======
The usual port is 3000, but it could easily be 4001.
>>>>>>> origin/develop
```

Here it looks like exactly one thing was changed in the `develop`
branch. The port number is `4001` instead of `3001`.

Go ahead and decide which line you like better, and delete the
other line, plus the three marker lines. So basically, here
just delete every line except the one line you want to keep.

The last conflict in the file looks like:

```
<<<<<<< HEAD
Express is used in a lot of different projects.
=======
Express can do a lot of things!
>>>>>>> origin/develop
```

...and I'm sure this is getting more familiar now, so either
keep one of the new lines and delete all the marker lines,
or just delete everything to not include either of these.

## check your work

And that's it! Type `git add .` and then `git commit -m 'resolve
merge'` (or whatever commit message you feel like).

If you succeeded, you should see a nice congratulations message.
If there are still unremoved marker lines in the file, you will
be prompted to fix those.
