# **Ansible Role: AWSLogs**

This role installs, configures the AWS CloudWatch Logs and setup systemd service. This roles works only for the host with Ubuntu 16.04 or above.

## Installation

ansible-galaxy install amritsingh.awslogs

## Requirements

This role only requires Ansible version 1.9+ and EC2_FACTS module.

## Role Variables

This role only uses one variable, `awslogs_logs`, which is a dictionary comprised of the following items:

```yaml

awslogs_logs:
  - file: /var/log/syslog            # The path to the log file you want to ship (required)
    format: "%b %d %H:%M:%S"         # The date and time format of the log file
    time_zone: "LOCAL"               # Timezone, can either be LOCAL or UTC
    initial_position: "end_of_file"  # Where log shipping should start from
    group_name: syslog               # The Cloudwatch Logs group name (required)
    stream_name: "{instance_id}"     # You can use a literal string and/or predefined variables ({instance_id}, {hostname}, {ip_address})
```

In addition, there are three variables that are not used by default:

```yaml
awslogs_region: eu-west-1            # Overrides the local region for log shipping
awslogs_access_key_id: XXX           # AWS key ID, used instead of IAM roles
awslogs_secret_access_key: XXX       # AWS secret key, used instead of IAM roles
```

This configuration is further expanded on in the [Amazon Cloudwatch Logs Documentation](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html#d0e2872).

## Dependencies

None

## Example Playbook

```yaml
---

- hosts: all

  vars:
    awslogs_logs:
      - file: /var/log/syslog
        format: "%b %d %H:%M:%S"
        time_zone: "LOCAL"
        initial_position: "end_of_file"
        group_name: syslog

      - file: /var/log/boot.log
        time_zone: "UTC"
        initial_position: "start_of_file"
        group_name: boot

  roles:
    - amritsingh.awslogs

```

## License

MIT / BSD

## Author Information

Amrit Singh
