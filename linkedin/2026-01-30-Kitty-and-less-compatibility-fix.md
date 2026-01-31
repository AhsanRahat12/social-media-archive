**Ever pressed `/` in less to search for something and then... your keyboard just stops working? 😤**

I was deep into reviewing Kubernetes help documentation in my Kitty terminal, looking for specific `kubectl run` flags. Found what I needed, piped it to `less` for easier reading, and hit `/` to search for a pattern.

The search prompt appeared like normal. The cursor was blinking. Everything looked fine.

Then I tried to type my search term... and nothing. Every keystroke vanished into the void. Backspace didn't work. Letters didn't appear. The keyboard was completely unresponsive in the search field.

Turns out, Kitty's `xterm-kitty` terminfo has a keyboard input protocol incompatibility with `less`. The search prompt appears fine, but keyboard input doesn't register during search.

**Here's the fix that actually works:**

✅ Add `export TERM=xterm-256color` to your `~/.bashrc`  
✅ Reload with `source ~/.bashrc`  
✅ Keyboard input immediately works in less search

**Alternative approaches if you prefer:**

1. Create a less-specific alias: `alias less="TERM=xterm-256color less"`
2. Use grep for quick searches: `kubectl run nginx --help | grep -i "search term"`
3. Switch to vim: `kubectl run nginx --help | vim -`

The permanent fix took 30 seconds. Now I can actually type my search patterns without fighting my terminal emulator.

Small terminal quirks like this can waste hours of troubleshooting. Documenting solutions saves you (and others) significant debugging time.

**What's the most frustrating terminal quirk you've encountered? How did you solve it?** I'd genuinely love to learn from your experiences 👇

Follow me for real DevOps troubleshooting and Linux system administration insights.

#DevOps #Linux #Troubleshooting #ArchLinux #KittyTerminal
