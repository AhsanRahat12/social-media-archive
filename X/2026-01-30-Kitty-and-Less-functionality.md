Ever pressed `/` in less to search and then... your keyboard just stops working? 😤

I was reviewing Kubernetes docs in Kitty terminal. Hit `/` to search, cursor appeared, but I couldn't TYPE anything. Every keystroke vanished.

The issue: Kitty's `xterm-kitty` terminfo has a keyboard input incompatibility with `less`.

**The fix:**

Add to `~/.bashrc`:
```bash
export TERM=xterm-256color
```

Then: `source ~/.bashrc`

That's it. 30 seconds. Search works perfectly now.

**Alternatives:**
- Use grep: `kubectl run nginx --help | grep -i "pattern"`
- Use vim: `kubectl run nginx --help | vim -`

Small terminal quirks waste hours. Document your solutions.

What's your most annoying terminal quirk? 

#DevOps #Linux #Troubleshooting
