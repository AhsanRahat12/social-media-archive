# Docker Environment Files Best Practice

**Platform:** X (Twitter)  
**Date:** 2026-01-22  
**Topic:** Docker Security, .env Files  
**Status:** Ready to Post

---

## Post Content

Stopped hardcoding Docker secrets in my terminal after realizing my entire shell history was a security disaster 💀

Before:
```bash
docker run -e POSTGRES_PASSWORD=secret123 postgres
```

Now:
```bash
# db.env file
POSTGRES_PASSWORD=secret
POSTGRES_USER=myuser

docker run --env-file db.env postgres
```

Pro tip: Always add .env to .gitignore

Your future self (and security team) will thank you 🔐

#Docker #DevOps #Security

---

## Metadata

- **Hashtags:** #Docker #DevOps #Security
- **Character Count:** ~350 characters (well within X limits)
- **Format:** Punchy before/after with code snippets
- **Tone:** Casual, relatable, with light humor
