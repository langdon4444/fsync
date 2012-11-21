#fsync
------

*Inside your SSH session, launch your local editor to work on remote files.*

This utility will let you, from you SSH session to a remote machine, open up a
file or directory on your local workstation. Any edits you make will by synced
back to the remote machine (like dropbox). To get going, you need to run the
fsync server on your local workstation -- it's a tiny little status bar app.

Sort of like mounting your remote server via SSHFS, but better. That solution
doesn't let you easily activate your editor via command line from within your
SSH session.

## Using

From your SSH session into a remote machine:

`$ fs foo.bar`

Pops up your editor, on your workstation. When you hit save, your modified file
will be automatically synced back to your workstation. Note, you need to be
running the little fsync menu bar app on your workstation.

You can make some customizations by setting the environment variables
`$FSYNC_EDITOR`, `$FSYNC_USER`, and `$FSYNC_SERVER`. See the `-h` help for the
`fs` utility for details.

## Install

The server installs as a regular GUI app.

### Mac Menu Bar App (for your workstation)
[Download](https://github.com/rmcgibbo/fsync/downloads) the .zip. The app is
inside. Drag it to your Applications folder.

### Linux Menu Bar App (for your workstation)
[Get](https://github.com/rmcgibbo/fsync/downloads) the .deb. Install it, either
by the command line (`sudo dpkg -i fsync_ubuntu_0.1.deb`) or by double clicking
it, using the Ubuntu application store.

### Client Utility (for your remote machine)
Use the `setup.py` script in the client/ folder. `$ python setup.py install` 
installs the  command line program `fs`.

## Requirements

- fsync server app running on local workstation
- symmetrical passwordless SSH via public keys between your workstation
    and remote.

On your workstation (mac or linux) you run a little server. The server is written
in Objective-C (mac) and python/PyGTK (linux). For the linux system, you need to
have [libzmq](http://www.zeromq.org/intro:get-the-software) installed. For mac,
it's packaged within.

[Built versions](https://github.com/rmcgibbo/fsync/downloads) of the server app
are available on the downloads page.

##How it works

The client (remote machine) looks in your environment variables for `SSH_CLIENT`,
and it opens a ZeroMQ connection over ssh tunnel via the client ip listed in `SSH_CLIENT`.

Then, it rsyncs the files back and alerts the server (your workstation) to open
the files with the editor.

The server uses mac's FSEvents API to monitor for filesystem events on the transferred
files. When any event gets triggered (i.e. saving), it runs an rsync to transfer
the files back to the client.

