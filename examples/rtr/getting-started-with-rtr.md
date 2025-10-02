# ⚡️ Getting Started with Falcon Real-Time Response (RTR)

This guide provides a practical introduction to using CrowdStrike Falcon Real-Time Response (RTR), a powerful feature that allows you to remotely access and interact with your hosts in real-time. With RTR, you can perform administrative tasks, investigate security incidents, and remediate threats without needing to physically access the endpoint.

## 🎯 Prerequisites

Before you begin, ensure you have:

-   Access to a CrowdStrike Falcon environment with RTR enabled.
-   The "Real Time Responder" role assigned to your user account.
-   A host with the Falcon sensor installed and online.

## 📦 Step 1: Connecting to a Host

First, you need to establish an RTR session with the host you want to interact with.

1.  **Log in to the CrowdStrike Falcon Console**.
2.  Navigate to **Host management** and select the host you want to connect to.
3.  Click the **Connect to host** button in the upper-right corner of the host details page.

This will open a new browser tab with the RTR command-line interface, and you will be connected to the host in a secure, sandboxed environment.

## ⌨️ Step 2: Running Basic Commands

Once you are connected, you can run a variety of commands to interact with the host. The RTR command set is a subset of standard shell commands, with some additional commands specific to RTR.

### Basic RTR Commands

| Command | Description |
|---|---|
| `ls` | List files and directories. |
| `cd` | Change the current directory. |
| `ps` | List running processes. |
| `cat` | View the contents of a file. |
| `get` | Download a file from the host to your local machine. |
| `put` | Upload a file from your local machine to the host. |

### Example: Listing Running Processes

To list the running processes on the host, you can use the `ps` command:

```
ps
```

This will display a list of all running processes, similar to the output of the `ps` command in a standard Linux shell.

## 🚀 Step 3: Remediating a Threat

RTR is a powerful tool for threat remediation. For example, if you have identified a malicious process running on a host, you can use RTR to kill the process and remove the associated file.

### Example: Killing a Malicious Process

1.  **Identify the Process ID (PID)**: Use the `ps` command to find the PID of the malicious process.

    ```
    ps | grep malicious_process
    ```

2.  **Kill the Process**: Use the `kill` command to terminate the process.

    ```
    kill <PID>
    ```

    Replace `<PID>` with the actual process ID of the malicious process.

3.  **Remove the File**: Use the `rm` command to delete the malicious file.

    ```
    rm /path/to/malicious_file
    ```

## 🚀 Next Steps

Real-Time Response is a versatile tool that can be used for a wide range of security and administrative tasks. Now that you have a basic understanding of how to use RTR, you can explore its more advanced capabilities:

-   **[Run Scripts](../rtr/running-scripts.md)**: Execute custom scripts on your hosts to automate complex tasks.
-   **[Manage Files and Directories](../rtr/managing-files.md)**: Create, delete, and modify files and directories on your hosts.
-   **[Interact with the Registry (Windows)](../rtr/interacting-with-the-registry.md)**: View and modify the Windows Registry.

By mastering Falcon Real-Time Response, you can significantly enhance your ability to investigate and respond to threats, reducing your mean time to remediation (MTTR) and improving your overall security posture.

