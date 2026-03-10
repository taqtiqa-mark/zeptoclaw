## MODIFIED Requirements

### Requirement: Request Handling Flow
The gateway SHALL process incoming requests by first validating authentication if required, then routing to the appropriate handler. (Modified to include auth middleware as the first step in the chain.)

#### Scenario: Authenticated Request
- **WHEN** a request arrives with a valid JWT token in Authorization header
- **THEN** the system extracts and validates the token, attaches user context to the request, and proceeds to routing

#### Scenario: Unauthenticated Request to Protected Endpoint
- **WHEN** a request to a protected endpoint lacks a valid token
- **THEN** the system returns 401 Unauthorized immediately, before any further processing

#### Scenario: Public Endpoint Access
- **WHEN** a request to a public endpoint (e.g., health check) arrives without token
- **THEN** the system allows access without authentication, maintaining backward compatibility

### Requirement: Middleware Chain
The gateway SHALL execute middleware in order: auth -> rate-limit -> routing. (Modified to insert auth middleware at the beginning.)

#### Scenario: Full Chain Execution
- **WHEN** a secured request passes through the gateway
- **THEN** auth middleware runs first, followed by others only if auth succeeds