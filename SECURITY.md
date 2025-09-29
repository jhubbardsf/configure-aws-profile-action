# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 1.x.x   | :white_check_mark: |

## Security Features

This action implements several security best practices:

- **Input Validation**: All inputs are validated using regex patterns to prevent injection attacks
- **Credential Masking**: AWS account IDs are masked in GitHub Actions logs
- **Environment Cleanup**: All AWS environment variables are cleared after profile creation
- **ARN Validation**: Role ARNs are validated against expected format patterns
- **Latest Dependencies**: Uses the latest version of `aws-actions/configure-aws-credentials`

## Reporting a Vulnerability

Please report security vulnerabilities by creating a private security advisory through GitHub's security tab.

Do not report security vulnerabilities through public GitHub issues.

## Security Considerations

1. **OIDC Only**: This action only supports OIDC authentication, not long-lived access keys
2. **Profile Isolation**: Each profile is isolated and credentials are not shared between profiles
3. **Temporary Credentials**: All credentials are temporary session tokens with limited lifetime
4. **Minimal Permissions**: Configure IAM roles with minimal required permissions (principle of least privilege)

## Best Practices

- Use dedicated IAM roles for GitHub Actions with minimal permissions
- Regularly rotate and review OIDC trust relationships
- Monitor CloudTrail logs for unusual activity from GitHub Actions
- Use branch protection rules to prevent unauthorized changes to workflows