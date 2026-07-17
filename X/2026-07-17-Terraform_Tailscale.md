1/ terraform init kept timing out trying to reach the provider registry. No useful error message, just a hang.

2/ Checked my internet. Fine. Opened the registry URL in a browser. Loaded instantly. Reran terraform init anyway. Same timeout.

3/ Root cause: IPv6. Tailscale was routing the registry lookup over IPv6, and that path was silently broken. Switching to IPv4 worked immediately, same command, same machine.

4/ Temporary fix: force IPv4, confirm the request goes through, rerun.

5/ Permanent fix: split DNS instead of fighting IPv6 routing directly. Tailnet devices and hostnames still resolve through Tailscale, everything else routes through Google's resolver. Sidesteps the broken IPv6 path for anything outside the tailnet.

6/ Lesson: a vague timeout doesn't mean your internet is down or the tool is broken. Check which IP family the request is actually using before you go looking anywhere else.
