# Documentation Skill — General

This directory contains examples for the `/documentation` prompt skill.

## Skill File

**Prompt file**: [`.github/prompts/documentation.prompt.md`](../../../.github/prompts/documentation.prompt.md)

## How to Use

1. Open the file you want to document in VS Code.
2. Open Copilot Chat (`Ctrl+Alt+I` / `Cmd+Option+I`).
3. Type `/documentation` and press Enter.

## What It Generates

- Module-level overview
- Public API reference (parameters, return values, exceptions)
- Usage examples
- Configuration reference
- Notes & caveats

## Example Input (Undocumented Module)

```python
import hashlib
import secrets
from datetime import datetime, timedelta

class SessionManager:
    def __init__(self, expiry_minutes=30):
        self._sessions = {}
        self._expiry = timedelta(minutes=expiry_minutes)

    def create_session(self, user_id):
        token = secrets.token_hex(32)
        self._sessions[token] = {
            "user_id": user_id,
            "expires_at": datetime.utcnow() + self._expiry
        }
        return token

    def validate_session(self, token):
        session = self._sessions.get(token)
        if not session:
            return None
        if datetime.utcnow() > session["expires_at"]:
            del self._sessions[token]
            return None
        return session["user_id"]

    def revoke_session(self, token):
        return self._sessions.pop(token, None) is not None
```

## Example Output (Generated Documentation)

```markdown
# SessionManager

Manages user authentication sessions using cryptographically secure tokens.

Sessions are stored in memory and automatically expire after a configurable timeout.
This class is suitable for single-process applications; for distributed systems,
use a shared backend (e.g., Redis) instead.

## Dependencies

No external dependencies. Uses Python standard library only (`hashlib`, `secrets`, `datetime`).

## Public API

### `__init__(expiry_minutes=30)`

Initialize a new SessionManager.

**Parameters:**
| Name | Type | Default | Description |
|------|------|---------|-------------|
| expiry_minutes | int | 30 | Session lifetime in minutes |

### `create_session(user_id) → str`

Create a new authenticated session for a user.

**Parameters:**
| Name | Type | Description |
|------|------|-------------|
| user_id | Any | The unique identifier of the authenticated user |

**Returns:** `str` — A 64-character hex session token.

### `validate_session(token) → Any | None`

Validate a session token and return the associated user ID.
Expired sessions are automatically removed.

**Parameters:**
| Name | Type | Description |
|------|------|-------------|
| token | str | The session token to validate |

**Returns:** The `user_id` associated with the token, or `None` if invalid/expired.

### `revoke_session(token) → bool`

Revoke an active session immediately.

**Returns:** `True` if the session existed and was removed, `False` otherwise.

## Usage Example

```python
manager = SessionManager(expiry_minutes=60)

# Login
token = manager.create_session(user_id=42)

# On subsequent requests
user_id = manager.validate_session(token)
if user_id is None:
    raise PermissionError("Session expired or invalid")

# Logout
manager.revoke_session(token)
```

## Notes & Caveats

- **Not thread-safe**: Use a lock if accessing from multiple threads.
- **Not persistent**: Sessions are lost on process restart.
- **Memory**: Sessions accumulate until they are validated (triggering cleanup) or the process restarts. Consider adding a background cleanup task for long-running processes.
```
