# Splunk Home Lab — Setup Guide

This guide explains how to build the Splunk Home Lab on Ubuntu/Linux for security monitoring, log analysis, attack simulation, and SPL practice.

The lab focuses on ingesting Linux authentication logs and HTTP/web logs into Splunk and using SPL to investigate security events.

---

## 1. Lab Architecture

The basic lab consists of:

```text
                    ┌─────────────────────┐
                    │     Kali Linux      │
                    │  Security Testing   │
                    │                     │
                    │ Nmap / HTTP / SSH   │
                    └──────────┬──────────┘
                               │
                         Security Tests
                               │
                               ▼
┌──────────────────────────────────────────────────────┐
│                    Ubuntu Linux                      │
│                                                      │
│  ┌────────────────┐       ┌──────────────────────┐  │
│  │ Authentication │       │   HTTP/Web Server    │  │
│  │     Logs       │       │      Logs            │  │
│  │                │       │                      │  │
│  │ /var/log/      │       │ Apache/Nginx logs    │  │
│  │ auth.log       │       │                      │  │
│  └───────┬────────┘       └──────────┬───────────┘  │
│          │                           │               │
│          └─────────────┬─────────────┘               │
│                        ▼                             │
│                 ┌─────────────┐                     │
│                 │   Splunk    │                     │
│                 │ Enterprise  │                     │
│                 └─────────────┘                     │
└──────────────────────────────────────────────────────┘
```

---

# 2. Requirements

## Hardware

Recommended minimum configuration for the Splunk VM:

| Resource |            Recommended |
| -------- | ---------------------: |
| CPU      |                2 cores |
| RAM      |                   4 GB |
| Storage  |                 40 GB+ |
| OS       | Ubuntu 22.04/24.04 LTS |
| Network  | NAT or Bridged Adapter |

For a larger lab, 8 GB RAM is recommended.

---

# 3. Ubuntu Preparation

Update the system:

```bash
sudo apt update
sudo apt upgrade -y
```

Install basic utilities:

```bash
sudo apt install -y curl wget net-tools unzip vim git
```

Verify the hostname:

```bash
hostname
```

Check the IP address:

```bash
ip addr
```

Check network connectivity:

```bash
ping -c 4 8.8.8.8
```

---

# 4. Install Splunk Enterprise

Download Splunk Enterprise from the official Splunk website.

Install the downloaded `.deb` package:

```bash
sudo dpkg -i splunk-<version>-linux-2.6-amd64.deb
```

If dependencies are missing:

```bash
sudo apt --fix-broken install -y
```

Then run the installation again if required:

```bash
sudo dpkg -i splunk-<version>-linux-2.6-amd64.deb
```

---

# 5. Start Splunk

Start Splunk manually:

```bash
sudo /opt/splunk/bin/splunk start
```

Accept the license agreement and create the Splunk administrator account.

Enable Splunk to start automatically:

```bash
sudo /opt/splunk/bin/splunk enable boot-start
```

Check Splunk status:

```bash
sudo /opt/splunk/bin/splunk status
```

The Splunk Web interface should be available at:

```text
https://<SPLUNK-IP>:8000
```

Example:

```text
https://192.168.1.5:8000
```

---

# 6. Verify Splunk Installation

Check the installed version:

```bash
/opt/splunk/bin/splunk version
```

Check the Splunk process:

```bash
ps aux | grep splunk
```

Check listening ports:

```bash
sudo ss -tulpn | grep splunk
```

The Splunk Web interface normally listens on:

```text
8000/tcp
```

---

# 7. Create a Dedicated Index

Using a dedicated index makes the lab easier to manage.

Open Splunk Web:

```text
Settings → Indexes → New Index
```

Create:

```text
Index Name: security_lab
```

The same index can be used for the authentication and HTTP logs in this lab.

---

# 8. Authentication Log Monitoring

Ubuntu systems commonly store authentication events in:

```text
/var/log/auth.log
```

Verify that the file exists:

```bash
ls -l /var/log/auth.log
```

View recent events:

```bash
sudo tail -f /var/log/auth.log
```

Generate a normal authentication event by logging in through SSH or using another authentication-related action.

