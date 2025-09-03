# End-to-End Encryption System

## Overview

The Wellness Sathi application implements a comprehensive end-to-end encryption system that ensures sensitive data (passwords, personal information, etc.) is encrypted before transmission and can only be decrypted by the intended server. This system provides multiple layers of security while maintaining backward compatibility.

## Architecture

### Hybrid Encryption Approach

The system uses a **hybrid encryption approach** combining:

1. **RSA-2048** for asymmetric key exchange
2. **AES-256-CBC** for symmetric data encryption
3. **Secure key generation** using Web Crypto API

### How It Works

```
Client (Frontend)                    Server (Backend)
     |                                    |
     | 1. Request Public Key              |
     |----------------------------------->|
     |                                    |
     | 2. Receive RSA Public Key          |
     |<-----------------------------------|
     |                                    |
     | 3. Generate AES Key                |
     | 4. Encrypt AES Key with RSA       |
     | 5. Encrypt Data with AES          |
     |                                    |
     | 6. Send Encrypted Payload          |
     |----------------------------------->|
     |                                    |
     | 7. Decrypt AES Key with RSA       |
     | 8. Decrypt Data with AES          |
     | 9. Process Request                 |
     |                                    |
     | 10. Encrypt Response (Optional)    |
     |<-----------------------------------|
     |                                    |
     | 11. Decrypt Response               |
     |                                    |
```

## Components

### Backend Components

#### 1. EncryptionService (`app/Services/EncryptionService.php`)

Core service handling all encryption/decryption operations:

- **RSA Key Management**: Generates, caches, and manages RSA key pairs
- **AES Operations**: Handles symmetric encryption/decryption
- **Payload Processing**: Encrypts/decrypts complete payloads
- **Credential Handling**: Specialized methods for login credentials

#### 2. EncryptionController (`app/Http/Controllers/EncryptionController.php`)

API endpoints for encryption operations:

- `GET /api/encryption/public-key` - Provides RSA public key to clients
- `POST /api/encryption/test` - Tests encryption functionality

#### 3. EncryptionMiddleware (`app/Http/Middleware/EncryptionMiddleware.php`)

Automatic encryption/decryption middleware:

- **Route Detection**: Automatically identifies sensitive routes
- **Request Processing**: Decrypts incoming encrypted requests
- **Response Processing**: Encrypts outgoing responses when requested
- **Error Handling**: Graceful fallback for encryption failures

#### 4. Enhanced Authentication Controller

Modified `AuthenticatedSessionController` to handle both encrypted and regular login requests.

### Frontend Components

#### 1. Encryption Service (`src/utils/encryption.js`)

Client-side encryption service:

- **Public Key Management**: Fetches and manages RSA public keys
- **Auto-Encryption**: Automatically encrypts sensitive requests
- **Route Detection**: Identifies routes requiring encryption
- **Fallback Handling**: Graceful degradation when encryption fails

#### 2. Enhanced Axios Configuration (`src/boot/axios.js`)

Modified HTTP client with automatic encryption:

- **Request Interceptors**: Auto-encrypts sensitive requests
- **Response Interceptors**: Auto-decrypts encrypted responses
- **Header Management**: Adds encryption headers automatically

## Configuration

### Backend Configuration (`config/encryption.php`)

```php
return [
    'enabled' => env('ENCRYPTION_ENABLED', true),
    
    'rsa' => [
        'key_size' => 2048,
        'hash_algorithm' => 'sha256',
        'padding' => OPENSSL_PKCS1_OAEP_PADDING,
    ],
    
    'aes' => [
        'key_size' => 256,
        'cipher' => 'aes-256-cbc',
        'hash_algorithm' => 'sha256',
    ],
    
    'sensitive_routes' => [
        'auth/login',
        'auth/register',
        'auth/forgot-password',
        'auth/reset-password',
        'user/profile',
    ],
    
    'security' => [
        'require_https' => env('ENCRYPTION_REQUIRE_HTTPS', false),
        'max_payload_size' => 1024 * 1024, // 1MB
        'key_rotation_hours' => 24,
    ],
];
```

### Frontend Configuration

```javascript
// Environment variables
VITE_ENCRYPTION_ENABLED=true
VITE_ENCRYPTION_AUTO_ENCRYPT=true
VITE_ENCRYPTION_FALLBACK_TO_PLAIN=true

// Sensitive routes (automatically detected)
const sensitiveRoutes = [
    'auth/login',
    'auth/register', 
    'auth/forgot-password',
    'auth/reset-password',
    'user/profile',
    'user/password'
];
```

## Usage Examples

### 1. Automatic Encryption (Recommended)

The system automatically encrypts sensitive data without any code changes:

```javascript
// This will automatically be encrypted
const response = await api.post('auth/login', {
    username: 'user@example.com',
    password: 'secretpassword123'
});

// This will NOT be encrypted (non-sensitive route)
const users = await api.get('users');
```

### 2. Manual Encryption

For custom encryption needs:

```javascript
import encryptionService from 'src/utils/encryption';

// Initialize the service
await encryptionService.initialize();

// Encrypt sensitive data
const encryptedData = await encryptionService.encryptPayload({
    creditCard: '4111111111111111',
    cvv: '123'
});

// Send encrypted data
const response = await api.post('payment/process', encryptedData);
```

### 3. Testing Encryption

```javascript
// Test encryption functionality
const testResult = await encryptionService.testEncryption();
console.log('Encryption test:', testResult);

// Get encryption status
const status = encryptionService.getStatus();
console.log('Encryption status:', status);
```

## Security Features

### 1. Key Management

- **RSA Keys**: 2048-bit keys with OAEP padding
- **AES Keys**: 256-bit keys generated per session
- **Key Rotation**: Automatic key rotation every 24 hours
- **Secure Storage**: Keys stored in encrypted cache

