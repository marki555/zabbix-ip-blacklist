# zabbix-ip-blacklist

This repository provides a Zabbix template to check if a specified SMTP domain is listed on Real-time Blackhole Lists (RBLs) using MXToolbox and/or HetrixTools APIs. The solution is designed for Zabbix integration and offers alerts based on customizable warning and critical thresholds.

Based on bash script by https://github.com/a-stoyanov/zabbix-domain-blacklist

It is rewritten in Javascript and is utilizing Zabbix Script Item type, which executes the JS code and outputs JSON data. That is then processed in dependent items (for count and error) via JSONPath preprocessing step. Script items are executed directly on the Zabbix server/proxy, not on the monitored host (so you can create a host without agent, configure its IP and assign the template and it will work).
This version checks the IP address of the Host it is assigned to in Zabbix. Item name is generic (as it doesn't support macros), but trigger name will contain the actual IP address.

</br>

## Features

- **Multi-API Support**: Queries MXToolbox and/or HetrixTools APIs to check domain blacklist status.
- **Customizable**: Uses Template/Host user macros for API keys and warning/critical threshold values
- **JSON Output**: Provides structured JSON output with blacklist count, blacklist names, IP queried and error message.
- **Deduplication**: Normalizes and deduplicates blacklist names from both APIs for consistent reporting.
- **Error Handling**: For now, only generic error message on API error / parameter error is output a checked by a trigger.

## Requirements

- **Zabbix**: Version 6.0 or higher (uses Script Item type)
- **API Keys** (at least one required):
  - MXToolbox API key (optional; register your own [MXToolbox account](https://mxtoolbox.com/) to get a key).
  - HetrixTools API key (optional; register your own [HetrixTools account](https://hetrixtools.com) to get a key).

## Installation (Zabbix Server)

1. Import the zbx_blacklist.yaml template to your Zabbix server.
2. Assign the template to a host.
3. On Host level, enter at least 1 API key to inherited macro `{$MXTOOLBOX_API_KEY}` or `{$HETRIXTOOLS_API_KEY}`.
4. Optionally, change the warning (Average severity) and critical (High severity) threshold in macros `{$BLACKLIST_WARNING_CNT}` / `{$BLACKLIST_CRITICAL_CNT}`