Example:

```bash
ssh <username>@<ubuntu-ip>
```

Then check:

```bash
sudo tail -n 20 /var/log/auth.log
```

---

# 9. Configure Splunk Authentication Log Input

Splunk needs permission to read the log file.

Check the file permissions:

```bash
ls -l /var/log/auth.log
```

On Ubuntu, the file may belong to:

```text
syslog:adm
```

Because Splunk normally runs as the `splunk` user, direct access may need to be configured.

Add the Splunk user to the appropriate log-reading group:

```bash
sudo usermod -aG adm splunk
```

Restart Splunk after changing group membership:

```bash
sudo /opt/splunk/bin/splunk restart
```

Verify the Splunk user:

```bash
id splunk
```

---

# 10. Add auth.log as a Splunk Data Input

Using Splunk Web:

```text
Settings
→ Data Inputs
→ Files & Directories
→ New
```

Select:

```text
File or Directory
```

Specify:

```text
/var/log/auth.log
```

Configure:

```text
Source type: linux_secure
Index: security_lab
Host: <Ubuntu hostname>
```

Save the input.

---

# 11. Verify Authentication Logs

Run:

```spl
index=security_lab sourcetype=linux_secure
```

To display the most recent events:

```spl
index=security_lab sourcetype=linux_secure
| table _time host source sourcetype _raw
| sort - _time
```

Search for SSH activity:

```spl
index=security_lab sourcetype=linux_secure ssh
```

Search for failed authentication:

```spl
index=security_lab sourcetype=linux_secure ("Failed password" OR "authentication failure")
```

Search for successful authentication:

```spl
index=security_lab sourcetype=linux_secure ("Accepted password" OR "Accepted publickey")
```

---

# 12. HTTP/Web Log Monitoring

For HTTP monitoring, install Apache:

```bash
sudo apt install -y apache2
```

Start Apache:

```bash
sudo systemctl enable --now apache2
```

Verify:

```bash
sudo systemctl status apache2
```

Test locally:

```bash
curl http://127.0.0.1
```

The default Apache access log is normally:

```text
/var/log/apache2/access.log
```

The error log is:

```text
/var/log/apache2/error.log
```

Verify:

```bash
ls -lh /var/log/apache2/
```

---

# 13. Generate HTTP Traffic

From another machine such as Kali Linux, access the Ubuntu web server:

```bash
curl http://<UBUNTU-IP>
```

You can also open the following in a browser:

```text
http://<UBUNTU-IP>
```

Generate additional requests:

```bash
curl http://<UBUNTU-IP>/
curl http://<UBUNTU-IP>/index.html
curl http://<UBUNTU-IP>/does-not-exist
```

The requests should appear in:

```bash
sudo tail -f /var/log/apache2/access.log
```

---

# 14. Configure Apache Logs in Splunk

In Splunk Web:

```text
Settings
→ Data Inputs
→ Files & Directories
→ New
```

Add:

```text
/var/log/apache2/access.log
```

Configure:

```text
Source type: access_combined
Index: security_lab
```

Add the Apache error log separately if required:

```text
/var/log/apache2/error.log
```

Use:

```text
Source type: apache_error
Index: security_lab
```

---

# 15. Verify HTTP Logs

Search:

```spl
index=security_lab sourcetype=access_combined
```

Display useful fields:

```spl
index=security_lab sourcetype=access_combined
| table _time clientip method uri_path status
```

Find HTTP errors:

```spl
index=security_lab sourcetype=access_combined status>=400
```

Find 404 requests:

```spl
index=security_lab sourcetype=access_combined status=404
```

Count requests by HTTP status:

```spl
index=security_lab sourcetype=access_combined
| stats count by status
| sort - count
```

Count requests by source IP:

```spl
index=security_lab sourcetype=access_combined
| stats count by clientip
| sort - count
```

---

# 16. Nmap Security Testing

Kali Linux can be used as the security-testing machine.

Verify that Nmap is installed:

```bash
nmap --version
```

Identify the Ubuntu target:

```bash
nmap -sn <NETWORK/CIDR>
```

Perform a basic TCP scan:

