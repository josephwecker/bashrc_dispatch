bashrc\_dispatch
=================

**Different bash configurations for Linux vs OSX, interactive vs batch**

(For related discussion see [hackernews](http://news.ycombinator.com/item?id=4369485))

Are you tired of trying to remember what `.bashrc` does vs `.bash_profile`
vs `.profile`?

Are you tired of trying to remember how Darwin (Mac OS X) treats them
differently from Linux?

Are you tired of not having your `~/.bash*` stuff work the way you expect?


Setup / Usage
--------------

Symlink all of the following files to `bashrc_dispatch` (after reading warnings
below):

*  `~/.bashrc`
*  `~/.bash_profile`
*  `~/.profile`  (only if your scripts are all 'sh' compatible- otherwise get rid of it altogether)
*  `~/.bash_login`

And then you can use these instead:

*  `~/.bashrc_once`: sourced at least once, and in most circumstances only once- before anything else.
*  `~/.bashrc_all`: sourced on every bash instantiation
*  `~/.bashrc_script`: sourced only when non-interactive / batch
*  `~/.bashrc_interactive`: the one you'll probably fill up (*mutually exclusive* with `~/.bashrc_script`)
*  `~/.bashrc_login`: sourced only when an interactive shell is also a login.

To reiterate,

1. `~/.bashrc_once` will be run before any of the others, but then won't be run
   again (ideally).
2. `~/.bashrc_all` will run next, and will be run on _every_ bash invocation.
   (so keep it light)
2. Then either `~/.bashrc_script` *or* `~/.bashrc_interactive` will be run
   next depending on whether or not the bash invocation is interactive.
3. Finally, sometimes, like when you first ssh into a machine or often when
   opening a new terminal window on a mac, the `~/.bashrc_login` will be run
   after the `~/.bash_interactive`. So `~/.bashrc_login` is the one where
   you'd echo a banner or whatever. For things like setting the PATH, use
   `.bashrc_once` instead.


Exported Stuff
--------------

In addition to the dispatching, you'll forever have the following available:

* `$SHELL_PLATFORM` (either `LINUX`, `OSX`, `BSD`, `CYGWIN` or `OTHER`),
* `shell_is_linux`,
* `shell_is_osx`,
* `shell_is_cygwin`,
* `shell_is_interactive`,
* `shell_is_script`.

The functions are meant for clean conditionals in your new `~/.bashrc_*`
scripts like:

    $  shell_is_linux && echo 'leenux!'

or something like:

    $  if shell_is_interactive ; then echo 'interact' ; fi

Warnings
---------

* Obviously don't simply blow away your existing startup scripts if they have
  anything in them- you'll need their content to populate the new `bashrc_*`
  stuff.

* If you symlink `~/.profile` to this script you may be fine, but since it
  sometimes gets sourced by a true `sh` command and the script currently has
  some bash-only stuff, it might not. Specifically, remove the symlink to
  `~/.profile` if anything starts acting strange on startup or xwindows-based
  login.

* Be very careful what you put in `.bashrc_all` and `.bashrc_script` - it may
  slow the system down. I put them there for conceptual completeness- that
  doesn't mean you have to use them (:

Additional Configuration
-------------------------

There are few knobs you can turn to make `bashrc_dispatch` behave as you prefer.

* `EXPORT_FUNCTIONS`: set it to `false` to disable the export of
  `$SHELL_PLATFORM` and all the `shell_is_*` functions and avoid polluting all
   the other shells' environments.

* Inside `bashrc_dispatch` you can change `PRF=` to a location other than
  `${HOME}/.` to have it look for your new `bashrc_*` scripts somewhere else.

* In general, modify your `bashrc_dispatch` script as much as you need to.
  You'll see there's not a lot of code there. Much less code then there are
  comments in this readme. Please share a patch if you like your modification.

Authors
-------

* Joseph Wecker
* Gioele Barabucci <http://svario.it/gioele> (fixes and optimizations)


Development
-----------

Code
: <https://github.com/josephwecker/bashrc_dispatch>

Report issues
: <https://github.com/josephwecker/bashrc_dispatch/issues>


License
-------

This is free software released into the public domain.

