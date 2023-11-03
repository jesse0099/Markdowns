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
    - Open the provided switch role URL, that will be in the next format: `[Switch Role](https://signin.aws.amazon.com/switchrole?roleName=YOUR_ROLE_NAME&account=YOUR_ACCOUNT_NAME)`.
    - _Example_: [https://signin.aws.amazon.com/switchrole?roleName=user-cross-account-s3-role&account=getbuyside](https://signin.aws.amazon.com/switchrole?roleName=user-cross-account-s3-role&account=getbuyside).
    - AWS will prompt you to sign in if you haven't.
    - Confirm the switch, and you will be switched to the role provided by the external account.

> **Note**: After switching roles successfully, you'll see a chip in the top right corner of the web console displaying the name of the assumed role. This verifies that you have switched roles correctly.

1. **Accessing the S3 Bucket**:
    - After switching roles, navigate to the provided S3 bucket URL in the format: `[S3 Bucket URL](https://s3.console.aws.amazon.com/s3/buckets/YOUR_BUCKET_NAME?)`.
    - _Example_: [https://s3.console.aws.amazon.com/s3/buckets/user-prod-s3](https://s3.console.aws.amazon.com/s3/buckets/user-prod-s3).
    - You can now view and manage the objects within the bucket, based on permissions granted.

### Programatic access:

#### For CLI use only: 

1. **Setting Up AWS CLI**:
    - If you haven't set up the AWS CLI yet, follow [this guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) to install and configure it.
    
> **Note:** This requires using a method to authenticate the CLI. For workloads, static key sets are a preferred approach, but for the workforce, configure IAM Identity Center (Successor to single sign-on) instead.

***Access Keys Setup:***

Once an administrator provides you with a set of access keys, you should configure your CLI to use them. This can be achieved by executing `aws configure` or directly editing the credentials file located at `~/.aws/credentials` (~ is used to refer to the home directory, use your OS's supported notation).

When prompted for the preferred region and output format, select `us-east-1` for the `region` and json for the format.

The ***`~/.aws/credentials`*** file should have entries in the following format:

```
[default]
aws_access_key_id=**********************
aws_secret_access_key=******************
```
> **Note:** The name inside the [] is arbitrary and is a way to name your key sets

**Once you've confirmed that the credentials are in place, edit the** ***`~/.aws/config`***. It should look like this:

```
[default]
region = us-east-1
output = json 
```

**Now, we need to add the next entry:**

```credentials
[profile chalk-s3]
role_arn = arn:aws:iam::PercyAccount:role/PercyRole
source_profile = default
```

*The* ***`~/.aws/config`*** **should look like this at the end**:

```credentials
[default]
region = us-east-1
output = json 

[profile chalk-s3]
role_arn = arn:aws:iam::PercyAccount:role/PercyRole
source_profile = default
```

> **Note**: Remember to change the **role_arn** value with the one with provided for you.

Once this is done, you can call AWS CLI commands with the `--profile` flag to assume the role.

`aws s3 ls s3-bucket-name --profile chalk-s3`

> **Note**: Substitute `s3-bucket-name` with the name of the bucket provided to you. 

> **Note**: Not including the `--profile {value}` flag will execute CLI commands using your own credentials, essentially rejecting actions over our bucket.
`An error occurred (AccessDenied) when calling the ListObjectsV2 operation: Access Denied`

***IAM Identity Center:***

#### For CLI and third party tools use:

1. **Download the Script**:
    
	- Download the provided script for assuming the role from these gists: [PowerShell](https://gist.github.com/jesse0099/d4f5399e68c1459057b09500724b561d), [Bash](https://gist.github.com/jesse0099/fafcfbee431bd69bef71d8a57fc9586e). Save the script on your local machine.

2. **Assuming Role using the Script**:

    The script's assume role functionality will return a set of temporary credentials that can be used with third-party tools or for CLI usage (it actually sets the CLI credentials at the environment level with the returned values; reset the credentials when you want to use your own keys).

    - **PowerShell**:
        - Open PowerShell Core and navigate to the location of the downloaded script.
        - To assume a role, execute the script by running the command:
          ```powershell
          ./script_name.ps1 -role_arn "arn:aws:iam::AccountA:role/AccountARole" -session_name "MySessionName"
          ```
        - To reset credentials, run:
          ```powershell
          ./script_name.ps1 -reset_credentials
          ```

    - **Bash**:
        - Open Terminal and navigate to the location of the downloaded script.
        - Make the script executable with the command:
          ```bash
          chmod +x script_name.sh
          ```
        - To assume a role, execute the script by running the command:
          ```bash
          ./script_name.sh --assume_role "arn:aws:iam::AccountA:role/AccountARole" "MySessionName"
          ```
        - To reset credentials, run:
          ```bash
          ./script_name.sh --reset_credentials
          ```
> **Note:** If environment variables need to be available after the Bash script execution ends, consider sourcing it instead of just executing it. If you source it for assuming a role, remember to source it again for resetting credentials.

**Example:**

```bash
# Assuming Role
source ./script_name.sh --assume_role "arn:aws:iam::AccountA:role/AccountARole" "MySessionName"

# Resetting **Credentials**
source ./script_name.sh --reset_credentials
```
 
> **Note**: substitute **"arn:aws:iam::AccountA:role/AccountARole"**  with the role arn provided to you.

> **Note:**: substitute **"MySession"** an arbitrary string to identify tour session.

> **Note**: After assuming the role through CLI, you can verify your current identity by running the command `aws sts get-caller-identity`. If successful, this will display the ARN of the assumed role.

1. **Using S3 Clients with Temporary Credentials**:
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