# Amazon Bedrock AgentCore Identity L2 Construct

* **Original Author(s):**: @krokoko , @aws-rafams , @dineshSajwan
* **Tracking Issue**: #787
* **API Bar Raiser**: @alvazjor,

The Amazon Bedrock AgentCore Identity L2 constructs simplify the creation and management of credential providers for AI agents by wrapping the AgentCore Identity L1 constructs.
It provides high-level, object-oriented approaches to creating and managing API Key and OAuth identity providers.
These providers enable secure authentication for gateway targets and external services.

A quick comparison between L1 and L2 Identity constructs:

1. Quick and easy creation of Identity resources:
   - API Key credential providers with secure storage
   - OAuth 2.0 providers for major platforms (Google, GitHub, Slack, etc.)
   - Custom OAuth provider support

2. Simplified infrastructure management:
   - Automatic AWS Secrets Manager integration
   - Automatic IAM role and policy management
   - Token vault management

3. Helper methods for better developer experience:
   - Pre-configured OAuth providers for major platforms
   - Import methods for existing identities
   - Integration with Gateway credential configurations

4. Validation and error handling:
   - Compile-time configuration validation
   - User-friendly error messages
   - Automatic dependency management

**CHANGELOG**:
```feat(bedrock-agentcore): Amazon Bedrock AgentCore Identity L2 construct```

