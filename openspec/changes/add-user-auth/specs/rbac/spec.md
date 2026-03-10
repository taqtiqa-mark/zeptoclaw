## ADDED Requirements

### Requirement: Role Assignment
The system SHALL assign roles to users during registration or via admin update, with default role 'user'.

#### Scenario: Default Role Assignment
- **WHEN** a new user registers
- **THEN** the system assigns the 'user' role unless specified otherwise

#### Scenario: Admin Role Update
- **WHEN** an admin user updates another user's role
- **THEN** the system updates the role if the admin has permission

### Requirement: Permission Checks
The system SHALL enforce permissions based on user role before allowing access to protected operations.

#### Scenario: Authorized Access
- **WHEN** a user with 'admin' role attempts a protected operation like config change
- **THEN** the system grants access and proceeds with the operation

#### Scenario: Unauthorized Access
- **WHEN** a user with 'viewer' role attempts an admin operation
- **THEN** the system denies access and returns a 403 Forbidden error

### Requirement: Role Definitions
The system SHALL define standard roles: 'viewer' (read-only), 'user' (basic operations), 'admin' (full access).

#### Scenario: Role Validation
- **WHEN** checking permissions for an operation
- **THEN** the system maps the operation to required role and compares with user's role