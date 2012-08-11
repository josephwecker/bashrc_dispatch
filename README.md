bashrc_dispatch: Different bash configurations for Linux vs OSX, interactive vs batch
=====================================================================================

Are you tired of trying to remember what `.bashrc` does vs `.bash_profile`
vs `.profile`?

Are you tired of trying to remember how Darwin (Mac OS X) treats them
differently from Linux?

Are you tired of not having your `~/.bash*` stuff work the way you expect?

Symlink all of the following files to `bashrc_dispatch`:

*  `~/.bashrc`
*  `~/.bash_profile`
*  `~/.profile`
*  `~/.bash_login`

And then you can use these instead:

*  `~/.bashrc_all`: sourced on every bash instantiation;
*  `~/.bashrc_script`: sourced only when non-interactive;
*  `~/.bashrc_interactive`: the one you'll probably fill up (*mutually
   exclusive* with `~/.bashrc_script`);
*  `~/.bashrc_login`: sourced only when an interactive shell is also a login.

To reiterate,

1. `~/.bashrc_all` will always be run first.
2. Then either `~/.bashrc_script` *or* `~/.bashrc_interactive` will be run
   next depending on whether or not the bash invocation is interactive.
3. Finally, sometimes, like when you first ssh into a machine or often when
   opening a new terminal window on a mac, the `~/.bashrc_login` will be run
   after the `~/.bash_interactive`. So `~/.bashrc_login` is the one where
   you'd echo a banner or whatever.

In addition to the dispatching, you'll forever have the following available:

* `$SHELL_PLATFORM` (either `LINUX`, `OSX`, `BSD` or `OTHER`),
* `shell_is_linux`,
* `shell_is_osx`,
* `shell_is_interactive`,
* `shell_is_script`.

The functions are meant for clean conditionals in your new `~/.bashrc_*`
scripts like:

    $  shell_is_linux && echo 'leenux!'

or something like:

    $  if shell_is_interactive ; then echo 'interact' ; fi

And now I think these comments have reached parity with the code itself which
should be easy to extend.


Configuration
-------------

There are few knobs you can turn to make `bashrc_dispatch` behave as you prefer.

* `EXPORT_FUNCTIONS`: set it to `false` to disable the export of
  `$SHELL_PLATFORM` and all the `shell_is_*` functions and avoid polluting all
   the other shells' environments.

Authors
-------

* Joseph Wecker (initial author)
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

