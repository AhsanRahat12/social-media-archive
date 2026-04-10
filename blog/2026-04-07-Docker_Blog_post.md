# I Spent Hours Fighting Python Versions. There Was a Better Way.

I just wanted to build a feature store.

That was it. I was working on a data project, wanted to use Hopsworks — a feature store platform for managing machine learning data — and suddenly I was knee-deep in Python version conflicts I didn't ask for and didn't understand.

My environment was Python 3.11. Hopsworks wanted something else. Fine — how hard could it be?

Three hours later I had broken my environment twice, half-fixed it manually, and hadn't written a single line of actual data code.

## The Wrong Fix

I eventually got it working. Manually forced the version change, untangled the dependencies, made it run.

But here's the thing that stuck with me: I had no idea if anyone else could run it. I had no idea if *future me* could run it. The fix lived entirely in the specific, messy state of my machine. It wasn't a solution — it was duct tape.

I'd heard of virtualenv. I tried it. It felt like more manual work on top of manual work. I still had to think about versions. I still had to manage the environment myself. I just wanted to do data work, not babysit my Python installation.

There had to be a better way.

## The Idea That Changed How I Think

When I joined the KubeCraft internship program and started getting serious about DevOps, I kept running into the same concept: isolation.

Not just as a Docker feature. As a philosophy.

The idea is simple but it took me a while to really feel it: your code and everything it needs should travel together, sealed, reproducible, identical everywhere it runs. Not "works on my machine." Works on any machine. Works six months from now. Works when someone else clones your repo.

That Hopsworks nightmare wasn't really a Python version problem. It was an environment problem. And Docker doesn't fix version conflicts — it makes version conflicts irrelevant.

That realization hit differently once I understood it.

## Proving It To Myself

I learn best by building something, so I found a project to dockerize.

I'd been working through Python Crash Course by Eric Matthes and built the Alien Invasion game from the exercises. Then I had an idea: what if I containerized it?

A GUI game is actually one of the harder things to put in a container. Docker containers don't have displays. To get a game window to appear you need to forward the X11 display from your host machine into the container — something that trips up a lot of beginners.

It ran. `xhost +local:docker` to grant display access, mount the X11 socket as a volume, pass the DISPLAY environment variable into the container. Game window opened. High scores persisted between sessions via a volume mount.

Was it perfect? No — it runs as root, which works fine for a local project that's never exposed to the internet. But it was *reproducible*. Clone the repo, run two commands, game opens. No Python version negotiation. No environment setup. No "works on my machine."

That feeling — of handing someone a self-contained thing that just works — was addictive.

## Going Deeper

The game project taught me the mechanics. My next project made me apply the principles properly.

The Dad Joke Dashboard sounds silly — it fetches a random dad joke every 30 seconds and serves it on a webpage. But the architecture is where I pushed myself.

Three containers working together: an init container that sets up volume permissions before anything else starts, an updater container running a bash script that hits the icanhazdadjoke API, and an nginx web container serving the result. Both main containers run as non-root users. The web container mounts the shared volume as read-only. Restart policies handle failures automatically.

Compare that to the game project. Same core concepts — containers, volumes, environment variables. But now I was thinking about security, about service dependencies, about what happens when something fails.

The difference between those two projects is roughly what Docker actually teaches you if you push past the basics.

## What I'd Tell Past Me

Virtualenv isn't bad. It has its place. But if you're coming from data work and you've ever felt that particular frustration of fighting your environment when you just want to build something — Docker is worth your time.

Not because it's magic. Because it forces you to think clearly about what your code actually needs to run, write that down explicitly, and never think about it again.

That Hopsworks conflict would have been a ten-line Dockerfile. Three hours of my life, back.

I'm still early in this journey — working as a data analyst at Green World by day, building DevOps skills through the KubeCraft internship in the evenings. But Docker is one of those tools that changed how I think about building things, not just how I deploy them.

That's rare. It's worth writing about.

---

*Both projects are on my GitHub: [Dockerized-Projects](https://github.com/AhsanRahat12/Dockerized-Projects/tree/main)*

*This is part of an ongoing series documenting my path from Environmental Science grad and data analyst to full-time DevOps engineer.*

💼 LinkedIn: [Rahat Ahsan](https://www.linkedin.com/in/rahatahsan/)  
🐦 X: [RahatAhsan20](https://x.com/RahatAhsan20)  
🐙 GitHub: [AhsanRahat12](https://github.com/AhsanRahat12)
