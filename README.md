# Step 1: Download the CloudWatch Agent Package

```wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb```

# Step 2: Install the CloudWatch Agent Package

```sudo dpkg -i amazon-cloudwatch-agent.deb```
# Step 3: Verify the CloudWatch Agent Status
```sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a status```

# Create or Update the Configuration File
```sudo nano /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json```

# Define the Log Configuration
```{
    "logs": {
        "logs_collected": {
            "files": {
                "collect_list": [
                    {
                        "file_path": "/var/log/apache2/access.log",
                        "log_group_name": "ApacheLogGroup",
                        "log_stream_name": "{instance_id}/access.log",
                        "retention_in_days": 14
                    },
                    {
                        "file_path": "/var/log/apache2/error.log",
                        "log_group_name": "ApacheLogGroup",
                        "log_stream_name": "{instance_id}/error.log",
                        "retention_in_days": 14
                    },
                    {
                        "file_path": "/var/log/apache2/other_vhosts_access.log",
                        "log_group_name": "ApacheLogGroup",
                        "log_stream_name": "{instance_id}/other_vhosts_access.log",
                        "retention_in_days": 14
                    }
                ]
            }
        }
    }
}```


#Start or Restart the CloudWatch Agent
```sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
    -a fetch-config \
    -m ec2 \
    -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json \
    -s```

