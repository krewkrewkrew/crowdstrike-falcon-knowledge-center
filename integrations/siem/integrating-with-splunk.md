# 🔌 Integrating CrowdStrike Falcon with Splunk

This guide provides a practical walkthrough for integrating CrowdStrike Falcon with Splunk. By sending Falcon data to Splunk, you can centralize your security data, correlate Falcon events with other data sources, and leverage Splunk’s powerful search and visualization capabilities for advanced threat hunting and reporting.

## 🎯 Prerequisites

Before you start, ensure you have the following:

-   A running Splunk instance (Splunk Cloud or Splunk Enterprise).
-   Administrative access to both your CrowdStrike Falcon and Splunk environments.
-   The **Splunk Add-on for CrowdStrike** installed in your Splunk environment. You can download it from [Splunkbase](https://splunkbase.splunk.com/app/5094/).
-   CrowdStrike Falcon API credentials (client ID and client secret) with the "Falcon SIEM Connector" role.

## 📦 Step 1: Configure the Falcon SIEM Connector

First, you need to configure the Falcon SIEM Connector in your CrowdStrike environment to forward events to Splunk.

1.  **Log in to the CrowdStrike Falcon Console**.
2.  Navigate to **Support and resources** > **Tools** > **Falcon SIEM Connector**.
3.  Download and install the SIEM Connector on a host that can communicate with both the CrowdStrike cloud and your Splunk instance.
4.  During the installation, you will be prompted to configure the connector. Provide your API credentials and set the output format to JSON.
5.  Configure the connector to forward events to a local file or a TCP/UDP port that your Splunk instance can ingest.

## 🧩 Step 2: Configure the Splunk Add-on for CrowdStrike

Next, configure the Splunk Add-on for CrowdStrike to ingest the data from the Falcon SIEM Connector.

1.  **In Splunk, navigate to the Splunk Add-on for CrowdStrike**.
2.  Go to the **Configuration** tab.
3.  Add your CrowdStrike API credentials. This allows the add-on to make API calls back to CrowdStrike for data enrichment.
4.  Go to the **Inputs** tab and create a new input to ingest the data from the Falcon SIEM Connector. This could be a file monitor or a TCP/UDP input, depending on how you configured the SIEM Connector.

### Example Input Configuration (File Monitor)

-   **Input Type**: Monitor
-   **Source Type**: `crowdstrike:falcon:json`
-   **File or Directory**: The path to the JSON file where the SIEM Connector is writing events.
-   **Index**: `crowdstrike` (or your preferred index for security data).

## 🔍 Step 3: Search and Verify the Data

Once the integration is configured, you should start seeing CrowdStrike Falcon data flowing into Splunk. You can verify this by running a search in Splunk.

### Sample Splunk Search Query

This query searches for the top 10 most frequent detections by technique:

```spl
index=crowdstrike sourcetype="crowdstrike:falcon:json" event_simpleName=DetectionSummaryEvent
| top limit=10 Technique
```

This search will return a table showing the most common MITRE ATT&CK® techniques observed in your environment, based on CrowdStrike detections.

## 🚀 Next Steps

With CrowdStrike Falcon data now in Splunk, you can unlock a wide range of security use cases:

-   **[Create Dashboards](../analytics/reporting/creating-splunk-dashboards.md)**: Build custom dashboards to visualize Falcon data and monitor key security metrics.
-   **[Set Up Alerts](../analytics/reporting/setting-up-splunk-alerts.md)**: Create alerts to notify you of high-severity detections or specific patterns of activity.
-   **[Correlate with Other Data Sources](../analytics/threat-hunting/correlating-data-sources.md)**: Correlate Falcon data with other data sources, such as firewall logs or authentication events, for deeper threat investigation.
-   **[Automate Responses with SOAR](../soar/automating-responses-with-splunk-soar.md)**: Use Splunk SOAR to automate response actions based on CrowdStrike detections.

By integrating CrowdStrike Falcon with Splunk, you create a powerful, unified security analytics platform that enhances your ability to detect, investigate, and respond to threats across your entire enterprise.