```bash
nmap <UBUNTU-IP>
```

Service/version detection:

```bash
nmap -sV <UBUNTU-IP>
```

More detailed lab reconnaissance:

```bash
nmap -sC -sV <UBUNTU-IP>
```

These scans can be used to generate network activity while Splunk monitors the host's available logs.

> Note: Nmap traffic will not automatically appear in Splunk unless the monitored data source records that traffic. For network-level visibility, additional telemetry such as firewall logs, IDS logs, or packet capture data is required.

---

# 17. SSH Security Testing

Verify that SSH is running:

```bash
sudo systemctl status ssh
```

From Kali:

```bash
ssh <username>@<UBUNTU-IP>
```

Generate an intentional failed login for lab testing:

```bash
ssh invaliduser@<UBUNTU-IP>
```

Check the Ubuntu authentication log:

```bash
sudo tail -n 30 /var/log/auth.log
```

Search the events in Splunk:

```spl
index=security_lab sourcetype=linux_secure "Failed password"
```

Group failed attempts by source IP:

```spl
index=security_lab sourcetype=linux_secure "Failed password"
| stats count by src_ip
| sort - count
```

Field extraction can vary depending on the Linux distribution and Splunk sourcetype. If `src_ip` is not extracted automatically, inspect the raw event first:

```spl
index=security_lab sourcetype=linux_secure "Failed password"
| table _time _raw
```

---

# 18. Basic Security Investigation Queries

## Failed SSH Attempts

```spl
index=security_lab "Failed password"
| stats count by host
```

---

## Successful SSH Logins

```spl
index=security_lab ("Accepted password" OR "Accepted publickey")
| stats count by host
```

---

## HTTP Errors

```spl
index=security_lab sourcetype=access_combined status>=400
| stats count by status
```

---

## Top HTTP Clients

```spl
index=security_lab sourcetype=access_combined
| stats count by clientip
| sort - count
| head 10
```

---

## Most Requested URLs

```spl
index=security_lab sourcetype=access_combined
| stats count by uri_path
| sort - count
| head 20
```

---

## Requests by HTTP Method

```spl
index=security_lab sourcetype=access_combined
| stats count by method
```

---

## Potential Web Scanning Activity

A simple lab query for clients generating many different URLs:

```spl
index=security_lab sourcetype=access_combined
| stats dc(uri_path) as unique_paths count as requests by clientip
| where unique_paths > 10
| sort - unique_paths
```

This is only a basic detection heuristic and should not be treated as proof of malicious activity.

---

# 19. Useful SPL Investigation Workflow

A simple investigation workflow is:

```text
1. Identify the event
        ↓
2. Identify the source
        ↓
3. Examine timestamps
        ↓
4. Examine source IP
        ↓
5. Examine destination/host
        ↓
6. Look for repeated activity
        ↓
7. Correlate authentication and web events
        ↓
8. Determine whether additional investigation is required
```

Example:

```spl
index=security_lab
| stats count by sourcetype
```

This provides an overview of the data sources currently indexed.

---

# 20. Data Source Verification

Check all events:

```spl
index=security_lab
```

Check available sourcetypes:

```spl
index=security_lab
| stats count by sourcetype
| sort - count
```

Check hosts:

```spl
index=security_lab
| stats count by host
| sort - count
```

Check sources:

```spl
index=security_lab
| stats count by source
| sort - count
```

Check the latest events:

```spl
index=security_lab
| table _time host source sourcetype _raw
| sort - _time
| head 50
```

---

# 21. Useful Splunk CLI Commands

List monitored inputs:

```bash
sudo -u splunk /opt/splunk/bin/splunk list monitor
```

List configured forward servers:

```bash
sudo -u splunk /opt/splunk/bin/splunk list forward-server
```

Check Splunk status:

```bash
sudo /opt/splunk/bin/splunk status
```

Restart Splunk:

```bash
sudo /opt/splunk/bin/splunk restart
```

Stop Splunk:

```bash
sudo /opt/splunk/bin/splunk stop
```

Start Splunk:

```bash
sudo /opt/splunk/bin/splunk start
```

---

# 22. Troubleshooting

## Splunk Is Not Starting

