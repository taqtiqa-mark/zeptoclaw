## Why

Add user authentication to the ZeptoClaw agent system to secure access to sensitive operations like tool execution, channel management, and configuration changes. This is needed now to comply with enterprise security requirements and prevent unauthorized access in multi-user deployments.

## What Changes

- Introduce user login and session management for the CLI and API endpoints.
- Add authentication middleware to all incoming requests in the gateway.
- Implement role-based access control (RBAC) for different user privileges (e.g., admin, user, viewer).
- **BREAKING**: Existing unauthenticated access to certain CLI commands will require login.
- Add logout and session expiration mechanisms.

## Capabilities

### New Capabilities
- `user-auth`: Handles user registration, login, password hashing, and JWT token generation for session management.
- `rbac`: Manages role assignments, permission checks, and access denial for protected operations.

### Modified Capabilities
- `gateway`: Extend the existing gateway to include auth checks on all routes, modifying the request handling flow to validate tokens before processing.

## Impact

- Core affected modules: `src/gateway/`, `src/cli/`, `src/config/types.rs` for new auth config fields.
- Dependencies: Add `jsonwebtoken` and `bcrypt` crates for token and password handling.
- APIs: New endpoints under `/auth/` for login/register; all existing endpoints protected.
- Systems: Integrates with existing provider and channel systems without disrupting unauthenticated fallback modes (opt-in security).