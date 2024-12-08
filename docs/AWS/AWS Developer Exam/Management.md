# Management

## CloudWatch

Create a **CloudWatch metric filter** to extract metrics from the log files with location as a dimension. CloudWatch Logs uses these metric filters to turn log data into numerical CloudWatch metrics that you can graph or set an alarm on:

* Example: Count log events
* Example: Count occurrences of a term
* Example: Count HTTP 404 codes
* Example: Count HTTP 4xx codes
* Example: Extract fields from an Apache log and assign dimensions

Run in a cron job on an Amazon EC2 instance that sends status information about the application to Amazon CloudWatch using the AWS CLI **put-metric-data** command

If you set an alarm on a high-resolution metric, you can specify a high-resolution **alarm with a period of 10 seconds or 30 seconds**, or you can set a regular alarm with a period of any multiple of 60 seconds. There is a higher charge for high-resolution alarms.