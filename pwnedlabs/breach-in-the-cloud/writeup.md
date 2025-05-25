## Scenario

We've been alerted to a potential security incident. The Huge Logistics security team have provided you with AWS keys of an account that saw unusual activity, as well as AWS CloudTrail logs around the time of the activity. We need your expertise to confirm the breach by analyzing our CloudTrail logs, identifying the compromised AWS service and any data that was exfiltrated.

Access key id: **REDACTED_FOR_LAB**
Secret access key: **REDACTED_FOR_LAB**

Shows this:
![Screenshot](images/pasted_image 20250518211436.png)
Tried to access this bucket:
![Screenshot](images/pasted_image 20250518211819.png)
The user was using AssumedRole to assume as admin:
![Screenshot](images/pasted_image 20250518212624.png)
Used the command to try assuming role and it worked:
aws sts assume-role \
  --role-arn arn:aws:iam::107513503799:role/AdminRole \
  --role-session-name MySession
![Screenshot](images/pasted_image 20250518212759.png)
Configured the profile:
aws configure --profile assumed-admin
aws configure set profile.assumed-admin.aws_session_token "<session_token>"

The command aws s3 ls --profile assumed-admin showed access denied:
![Screenshot](images/pasted_image 20250518212909.png)

But command aws s3 ls s3://emergency-data-recovery --profile assumed-admin provided the list of files:
![Screenshot](images/pasted_image 20250518213016.png)

Downloaded the emergency.txt file with command aws s3 cp s3://emergency-data-recovery/emergency.txt ./ --profile assumed-admin

The file contained the flag and other sensitive info like credentials:
![Screenshot](images/pasted_image 20250518213151.png)