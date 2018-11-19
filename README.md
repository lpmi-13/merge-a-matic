# MERGE-A-MATIC 

One of the difficult parts of collaboration is dealing with merge
conflicts. This can happen, for example, when Person A alters
a file (eg, `server.js`), and a second person also alters the same
file.

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
checking scripts (here, using husky hooks) are installed.

Then type `git merge origin/develop`. You could also type
`git checkout develop`, then `git checkout master` to merge
from a local copy of `develop` that you set up on your
machine, but this isn't necessary.

(we're using `git merge origin/develop` instead of `git pull`, but for this
exercise it has the same result. The command means,
"merge the develop branch from the remote repository into
my current branch")

## you have a conflict!

The first step will be a prompt like this:
```
Merge remote-tracking branch 'origin/develop'

# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```

This is just git suggesting you add in a reason why you are merging these two branches, so the default message is probably fine.
Just save and close the file.

Now let's move on to our actual conflict!

you will see something like this:

```
Auto-merging server.js
CONFLICT (add/add): Merge conflict in server.js
Auto-merging extra.md
CONFLICT (add/add): Merge conflict in extra.md
```

ah, looks like we have a conflict in `server.js` and `extra.md`...

...well let's start with `server.js`. First, just to confirm what 
git wants us to do, type `git status` and you should see output 
like this:

```
On branch master
Your branch is up to date with 'origin/master'.

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both added:      extra.md
        both added:      server.js
```

We can see here that if anything goes wrong, we can use 
`git merge --abort` to get back to before we started the merge.

## the easy example

so let's open the file `server.js`, and you should see the following:

```
<<<<<<< HEAD
const http = require('http')
const port = 3000

const requestHandler = (request, response) => {
  console.log(request.url)
  response.end('Hello Node.js Server!')
}

const server = http.createServer(requestHandler)

server.listen(port, (err) => {
  if (err) {
    return console.log('something bad happened', err)
  }

  console.log(`server is listening on ${port}`)
})
=======
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
>>>>>>> origin/develop
```

This is an easy example, since the two files are completely 
different, and so there's no overlap of similar content to 
make things tricky.

The first thing to notice is the line right at the top:
`<<<<<<< HEAD`
This tells us where the conflict starts, and also the branch it
refers to. Here, `HEAD` refers to the current place that our
`master` branch is at, so it's basically the same as `master`.

The next important line is 
`=======`

This is telling us where the first part of the conflict ends. So
everything above it is in our current branch's file, and everything
below it is in the develop branch's file.

The final interesting line is right at the bottom:
`>>>>>>> origin/develop`

This tells us where the conflict ends. This is a simple example
since everything from the first marker line to the middle line is
the version in `master`, and everything from the middle line to the
final line is the version in `develop`. So to resolve the conflict in
this file, since the two files are completely different and we
probably only want one version or the other, just keep the version
from `develop` using express.

In our example here, you want to, delete everything between
`<<<<<<< HEAD` and `=======`, including those lines themselves.
Remember to also delete the final marker line `>>>>>>> origin/develop`.
(git has actually written those marker lines into our file, so we
need to delete them manually)

So that file's conflict is now fixed, let's move on to `extra.md`...

## another still simple example

So when we open the file, we know exactly what to look for 
`<<<<<<< HEAD`, `=======` and `>>>>>>> origin/develop` lines.

Let's open up `extra.md` and look at the conflicts. The first one
looks like:

```
<<<<<<< HEAD
=======
Express is super awesome and lets us do a lot of cool
stuff.

>>>>>>> origin/develop
```

Here are our three marker lines again, so we know where the
conflict is and what the content of it is. The reason we don't
have anything between the first line and the second line is
because something was added to the file in the `develop` branch
that didn't exist in the `master` branch, so git isn't sure
which version we want.

In this case, since the awesomeness of express is fairly obvious,
we can delete everything between the second and third marker
lines (as well as the three marker lines themselves).

So in this case we're deleting everything on these 6 lines. Easy,
right?!?!

...now on to the second conflict in the file:

```
<<<<<<< HEAD
The usual port is 3000, but it could easily be 4000.
=======
The usual port is 3000, but it could easily be 4001.
>>>>>>> origin/develop
```

Here it looks like exactly one thing was changed in the `develop`
branch. The port number is `4001` instead of `4000`.

Go ahead and decide which line you like better, and delete the other
line, plus the three marker lines. So basically, here just delete
every line except the one line you want to keep.

The last conflict in the file looks like:

```
<<<<<<< HEAD
=======

You could do all of this with just the simple `http` package,
but it would take a lot longer...why wait?!? Go do awesome!
>>>>>>> origin/develop
```

...and I'm sure this is getting more familiar now, so either keep the
new line and delete all the marker lines, or just delete everything
to keep the version from `master`.

## check your work

And that's it! Type `git add .` and then `git commit -m 'resolve
merge'` (or whatever commit message you feel like).

If you succeeded, you should see a nice congratulations message.
If there are still unremoved marker lines in the file, you will
be prompted to fix those.
