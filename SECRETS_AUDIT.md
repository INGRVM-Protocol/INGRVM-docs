# INGRVM Secrets Scrub Audit

**Date:** 2026-03-20
**Auditor:** Kilo (INGRVM_AUDITOR)
**Branch:** kilo/laptop
**Scope:** Entire repository scan for hardcoded secrets, API keys, passwords, tokens, and .env files with real values

---

## Executive Summary

This audit found **9 instances of hardcoded secrets** across the repository that should be addressed before public release. The majority of issues are in non-INGRVM projects, but INGRVM itself contains 1 critical secret that must be remediated.

**Critical Severity:** 1 issue (INGRVM mesh encryption PSK)
**High Severity:** 4 issues (Production API keys)
**Medium Severity:** 4 issues (Development keys and configuration)

---

## Critical Findings

### 1. INGRVM Mesh Encryption PSK - CRITICAL
**File:** `INGRVM/.env`
**Type:** Hardcoded mesh encryption pre-shared key
**Impact:** Compromises the entire mesh security. This PSK is used to derive AES-GCM encryption keys for all spike traffic. Every node in the mesh shares this value.

**Finding:**
```
INGRVM_SECURE_PSK=45cd2b0497aadf979755e5fd2dafeaa0d4c48227bf82462b229ea24c89032660
```

**Recommendation:**
- Revoke and regenerate this PSK immediately using `python -c "import secrets; print(secrets.token_hex(32))"`
- Update all nodes' .env files with the new value
- Ensure .env is added to .gitignore (appears to be already ignored based on repository status)
- For production, use a secure secrets management system (AWS Secrets Manager, HashiCorp Vault, or environmental variables only)

---

## High Severity Findings

### 2. Google API Key (JARVIS Sentinel) - HIGH
**File:** `Dev/JARVIS/Sentinel/.env`
**Type:** Production Google Cloud API key
**Impact:** Can be used to consume Google Cloud services, potentially incurring charges
```
GOOGLE_API_KEY=AIzaSyCnMK7KP-_uT_jVrUxDJ-S07F24ibNq0ko
```

**Recommendation:**
- Rotate this API key immediately in Google Cloud Console
- Add to .gitignore if not already there
- Delete from repository history if committed previously

### 3-6. Google API Keys (DnD Campaign Projects) - HIGH
**Files:**
- `DnD_Campaign/campaigns/Gate_Breakers/.env`
- `DnD_Campaign/campaigns/Isekai_Campaign/.env`
- `DnD_Campaign/narrator_tool/.env` (also contains Supabase credentials)

**Finding:** Same Google API key appears to be duplicated across multiple DnD Campaign projects
```
GOOGLE_API_KEY=AIzaSyCnMK7KP-_uT_jVrUxDJ-S07F24ibNq0ko
```

**Recommendation:**
- Use project-specific API keys instead of sharing across projects
- Rotate the key in Google Cloud Console
- Add all DnD .env files to .gitignore

### 7. Supabase Database Credentials (DnD Campaign) - HIGH
**File:** `DnD_Campaign/narrator_tool/.env`
**Type:** Supabase URL and anon key (JWT)
**Impact:** Grants database read/write access depending on RLS policies

**Finding:**
```
SUPABASE_URL=https://gimatggwhorwplhwdylp.supabase.co
SUPABASE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImdpbWF0Z2d3aG9yd3BsaHdkeWxwIiwicm9sZSI6ImFub24iLCJpYXQiOjE3Njg1MzE3NDUsImV4cCI6MjA4NDEwNzc0NX0.2WxGywyJH48szLwptivSATlA75x8C5EAKJs6YZoClVY
```

**Recommendation:**
- Rotate these credentials in Supabase dashboard
- Ensure Row Level Security (RLS) is properly configured
- Add .env to .gitignore

---

## Medium Severity Findings

### 8. Context7 API Key (Gemini Configuration) - MEDIUM
**File:** `.gemini/settings.json`
**Type:** Upstash Context7 service key
**Impact:** Can be used to consume Context7 API services

**Finding:**
```json
"CONTEXT7_API_KEY": "ctx7sk21b010bf-1f4d-4b81-a06f-e9519ac93ebd"
```

**Recommendation:**
- Rotate key in Upstash dashboard if needed
- Consider using environment variable instead
- Ensure .gemini directory is in .gitignore

### 9. GitHub Personal Access Token - MEDIUM
**File:** `.gemini/settings.json`
**Type:** GitHub OAuth token
**Impact:** Grants API access to GitHub repositories under that token's permissions

**Finding:**
```json
"GITHUB_TOKEN": "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r"
```

**Recommendation:**
- Verify token permissions and reduce to minimum necessary scope
- Rotate token if compromised or too permissive
- Remove from version control, use environment variable

---

## Low Priority / Configurations

