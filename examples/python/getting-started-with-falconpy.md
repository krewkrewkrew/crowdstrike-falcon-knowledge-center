# 🐍 Getting Started with FalconPy: A Practical Guide

This guide provides a step-by-step walkthrough of how to get started with FalconPy, the official CrowdStrike Python SDK. By the end of this guide, you will be able to install FalconPy, configure it with your API credentials, and retrieve a list of hosts from your Falcon environment.

## 🎯 Prerequisites

Before you begin, you will need:

-   Python 3.6 or later
-   CrowdStrike Falcon API credentials (client ID and client secret)
-   Access to a CrowdStrike Falcon environment

## 📦 Step 1: Installation

First, you need to install the FalconPy library. You can do this using `pip`:

```bash
pip install crowdstrike-falconpy
```

This command will download and install the latest version of FalconPy and its dependencies.

## 🔐 Step 2: Configuration

Next, you need to configure FalconPy with your API credentials. You can do this by setting environment variables, which is the recommended approach for security.

```bash
export FALCON_CLIENT_ID="YOUR_CLIENT_ID"
export FALCON_CLIENT_SECRET="YOUR_CLIENT_SECRET"
```

Replace `YOUR_CLIENT_ID` and `YOUR_CLIENT_SECRET` with your actual CrowdStrike API credentials. FalconPy will automatically detect and use these environment variables.

## 主机 (hōjī) Step 3: Retrieving a List of Hosts

Now that you have installed and configured FalconPy, you can start interacting with the Falcon API. The following example demonstrates how to retrieve a list of hosts from your environment.

```python
from falconpy import Hosts

# Initialize the Hosts service class
falcon = Hosts()

# Retrieve a list of all host IDs
response = falcon.query_devices_by_filter()

if response["status_code"] == 200:
    host_ids = response["body"]["resources"]
    print(f"Found {len(host_ids)} hosts.")

    # Retrieve details for the first 10 hosts
    if host_ids:
        response = falcon.get_device_details(ids=host_ids[:10])
        if response["status_code"] == 200:
            hosts = response["body"]["resources"]
            for host in hosts:
                print(f"- Hostname: {host.get("hostname", "N/A")}, OS: {host.get("os_version", "N/A")}")
        else:
            print(f"Error getting host details: {response["body"]["errors"][0]["message"]}")
else:
    print(f"Error querying hosts: {response["body"]["errors"][0]["message"]}")
```

### Code Explanation

1.  **`from falconpy import Hosts`**: This line imports the `Hosts` service class, which contains all the methods for interacting with the Falcon Hosts API.
2.  **`falcon = Hosts()`**: This line initializes the `Hosts` service class. FalconPy automatically handles API authentication using the environment variables you set earlier.
3.  **`falcon.query_devices_by_filter()`**: This method queries the Falcon API to retrieve a list of all host IDs in your environment.
4.  **`falcon.get_device_details(ids=host_ids[:10])`**: This method retrieves detailed information for the specified host IDs. In this example, we are retrieving details for the first 10 hosts.

## 🚀 Next Steps

Now that you have successfully retrieved a list of hosts, you can explore other capabilities of the FalconPy SDK. Here are some ideas for what you can do next:

-   **[Manage Detections](../detections/manage-detections.md)**: Learn how to retrieve and manage security detections.
-   **[Perform Real-Time Response (RTR)](../rtr/perform-rtr-actions.md)**: Interact with hosts in real-time to perform administrative and investigative tasks.
-   **[Automate Workflows](../automation/automate-workflows.md)**: Build automation scripts to streamline your security operations.

We encourage you to explore the full capabilities of the FalconPy SDK and share your creations with the CrowdStrike community!

