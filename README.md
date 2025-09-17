# Baremetal Observability Ansible Playbook

An Ansible playbook that deploys IPMI monitoring infrastructure on OpenShift clusters to collect hardware metrics from baremetal nodes.

In the future, this will also include SNMP metrics.

## Overview

This playbook automates the deployment of:

1. **Monitoring Namespace**: Creates a dedicated namespace with cluster monitoring labels
2. **IPMI Exporter**: Collects hardware metrics (temperature, power, fan speeds, etc.) from baremetal nodes via IPMI
3. **Service**: Exposes the IPMI exporter through this internal cluster service
4. **ServiceMonitor**: Configures Prometheus to scrape IPMI metrics with proper relabeling

## What it does

- Queries OpenStack to discover all baremetal nodes and their IPMI addresses
- Extracts IPMI addresses with appropriate ports (defaults to 623)
- Deploys IPMI exporter with configurable authentication
- Creates Kubernetes services for the exporters
- Generates ServiceMonitor configurations with proper relabelings for each IPMI target
- Integrates with OpenShift cluster monitoring and ACM observability

## Usage

This is designed to run on cloudkit-aap after cluster installation (either OCP or baremetal) to enable hardware monitoring for baremetal infrastructure.

Run this command to deploy:
```
ansible-playbook -i inventory playbook.yaml -e bmo_state=present
```

Run this command to undeploy:
```
ansible-playbook -i inventory playbook.yaml -e bmo_state=absent`
```
