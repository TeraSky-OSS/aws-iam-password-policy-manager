# AWS IAM Password Policy Manager

This project contains a CloudFormation template that sets up an AWS Lambda function, a custom resource, and an optional EventBridge rule to manage the AWS account password policy. The primary function of this tool is to ensure that your desired password policy settings are consistently maintained. Any external modifications or deletions to the password policy are automatically reverted to align with the defined parameters in this CloudFormation stack.

> **Note:** You can also deploy the CloudFormation template as a StackSet, so it'll update the password policy for all/some of the member/linked accounts in the organization.

## Features

- Configurable IAM account password policy settings via CloudFormation parameters.
- Centralized logging of password policy changes via CloudWatch.
- Optional EventBridge rule to monitor and revert any unauthorized changes or deletions to the password policy.
- Least privilege IAM role for Lambda function.

## Requirements

- AWS Account
- AWS CLI (for deployment via CLI)

## Parameters

The CloudFormation template accepts the following parameters to configure the IAM account password policy:

- `AllowUsersToChangePassword`: Allow all IAM users to change their own passwords (Default: true).
- `HardExpiry`: Force users to set a new password after it has expired (Default: true).
- `MaxPasswordAge`: The maximum number of days that an IAM user password is valid (Default: 90).
- `MinimumPasswordLength`: Minimum number of characters allowed in an IAM user password (Default: 12).
- `PasswordReusePrevention`: Prevent IAM users from reusing previous passwords (Default: 5).
- `RequireLowercaseCharacters`: Require lowercase characters in IAM user passwords (Default: true).
- `RequireNumbers`: Require numerical characters in IAM user passwords (Default: true).
- `RequireSymbols`: Require symbols in IAM user passwords (Default: true).
- `RequireUppercaseCharacters`: Require uppercase characters in IAM user passwords (Default: true).
- `EnableRevertOnPolicyChange`: When set to 'true', deploys an EventBridge rule to monitor and revert any unauthorized changes or deletions to the password policy (Default: true).

## Deployment

### AWS Console

1. Log in to the AWS Management Console.
2. Navigate to the CloudFormation service.
3. Create a new stack and upload the CloudFormation template file.
4. Specify the stack parameters to configure the password policy settings as desired.
5. Create the stack to deploy the resources and set the password policy.

### AWS CLI

1. Install and configure the [AWS CLI](https://aws.amazon.com/cli/) or use AWS CloudShell.
2. Use the following command to create the CloudFormation stack:

    ```sh
    aws --region us-east-1 cloudformation create-stack \
      --stack-name iam-password-policy-manager \
      --template-body file://cloudformation_template.yml \
      --capabilities CAPABILITY_NAMED_IAM \
      --parameters \
        ParameterKey=EnableRevertOnPolicyChange,ParameterValue=true \
        ParameterKey=AllowUsersToChangePassword,ParameterValue=true \
        ParameterKey=HardExpiry,ParameterValue=true \
        ParameterKey=MaxPasswordAge,ParameterValue=90 \
        ParameterKey=MinimumPasswordLength,ParameterValue=12 \
        ParameterKey=PasswordReusePrevention,ParameterValue=5 \
        ParameterKey=RequireLowercaseCharacters,ParameterValue=true \
        ParameterKey=RequireNumbers,ParameterValue=true \
        ParameterKey=RequireSymbols,ParameterValue=true \
        ParameterKey=RequireUppercaseCharacters,ParameterValue=true
    ```

3. Replace `iam-password-policy-manager` with your preferred stack name and `cloudformation_template.yml` with the actual path to the CloudFormation template file.
4. Replace the password policy parameter values based on your desired values.
5. Run the command to create the CloudFormation stack and set the password policy based on the specified parameters.

## Usage

Once deployed:

- The IAM account password policy is automatically set based on the parameters specified during stack creation.
- If `EnableRevertOnPolicyChange` is set to `true`, an EventBridge rule will monitor any changes or deletions to the password policy. If an unauthorized change or deletion is detected, the Lambda function will automatically revert the policy back to its desired state.

## Support

For support or issues with this template, please raise an issue in the project repository or contact us.