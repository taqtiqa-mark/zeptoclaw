## Context

ZeptoClaw is a Rust-based agent system with CLI, gateway, and various channels/providers. Currently, there is no built-in authentication, relying on external mechanisms or unauthenticated access. This design introduces JWT-based auth with RBAC to secure sensitive operations. Constraints include maintaining backward compatibility where possible, minimal performance overhead, and integration with existing async runtime (Tokio). Stakeholders: security team (compliance), devs (ease of use), ops (deployment simplicity).

## Goals / Non-Goals

**Goals:**
- Secure CLI commands and API endpoints with login-required access.
- Implement RBAC for granular permissions (e.g., read-only vs. full admin).
- Support opt-in mode for legacy unauthenticated use.
- Handle user sessions with expiration and refresh.

**Non-Goals:**
- Multi-factor authentication (MFA) - future enhancement.
- External identity providers (e.g., OAuth) - focus on internal user management.
- Database persistence for users - use in-memory or file-based for now; DB integration later.

## Decisions

1. **JWT for Sessions**: Use JSON Web Tokens for stateless session management. Rationale: Fits async, distributed nature of gateway; no session storage needed. Alternative: Cookies - rejected for API/CLI incompatibility. Library: `jsonwebtoken` crate.

2. **Password Hashing**: bcrypt for secure hashing. Rationale: Proven, slow-hash resistance to brute-force. Alternative: Argon2 - similar but bcrypt is more mature in Rust ecosystem.

3. **RBAC Model**: Simple role-permission mapping stored in config or user struct. Permissions checked via middleware. Rationale: Lightweight for agent system; avoids complex ACLs. Alternative: Attribute-based (ABAC) - overkill for initial impl.

4. **Middleware Placement**: Auth middleware as first layer in gateway request handler chain, before routing. For CLI, wrap commands in auth guard. Rationale: Early failure for unauthorized requests. Alternative: Per-endpoint guards - more boilerplate.

5. **Token Expiration**: 1-hour access + 24-hour refresh tokens. Rationale: Balances security and usability. Stored in-memory for refresh validation.

6. **User Storage**: In-memory HashMap for prototype; configurable to file/JSON. Rationale: Quick start; easy to extend. Alternative: SQLite - adds dependency overhead.

## Risks / Trade-offs

- [JWT Secret Management] → Use env var or config file; warn on weak secrets in logs.
- [Performance Overhead] → Token validation is fast (~1ms); benchmark in integration tests. Trade-off: Slight latency increase for secured paths.
- [Breaking Changes] → Opt-in flag `--no-auth` for CLI to mitigate; document migration in README.
- [Key Rotation] → Not implemented initially; risk of compromised keys. Mitigation: Runtime reloadable secret via SIGHUP.
- [In-Memory Loss] → Sessions lost on restart; trade-off for simplicity. Mitigation: Persist to file on shutdown.