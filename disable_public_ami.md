## disable publics AMIs 

1) Run describe-images command (OSX/Linux/UNIX) with appropriate filtering to list the IDs of all Amazon Machine Images (AMIs) currently available in the selected region:

```bash
aws ec2 describe-images
	--region us-east-1
	--owners self
	--output table
	--query 'Images[*].ImageId'
```

2) The command output should return the AMI IDs requested:

```bash
------------------
| DescribeImages |
+----------------+
|  ami-3fad5252  |
|  ami-cdab54a0  |
+----------------+
```

3) Run again describe-images command (OSX/Linux/UNIX) using each image ID returned at the previous step to expose each AMI configuration metadata:

```bash
aws ec2 describe-images
	--region us-east-1
	--image-ids ami-3fad5252
```

4) The command output should return the metadata for the selected AMI:

```json
{
    "Images": [
        {
            "VirtualizationType": "hvm",
            "Name": "Web App Stack AMI ver. 1.4",
            "Hypervisor": "xen",
            "SriovNetSupport": "simple",
            "ImageId": "ami-3fad5252",
            "State": "available",
            ...
            "RootDeviceType": "ebs",
            "OwnerId": "123456789012",
            "RootDeviceName": "/dev/xvda",
            "CreationDate": "2016-06-03T15:35:51.000Z",
            "Public": true,
            "ImageType": "machine",
            "Description": "Full LAMP Stack + Web App + Local DB"
        }
    ]
}
```

If the Public parameter value is set to true (as shown in the example above), the selected AMI is accessible 
to all AWS accounts and your data is publicly exposed, otherwise the AMI is private.

4) Repeat steps no. 3 and 4 to verify the launch permissions for the rest of the AMIs available in the current region.

5) Repeat steps no. 1 – 5 to repeat the entire audit process for the other AWS regions.


> https://www.trendmicro.com/cloudoneconformity/knowledge-base/aws/EC2/publicly-shared-ami.html