# AWS IAM Identity Center User and Group API Operations

This project provides examples and sample code to manage and audit AWS IAM identity store User and Group operations at scale using APIs. With these APIs, you can build automation workflows to:
* Provision and de-provision users and groups 
* Add new members to a group or remove them from a group
* Query information about users and groups in IAM Identity Center
* Update information about these users and groups
* Find out which users are members of which groups

## Prerequisites

Before you start you should have the following prerequisites:
  * An Organization in AWS Organizations
  * Administrative access to the AWS IAM Identity Center
  * Python version 3.10.5 or later
  * AWS CLI
  * Boto3


## Install prerequisites

Install Python 
```
sudo yum update
sudo yum install -y python3-pip python3 python3-setuptools
```
Install Boto3
```
pip3 install boto3
```
Install AWS Cli
```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```


## Environment Setup

Clone this repo:

```
git clone https://github.com/aws-samples/iam-identitycenter-identitystoreapi-operations
```
Set up your access to to AWS Console
```
aws configure
```
Verify your access
```
aws sts get-called-identity
```

## Test

Here is an example to see all supported operations available in the sample script.

```
python identitystore_operations.py —h

*Sample Output:*
usage: identitystore_operations.py [-h]
                                   {create_user,create_group,adduser_to_group,delete_group,list_members,list_membership}
                                   ...
positional arguments:
  {create_user,create_group,adduser_to_group,delete_group,list_members,list_membership}

options:
  -h, --help            show this help message and exit

```

## AWS IAM Identity Center User and Group API Operations

Here is an example of how you can create a new user “John Doe” in the IAM Identity Center identity store and add the user to an existing “AWS_Data_Science” Group.

```
python identitystore_operations.py create_user --identitystoreid d-123456a7890 --username johndoe --givenname John --familyname Doe --groupname AWS_SSO_Data_Science --email johndoe@company.com

*Sample Output:*
User:johndoe with UserId:94482488-3041-7026-18f3-be45837cd0e4 created successfully
User:johndoe added to Group:AWS_Data_Science successfully
```

Create bulk users from a CSV file

You can download the sample [CSV file](/newaccounts.csv) and keep it in the same folder as code
```
python identitystore_operations.py create_user_bulk --filename newaccounts.csv

*Sample Output:*
User:SDG51 with UserId:a8a163f0-a011-70e0-3ed3-3f8b93f78ca7 created successfully
User:SDG51 added to Group:ScienceGroup successfully
User:SDG52 with UserId:68e173b0-c021-70e6-9930-dddca1d9464e created successfully
User:SDG52 added to Group:ScienceGroup successfully
User:SDG53 with UserId:f8d19370-f031-7077-1df6-b27f1a2beada created successfully
User:SDG53 added to Group:ScienceGroup successfully
User:SDG54 with UserId:98d12350-8041-705f-9948-b22e5a9f47a4 created successfully
User:SDG54 added to Group:ScienceGroup successfully
User:SDG55 with UserId:6801c3b0-50e1-70d9-a66c-1fa91b195a11 created successfully
User:SDG55 added to Group:ScienceGroup successfully
User:SDG56 with UserId:a8317340-a031-7056-d927-28bb403b7758 created successfully
```


Now, consider the data scientist transitions to an applied scientist role and needs access to additional AWS applications and resources. Previously, you had to manually update their information and add them to the “AWS_Applied_Scientists” group so they get the right access. Now, your automation can update the user and provide them with the access they need. 
 
Here is an example of how a previously created user “John Doe” can be added to“AWS_Applied_Scientists” group

```
python identitystore_operations.py adduser_to_group --identitystoreid d-123456a7890 --groupname AWS_SSO_Applied_Scientists --username johndoe

*Sample Output:*
User:johndoe added to Group:AWS_Applied_Scientists successfully
```

## AWS IAM Identity Center User and Group Audit Operations

Here is an example of how you can find all members of “AWS_Applied_Scientists” group

```
python identitystore_operations.py list_members --identitystoreid d-123456a7890 --groupname AWS_SSO_Applied_Scientists 

*Sample Output:*
UserName:johndoe,Display Name: John Doe 
```

Here is an example of how you can find group memberships of a specific user “johndoe”

```
python identitystore_operations.py list_membership --identitystoreid d-123456a7890 --username johndoe

*Sample Output*
User :johndoe is a member of the following groups
AWS_Data_Science
AWS_Applied_Scientists
```

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