Check status:

```bash
sudo /opt/splunk/bin/splunk status
```

Check Splunk logs:

```bash
sudo tail -n 100 /opt/splunk/var/log/splunk/splunkd.log
```

Check whether port 8000 is already in use:

```bash
sudo ss -lntp | grep 8000
```

---

## Splunk Cannot Read auth.log

Check permissions:

```bash
ls -l /var/log/auth.log
```

Check the Splunk user's groups:

```bash
id splunk
```

Add Splunk to the `adm` group:

```bash
sudo usermod -aG adm splunk
```

Restart Splunk:

```bash
sudo /opt/splunk/bin/splunk restart
```

If the input still does not work, verify access as the Splunk user:

```bash
sudo -u splunk head /var/log/auth.log
```

Do not weaken system log permissions unnecessarily just to make Splunk read the file.

---

## No Events Appearing in Splunk

First confirm that the log is actually receiving events:

```bash
sudo tail -f /var/log/auth.log
```

or:

```bash
sudo tail -f /var/log/apache2/access.log
```

Then verify the Splunk input:

```bash
sudo -u splunk /opt/splunk/bin/splunk list monitor
```

Search the correct index:

```spl
index=security_lab
```

If necessary, check Splunk's internal logs:

```spl
index=_internal
```

---

## Apache Logs Are Empty

Check Apache:

```bash
sudo systemctl status apache2
```

Generate traffic:

```bash
curl http://127.0.0.1
```

Then verify:

```bash
sudo tail -n 20 /var/log/apache2/access.log
```

---

## HTTP Logs Are Indexed but Fields Are Missing

Inspect the raw event:

```spl
index=security_lab sourcetype=access_combined
| table _raw
```

Check the source type configured for the input.

The exact extracted field names can depend on the log format and Splunk configuration.

---

# 23. Lab Validation Checklist

Use this checklist after completing the setup.

### Splunk

- [ ] Splunk Enterprise installed
- [ ] Splunk Web accessible
- [ ] Splunk service running
- [ ] `security_lab` index created

### Linux Authentication

- [ ] `/var/log/auth.log` exists
- [ ] Splunk can read `auth.log`
- [ ] Authentication input configured
- [ ] Authentication events visible in Splunk

### HTTP Monitoring

- [ ] Apache installed
- [ ] Apache service running
- [ ] `access.log` receiving events
- [ ] Apache input configured
- [ ] HTTP events visible in Splunk

### Security Testing

- [ ] Kali Linux available
- [ ] Nmap installed
- [ ] Ubuntu target reachable
- [ ] SSH testing performed
- [ ] HTTP requests generated
- [ ] Security events visible in Splunk

### SPL

- [ ] Authentication events searchable
- [ ] HTTP events searchable
- [ ] Failed authentication query tested
- [ ] HTTP error query tested
- [ ] Source IP analysis tested
- [ ] Data-source verification tested

---

# 24. Expected Result

After completing this setup, the lab should provide the following data flow:

```text
Kali Linux
   │
   ├── Nmap
   ├── SSH Testing
   └── HTTP Requests
          │
          ▼
     Ubuntu Linux
          │
          ├── /var/log/auth.log
          │
          └── /var/log/apache2/access.log
          │
          ▼
       Splunk
          │
          ▼
    security_lab index
          │
          ▼
      SPL Analysis
          │
          ├── Authentication Monitoring
          ├── HTTP Monitoring
          ├── Source IP Analysis
          ├── Error Analysis
          └── Security Investigation
```

The resulting dashboards and screenshots are stored in:

```text
screenshots/
├── http-logs-dashboard.png
└── ssh-logs-dashboard.png
```

---

# 25. Project Structure

The final repository structure is:

```text
Splunk-Home-Lab/
│
├── README.md
├── setup.md
├── .gitignore
│
├── dashboards/
│   └── README.md
│
└── screenshots/
    ├── http-logs-dashboard.png
    └── ssh-logs-dashboard.png
```

This setup provides a reproducible Splunk-based security monitoring environment suitable for practicing log ingestion, SPL investigation, authentication monitoring, HTTP analysis, and basic SOC workflows.
