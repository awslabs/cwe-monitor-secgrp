# cwe-monitor-secgrp
This CloudWatch Events rule Lambda function is used to look at invocations of the authorize_security_group_ingress() and revoke_security_group_ingress() API calls.  When such as call is made, the function examines the contents of the applicable security group to determine if it matches a pre-configured policy.  The policy is hardcoded into the Lambda function using an IpPermisions structure which is in the same format as the IpPermissions structure used by the describe_security_groups() API call.  If the security group permissions do not match the pre-configured policy, the function sends log messages to CloudWatch logs saying what permissions need to be added or revoked in order to bring the group ingress rules into compliance.

In order to select the appropriate API call and security group for the CloudWatch events rule, you should use a JSON event selector.  An example of such a selector appears below.  Substitute "sg-abc12345" with the id of the specific security group you wish to inspect.

```
{
  "detail-type": [
    "AWS API Call via CloudTrail"
  ],
  "detail": {
    "eventSource": [
      "ec2.amazonaws.com"
    ],
    "eventName": [
      "AuthorizeSecurityGroupIngress",
      "RevokeSecurityGroupIngress"
    ],
    "requestParameters": {
      "groupId": [
        "sg-abc12345"
      ]
    }
  }
}
```
