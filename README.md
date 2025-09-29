# Configure AWS Credential Profiles for GitHub Actions

This action uses the official `aws-actions/configure-aws-credentials@v5` action and only supports assuming roles via OIDC.

The official action is not sufficient for multiple account usage as it can only set one set of AWS environment variables at a time.

> The primary reason this action exists is to address using multiple profiles at the same time. Region defaults to `us-west-2`.

## ✨ Features

- **Input Validation**: Validates profile names and role ARNs before AWS API calls
- **Security First**: Account ID masking and comprehensive credential cleanup
- **Multi-Profile Support**: Configure multiple AWS profiles in a single workflow
- **Error Prevention**: Fail-fast validation with clear error messages

## Usage

```yaml
jobs:
  test_new_action:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Repo
      uses: actions/checkout@v3

    - uses: mcblair/configure-aws-profile-action@v1.0.0
      with:
        role-arn: arn:aws:iam::<ACCOUNT_ID>:role/<ROLE_NAME>
        profile-name: test

    - uses: mcblair/configure-aws-profile-action@v1.0.0
      with:
        role-arn: arn:aws:iam::<ACCOUNT_ID>:role/<ROLE_NAME>
        profile-name: production
        region: us-east-2

    - run: aws s3 ls --profile test

    - run: aws s3 ls --profile production
```

## Inputs

| Input | Required | Description | Default |
|-------|----------|-------------|---------|
| `role-arn` | Yes | ARN of the IAM role to assume | - |
| `profile-name` | Yes | Name of the AWS profile to create. Allowed characters: letters, numbers, ., _, - | - |
| `region` | No | AWS region to use | `us-west-2` |

## Error Handling

This action validates inputs before making AWS API calls:

- **Invalid profile names** are rejected with clear error messages
- **Malformed role ARNs** are caught during preflight validation
- **Empty required fields** fail immediately with descriptive errors

This prevents workflows from failing after AWS authentication, saving time and providing clearer feedback.
