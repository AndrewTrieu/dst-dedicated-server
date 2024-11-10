# Don't Starve Together Dedicated Server on AWS

This CloudFormation template deploys a dedicated server for the game *Don't Starve Together* on AWS. The setup includes a VPC, a subnet, an EC2 instance configured for the DST server, and appropriate networking configurations.

## Prerequisites

1. **AWS CLI**: Ensure you have the AWS CLI installed and configured.
2. **Key Pair**: Replace `myserverkey` with the name of an existing EC2 key pair in your AWS account to access the instance.
3. **IAM Permissions**: Your AWS user needs permissions to deploy CloudFormation stacks and create EC2, VPC, and related resources.

## Resources Created

- **VPC**: A Virtual Private Cloud for network isolation.
- **Subnet**: A subnet within the VPC.
- **Security Group**: Allows necessary ports for the DST server and SSH access from a specific IP range.
- **Internet Gateway and Route Table**: Provides internet access to the subnet.
- **EC2 Instance**: An Ubuntu-based instance running the DST dedicated server.

## Security Group Configuration

| Protocol | Ports   | Purpose                              | IP Range           |
|----------|---------|--------------------------------------|--------------------|
| UDP      | 11000-11001, 27018-27019, 8768-8769 | Game server ports for Master and Caves shards | 0.0.0.0/0          |
| TCP      | 22      | SSH for server access               | Your IP range      |

## Deployment

Replace the key pair name mentioned above, the AMI ID, the instance type, the SSH IP range, and the variables `CLUSTER_NAME`, `CLUSTER_DESCRIPTION`, `CLUSTER_PASSWORD`, `CLUSTER_KEY`, `ADMIN_ID`, `TOKEN`, and world generation settings in the `dst.yml` file to match your requirements.

To deploy the stack:

```bash
aws cloudformation create-stack --stack-name DST-Server-Stack --template-body dst.yml --capabilities CAPABILITY_IAM
```

## Cleanup

To delete the stack:

```bash
aws cloudformation delete-stack --stack-name DST-Server-Stack
```

## References

- [Don't Starve Together Dedicated Servers](https://forums.kleientertainment.com/forums/topic/64441-dedicated-server-quick-setup-guide-linux/)
- [worldgenoverride.lua Settings for March QoL Update](https://forums.kleientertainment.com/forums/topic/127830-worldgenoverridelua-settings-for-march-qol-update/)
