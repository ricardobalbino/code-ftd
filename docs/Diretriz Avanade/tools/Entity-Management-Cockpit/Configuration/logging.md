# Logging

> **Note:** This is relevant only when the tool is run from the command-line or Windows Forms and not when it is run via the Deployment Tool (Cmdlets).

The tool is using NLog as logging framework. The rules, targets and log location can be found in the `NLog.config` file.

## Rules

## logfile (Enabled by Default)

The `logfile` rule provides the range Info - Fatal.

## logfile_error_min

The `logfile_error_min` rule provides only error logs and does not show
any StackTrace information. The file can be forwarded to the client,
e.g. when wrong input files were provided by the client.

## logfile_fatal

The `logfile_fatal` rule provides fatal errors which causes the
application to stop.

## logfile_error (Optional - Remove XML Comments to Enable)

The `logfile_error` rule provides an error log with extended information
(stack trace). This should not be forwarded to client.

## console (Optional - Remove XML Comments to Enable)

The console rule provides console logging. It is disabled by default due
to performance reasons.
