# aws-export-ami-to-vmdk

1. Create EC2 Instance temporary, for running script. Choose minimum spesification like t3.micro 8gib.
2. Create IAM user with role AdministratorAccess
3. Generate AccessKey & SecretKey
4. Connect to instance, install aws-cli with this command : yum instal aws-cli
5. Config the credential aws-cli with command : aws configure
6. Insert AccessKey, SecretKey, Region, and skip others.
7. Copy file trust-policy.json on this repository to path /tmp/
8. Run this command on the /tmp/ path : aws iam create-role --role-name vmimport --assume-role-policy-document "file:///tmp/trust-policy.json"
9. Copy another file role-policy.json on this repo to same path.
10. Run this command : aws iam put-role-policy --role-name vmimport --policy-name vmimport --policy-document "file:///tmp/role-policy.json"

After the prerequisite above is done. You can start for export AMI to VMDK. with this command:
aws ec2 export-image --image-id {{ami-id}} --disk-image-format VMDK --s3-export-location S3Bucket={{my-export-bucket}},S3Prefix={{exports/}}

you can adjust for ami-id, S3 Bucket name, and prefix

Example result:
{
    "DiskImageFormat": "vmdk",
    "ExportImageTaskId": "export-ami-1234567890abcdef0"
    "ImageId": "ami-1234567890abcdef1",
    "RoleName": "vmimport",
    "Progress": "0",
    "S3ExportLocation": {
        "S3Bucket": "my-export-bucket",
        "S3Prefix": "exports/"
    },
    "Status": "active",
    "StatusMessage": "validating"
}


This command for check status:
aws ec2 describe-export-image-tasks --export-image-task-id export-ami-id

result :
{
    "ExportImageTasks": [
        {
            "ExportImageTaskId": "export-ami-1234567890abcdef0"
            "Progress": "21",
            "S3ExportLocation": {
                "S3Bucket": "my-export-bucket",
                "S3Prefix": "exports/"
            },
            "Status": "active",
            "StatusMessage": "updating"
        }
    ]
}
