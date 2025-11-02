# lib-maia-passkey

Clean WebAuthn/passkey service for MAIA (Medical AI Assistant).

## Purpose

Extract WebAuthn registration and authentication logic from `maia-cloud-clean/src/routes/passkey-routes.js` into a simple, reusable service with configurable rpID and origin.

## Features

- **Registration flow**: generateRegistrationOptions, verifyRegistration
- **Authentication flow**: generateAuthenticationOptions, verifyAuthentication
- **Configurable**: rpID and origin via constructor options
- **Simple API**: Promise-based functions with clear error handling

## Installation

### Development (symlink)
```bash
npm link
```

### Production (npm)
```bash
npm install lib-maia-passkey
```

## Usage

```javascript
import { PasskeyService } from 'lib-maia-passkey';

// Initialize with config
const passkeyService = new PasskeyService({
  rpID: 'user.agropper.xyz',
  origin: 'https://user.agropper.xyz'
});

// Generate registration options
const options = await passkeyService.generateRegistrationOptions({
  userId: 'user123',
  displayName: 'User Name'
});

// Verify registration
const result = await passkeyService.verifyRegistration({
  userId: 'user123',
  response: credential
});

// Generate authentication options
const authOptions = await passkeyService.generateAuthenticationOptions({
  userId: 'user123'
});

// Verify authentication
const authResult = await passkeyService.verifyAuthentication({
  userId: 'user123',
  response: assertion
});
```

## Development

Extract from:
- `maia-cloud-clean/src/routes/passkey-routes.js`
- No dependencies on cache or session management

## Configuration

Passkey configuration is injectable:
- `rpID`: Relying party ID (e.g., 'user.agropper.xyz')
- `origin`: Allowed origin (e.g., 'https://user.agropper.xyz')

Different apps can use different rpIDs/origins by instantiating with different configs.

## Features

- **Platform authenticators**: Prefers TouchID/FaceID over cross-platform passkeys
- **Registration flow**: Generate and verify passkey registration
- **Authentication flow**: Generate and verify passkey authentication
- **Counter tracking**: Automatic credential counter updates for security
- **Error handling**: Clear error messages for common failure scenarios

## API Reference

```javascript
import { PasskeyService } from 'lib-maia-passkey';

const passkeyService = new PasskeyService({
  rpID: 'user.agropper.xyz',
  origin: 'https://user.agropper.xyz'
});

// Registration
const regOptions = await passkeyService.generateRegistrationOptions({
  userId: 'user123',
  displayName: 'User Name'
});

const regResult = await passkeyService.verifyRegistration({
  response: credentialResponse,
  expectedChallenge: regOptions.challenge,
  userDoc: existingUserDoc
});

// Authentication
const authOptions = await passkeyService.generateAuthenticationOptions({
  userId: 'user123',
  userDoc: userDoc
});

const authResult = await passkeyService.verifyAuthentication({
  response: assertionResponse,
  expectedChallenge: authOptions.challenge,
  userDoc: userDoc
});
```

## Status

✅ **Complete** - Fully implemented and tested

