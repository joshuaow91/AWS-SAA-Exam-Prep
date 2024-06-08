# IAM

Identity and access management - users and groups
Root user shouldnt be used for services
Create user in iam, represents one user within org
Iam is a global service  

Policy inheritance - user will inherit from any group it belongs to
Policy structure 
- Version
- Id
- Statement
	- Sid
	- Effect
	- Principal 
		- Aws
	- Action
	- Resource
#### Password policy 

- You can set up min pas length, types for chars
- Allow or not for users to change pass or require pass change over duration of time set
- Prevent pass re-use
- MFA
	- Protect root and iam with MFA - password + security device
	- If a pass is stolen or hacked, account is not compromised
	- Virtual MFA (google auth app or Authy)
	- Universal second factor - yubikey by yubico - physical device
		- Supports mult users w/ single key
#### IAM Roles

- Some services need to perform actions on your behalf
- IAM Roles are like users but intended for use by AWS services
