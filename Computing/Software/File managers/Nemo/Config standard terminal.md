# Config standard terminal

Google brought me here, so I'm reviving this thread in hope of saving at least one person from a few unnecessary headaches.

I'm using Debian and [the suggestion from L. D. James here](https://unix.stackexchange.com/a/336587/114401) didn't work for me. So I took a look at Nemo's source code, and on [line 132 of **nemo-global-preferences.c**](https://github.com/linuxmint/nemo/blob/015db702c7234d6c2ceda8707045186480dcc62e/libnemo-private/nemo-global-preferences.c#L132) I found that the (upstream) config schema is the following:

> org.cinnamon.desktop.default-applications.terminal

I'm using Nemo as a substitute for Nautilus, and since I'm using Gnome3 instead of Cinnamon, for me this schema didn't even exist. So, I created it with the following command. After issuing this command, the 'Open in Terminal' action opens `gnome-shell`, in the correct directory:

```
gsettings set org.cinnamon.desktop.default-applications.terminal exec gnome-shell 
```

Just replace `gnome-shell` in the command with whatever terminal you'd like to use. Ex: for `gnome-terminal`, use:

```
gsettings set org.cinnamon.desktop.default-applications.terminal exec gnome-terminal
```

And for [`terminator`](https://en.wikipedia.org/wiki/Terminator_(terminal_emulator)) (`sudo apt install terminator`) use:

```
gsettings set org.cinnamon.desktop.default-applications.terminal exec terminator
``` 