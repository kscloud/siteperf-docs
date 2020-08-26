# About SitePerf
SitePerf is an ultimate endpoint monitoring tool for DevOps and QA Engineers. 


## Configuration
Over last several years we have used multiple different tools. They usually had one thing in common - inconvenient configuration using clumsy and slow UI interface. 

As a modern engineers we all prefere to utilize the Infrastructure as Code principle - yet many of those tools do not provide proper public APIs to manage them. 

At SitePerf we took a different approach. We allow only programmatic configuration through API - to make it easy we provide a fully operational Terraform provider that can configure literally every aspect of your SitePerf account.


# Terraform provider documentation

## Resources

This section describe available Terraform resources.

### Resource: siteperf_account
---
Creates an SitePerf account. 

#### Example usage

```hcl
resource "siteperf_account" "test_account" {
  name = 
  description = "Test account for testing purposes."
}
```

#### Argument Reference

The following arguments are available

* `name` - (Required) - Name of the account
* `description` - (Optional) - Description of the account


#### Attributes Reference

The following attributes are exported

* `name` - (Required) - Name of the account
* `description` - (Optional) - Description of the account





