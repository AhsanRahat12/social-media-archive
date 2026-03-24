Most people think a VPN just creates a secure tunnel.

IPSec actually builds it in two phases.

→ Phase 1: endpoints authenticate and agree on encryption. No data moves yet.
→ Phase 2: runs inside Phase 1. This is where actual data transfer happens, with faster lighter encryption.

Why two? Do the expensive security work once, let the faster tunnel handle the rest.

#Networking #DevOps #LearningInPublic
