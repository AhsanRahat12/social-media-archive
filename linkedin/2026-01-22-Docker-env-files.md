# Docker Environment Files Best Practice

**Platform:** LinkedIn  
**Date:** 2026-01-22  
**Topic:** Docker Security, .env Files  
**Status:** Ready to Post

---

## Post Content

Hardcoding secrets in docker run commands? That's a security incident waiting to happen.

I used to paste passwords directly in my terminal:
```bash
docker run -d -e POSTGRES_PASSWORD=secret123 postgres
```

Now my terminal history is a security risk.

**The better approach:**

✅ Create a .env file with your secrets
✅ Reference it with --env-file flag
✅ Add .env to .gitignore immediately
```bash
# db.env
POSTGRES_PASSWORD=secret
POSTGRES_USER=myuser

# Clean terminal, secure secrets
docker run -d --env-file db.env postgres
```

Your credentials stay out of shell history, git repos, and prying eyes.

Have you caught yourself hardcoding secrets before switching to .env files? What was your wake-up moment? 👇

#Docker #DevOps #Security #BestPractices #LearningInPublic

---

## Metadata

- **Hashtags:** #Docker #DevOps #Security #BestPractices #LearningInPublic
- **CTA:** Personal experience question about hardcoding secrets
- **Format:** Problem-solution with code examples
- **Engagement Strategy:** Relatable mistake, practical solution
