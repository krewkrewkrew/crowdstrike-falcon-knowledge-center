# 🏹 Threat Hunting with CrowdStrike Falcon LogScale

This guide provides a practical introduction to threat hunting using CrowdStrike Falcon LogScale, CrowdStrike's next-generation SIEM. With LogScale, you can ingest and analyze vast amounts of data in real-time, enabling you to proactively hunt for threats and uncover hidden adversary activity.

## 🎯 Prerequisites

Before you begin, ensure you have:

-   Access to a CrowdStrike Falcon LogScale environment.
-   Data flowing into LogScale from your endpoints, cloud workloads, and other security tools.
-   A basic understanding of threat hunting concepts and the MITRE ATT&CK® framework.

## 📦 Step 1: Understanding the LogScale Query Language

LogScale uses a powerful and intuitive query language that allows you to search and filter data with speed and precision. The query language is pipe-based, similar to Unix shell commands, making it easy to learn and use.

### Basic Query Structure

A typical LogScale query consists of a search term followed by a series of piped commands:

```
<search_term> | <command1> | <command2> | ...
```

-   **`<search_term>`**: The initial filter for your data. This can be a simple string, a key-value pair, or a more complex expression.
-   **`<command>`**: A function that processes the data, such as `groupBy()`, `sort()`, or `top()`.

## 🔍 Step 2: Hunting for Suspicious Processes

One of the most common threat hunting techniques is to look for suspicious processes running on your endpoints. The following example demonstrates how to hunt for PowerShell processes with encoded commands, a common technique used by adversaries to obfuscate their activity.

### Sample LogScale Query

```
#event_simpleName=ProcessRollup2 event_platform=Win
| ImageFileName=/powershell\.exe/i
| CommandLine=/\-enc|\-encoded|\-e /i
| groupBy([aid, ComputerName, UserName, ParentImageFileName, ImageFileName, CommandLine])
```

### Query Explanation

1.  **`#event_simpleName=ProcessRollup2 event_platform=Win`**: This line filters for process execution events on Windows hosts.
2.  **`| ImageFileName=/powershell\.exe/i`**: This filters for processes where the image file name is `powershell.exe` (case-insensitive).
3.  **`| CommandLine=/\-enc|\-encoded|\-e /i`**: This filters for command lines that contain the `-enc`, `-encoded`, or `-e` flags, which are used to execute encoded PowerShell commands.
4.  **`| groupBy([aid, ComputerName, UserName, ParentImageFileName, ImageFileName, CommandLine])`**: This groups the results by host, user, parent process, and command line, allowing you to see the context of each suspicious process execution.

## 🚀 Step 3: Visualizing the Data

LogScale's visualization capabilities allow you to create dashboards and charts to help you identify patterns and anomalies in your data. For example, you can create a time chart to visualize the frequency of suspicious PowerShell executions over time.

### Sample Time Chart Query

```
#event_simpleName=ProcessRollup2 event_platform=Win
| ImageFileName=/powershell\.exe/i
| CommandLine=/\-enc|\-encoded|\-e /i
| timechart(span=1h)
```

This query will generate a time chart showing the number of suspicious PowerShell executions per hour, which can help you identify spikes in activity that may indicate a security incident.

## 🚀 Next Steps

Threat hunting is an iterative process of exploration and discovery. Now that you have a basic understanding of how to hunt for threats with LogScale, you can start exploring other hunting techniques and use cases:

-   **[Hunt for Lateral Movement](../analytics/threat-hunting/hunting-for-lateral-movement.md)**: Look for signs of adversaries moving laterally within your network.
-   **[Investigate Suspicious Network Connections](../analytics/threat-hunting/investigating-network-connections.md)**: Hunt for connections to known malicious IP addresses or domains.
-   **[Create Custom Dashboards](../analytics/reporting/creating-logscale-dashboards.md)**: Build custom dashboards to monitor for specific threats and track your hunting activities.

By leveraging the power of CrowdStrike Falcon LogScale, you can transform your threat hunting capabilities and stay one step ahead of the adversary.
