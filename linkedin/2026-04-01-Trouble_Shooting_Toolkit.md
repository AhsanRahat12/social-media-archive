Early in my career, when I couldn't reach a service, I would panic a little, stare at the logs, then hop straight to the internet or AI for a fix.

Although there is nothing wrong with that, lacking the proper mental model and the correct system to approach the problem meant I would spend hours fixing something simple. And worse, relying on quick fixes meant I never really learned what was happening under the hood. So the next time something broke, I was starting from zero all over again.

Here is the mental map and the toolkit I use now:

1. Is the host even reachable?
   ping google.com
   If this fails, you have a connectivity problem. Stop here and fix it first.

2. Which hop is dropping my traffic?
   traceroute google.com
   Tells you exactly where in the network things are breaking down.

3. Is this specific port actually open?
   nc -zv google.com 443
   If the host is reachable but the port is closed, the problem is your firewall or security group, not the app.

4. Is the app responding?
   curl -I https://google.com
   If the port is open but curl fails, the problem is at the application layer, not the network.

5. What ports are exposed on this server?
   nmap -sV google.com
   Useful for auditing what is actually open and catching anything unexpected.

Start broad and work your way down. By the time you reach step four you usually already know exactly what is wrong.

Having this mental map does not just save time. It gives you a much better understanding of what is actually happening under the hood, which makes the next fix faster and the one after that even faster.

What is the first command you run when something breaks? Drop it below 👇

Follow me for more tidbits on networking and DevOps from someone learning it hands-on.

#Networking #DevOps #LearningInPublic #CloudNative #Linux