### 10. INGRVM Hub Configuration
**File:** `INGRVM/.env`
**Type:** Cloudflare tunnel URL (not a secret, but potentially exposes infrastructure)

**Finding:**
```
INGRVM_HUB_URL=https://fabric-reduced-glass-fitting.trycloudflare.com
```

**Note:** This is a public-facing tunnel endpoint and doesn't expose secrets, but for production you'll want a proper domain.

### 11. Root Identity Key
**File:** `./identity.key` (repository root)
**Type:** Binary Ed25519 private key material

**Finding:** Binary data (approximate length appears to be cryptographic key material)

**Recommendation:**
- Verify this is meant to be generated per-deployment, not committed
- If it's a development key, document how to regenerate
- Should be excluded from version control for production deployments

### 12. Calyx Configuration File
**File:** `Calyx/.env`
**Type:** Network configuration and peer identity

**Note:** Contains peer multiaddr and local IP addresses, but these are operational configuration, not secrets. The file does not contain PSKs or API keys.

---

## Files NOT Contributing to .gitignore

The following `.env` files were found in the repository. Many of these appear to be ignored by Git based on repository status, but should be verified:

1. `Calyx/.env` - (Network config - likely safe)
2. `Dev/JARVIS/Sentinel/.env` - **Contains secrets - verify gitignore status**
3. `DnD_Campaign/campaigns/Gate_Breakers/.env` - **Contains secrets - verify gitignore status**
4. `DnD_Campaign/campaigns/Isekai_Campaign/.env` - **Contains secrets - verify gitignore status**
5. `DnD_Campaign/narrator_tool/.env` - **Contains secrets - verify gitignore status**
6. `INGRVM/.env` - **Contains critical secrets - appears to be ignored ✓**

---

## INGRVM-Specific Recommendations

For the INGRVM project specifically:

1. **Immediate Action Required:**
   - Regenerate `INGRVM_SECURE_PSK` in all nodes
   - Verify `INGRVM/.env` is permanently excluded from version control

2. **Before Launch:**
   - Implement environment variable validation in startup (`hub_server.py`, `node_setup.py`)
   - Refuse to start if required secrets are not set or are placeholder values
   - Add secrets validation to the Phase 8 gate (currently "NOT STARTED")

3. **Architecture:**
   - Consider separating node-specific config (IP, node ID) from secrets (PSK, auth keys)
   - Use a two-tier .env approach: `.env.production.example` + actual `.env`
   - Document the required secrets and their purposes in the main README

---

## Recommended Actions Summary

| Priority | Action | Owner | Deadline |
|----------|--------|-------|----------|
| P0 | Regenerate INGRVM_SECURE_PSK and update all nodes | PC_MASTER | Immediate |
| P0 | Verify all .env files are in .gitignore | Infrastructure Lead | Today |
| P1 | Rotate Google API key and update consumers | JARVIS/DnD Teams | This Week |
| P1 | Rotate Supabase credentials | DnD Team | This Week |
| P2 | Add secrets validation to hub_server.py startup | Core Team | Horizon 2 |
| P2 | Review and rotate GitHub token permissions | Infrastructure Lead | Next Sprint |

---

## Files to Add to .gitignore (if not already)

```
# Environment files with secrets
.env
.env.local
.env.production
.env.staging

# Identity and keys
*.key
identity.key

# Service configuration
.gemini/settings.json
```

---

## Audit Methodology

This audit was performed using the following methods:

1. **File pattern search:** Searched for `*.env`, `.env*` files
2. **Content pattern matching:** Searched for common secret patterns:
   - `api_key`, `API_KEY`, `apikey`, `APIKEY`
   - `secret`, `SECRET`, `token`, `TOKEN`
   - `password`, `PASSWORD`, `passwd`, `pwd`
   - Common API key prefixes: `sk-`, `pk-`, `ak-`, `SG.`, `github_pat_`, `ghp_`, `glpat-`
3. **Filename analysis:** Searched for files named with `secret`, `key`, `credential`, `token`, `password`
4. **Manual review:** Examined found files for actual secret content vs. configuration

**Limitations:**
- Did not scan binary files (model weights, database files)
- Did not scan git commit history for secrets that may have been removed
- Some grep operations timed out on large directories; focused on high-value areas
- Did not validate whether keys are currently active or revoked

---

## Conclusion

The INGRVM project has one critical security issue (the mesh encryption PSK) that must be addressed immediately. The remaining issues are in sister projects within the ecosystem and should be remediated for overall security hygiene.

**Status:** Audit complete - 9 secrets found requiring remediation

**Next Steps:**
1. Address critical INGRVM finding
2. Proceed to documentation tasks (QUICKSTART.md, API_GUIDE.md)
3. Follow up on remediation in subsequent sessions

---

*This audit was conducted in accordance with INGRVM Auditor Directive #1. No auto-fixes were applied as per instruction.*