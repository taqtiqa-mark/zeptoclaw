## 1. Dependencies and Setup

- [ ] 1.1 Add `jsonwebtoken` and `bcrypt` crates to Cargo.toml
- [ ] 1.2 Define config fields for auth in src/config/types.rs (e.g., jwt_secret, bcrypt_cost, enable_auth)
- [ ] 1.3 Create new modules: src/auth/mod.rs, src/auth/user.rs, src/auth/rbac.rs

## 2. User Authentication Implementation

- [ ] 2.1 Implement User struct with id, username, hashed_password, role in src/auth/user.rs
- [ ] 2.2 Implement password hashing function using bcrypt in src/auth/user.rs (cost >=12)
- [ ] 2.3 Create in-memory user store (HashMap<UserId, User>) with load/save to file option
- [ ] 2.4 Implement registration logic: validate input, hash password, store user, return success (covers User Registration, Duplicate Username scenarios)
- [ ] 2.5 Implement login logic: find user, verify password, generate JWT with claims {user_id, role, exp}, return token (covers User Login, Invalid Credentials, JWT Token Generation scenarios)
- [ ] 2.6 Add token refresh logic: validate refresh token, issue new access token (covers Token Refresh scenario)

## 3. RBAC Implementation

- [ ] 3.1 Define Role enum: Viewer, User, Admin in src/auth/rbac.rs
- [ ] 3.2 Implement role assignment: default 'User' on registration, admin update endpoint (covers Role Assignment scenarios)
- [ ] 3.3 Define permission mapping: HashMap<Role, Vec<Operations>> where Operations are enums like ConfigChange, ToolExec
- [ ] 3.4 Implement has_permission function: check user role against required operation (covers Permission Checks, Role Validation scenarios)

## 4. Gateway Integration

- [ ] 4.1 Create auth middleware: extract token from header, validate JWT, attach UserContext to request (covers Authenticated Request scenario)
- [ ] 4.2 Update gateway request handler: insert auth middleware first in chain, skip for public endpoints (covers Request Handling Flow, Middleware Chain, Unauthenticated Request, Public Endpoint scenarios)
- [ ] 4.3 Add /auth/register, /auth/login, /auth/refresh, /auth/update-role endpoints with RBAC guards
- [ ] 4.4 Implement opt-in auth: config flag to disable auth, fallback to unauthenticated mode

## 5. CLI Integration

- [ ] 5.1 Add auth commands: zeptoclaw auth login, logout, register (interactive or flags)
- [ ] 5.2 Wrap sensitive CLI commands (e.g., config, channel) with auth guard: check token, refresh if expired
- [ ] 5.3 Add --no-auth flag for backward compatibility on all CLI commands

## 6. Testing and Polish

- [ ] 6.1 Write unit tests for auth functions: hashing, login, token gen/validate
- [ ] 6.2 Write integration tests for endpoints: successful/failed auth flows
- [ ] 6.3 Add logging for auth events (success, failure without details)
- [ ] 6.4 Document new config options and migration steps in README.md
- [ ] 6.5 Benchmark token validation performance, ensure <5ms overhead