# Baremetal Observability Ansible Playbook

An Ansible playbook that deploys Prometheus metric exporters on OpenShift clusters to collect hardware and network metrics from baremetal infrastructure. This is an implementation of the Open Sovereign AI Cloud (OSAC) [enhancement proposal](https://github.com/innabox/enhancement-proposals/pull/10).

## Overview

This playbook automates the deployment of multiple metric exporters using a configurable inventory service:

1. **Monitoring Namespace**: Creates a dedicated `baremetal-observability` namespace
2. **Inventory Service**: Is reusable across different types of inventory services
3. **Metric Exporters**: Pick and choose which exporters to use
4. **Configuration Management**: Creates default ConfigMaps and Secrets for exporter configurations

## Configuration

### Required Variables
- `baremetal_observability_state`: Variable to indicate whether to deploy or destroy baremetal-observability

### Optional Variables
- `baremetal_observability_inventory_service`: Role name that provides BMC endpoint discovery
- `baremetal_observability_exporters`: List of exporter roles to deploy (e.g., `['ipmi_exporter', 'snmp_exporter']`)
- `baremetal_observability_namespace`: Namespace name (default: `baremetal-observability`)
- `baremetal_observability_default_ipmi_port`: Default IPMI port (default: `623`)
- `baremetal_observability_default_snmp_port`: Default SNMP port (default: `161`)
- `baremetal_observability_default_scrape_interval`: Scraping interval (default: `60s`)

## Usage

### Prerequisites
- OpenShift cluster with cluster monitoring enabled
- Ansible with `kubernetes.core` and `openstack.cloud` collections
- Access to BMC/IPMI endpoints from the cluster network

### Installation
```bash
ansible-galaxy collection install -r requirements.yml
```

### Deployment
```bash
ansible-playbook -i inventory playbook.yaml -e baremetal_observability_state=present -e baremetal_observability_inventory_service=openstack -e baremetal_observability_exporters='["ipmi_exporter","snmp_exporter"]'
```

### Removal
```bash
# Remove all resources (deletes the baremetal-observability namespace)
ansible-playbook -i inventory playbook.yaml -e baremetal_observability_state=absent
```
