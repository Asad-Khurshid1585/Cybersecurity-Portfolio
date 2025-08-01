# Wazuh SIEM with Grafana Integration

## Introduction
Security Information and Event Management (SIEM) systems are essential for improving an organization's security posture. This project demonstrates the deployment and configuration of the Wazuh SIEM solution, integrated with Grafana for advanced monitoring and analysis. The integration enables centralized log management, real-time event monitoring, and interactive dashboards for effective security data visualization.

---

## Table of Contents
- [Features](#features)
- [Architecture Overview](#architecture-overview)
- [Installation Guide](#installation-guide)
  - [Wazuh Indexer Cluster Installation](#wazuh-indexer-cluster-installation)
  - [Agent Installation](#agent-installation)
  - [Grafana Installation](#grafana-installation)
  - [Filebeat Installation](#filebeat-installation)
  - [Elasticsearch Installation](#elasticsearch-installation)
- [Grafana Dashboard Setup](#grafana-dashboard-setup)
- [Conclusion](#conclusion)

---

## Features
- Centralized log management
- Real-time event monitoring
- Interactive dashboards with Grafana
- Secure communication between components
- Scalable and user-friendly SIEM environment

---

## Architecture Overview
- **Wazuh Server & Indexer**: Collects and indexes security events.
- **Wazuh Agent**: Installed on endpoints to forward logs.
- **Filebeat**: Ships Wazuh logs to Elasticsearch.
- **Elasticsearch**: Stores and indexes logs for analysis.
- **Grafana**: Visualizes security data via dashboards.

---

## Installation Guide

### Wazuh Indexer Cluster Installation
1. **Download the Wazuh installation assistant and configuration file:**
   ```sh
   curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh
   curl -sO https://packages.wazuh.com/4.9/config.yml
   ```
2. **Edit `./config.yml`:**
   - Replace node names and IPs for all Wazuh server, indexer, and dashboard nodes.
3. **Generate configuration files:**
   ```sh
   sudo bash wazuh-install.sh --generate-config-files
   ```
   - Credentials are stored in `Wazuh-install-files` for dashboard login.

### Agent Installation
- Install the Wazuh agent on Ubuntu endpoints via the command line.
- Registered agents will appear on the Wazuh server dashboard.

### Grafana Installation
1. **Install Grafana** on the server.
2. **Configure Grafana** to use Elasticsearch as a data source.
3. **Create dashboards** to visualize Wazuh metrics and logs.

### Filebeat Installation
1. **Install Filebeat** on the server.
2. **Configure `filebeat.yml`:**
   ```yaml
   filebeat.inputs:
     - type: log
       enabled: true
       paths:
         - /var/ossec/logs/ossec.log
       fields:
         log_type: wazuh_agent
       fields_under_root: true
       json.keys_under_root: true

   processors:
     - dissect:
         tokenizer: "%{timestamp} %{log_level}: %{message}"
         field: "message"
         target_prefix: "wazuh"
     - timestamp:
         field: "timestamp"
         layouts:
           - "2006/01/02 15:04:05"
         target_field: "@timestamp"
     - drop_fields:
         fields: ["timestamp"]

   output.elasticsearch:
     hosts: ["localhost:9200"]
     index: "wazuh-agent-%{+yyyy.MM.dd}"

   setup.template.name: "wazuh-agent"
   setup.template.pattern: "wazuh-agent-*"
   setup.ilm.enabled: true
   ```

### Elasticsearch Installation
- **Create index pattern:**
  ```sh
  curl -X PUT "localhost:9200/_template/wazuh-agent" -H 'Content-Type: application/json' -d'
  {
    "index_patterns": ["wazuh-agent-*"] ,
    "settings": { "number_of_shards": 1 },
    "mappings": {
      "properties": {
        "@timestamp": { "type": "date" },
        "wazuh.log_level": { "type": "keyword" },
        "wazuh.message": { "type": "text" }
      }
    }
  }'
  ```

---

## Grafana Dashboard Setup
- Add Elasticsearch as a data source in Grafana.
- Import or create dashboards to visualize Wazuh logs and metrics.

---

## Conclusion
Integrating Wazuh with Grafana provides a robust SIEM solution for enhanced security monitoring and log analysis. This setup streamlines incident response, improves threat detection, and supports compliance efforts. By following this guide, organizations can establish a secure, scalable, and user-friendly SIEM environment with actionable insights for proactive threat mitigation.

---

## References
- [Wazuh Documentation](https://documentation.wazuh.com/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Elasticsearch Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
