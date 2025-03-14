# Configuration

The Daytona SDK provides flexible configuration options to customize its behavior and connection settings.

## Configuration Options

### Python Configuration

The `DaytonaConfig` class accepts the following parameters:

```python
from daytona_sdk import DaytonaConfig

config = DaytonaConfig(
    api_key="your-api-key",          # Your Daytona API key
    server_url="your-server-url",     # Daytona server URL
    target="local",                   # Target environment
    timeout=30,                       # Request timeout in seconds
    verify_ssl=True                   # SSL verification
)
```

### TypeScript Configuration

The `DaytonaConfig` interface includes these properties:

```typescript
import { DaytonaConfig } from '@daytona/sdk';

const config: DaytonaConfig = {
    apiKey: "your-api-key",          // Your Daytona API key
    serverUrl: "your-server-url",     // Daytona server URL
    target: "local",                  // Target environment
    timeout: 30000,                   // Request timeout in milliseconds
    verifySsl: true                   // SSL verification
};
```

## Environment Variables

The SDK automatically looks for these environment variables:

| Variable | Description | Default |
|----------|-------------|---------|
| `DAYTONA_API_KEY` | Your Daytona API key | None |
| `DAYTONA_SERVER_URL` | URL of your Daytona server | None |
| `DAYTONA_TARGET` | Target environment | "local" |
| `DAYTONA_TIMEOUT` | Request timeout | 30 seconds |
| `DAYTONA_VERIFY_SSL` | SSL verification | true |

### Setting Environment Variables

#### Using a .env File

Create a `.env` file in your project root:

```bash
DAYTONA_API_KEY=your-api-key
DAYTONA_SERVER_URL=https://your-server-url
DAYTONA_TARGET=local
DAYTONA_TIMEOUT=30
DAYTONA_VERIFY_SSL=true
```

#### Using Shell Environment

```bash
# Bash/Zsh
export DAYTONA_API_KEY=your-api-key
export DAYTONA_SERVER_URL=https://your-server-url

# Windows PowerShell
$env:DAYTONA_API_KEY="your-api-key"
$env:DAYTONA_SERVER_URL="https://your-server-url"
```

## Target Environments

The SDK supports different target environments:

| Target | Description |
|--------|-------------|
| `local` | Local development environment |
| `docker` | Docker-based environment |
| `kubernetes` | Kubernetes cluster |
| `aws` | Amazon Web Services |
| `gcp` | Google Cloud Platform |
| `azure` | Microsoft Azure |

## Configuration Precedence

The SDK uses the following precedence order for configuration (highest to lowest):

1. Explicitly passed configuration in code
2. Environment variables
3. Configuration file
4. Default values

## Advanced Configuration

### SSL/TLS Configuration

```python
# Python
config = DaytonaConfig(
    verify_ssl=True,                  # Enable SSL verification
    ssl_cert="/path/to/cert.pem",     # Custom SSL certificate
    ssl_key="/path/to/key.pem"        # Custom SSL key
)
```

```typescript
// TypeScript
const config: DaytonaConfig = {
    verifySsl: true,                  // Enable SSL verification
    sslCert: "/path/to/cert.pem",     // Custom SSL certificate
    sslKey: "/path/to/key.pem"        // Custom SSL key
};
```

### Proxy Configuration

```python
# Python
config = DaytonaConfig(
    proxy="http://proxy.example.com:8080",
    proxy_auth=("username", "password")
)
```

```typescript
// TypeScript
const config: DaytonaConfig = {
    proxy: "http://proxy.example.com:8080",
    proxyAuth: {
        username: "username",
        password: "password"
    }
};
```

### Retry Configuration

```python
# Python
config = DaytonaConfig(
    max_retries=3,                    # Maximum retry attempts
    retry_delay=1,                    # Delay between retries (seconds)
    retry_on_status=[500, 502, 503]   # Status codes to retry on
)
```

```typescript
// TypeScript
const config: DaytonaConfig = {
    maxRetries: 3,                    // Maximum retry attempts
    retryDelay: 1000,                // Delay between retries (milliseconds)
    retryOnStatus: [500, 502, 503]   // Status codes to retry on
};
```

## Best Practices

1. **Environment Variables**: Use environment variables for sensitive information like API keys
2. **SSL Verification**: Keep SSL verification enabled in production
3. **Timeouts**: Set appropriate timeouts based on your use case
4. **Error Handling**: Implement proper error handling for configuration issues
5. **Logging**: Enable logging for debugging configuration problems

## Troubleshooting

### Common Configuration Issues

1. **Invalid API Key**
   ```python
   # Check API key validity
   daytona = Daytona()
   try:
       workspace = daytona.create()
   except AuthenticationError as e:
       print(f"API key invalid: {e}")
   ```

2. **Server Connection Failed**
   ```python
   # Verify server connection
   try:
       daytona = Daytona()
       daytona.health_check()
   except ConnectionError as e:
       print(f"Server connection failed: {e}")
   ```

3. **SSL Certificate Issues**
   ```python
   # Custom SSL configuration
   config = DaytonaConfig(
       verify_ssl=False,  # Disable for testing only
       ssl_cert="custom-cert.pem"
   )
   ```

### Configuration Validation

```python
# Python
from daytona_sdk import validate_config

try:
    validate_config(config)
except ConfigurationError as e:
    print(f"Configuration error: {e}")
```

```typescript
// TypeScript
import { validateConfig } from '@daytona/sdk';

try {
    validateConfig(config);
} catch (e) {
    console.error("Configuration error:", e);
}
``` 