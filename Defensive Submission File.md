# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology

The following machines were identified on the network:
- ML-RefVm-684427
  - **Operating System**: Windows 10
  - **Purpose**: Hosts the hypervisor which contains the rest of the virtual machines used in this project.
  - **IP Address**: 192.168.1.1
- Capstone
  - **Operating System**: Ubuntu 18.04.1 LTS
  - **Purpose**: Filebeat and Metricbeat are installed and will forward logs to the ELK machine. This VM is in the network solely for the purpose of testing alerts.
  - **IP Address**: 192.168.1.105
- ELK
  - **Operating System**: Ubuntu 18.04.4 LTS
  - **Purpose**: Hosts the ELK stack and Kibana dashboard.
  - **IP Address**: 192.168.1.100
- Kali
  - **Operating System**: Kali 2020.1
  - **Purpose**: A standard Kali Linux machine used to pentest other machines on the network.
  - **IP Address**: 192.168.1.90
- Target 1
  - **Operating System**: Debian 8
  - **Purpose**: Exposes a vulnerable WordPress server. We will attempt to penetrate this server.
  - **IP Address**: 192.168.1.110
- Target 2
  - **Operating System**: Debian 8
  - **Purpose**: A secondary web server which can be attacked as well.
  - **IP Address**: 192.168.1.115


### Description of Targets

The target of this attack was: `Target 1` (192.168.1.110).

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### Excessive HTTP Errors

Alert 1 is implemented as follows:
  - **Metric**: 'http.response.status_code'
  - **Threshold**: More than 5 HTTP error responses in 5 minutes
  - **Vulnerability Mitigated**: This could indicate improper error handling on the server which may lead to the application being vulnerable to an attack.
  - **Reliability**: Medium

#### HTTP Request Size Monitor
Alert 2 is implemented as follows:
  - **Metric**: http.request.bytes
  - **Threshold**: Sum of HTTP request bytes is greater than 3500 for the last 1 minute
  - **Vulnerability Mitigated**: This could indicate a potential DDOS attack against the server. Amplified requests may overwhelm the server with excessive data.
  - **Reliability**: Medium

#### CPU Usage Monitor
Alert 3 is implemented as follows:
  - **Metric**: system.process.cpu.total.pct
  - **Threshold**: Usage is greater than 50% for the last 5 minutes.
  - **Vulnerability Mitigated**: If the CPU usage continually stays high, the server may become unresponsive causing downtime.
  - **Reliability**: High

_TODO Note: Explain at least 3 alerts. Add more if time allows._
