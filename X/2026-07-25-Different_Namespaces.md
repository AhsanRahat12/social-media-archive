1/ Ran into a small bug while tinkering in my Cloudlab project, simple one but it took longer to fix than I would've liked. Sharing it anyway, coming from a home lab setup I'd never had to think about this before.
·
2/ File was helmrelease.yaml. HelmRelease reconciling with no errors, everything green, but the thing it was supposed to install never showed up. No failure message, just silence.

3/ Cloudlab's on Azure, and this was one of the small differences from home lab. A HelmRelease has two separate namespace fields: targetNamespace (where resources end up) and sourceRef.namespace (where Flux finds the HelmRepository to pull from).

4/ Hadn't seen that structure before, so I set both to the same value, the namespace I wanted the stack to run in. Made sense in my head, Flux disagreed.

5/ Fix: one line, set sourceRef.namespace to the namespace where Flux itself lives, instead of assuming it matched targetNamespace.

6/ Nothing complicated once you see it written out, target answers "where does this end up," source answers "where does Flux go looking for it." I just hadn't seen the structure before, so I quietly assumed they'd match. Flux had no reason to agree with me.