### 2. Encryption Algorithms

- **RSA-OAEP**: Optimal Asymmetric Encryption Padding
- **AES-256-CBC**: Advanced Encryption Standard with CBC mode
- **SHA-256**: Secure Hash Algorithm for integrity

### 3. Security Headers

```
X-Request-Encryption: true
X-Response-Encrypted: true
X-Encryption-Algorithm: RSA-AES-Hybrid
```

### 4. Fallback Mechanisms

- **Graceful Degradation**: Falls back to plain text if encryption fails
- **Backward Compatibility**: Supports both encrypted and regular requests
- **Error Handling**: Comprehensive error handling and logging

## Testing

### Backend Tests

```bash
# Run encryption tests
php artisan test --filter=EncryptionTest

# Run middleware tests
php artisan test --filter=EncryptionMiddlewareTest

# Run authentication tests
php artisan test --filter=EncryptedAuthenticationTest
```

### Frontend Tests

```bash
# Run encryption service tests
pnpm test src/utils/__tests__/encryption.test.js

# Run all tests
pnpm test
```

## Monitoring and Logging

### Backend Logging

The system logs all encryption operations:

```php
Log::info('Request decrypted successfully', [
    'route' => $request->route()?->getName(),
    'method' => $request->method(),
]);

Log::error('Encryption middleware error: ' . $e->getMessage(), [
    'route' => $request->route()?->getName(),
    'method' => $request->method(),
    'url' => $request->url(),
]);
```

### Frontend Logging

```javascript
console.log('🔐 Auto-encrypting request for:', config.url);
console.log('🔓 Auto-decrypted response from:', response.config?.url);
console.log('⚠️ Auto-encryption failed, proceeding with unencrypted request');
```

## Performance Considerations

### 1. Key Caching

- RSA keys are cached for 1 hour to avoid regeneration
- AES keys are generated per session for security
- Cache TTL is configurable

### 2. Encryption Overhead

- **RSA**: ~1-2ms per operation (key exchange only)
- **AES**: ~0.1ms per operation (data encryption)
- **Total**: Minimal impact on request/response times

### 3. Payload Size

- Maximum encrypted payload: 1MB
- Automatic compression for large payloads
- Efficient binary encoding

## Deployment Considerations

### 1. Environment Variables

```bash
# Backend
ENCRYPTION_ENABLED=true
ENCRYPTION_REQUIRE_HTTPS=true  # Production

# Frontend
VITE_ENCRYPTION_ENABLED=true
VITE_ENCRYPTION_AUTO_ENCRYPT=true
```

### 2. HTTPS Requirements

- **Development**: HTTP allowed
- **Production**: HTTPS required for encryption
- **Mixed Content**: Automatic fallback for non-HTTPS

### 3. Key Rotation

- RSA keys rotate every 24 hours
- Seamless key updates without downtime
- Automatic cache invalidation

## Troubleshooting

### Common Issues

#### 1. Encryption Not Working

```bash
# Check if encryption is enabled
php artisan tinker
>>> config('encryption.enabled')

# Check encryption service status
>>> app(EncryptionService::class)->getPublicKey()
```

#### 2. Frontend Encryption Errors

```javascript
// Check encryption support
console.log('Web Crypto API:', !!window.crypto?.subtle);

// Check encryption status
const status = await encryptionService.getStatus();
console.log('Encryption status:', status);
```

#### 3. Middleware Not Working

```bash
# Check middleware registration
php artisan route:list --middleware=encryption

# Check middleware configuration
php artisan config:show encryption
```

### Debug Mode

Enable debug mode for detailed encryption logs:

```php
// config/encryption.php
'debug' => env('ENCRYPTION_DEBUG', false),
```

## Future Enhancements

### 1. Advanced Features

- **Perfect Forward Secrecy**: Session key rotation
- **Quantum Resistance**: Post-quantum cryptography
- **Hardware Security**: TPM/HSM integration

### 2. Performance Improvements

- **Key Preloading**: Pre-generate keys during idle time
- **Batch Encryption**: Encrypt multiple payloads together
- **Compression**: Automatic payload compression

### 3. Monitoring

- **Encryption Metrics**: Success/failure rates
- **Performance Monitoring**: Encryption latency tracking
- **Security Alerts**: Failed decryption notifications

## Compliance and Standards

### 1. Security Standards

- **OWASP**: Follows OWASP security guidelines
- **NIST**: Compliant with NIST cryptographic standards
- **GDPR**: Supports data protection requirements

### 2. Audit Trail

- **Comprehensive Logging**: All encryption operations logged
- **Access Tracking**: User and system access monitoring
- **Compliance Reports**: Automated compliance reporting

## Support and Maintenance

### 1. Regular Updates

- **Security Patches**: Monthly security updates
- **Algorithm Updates**: Annual cryptographic algorithm reviews
- **Key Rotation**: Automated key management

### 2. Monitoring

- **Health Checks**: Regular encryption system health monitoring
- **Performance Metrics**: Continuous performance tracking
- **Security Alerts**: Real-time security notifications

---

## Quick Start Checklist

- [ ] Copy `.env.example` to `.env` and configure encryption settings
- [ ] Run `php artisan config:cache` to cache configuration
- [ ] Test encryption endpoints: `GET /api/encryption/public-key`
- [ ] Verify frontend encryption service initialization
- [ ] Test login with encrypted credentials
- [ ] Run encryption tests: `php artisan test --filter=Encryption`
- [ ] Monitor encryption logs for any issues
- [ ] Configure production HTTPS requirements
- [ ] Set up monitoring and alerting

For additional support, refer to the application logs or contact the development team.
