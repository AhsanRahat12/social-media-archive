terraform init kept failing to reach the provider registry. Timeout, no explanation, nothing in the error message pointing at the real cause.

I checked my internet connection. Fine. Checked the registry URL in a browser. Loaded fine. Reran terraform init. Same timeout.

The actual problem was IPv6. Tailscale was routing the registry lookup over IPv6, and that path was silently broken. Switching to IPv4 worked immediately, same command, same machine, nothing else changed.

The permanent fix ended up being split DNS instead of fighting IPv6 routing directly. Tailnet devices and hostnames still resolve through Tailscale, everything else routes through Google's resolver. That sidesteps the broken IPv6 path entirely for anything outside the tailnet.

The lesson that actually matters: when a tool times out with a vague network error, don't assume it's your internet or the tool itself. Check which IP family the request is actually going out on before you go looking anywhere else.

Ever had a tool fail for a reason that had nothing to do with the tool itself?
