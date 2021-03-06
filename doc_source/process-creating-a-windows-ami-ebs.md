## How the creation of a custom AMI works <a name="process-creating-a-windows-ami-ebs"></a>

First, launch an instance from an AMI that's similar to the AMI that you'd like to create\. You can connect to your instance and customize it\. When the instance is set up the way you want it, ensure data integrity by stopping the instance before you create an AMI and then create the image\. We automatically register the AMI for you\.

During the AMI\-creation process, Amazon EC2 creates snapshots of your instance's root volume and any other EBS volumes attached to your instance\. You're charged for the snapshots until you deregister the AMI and delete the snapshots\. For more information, see [Deregistering your Windows AMI](deregister-ami.md)\. If any volumes attached to the instance are encrypted, the new AMI only launches successfully on instance types that support Amazon EBS encryption\. For more information, see [Amazon EBS encryption](EBSEncryption.md)\.

Depending on the size of the volumes, it can take several minutes for the AMI\-creation process to complete \(sometimes up to 24 hours\)\. You may find it more efficient to create snapshots of your volumes prior to creating your AMI\. This way, only small, incremental snapshots need to be created when the AMI is created, and the process completes more quickly \(the total time for snapshot creation remains the same\)\. For more information, see [Creating Amazon EBS snapshots](ebs-creating-snapshot.md)\.

After the process completes, you have a new AMI and snapshot created from the root volume of the instance\. When you launch an instance using the new AMI, we create a new EBS volume for its root volume using the snapshot\.

If you add instance store volumes or Amazon EBS volumes to your instance in addition to the root device volume, the block device mapping for the new AMI contains information for these volumes, and the block device mappings for instances that you launch from the new AMI automatically contain information for these volumes\. The instance store volumes specified in the block device mapping for the new instance are new and don't contain any data from the instance store volumes of the instance you used to create the AMI\. The data on EBS volumes persists\. For more information, see [Block device mapping](block-device-mapping-concepts.md)\.

**Note**  
When you create a new instance from a custom AMI, you should initialize both its root volume and any additional EBS storage before putting it into production\. For more information, see [Initializing Amazon EBS Volumes](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ebs-initialize.html)\.
