## ADDED Requirements

### Requirement: User Registration
The system SHALL allow new users to register with a username and password, storing the hashed password securely.

#### Scenario: Successful Registration
- **WHEN** a new user submits valid username and password via /auth/register
- **THEN** the system creates a new user entry with hashed password and returns a success message

#### Scenario: Duplicate Username
- **WHEN** a user attempts to register with an existing username
- **THEN** the system returns an error indicating the username is taken

### Requirement: User Login
The system SHALL authenticate users by verifying credentials and issuing a JWT access token upon success.

#### Scenario: Successful Login
- **WHEN** a user submits correct username and password via /auth/login
- **THEN** the system validates credentials, issues a JWT token, and returns it along with user role

#### Scenario: Invalid Credentials
- **WHEN** a user submits incorrect username or password
- **THEN** the system returns an authentication failed error without logging the attempt details

### Requirement: Password Hashing
The system SHALL hash all passwords using bcrypt with a cost factor of at least 12 before storage.

#### Scenario: Hash Generation
- **WHEN** a new password is provided during registration
- **THEN** the system generates a bcrypt hash and stores only the hash, not the plain text

### Requirement: JWT Token Generation
The system SHALL generate JWT tokens containing user ID, role, and expiration, signed with a secret key.

#### Scenario: Token Issuance
- **WHEN** login succeeds
- **THEN** the system creates a JWT with claims {user_id, role, exp: now + 1h} and signs it

### Requirement: Token Refresh
The system SHALL support refreshing access tokens using a refresh token, issuing a new access token without re-authentication.

#### Scenario: Successful Refresh
- **WHEN** a valid refresh token is submitted to /auth/refresh
- **THEN** the system validates the refresh token and issues a new access token