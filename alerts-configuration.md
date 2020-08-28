# Alerts reference

This page discusses the alert configuration in depth. When using Terraform all the alerts are configurable through the alert block. A simple example can be found below:

```terraform
resource "siteperf_uptime_monitor" "example_monitor" {
  every = "1m"
  .........
  alert {
    handler_id = siteperf_alert_handler_slack.alerts.id
    metric = "response_time"
    crit = 6
    warn = 3
    window = "5m"
  }
  .........
}
```

## Argument: every

The `every` argument is very straightforward. It defines how often the uptime check is performed. It is defined on the uptime monitor. In the above example value `1m` means that the uptime check will be performed every 1 minute. All alerts created for this uptime monitor will inherit this value and alert evaluation will be performed every 1 minute as well.

## Argument: window

This allows you to construct more intelligent alerts that will not generate many false positives. 

Window defines the time interval used to calculate the value for the metric. In the above example value `5m` means that the data used for calculating the metric value will be coming from `NOW - 5 minutes` timeframe. Combining this with `every` set to `1m` the most current metric value will always be calculated using latest 5 values.     

# Allowed metrics

## response_time

Using this metric you can create the average response time alert for your endopoint:

### Example

```terraform
  alert {
    handler_id = siteperf_alert_handler_slack.alerts.id
    metric = "response_time"
    crit = 6
    warn = 3
    window = "5m"
    every = "1m"
  }
```

## uptime

Using this metric you can create a simple uptime percentage alert (over the provided window). Setting very low window will cause even a single uptime failure to emit a critical alert, you can fine tune the values here to make sure that a single failed uptime check will not emit alerts.

Example values:

* warn - 95 - if uptime falls below 95% over monitored window emit warning alert
* crit - 90 - if uptime falls below 90% over monitored window emit critical alert
* every - 1m - check the uptime percentage every minute
* window - 15m - use data from latest 15 minutes to calculate the uptime percentage. The bigger the window the smaller percentage drop will be due to single uptime check failure. 

You can easily calculate how many failures will trigger an alert. First calculate the number of checks that will run within a window (every `1m` and window `15m` means there will be 15 checks within window) and use following formula to calculate, for example, how many failure will trigger warning alert with threshold set to 95: `-0.95 * metrics_in_window + metrics_in_window`. 

### Example

```terraform
  alert {
    handler_id = siteperf_alert_handler_slack.alerts.id
    metric = "response_time"
    crit = 90
    warn = 95
    window = "15m"
  }
```

