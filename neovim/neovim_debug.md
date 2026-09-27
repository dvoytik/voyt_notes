
Start Neovim in its absolute default "factory" state
----------------------------------------------------
```
nvim --clean
```
What this does:
 * Skips your init.lua or init.vim file entirely.
 * Skips loading all plugins.
 * Skips reading your shada (shared data) file so command history and previous marks aren't loaded.
 * This is the best command to use if you are trying to reproduce a bug to see if it's caused by Neovim itself or your personal configuration.


Trace hot spots
---------------
Open the specific file.
Run the following commands:
```
:profile start profile.log
:profile func *
:profile file *
```
Do things that causes slow down
Pause the profiler with:
```
:profile pause
```
Quit neovim

Check profiler.log for stuff causing the slow-down.