**README**:
[Amazon Bedrock AgentCore Identity](https://aws.amazon.com/bedrock/agentcore/) provides secure credential management for AI agents to access external services.
With Amazon Bedrock AgentCore Identity, developers can manage API keys and OAuth credentials securely while maintaining enterprise-grade security.

This construct library facilitates the deployment of AgentCore API Key and OAuth credential providers.
It leverages underlying CloudFormation L1 resources and custom resources to provision these AgentCore Identity features.

For more details please refer here [Amazon Bedrock AgentCore Identity Documentation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html).

## API Key Credential Provider

The `ApiKeyIdentity` construct enables you to create API key credential providers for Amazon Bedrock AgentCore.
This construct automatically handles the secure storage of API keys in AWS Secrets Manager and provides a clean interface for managing API key-based authentication.

### Creating an API Key Identity

```typescript
import { ApiKeyIdentity } from "aws-cdk/bedrock-agentcore-alpha/identity/api-key-identity";

// Create a new API key identity
const apiKeyIdentity = new ApiKeyIdentity(this, "MyApiKeyIdentity", {
  name: "my-api-key-provider",
  apiKey: "your-api-key-here",
});
```

### Importing an Existing API Key Identity

```typescript
import { ApiKeyIdentity } from "aws-cdk/bedrock-agentcore-alpha/identity/api-key-identity";

// Import an existing API key identity
const importedIdentity = ApiKeyIdentity.fromApiKeyIdentityAttributes(
  this,
  "ImportedApiKeyIdentity",
  {
    name: "existing-api-key-provider",
    credentialProviderArn:
      "arn:aws:bedrock-agentcore:us-east-1:123456789012:token-vault/my-vault/apikeycredentialprovider/existing-provider",
    apiKeySecretArn:
      "arn:aws:secretsmanager:us-east-1:123456789012:secret:bedrock-agentcore-api-key-secret",
  }
);
```

### Using with Gateway Credential Provider Configuration

```typescript
import { ApiKeyIdentity } from "aws-cdk/bedrock-agentcore-alpha/identity/api-key-identity";
import { ApiKeyCredentialProviderConfiguration } from "aws-cdk/bedrock-agentcore-alpha/gateway/credential-provider";

// Create API key identity
const apiKeyIdentity = new ApiKeyIdentity(this, "MyApiKeyIdentity", {
  name: "my-api-key-provider",
  apiKey: "your-api-key-here",
});

// Use with gateway credential provider configuration
const credentialProviderConfig = new ApiKeyCredentialProviderConfiguration({
  provider: apiKeyIdentity,
  credentialLocation: "HEADER",
  credentialParameterName: "X-API-Key",
});
```

### API Key Identity Properties

| Property                | Type                   | Description                                  |
| ----------------------- | ---------------------- | -------------------------------------------- |
| `name`                  | `string`               | The name of the API key credential provider  |
| `apiKey`                | `string`               | The API key value to be stored securely      |
| `credentialProviderArn` | `string`               | The ARN of the created credential provider   |
| `apiKeySecretArn`       | `string`               | The ARN of the secret containing the API key |
| `identityType`          | `IdentityType.API_KEY` | The type of identity (always API_KEY)        |

## OAuth 2.0 Credential Provider

The `OAuthIdentity` construct enables you to create OAuth2 credential providers for Amazon Bedrock AgentCore.
This construct supports multiple OAuth2 providers including Google, GitHub, Slack, Salesforce, Microsoft, and custom OAuth2 providers.

### Creating a Standard OAuth Identity

```typescript
import { OAuthIdentity } from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-identity";
import { GoogleOauth2Config } from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-provider";

// Create a Google OAuth2 identity
const googleOAuthIdentity = new OAuthIdentity(this, "MyGoogleOAuthIdentity", {
  name: "my-google-oauth-provider",
  oauthProvider: new GoogleOauth2Config({
    clientId: "your-google-client-id",
    clientSecret: "your-google-client-secret",
  }),
});
```

### Creating GitHub OAuth Identity

```typescript
import { OAuthIdentity } from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-identity";
import { GithubOauth2Config } from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-provider";

const githubOAuthIdentity = new OAuthIdentity(this, "MyGithubOAuthIdentity", {
  name: "my-github-oauth-provider",
  oauthProvider: new GithubOauth2Config({
    clientId: "your-github-client-id",
    clientSecret: "your-github-client-secret",
  }),
});
```

### Creating Other Platform OAuth Identities

```typescript
import { OAuthIdentity } from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-identity";
import {
  SlackOauth2Config,
  SalesforceOauth2Config,
  MicrosoftOauth2Config,
} from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-provider";

// Slack OAuth2 identity
const slackOAuthIdentity = new OAuthIdentity(this, "MySlackOAuthIdentity", {
  name: "my-slack-oauth-provider",
  oauthProvider: new SlackOauth2Config({
    clientId: "your-slack-client-id",
    clientSecret: "your-slack-client-secret",
  }),
});

// Salesforce OAuth2 identity
const salesforceOAuthIdentity = new OAuthIdentity(this, "MySalesforceOAuthIdentity", {
  name: "my-salesforce-oauth-provider",
  oauthProvider: new SalesforceOauth2Config({
    clientId: "your-salesforce-client-id",
    clientSecret: "your-salesforce-client-secret",
  }),
});

// Microsoft OAuth2 identity
const microsoftOAuthIdentity = new OAuthIdentity(this, "MyMicrosoftOAuthIdentity", {
  name: "my-microsoft-oauth-provider",
  oauthProvider: new MicrosoftOauth2Config({
    clientId: "your-microsoft-client-id",
    clientSecret: "your-microsoft-client-secret",
  }),
});
```

### Creating a Custom OAuth Identity

```typescript
import { OAuthIdentity } from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-identity";
import { CustomOauth2Config } from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-provider";

// Create a custom OAuth2 identity with authorization server metadata
const customOAuthIdentity = new OAuthIdentity(this, "MyCustomOAuthIdentity", {
  name: "my-custom-oauth-provider",
  oauthProvider: new CustomOauth2Config({
    clientId: "your-custom-client-id",
    clientSecret: "your-custom-client-secret",
    oauthDiscovery: {
      authorizationServerMetadata: {
        authorizationEndpoint: "https://your-auth-server.com/oauth/authorize",
        issuer: "https://your-auth-server.com",
        tokenEndpoint: "https://your-auth-server.com/oauth/token",
        responseTypes: ["code", "token"],
      },
    },
  }),
});

// Create a custom OAuth2 identity with discovery URL
const customOAuthWithDiscoveryUrl = new OAuthIdentity(this, "MyCustomOAuthWithDiscovery", {
  name: "my-custom-oauth-discovery-provider",
  oauthProvider: new CustomOauth2Config({
    clientId: "your-custom-client-id",
    clientSecret: "your-custom-client-secret",
    oauthDiscovery: {
      discoveryUrl: "https://your-auth-server.com/.well-known/openid-configuration",
    },
  }),
});
```

### Importing an Existing OAuth Identity

```typescript
import { OAuthIdentity } from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-identity";

// Import an existing OAuth identity
const importedOAuthIdentity = OAuthIdentity.fromOAuthIdentityAttributes(
  this,
  "ImportedOAuthIdentity",
  {
    name: "existing-oauth-provider",
    credentialProviderArn:
      "arn:aws:bedrock-agentcore:us-east-1:123456789012:token-vault/my-vault/oauth2credentialprovider/existing-provider",
    clientSecretArn:
      "arn:aws:secretsmanager:us-east-1:123456789012:secret:bedrock-agentcore-oauth-secret",
  }
);
```

### Using OAuth Identity with Gateway Credential Provider Configuration

```typescript
import { OAuthIdentity } from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-identity";
import { GoogleOauth2Config } from "aws-cdk/bedrock-agentcore-alpha/identity/oauth-provider";
import { OAuthCredentialProviderConfiguration } from "aws-cdk/bedrock-agentcore-alpha/gateway/credential-provider";

// Create OAuth identity
const oauthIdentity = new OAuthIdentity(this, "MyOAuthIdentity", {
  name: "my-oauth-provider",
  oauthProvider: new GoogleOauth2Config({
    clientId: "your-google-client-id",
    clientSecret: "your-google-client-secret",
  }),
});

// Use with gateway credential provider configuration
const oauthCredentialProviderConfig = new OAuthCredentialProviderConfiguration({
  provider: oauthIdentity,
  scopes: ["read", "write"],
  customParameters: {
    audience: "https://api.example.com",
    response_type: "code",
  },
});
```

### Available OAuth Provider Types

The construct supports the following OAuth2 provider types:

- **GoogleOauth2Config**: Google OAuth2 provider
- **GithubOauth2Config**: GitHub OAuth2 provider
- **SlackOauth2Config**: Slack OAuth2 provider
- **SalesforceOauth2Config**: Salesforce OAuth2 provider
- **MicrosoftOauth2Config**: Microsoft OAuth2 provider
- **CustomOauth2Config**: Custom OAuth2 provider with configurable discovery

### OAuth Identity Properties

| Property                | Type                  | Description                                                                      |
| ----------------------- | --------------------- | -------------------------------------------------------------------------------- |
| `name`                  | `string`              | The name of the OAuth2 credential provider                                       |
| `oauthProvider`         | `OAuthProviderConfig` | The OAuth2 provider configuration (GoogleOauth2Config, GithubOauth2Config, etc.) |
| `credentialProviderArn` | `string`              | The ARN of the created credential provider                                       |
| `clientSecretArn`       | `string`              | The ARN of the secret containing the client secret                               |
| `identityType`          | `IdentityType.OAUTH`  | The type of identity (always OAUTH)                                              |

### Custom OAuth2 Configuration

For custom OAuth2 providers, you can specify either:

**Authorization Server Metadata:**

```typescript
oauthDiscovery: {
  authorizationServerMetadata: {
    authorizationEndpoint: 'https://your-auth-server.com/oauth/authorize',
    issuer: 'https://your-auth-server.com',
    tokenEndpoint: 'https://your-auth-server.com/oauth/token',
    responseTypes: ['code', 'token'], // Optional
  },
}
```

**Discovery URL:**

```typescript
oauthDiscovery: {
  discoveryUrl: 'https://your-auth-server.com/.well-known/openid-configuration',
}
```

---

Ticking the box below indicates that the public API of this RFC has been
signed-off by the API bar raiser (the `status/api-approved` label was applied to the
RFC pull request):

```
[ ] Signed-off by API Bar Raiser @xxxxx
```

## Public FAQ

### What are we launching today?

We are excited to announce the launch of our new L2 constructs for Amazon Bedrock AgentCore Identity.
These construct libraries provide high-level abstractions for managing credentials that AI agents use to access external services.
Key features include:

- **API Key Management**: Secure storage and management of API keys
- **OAuth 2.0 Support**: Pre-configured providers for major platforms
- **Custom OAuth**: Support for any OAuth 2.0 compatible service
- **Secure Storage**: Automatic integration with AWS Secrets Manager
- **Token Vault**: Managed token storage and lifecycle

### Why should I use this feature?

The Amazon Bedrock AgentCore Identity L2 constructs offer several compelling advantages:

1. **Simplified Credential Management**: Handle API keys and OAuth tokens easily
2. **Security First**: Automatic encryption and secure storage in AWS Secrets Manager
3. **Pre-configured Providers**: Ready-to-use configurations for Google, GitHub, Slack, etc.
4. **Flexible Integration**: Works seamlessly with Gateway targets
5. **Compliance Ready**: Enterprise-grade security and audit capabilities
6. **Cost Effective**: Pay only for the credentials you store

These L2 constructs eliminate the complexity of implementing secure credential management.
Developers can focus on building agent applications without worrying about credential security.

## Internal FAQ

### Why are we doing this?

Amazon Bedrock AgentCore Identity addresses critical needs for AI agents:

1. **External Service Access**: Agents need credentials to access external APIs
2. **Security Requirements**: Credentials must be stored and managed securely
3. **OAuth Complexity**: OAuth 2.0 flows are complex to implement correctly
4. **Compliance Needs**: Enterprises require audit trails for credential usage

The L2 constructs address these challenges by providing:

- Secure credential storage in AWS Secrets Manager
- Pre-configured OAuth providers
- Automatic token management
- Built-in audit capabilities

### Why should we _not_ do this?

Potential concerns to consider:

1. **Service Maturity**: AgentCore is in preview and subject to changes
2. **Custom Resource Dependency**: Currently relies on custom resources
3. **Limited OAuth Flows**: Only supports 2-legged OAuth (2LO)

However, these concerns are mitigated by:

- Clear migration path when CloudFormation support becomes available
- 2LO covers most service-to-service authentication needs
- Comprehensive security controls

### What is the technical solution (design) of this feature?

The L2 construct library is built using:

1. **TypeScript with Projen**: Modern tooling for construct development
2. **JSII**: Multi-language support (TypeScript, Python, Java, .NET)
3. **Custom Resources**: Temporary solution using Lambda functions
4. **Modular Architecture**: API Key and OAuth as separate constructs

Key design principles:

- **Security by Default**: Automatic encryption and secure storage
- **Sensible Defaults**: Pre-configured OAuth providers
- **Extensibility**: Support for custom OAuth providers
- **Type Safety**: Strong typing for better developer experience

## Interfaces

The construct library provides comprehensive interfaces for Identity services:

### Identity Interfaces

- [ApiKeyIdentityProps](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#apikeyidentityprops)
- [OAuthIdentityProps](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#oauthidentityprops)
- [OAuthProviderConfig](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#oauthproviderconfig)
- [OAuthDiscoveryConfig](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#oauthdiscoveryconfig)

## Classes

### Core Classes

- [ApiKeyIdentity](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#apikeyidentity)
- [OAuthIdentity](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#oauthidentity)

### Provider Configuration Classes

- [GoogleOauth2Config](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#googleoauth2config)
- [GithubOauth2Config](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#githuboauth2config)
- [SlackOauth2Config](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#slackoauth2config)
- [SalesforceOauth2Config](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#salesforceoauth2config)
- [MicrosoftOauth2Config](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#microsoftoauth2config)
- [CustomOauth2Config](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#customoauth2config)

## Enumerations

- [IdentityType](https://github.com/krokoko/agent-core-cdk-constructs/blob/main/API.md#identitytype)

### Is this a breaking change?

No. This is a new construct library for Amazon Bedrock AgentCore Identity.
It does not affect existing constructs.

### What alternative solutions did you consider?

1. Storing credentials in Parameter Store - lacks OAuth support
2. Using Cognito for all authentication - doesn't support API keys
3. Building custom credential management - requires significant development

### What are the drawbacks of this solution?

1. Relies on Lambda-based custom resources until CloudFormation support is available
2. AgentCore is in preview and APIs may change
3. Only supports 2-legged OAuth flows

### What is the high-level project plan?

**Phase 1: RFC**:

- Submit RFC proposal for creating the AgentCore Identity L2 constructs
- Design the initial interface and helper methods
- Monitor the release of L1 constructs

**Phase 2: Development**:

- Create API Key and OAuth Identity modules
- Add pre-configured OAuth providers
- Create comprehensive unit tests
- Write comprehensive API documentation

**Phase 3: Post-Launch**:

- Publish launch blog and announcement posts
- Regular updates to track AgentCore service changes
- Add support for additional OAuth providers

### Are there any open issues that need to be addressed later?

1. Waiting for the release of L1 construct for Bedrock AgentCore Identity
2. Replace all custom resources with L1 constructs
3. Consider adding support for 3-legged OAuth flows in the future
