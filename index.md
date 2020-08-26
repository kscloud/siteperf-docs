# About SitePerf
SitePerf is an ultimate endpoint monitoring tool for DevOps and QA Engineers. 


## Configuration
Over last several years we have used multiple different tools. They usually had one thing in common - inconvenient configuration using clumsy and slow UI interface. 

As a modern engineers we all prefere to utilize the Infrastructure as Code principle - yet many of those tools do not provide proper public APIs to manage them. 

At SitePerf we took a different approach. We allow only programmatic configuration through API - to make it easy we provide a fully operational Terraform provider that can configure literally every aspect of your SitePerf account.


# Terraform provider documentation


## Resources

This section describe available Terraform resources.

---

### Resource: siteperf_account
Creates an SitePerf account. 

#### Example usage

```hcl
resource "siteperf_account" "test_account" {
  name = "test_account"
  description = "Test account for testing environments."
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

---

### Resource: siteperf_slack_alert_handler

Configures an Slack alert handler allowing for routing alert notifications to Slack channel. 

#### Example usage

```hcl
resource "siteperf_alert_handler_slack" "slack_alerts" {
  account_id  = siteperf_account.test_account.id
  webhook_url = "https://hooks.slack.com/services/SOME/WEBHOOK/URL"
}
```

#### Argument Reference

The following arguments are available

* `account_id` - (Required) - Id of the SitePerf account. Reference siteperf_account resource.
* `webhook_url` - (Required) - Slack webhook URL.

#### Attributes Reference

* `id` - Id of this alert handler. 









