## Accessing an S3 Bucket Using an External AWS Account

### Prerequisites:
- An active AWS user account. If you do not have one, follow [this link](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/) to create and activate your account.

> **Note**: Ensure that your AWS user  has the necessary permission policy attached, which allows `sts:AssumeRole` on the specific role ARN of our account (We will provide it). 

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "sts:AssumeRole",
    "Resource": "arn:aws:iam::PercyAccount:role/PercyRole"
  }
}
```

### Using the Web Console:

1. **Switching Role**:
    - Open the provided switch role URL in the format: `[Switch Role](https://signin.aws.amazon.com/switchrole?roleName=YOUR_ROLE_NAME&account=YOUR_ACCOUNT_NAME)`.
    - _Example_: [https://signin.aws.amazon.com/switchrole?roleName=user-cross-account-s3-role&account=getbuyside]().
    - AWS will prompt you to sign in if you haven't.
    - Confirm the switch, and you will be switched to the role provided by the external account.

> **Note**: After switching roles successfully, you'll see a chip in the top right corner of the web console displaying the name of the assumed role. This verifies that you have switched roles correctly.

2. **Accessing the S3 Bucket**:
    - After switching roles, navigate to the provided S3 bucket URL in the format: `[S3 Bucket URL](https://s3.console.aws.amazon.com/s3/buckets/YOUR_BUCKET_NAME?)`.
    - _Example_: [https://s3.console.aws.amazon.com/s3/buckets/user-prod-s3]().
    - You can now view and manage the objects within the bucket, based on permissions granted.

### Programatic access:

#### For CLI use only: 

1. **Setting Up AWS CLI**:
    - If you haven't set up the AWS CLI yet, follow [this guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) to install and configure it.

AWS provides a 

#### For CLI and third party tools use:

1. **Download the Script**:
    
	- Download the provided script for assuming the role from these gists: [PowerShell](https://gist.github.com/jesse0099/d4f5399e68c1459057b09500724b561d), [Bash](https://gist.github.com/jesse0099/fafcfbee431bd69bef71d8a57fc9586e). Save the script on your local machine.

2. **Assuming Role using the Script**:

    - **PowerShell**:
        - Open PowerShell Core and navigate to the location of the downloaded script.
        - To assume a role, execute the script by running the command:
          ```powershell
          ./script_name.ps1 -role_arn "arn:aws:iam::AccountA:role/AccountARole" -session_name "MySessionName"
          ```
        - To reset credentials, run:
          ```powershell
          ./script_name.ps1 --reset_credentials
          ```

    - **Bash**:
        - Open Terminal and navigate to the location of the downloaded script.
        - Make the script executable with the command:
          ```bash
          chmod +x script_name.sh
          ```
        - To assume a role, execute the script by running the command:
          ```bash
          ./script_name.sh "arn:aws:iam::AccountA:role/AccountARole" "MySessionName"
          ```
        - To reset credentials, run:
          ```bash
          ./script_name.sh --reset_credentials
          ```

> **Note**: After assuming the role through CLI, you can verify your current identity by running the command `aws sts get-caller-identity`. If successful, this will display the ARN of the assumed role.

3. **Using S3 Clients with Temporary Credentials**:
    For tools like WinSCP or Filezilla, they require AWS access key, secret key, and a session token to access S3.

    - **WinSCP**:
        1. Open WinSCP.
        2. Choose "Amazon S3" protocol.
        3. Enter the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` into the respective fields.
        4. Click on "Advanced" and navigate to "Environment -> AWS".
        5. Enter the `AWS_SESSION_TOKEN`.
        6. Connect to your S3 bucket.

    - **Filezilla**:
        1. Open Filezilla.
        2. Go to "File -> Site Manager".
        3. Choose "New Site" and set Protocol to "Amazon S3".
        4. Enter the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` into the respective fields.
        5. In "Comments" section, input the `AWS_SESSION_TOKEN`.
        6. Connect to your S3 bucket.



For a deeper dive into AWS documentation on assuming roles across accounts and using the AWS CLI, refer to the official [AWS documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html).