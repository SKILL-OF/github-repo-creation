# Repo Creation Safety Patterns

**Gap:** skill-of/github-repo-creation had README only.  
**Fill:** Safety patterns for agent-driven repo creation.

## Pattern 1: Pre-Creation Validation

Before creating repo on GitHub:
1. Verify org exists (gh api orgs/{org})
2. Verify repo name doesn't collide (gh repo view {org}/{repo})
3. Verify actor has permission (gh api user --jq .login)
4. Document intention (why this repo exists)

Never create repo if any check fails.

## Pattern 2: Atomic Repo + Initial Content

Repo creation and initialization must be atomic:
1. Create repo via gh api (empty)
2. Immediately clone and add: README.md, .gitignore, LICENSE
3. Push initial commit
4. Verify remote has content

Never leave repo empty or partially initialized.

## Pattern 3: Governance Template

Every new repo includes:
- README (what this repo is for)
- LICENSE (copyright/usage)
- .gitignore (what not to commit)
- CONTRIBUTING.md (if accepting contributions)

Templates prevent half-baked repos.

## Pattern 4: Record Creation

After successful creation, record:
```json
{
  "created_timestamp": "ISO-8601",
  "repo": "org/name",
  "creator": "agent-id",
  "reason": "why this repo exists",
  "verification": "proof repo exists"
}
```

Creates audit trail for multi-agent environments.

## Pattern 5: Ownership Transfer

If repo will be owned by peer agent:
1. Create repo
2. Add peer agent as collaborator
3. Transfer admin to peer (if needed)
4. Record transfer with timestamp
5. Peer acknowledges receipt

Clear handoff prevents ownership ambiguity.

