---
subcategory: "Firewall Network"
layout: "aviatrix"
page_title: "Aviatrix: aviatrix_firewall_instance"
description: |-
  Creates and deletes Aviatrix Firewall Instances
---

# aviatrix_firewall_instance

The **aviatrix_firewall_instance** resource allows the creation and management of Aviatrix Firewall Instances.

## Example Usage

```hcl
# Create an Aviatrix Firewall Instance
resource "aviatrix_firewall_instance" "test_firewall_instance" {
  vpc_id            = "vpc-032005cc371"
  firenet_gw_name   = "avx-firenet-gw"
  firewall_name     = "avx-firewall-instance"
  firewall_image    = "Palo Alto Networks VM-Series Next-Generation Firewall Bundle 1"
  firewall_size     = "m5.xlarge"
  management_subnet = "10.4.0.16/28"
  egress_subnet     = "10.4.0.32/28"
}
```
```hcl
# Create an Aviatrix Firewall Instance with Native GWLB Enabled VPC
resource "aviatrix_firewall_instance" "test_firewall_instance" {
  vpc_id            = "vpc-032005cc371"
  firewall_name     = "avx-firewall-instance"
  firewall_image    = "Palo Alto Networks VM-Series Next-Generation Firewall Bundle 1"
  firewall_size     = "m5.xlarge"
  management_subnet = "10.4.0.16/28"
  egress_subnet     = "10.4.0.32/28"
  zone              = "us-east-1a"
}
```

## Argument Reference

The following arguments are supported:

### Required
* `vpc_id` - (Required) VPC ID of the Security VPC.
* `firenet_gw_name` - (Optional) Name of the primary FireNet gateway. Required for FireNet without Native GWLB VPC.
* `firewall_name` - (Required) Name of the firewall instance to be created.
* `firewall_image` - (Required) One of the AWS/Azure AMIs from Palo Alto Networks.
* `firewall_size` - (Required) Instance size of the firewall. Example: "m5.xlarge".  
* `management_subnet` - (Optional) Management Interface Subnet. Select the subnet whose name contains “gateway and firewall management”. Required for Palo Alto Networks VM-Series, and required to be empty for Check Point or Fortinet series.
* `egress_subnet` - (Required) Egress Interface Subnet. Select the subnet whose name contains “FW-ingress-egress”.
* `firewall_image_version` - (Optional) Version of firewall image. If not specified, Controller will automatically select the latest version available.
* `zone` - (Optional) Availability Zone. Required if creating a Firewall Instance with a Native AWS GWLB enabled VPC. Applicable to Azure and AWS only. Available as of provider version R2.17+.

### Authentication method
* `key_name`- (Optional) Applicable to AWS deployment only. AWS Key Pair name. If not provided a Key Pair will be generated.
* `username`- (Optional) Applicable to Azure deployment only. "admin" as a username is not accepted.
* `password`- (Optional) Applicable to Azure deployment only.
* `ssh_public_key` - (Optional) Applicable to Azure deployment only.

### Advanced Options
* `iam_role` - (Optional) Only available for AWS. In advanced mode, create an IAM Role on the AWS account that launched the FireNet gateway. Create a policy to attach to the role. The policy is to allow access to “Bootstrap Bucket”.
* `bootstrap_bucket_name`- (Optional) Only available for AWS. In advanced mode, specify a bootstrap bucket name where the initial configuration and policy file is stored.
* `bootstrap_storage_name` - (Optional) Advanced option. Bootstrap storage name. Applicable to Azure and Palo Alto Networks VM-Series/Fortinet Series deployment only. Available as of provider version R2.17.1+.
* `storage_access_key` - (Optional) Advanced option. Storage access key. Applicable to Azure and Palo Alto Networks VM-Series deployment only. Available as of provider version R2.17.1+.
* `file_share_folder` - (Optional) Advanced option. File share folder. Applicable to Azure and Palo Alto Networks VM-Series deployment only. Available as of provider version R2.17.1+.
* `share_directory` - (Optional) Advanced option. Share directory. Applicable to Azure and Palo Alto Networks VM-Series deployment only. Available as of provider version R2.17.1+.
* `sic_key` - (Optional) Advanced option. Bic key. Applicable to Azure and Check Point Series deployment only.
* `container_folder` - (Optional) Advanced option. Bootstrap storage name. Applicable to Azure and Fortinet Series deployment only.
* `sas_url_config` - (Optional) Advanced option. Bootstrap storage name. Applicable to Azure and Fortinet Series deployment only.
* `sas_url_license` - (Optional) Advanced option. Bootstrap storage name. Applicable to Azure and Fortinet Series deployment only.
* `user_data` - (Optional) Advanced option. Bootstrap storage name. Applicable to Check Point Series and Fortinet Series deployment only.

## Attribute Reference

In addition to all arguments above, the following attributes are exported:

* `instance_id`- ID of the firewall instance created.
* `lan_interface`- ID of Lan Interface created.
* `management_interface`- ID of Management Interface created.
* `egress_interface`- ID of Egress Interface created.
* `public_ip`- Management Public IP.

## Import

**firewall_instance** can be imported using the `instance_id`. For Azure FireNet instances, the value will be the `firewall_name` concatenated with a ":" and the Resource Group of the `vpc_id` set for that instance. e.g.

```
$ terraform import aviatrix_firewall_instance.test instance_id
```
