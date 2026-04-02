Early in my career I had no mental model for troubleshooting network issues. I would jump straight to AI or Google and apply quick fixes without understanding what was actually happening. The next time something broke, I was starting from zero again.

Here is the system that changed that for me:

1. ping → is the host even reachable? If not, pure connectivity issue.
2. traceroute → which hop is dropping the traffic? Pinpoints exactly where things break.
3. nc → is the specific port open? If not, firewall or security group issue, not the app.
4. curl → port is open but still failing? Now you know it is an app layer problem.
5. nmap → what ports are actually exposed on this server? Good for catching anything unexpected.

Start broad. Narrow it down. By step four you usually already know what is wrong.

Having this mental map saves time and gives you a real understanding of the infrastructure underneath.

#Networking #DevOps #LearningInPublic
