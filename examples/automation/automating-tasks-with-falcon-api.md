# ⚙️ Automating Security Tasks with the CrowdStrike Falcon API

This guide provides a practical introduction to automating common security tasks using the CrowdStrike Falcon API. By leveraging the API, you can streamline your security operations, reduce manual effort, and create powerful, customized workflows that meet your organization's specific needs.

## 🎯 Prerequisites

Before you begin, ensure you have:

-   A good understanding of REST APIs and how to make HTTP requests.
-   CrowdStrike Falcon API credentials (client ID and client secret) with the necessary permissions for the tasks you want to automate.
-   A programming language of your choice (Python is used in the examples below) and a library for making HTTP requests (e.g., `requests`).

## 🔐 Step 1: Authentication

First, you need to obtain an OAuth2 access token to authenticate your API requests. You can do this by making a POST request to the Falcon API's token endpoint.

### Python Example

```python
import requests

CLIENT_ID = "YOUR_CLIENT_ID"
CLIENT_SECRET = "YOUR_CLIENT_SECRET"
BASE_URL = "https://api.crowdstrike.com"  # Use the correct base URL for your cloud region

url = f"{BASE_URL}/oauth2/token"

data = {
    "client_id": CLIENT_ID,
    "client_secret": CLIENT_SECRET
}

response = requests.post(url, data=data)

if response.status_code == 201:
    access_token = response.json()["access_token"]
    print("Successfully obtained access token.")
else:
    print(f"Error obtaining access token: {response.text}")
    exit()

headers = {
    "Authorization": f"Bearer {access_token}"
}
```

## 🛡️ Step 2: Automating Device Containment

One of the most critical security tasks is to contain a compromised device to prevent an attacker from moving laterally within your network. The Falcon API allows you to automate this process.

### Python Example

This example demonstrates how to contain a device based on its hostname.

```python
# First, find the device ID (aid) for the hostname
def get_device_id(hostname):
    url = f"{BASE_URL}/devices/queries/devices/v1"
    params = {"filter": f"hostname:‘{hostname}’"}
    response = requests.get(url, headers=headers, params=params)
    if response.status_code == 200 and response.json()["resources"]:
        return response.json()["resources"][0]
    return None

# Now, contain the device
def contain_device(aid):
    url = f"{BASE_URL}/devices/actions/devices/v1?action_name=contain"
    payload = {
        "ids": [aid]
    }
    response = requests.post(url, headers=headers, json=payload)
    return response

hostname_to_contain = "compromised-host"
device_id = get_device_id(hostname_to_contain)

if device_id:
    containment_response = contain_device(device_id)
    if containment_response.status_code == 202:
        print(f"Successfully initiated containment for {hostname_to_contain}.")
    else:
        print(f"Error containing device: {containment_response.text}")
else:
    print(f"Could not find device with hostname: {hostname_to_contain}")
```

## 🔍 Step 3: Automating IOC Searches

Another common task is to search for Indicators of Compromise (IOCs) across your environment. The Falcon API allows you to search for file hashes, IP addresses, domains, and other IOCs.

### Python Example

This example demonstrates how to search for a specific file hash (SHA256).

```python
def search_for_hash(file_hash):
    url = f"{BASE_URL}/indicators/queries/devices/v1?type=sha256&value={file_hash}"
    response = requests.get(url, headers=headers)
    return response

file_hash_to_search = "YOUR_FILE_HASH"
search_response = search_for_hash(file_hash_to_search)

if search_response.status_code == 200:
    device_ids = search_response.json()["resources"]
    if device_ids:
        print(f"Found {len(device_ids)} devices with the specified file hash.")
        # You can now take further action on these devices, such as containment or investigation
    else:
        print("No devices found with the specified file hash.")
else:
    print(f"Error searching for file hash: {search_response.text}")
```

## 🚀 Next Steps

These are just a few examples of the many security tasks you can automate with the CrowdStrike Falcon API. Here are some other ideas to explore:

-   **[Automate Alert Triage](../automation/automating-alert-triage.md)**: Automatically enrich alerts with additional context and close out low-priority alerts.
-   **[Manage Vulnerabilities](../vulnerability-management/managing-vulnerabilities-with-api.md)**: Retrieve vulnerability data and create tickets in your ticketing system.
-   **[Generate Custom Reports](../reporting/generating-custom-reports-with-api.md)**: Extract data from the Falcon API to create custom reports and dashboards.

By integrating the Falcon API into your security workflows, you can build a more efficient, responsive, and proactive security operations program.
