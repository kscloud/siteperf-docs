# Terraform provider
## Resources

This section describe available Terraform resources.

## Resource: siteperf_account
---
Creates an SitePerf account. 

### Example usage

```terraform
resource "siteperf_account" "test_account" {
  name = "test_account"
  description = "Test account for testing environments."
}
```

### Argument Reference

The following arguments are available

* `name` - (Required) - Name of the account
* `description` - (Optional) - Description of the account

### Attributes Reference

The following attributes are exported

* `name` - (Required) - Name of the account
* `description` - (Optional) - Description of the account

## Resource: siteperf_slack_alert_handler
---

Configures an Slack alert handler allowing for routing alert notifications to Slack channel. 

### Example usage

```terraform
resource "siteperf_alert_handler_slack" "slack_alerts" {
  account_id  = siteperf_account.test_account.id
  webhook_url = "https://hooks.slack.com/services/SOME/WEBHOOK/URL"
}
```

### Argument Reference

The following arguments are available

* `account_id` - (Required) - Id of the SitePerf account. Reference `siteperf_account` resource.
* `webhook_url` - (Required) - Slack webhook URL.

### Attributes Reference

* `id` - Id of this alert handler. 

## Resource: siteperf_uptime_monitor
---
Configures an uptime monitor for a URL.

### Example usage

```terraform
resource "siteperf_uptime_monitor" "example_monitor" {
  account_id = siteperf_account.test_account.id
  url = "https://kscloud.pl"
  
  success_code = 200
  every = "1m"

  alert {
    handler_id = siteperf_alert_handler_slack.alerts.id
    metric = "response_time"
    crit = 9
    warn = 8
    window = "5m"
  }

  alert {
    handler_id = siteperf_alert_handler_slack.alerts.id
    metric = "uptime"
  }
}
```

### Argument Reference

The following arguments are available

* `account_id` - (Required) - Id of the SitePerf account. Reference `siteperf_account` resource.
* `url` - (Required) - URL to create uptime monitor for.
* `success_code` - (Optional) - Return code which should be considered successful. Defaults to 200.
* `every` - (Optional) - One of: `1m`, `5m`. Defaults to `1m`. How often the check is run.

### Optional block: alert

Specifies alerting for this uptime monitor

* `handler_id` - (Required) - The alert handler for this alert. Reference for example the `siteperf_slack_alert_handler` resource.
* `metric` - (Required) - One of: `uptime`, `response_time`
* `crit` - (Optional) - Critical threshold. Depending on a metric chosen it might be required value,
* `warn` - (Optional) - Warining threshold. Depending on a metric chosen it might be required value
* `window` - (Optional) - Value 1m,2m,3m...60m. Time interval used to calculate the value for the metric. For example - for metric `response_time` and window `5m` the most current value will be calculated as mean value of all response times during interval NOW - 5 minutes. 

### Attributes Reference

* `id` - Id of this uptime monitor. 