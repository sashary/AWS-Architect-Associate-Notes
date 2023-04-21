# IAM Policy Basics
- an IAM Policy is defined in a JSON Policy document, which includes:
    - A policy-wide information
        - An optional element that displays in the top of the document. 
        - Contains the following elements:
            - `Id`
            - `Version`
                - The version element pertains to the AWS policy language, which is usually a static value of `2012-10-17` as of now.
    - Statements
        - Statements are defined as an array of one or multiple statements.
        - If there are multiple statements, AWS implements a logical OR across the statements
        - Contains the follow elements:
            - `Sid`
                - (Optional) Statement ID - An Identifier to help AWS differentiate between statements
            - `Effect`
                - (Required) "Allow" or "Deny"
            - `Principal`
                - Refers to a particular IAM Identity.
                - The current IAM user or role is implied to be the principal element by default.
                - Constraints:
                    - **Mandatory if** you are creating an identity-based policy.
                    - If you create a resource-based policy, you must indicate the AWS Account, IAM User, IAM Role, or the federal user.
                    - **Cannot** be included if you are creating a IAM permissions policy to attach to a specific user or role.
            - `Action`
                - (Required) A list of action or APIs that the policy allows or denies.
                - Specifies specific actions
                    - Example : `[dynamodb:PutItem, dynamodb:UpdateItem]`
                - You can add a wildcard using a *.
                    - Example : `[dynamodb:*, s3:*]` - will allow all dynamodb and s3 actions
            - `Resource`
                - Similar to the principal element due to certain constraints
                - Constraints:
                    - **Mandatory if** you create an IAM permissions policy, you must specify a list of resources to which the API actions apply.
                    - **Optional if** you create a resource-based policy.
                - Utilizes asterisks and question mark characters. Asterisks represents a combination of multiple characters, while question mark represents a single character
                    - Examples:
                        - `arn:aws:s3:::confidential-data/*` - Affects all objects in from the S3 bucket named confidential-data (referred to as the ARN of the AWS resource).
                        - `arn:aws:s3:us-east-1:123456789012:confidential-data:table/Books` - Specifies a specific table name on the us-east-1 region
                        - 
            - `Condition`
                - (Optional) preliminary rules under which the policy grants permissions (can craft permissions using condition operators)
                    - `"Condition": {"{condition-operator}": {"condition-key": "condition-value"}}`

## Condition Operators
- Condition Operators include categories such as:
    - String
        - StringEquals
        - StringNotEquals
        - etc
    - Numeric
        - NumericEquals
        - NumericEquals
        - etc
    - Boolean
    - ARN
        - ARNEquals
        - ARNLike
        - etc
    - IfExists
    - etc...
- Can attach `IfExists` to all condition operators.
    - StringEqualsIfExists
    - NumericEqualsIfExists
    - IpAddressIfExists
    - ARNEqualsIfExists
    - etc...
- `IfExists` can be used to create fine-grained access to AWS resources.
    - Examples:
        ```
        "Condition": {
            "StringEquals": {
                "s3:x-amz-acl": "bucket-owner-full-control"
                }
            }
        ```
        - This uses the `s3:x-amz:acl` conditional key which refers to a canned ACL (also known as a predefined grants). Can be match with an ACL type of `bucket-owner-full-control` to ensure that the bucket owner is able to access all of the S3 objects.
        ```
        {
            "Sid": "DenyPutAllUsersNotUsingMFA",
            "Effect": "Deny",
            "NotAction": "s3:PutObject*",
            "Resource": "*",
            "Condition": {"BoolIfExists": {"aws:MultiFactorAuthPresent": "false"}}
        }
        ```
        - allows all actions in the s3 bucket except for PUT operation if MFA is not enabled.

## JSON Example
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FirstStatement",
      "Effect": "Allow",
      "Action": ["iam:ChangePassword"],
      "Resource": "*"
    },
    {
      "Sid": "SecondStatement",
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    },
    {
      "Sid": "ThirdStatement",
      "Effect": "Allow",
      "Action": [
        "s3:List*",
        "s3:Get*"
      ],
      "Resource": [
        "arn:aws:s3:::confidential-data",
        "arn:aws:s3:::confidential-data/*"
      ],
      "Condition": {"Bool": {"aws:MultiFactorAuthPresent": "true"}}
    }
  ]
}
```