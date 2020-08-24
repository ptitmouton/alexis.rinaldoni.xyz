---
layout: post
title:  "Remap keys on mac"
date:   2020-08-24 19:29:52 +0200
categories: mac keyboard
---
My development environment primarily consisting of [neovim][neovim] and [tmux][tmux-gh], I rely heavily on the keyboard and custom keybindings.
Regarding this, I of course want to remap the [Caps Lock][capslock-wikipedia] key to the Ctrl key, very much easing pressing combinations like <Ctrl-A>, <Ctrl-R> or <Ctrl-T>
(even for such a blessed pianist like I am :D)

I had used [karabiner][kabiner-gh] for a while in order to remap the keys on mac OS, unfortunately apple has decided it knows better than the user, and karabiner relying on
[kernel extensions][karabiner-gh-issue-kernelext] (very much restricted by Apple), Karabiner stopped working when I (not) cleverly installed Big Sur, and won't work
on newer macOS versions until a [driverkit][driverkit] version is developed.

After a little more research I found I do not even need a tool like karabiner: macOS has a built-in solution to remap keyboard keys: hidutil
![The hidutil manpage does not tell very much](/assets/images/2020-08-24-remap-keys-on-mac-hidutil.png)
The manpage is not very yielding, but there is a technical not on [apple's developer page][apple-dev-technote-hidutil] describing everything needed.
There is some lookup to do (ok, of course someone built a [react app][hidutil-generator] for this), but it is pretty straightforward.

For me, this command does the trick:

```
hidutil property --set {"UserKeyMapping":[{"HIDKeyboardModifierMappingSrc":0x700000039,"HIDKeyboardModifierMappingDst":0x7000000E0}]}
```

I very much like using a standard built-in command rather than a third-party software, especially for something that should be as trivial as remapping caps lock.


I am not sure how much of a standard hidutil is, but [the manpage][hidutil-man] suggests it is a BSD-thingy.
Anyways, a quick test showed it was not available on my Arch installation.

[neovim]: https://neovim.io/
[tmux-gh]: https://github.com/tmux/tmux
[capslock-wikipedia]: https://en.wikipedia.org/wiki/Caps_Lock
[karabiner-gh]: https://github.com/pqrs-org/Karabiner-Elements
[karabiner-gh-issue-kernelext]: https://github.com/pqrs-org/Karabiner-Elements/issues/2331
[driverkit]: https://developer.apple.com/documentation/driverkit
[hidutil][hidutil]:
[apple-dev-technote-hidutil]: https://developer.apple.com/library/archive/technotes/tn2450/_index.html
[hidutil-generator]: https://github.com/amarsyla/hidutil-key-remapping-generator
[hidutil-man]: https://manpagez.com/man/1/hidutil/
