# 🚀 Enterprise Infrastructure as Code - Ansible Project

> **Production-ready, enterprise-grade infrastructure automation with advanced DevOps practices**

[![Ansible](https://img.shields.io/badge/Ansible-2.15+-red.svg)](https://www.ansible.com/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Code Style](https://img.shields.io/badge/Code%20Style-YAML%20Lint-green.svg)](https://yamllint.readthedocs.io/)

## 📋 Table of Contents

- [Features](#features)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Deployment Strategies](#deployment-strategies)
- [Advanced Usage](#advanced-usage)
- [Monitoring & Observability](#monitoring--observability)
- [Security](#security)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

## ✨ Features

### 🎯 Core Features
- **Multi-Environment Support**: Production, Staging, Development
- **Zero-Downtime Deployments**: Blue-Green, Canary, Rolling updates
- **High Availability**: Load balancing, database replication, auto-scaling
- **Infrastructure as Code**: Complete infrastructure defined in version control
- **Idempotent Operations**: Safe to run multiple times
- **Modular Design**: Reusable roles and playbooks

### 🔒 Security Features
- SSH hardening with key-based authentication
- Firewall configuration (UFW)
- Fail2ban for brute-force protection
- SSL/TLS certificate management (Let's Encrypt)
- Secrets management with Ansible Vault
- Security scanning and compliance checks
- Two-factor authentication support

### 📊 Observability Stack
- **Monitoring**: Prometheus + Grafana
- **Logging**: ELK Stack (Elasticsearch, Logstash, Kibana)
- **Alerting**: AlertManager with Slack/PagerDuty integration
- **APM**: Application Performance Monitoring
- **Distributed Tracing**: Jaeger support

### 🔄 CI/CD Integration
- Jenkins pipeline integration
- GitLab Runner support
- ArgoCD for GitOps
- Automated testing and validation
- Rollback capabilities

### 💾 Backup & Disaster Recovery
- Automated database backups
- 3-2-1 backup strategy
- Point-in-time recovery
- Cross-region replication
- Disaster recovery procedures

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Load Balancer (ALB)                      │
│                     SSL Termination + WAF                       │
└────────────────────────┬────────────────────────────────────────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
    ┌────▼────┐    ┌────▼────┐    ┌────▼────┐
    │  Web 01 │    │  Web 02 │    │  Web 03 │
    │ (Nginx) │    │ (Nginx) │    │ (Nginx) │
    └────┬────┘    └────┬────┘    └────┬────┘
         │               │               │
         └───────────────┼───────────────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
    ┌────▼────┐    ┌────▼────┐    ┌────▼────┐
    │  App 01 │    │  App 02 │    │  App 03 │
    │(Node.js)│    │(Node.js)│    │(Node.js)│
    └────┬────┘    └────┬────┘    └────┬────┘
         │               │               │
         └───────────────┼───────────────┘
                         │
         ┌───────────────┴───────────────┐
         │                               │
    ┌────▼──────────┐           ┌───────▼───────┐
    │  PostgreSQL   │◄─────────►│  PostgreSQL   │
    │   Primary     │ Replication│    Replica    │
    └───────┬───────┘           └───────────────┘
            │
    ┌───────▼───────┐
    │  Redis Cache  │
    │    Cluster    │
    └───────────────┘

    Monitoring & Logging Layer:
    ┌─────────────┐  ┌──────────┐  ┌─────────────┐
    │ Prometheus  │  │ Grafana  │  │   ELK Stack │
    └─────────────┘  └──────────┘  └─────────────┘
```

## 📋 Prerequisites

### Required Software
- **Ansible**: 2.15 or higher
- **Python**: 3.8 or higher
- **SSH**: OpenSSH client
- **Git**: For version control

### Infrastructure Requirements
- Target servers with SSH access
- Minimum 2GB RAM per server
- Ubuntu 22.04 LTS or Debian 11+ (recommended)
- Root or sudo access on target servers

### Cloud Provider Support
- ✅ AWS (EC2, RDS, S3)
- ✅ Azure (VM, Database, Storage)
- ✅ GCP (Compute Engine, Cloud SQL)
- ✅ DigitalOcean
- ✅ VMware
- ✅ Bare Metal

## 🚀 Quick Start

### 1. Clone the Repository
```bash
git clone https://github.com/your-org/ansible-infrastructure.git
cd ansible-infrastructure
```

### 2. Install Dependencies
```bash
# Install Ansible
pip3 install ansible

# Install required collections
ansible-galaxy collection install -r requirements.yml

# Install required roles
ansible-galaxy role install -r requirements.yml
```

### 3. Configure Inventory
```bash
# Edit inventory file
vim inventory/production/hosts.yml

# Set your server IPs and credentials
```

### 4. Configure Variables
```bash
# Edit main configuration
vim infrastructure.yml

# Customize the infrastructure_config section at the top
```

### 5. Setup Secrets
```bash
# Create vault password file
echo "your-strong-password" > .vault_pass
chmod 600 .vault_pass

# Create encrypted secrets
ansible-vault create group_vars/all/vault.yml
```

Add your secrets:
```yaml
vault_grafana_password: "secure-password"
vault_slack_webhook: "https://hooks.slack.com/..."
vault_db_password: "database-password"
```

### 6. Run Playbook
```bash
# Test connection
ansible all -i inventory/production/hosts.yml -m ping

# Deploy infrastructure (dry-run)
ansible-playbook -i inventory/production/hosts.yml infrastructure.yml --check

# Deploy infrastructure (for real)
ansible-playbook -i inventory/production/hosts.yml infrastructure.yml

# Deploy specific components
ansible-playbook -i inventory/production/hosts.yml infrastructure.yml --tags "web,database"
```

## 📁 Project Structure

```
ansible-infrastructure/
├── README.md                          # This file
├── infrastructure.yml                 # Main playbook
├── advanced_deployment.yml            # Advanced deployment strategies
├── ansible.cfg                        # Ansible configuration
├── requirements.yml                   # Ansible dependencies
├── .vault_pass                       # Vault password (gitignored)
├── .ansible-lint                     # Linting rules
│
├── inventory/
│   ├── production/
│   │   ├── hosts.yml                 # Production servers
│   │   └── group_vars/
│   │       ├── all.yml               # Global variables
│   │       ├── web.yml               # Web tier variables
│   │       └── database.yml          # Database variables
│   ├── staging/
│   │   └── hosts.yml
│   └── development/
│       └── hosts.yml
│
├── group_vars/
│   ├── all/
│   │   ├── vars.yml                  # Common variables
│   │   └── vault.yml                 # Encrypted secrets
│   ├── production.yml
│   ├── staging.yml
│   └── development.yml
│
├── host_vars/
│   ├── web-01.yml                    # Host-specific variables
│   └── db-01.yml
│
├── roles/                            # Ansible roles
│   ├── common/                       # Common setup
│   │   ├── tasks/
│   │   ├── handlers/
│   │   ├── templates/
│   │   ├── files/
│   │   ├── vars/
│   │   └── defaults/
│   ├── security/                     # Security hardening
│   ├── web/                          # Web servers
│   ├── app/                          # Application servers
│   ├── database/                     # Database setup
│   ├── cache/                        # Redis/Memcached
│   ├── monitoring/                   # Prometheus/Grafana
│   ├── logging/                      # ELK stack
│   └── backup/                       # Backup solutions
│
├── playbooks/                        # Additional playbooks
│   ├── deploy.yml                    # Application deployment
│   ├── rollback.yml                  # Rollback procedure
│   ├── backup.yml                    # Backup operations
│   ├── restore.yml                   # Restore operations
│   ├── scale.yml                     # Scaling operations
│   └── maintenance.yml               # Maintenance tasks
│
├── templates/                        # Jinja2 templates
│   ├── nginx.conf.j2
│   ├── postgresql.conf.j2
│   └── prometheus.yml.j2
│
├── files/                            # Static files
│   ├── ssl/
│   └── scripts/
│
├── scripts/                          # Helper scripts
│   ├── deploy.sh                     # Deployment wrapper
│   ├── rollback.sh                   # Rollback wrapper
│   └── health-check.sh               # Health check script
│
├── docs/                             # Documentation
│   ├── architecture.md
│   ├── deployment-strategies.md
│   ├── troubleshooting.md
│   └── runbooks/
│
└── tests/                            # Testing
    ├── integration/
    ├── unit/
    └── molecule/                     # Molecule tests
```

## ⚙️ Configuration

### Main Configuration File

Edit `infrastructure.yml` at the top:

```yaml
infrastructure_config:
  global:
    environment: "production"
    region: "eu-west-1"
    domain: "yourdomain.com"
    
  compute:
    web_servers:
      count: 3
      instance_type: "t3.medium"
```

### Environment-Specific Configuration

```bash
# Production
vim group_vars/production.yml

# Staging
vim group_vars/staging.yml

# Development
vim group_vars/development.yml
```

### Secrets Management

```bash
# View encrypted file
ansible-vault view group_vars/all/vault.yml

# Edit encrypted file
ansible-vault edit group_vars/all/vault.yml

# Encrypt existing file
ansible-vault encrypt group_vars/all/secrets.yml

# Decrypt file
ansible-vault decrypt group_vars/all/secrets.yml
```

## 🎯 Deployment Strategies

### 1. Blue-Green Deployment

Zero-downtime deployment with instant rollback capability.

```bash
ansible-playbook -i inventory/production/hosts.yml advanced_deployment.yml \
  -e "deployment_strategy=blue_green" \
  -e "app_version=v2.0.0"
```

**How it works:**
1. Deploy new version to "green" environment
2. Run health checks on green
3. Switch traffic from "blue" to "green"
4. Keep blue running as backup
5. Stop blue after verification

### 2. Canary Deployment

Gradual rollout to percentage of users.

```bash
ansible-playbook -i inventory/production/hosts.yml advanced_deployment.yml \
  -e "deployment_strategy=canary" \
  -e "canary_percentage=10" \
  -e "app_version=v2.0.0"
```

**How it works:**
1. Deploy new version to canary instance
2. Route 10% of traffic to canary
3. Monitor metrics and error rates
4. Gradually increase percentage
5. Full rollout or rollback based on metrics

### 3. Rolling Update

Sequential update of servers one by one.

```bash
ansible-playbook -i inventory/production/hosts.yml advanced_deployment.yml \
  -e "deployment_strategy=rolling" \
  -e "app_version=v2.0.0"
```

## 🔍 Monitoring & Observability

### Access Monitoring Tools

```bash
# Grafana
https://grafana.yourdomain.com
User: admin
Pass: [from vault]

# Prometheus
https://prometheus.yourdomain.com

# Kibana
https://kibana.yourdomain.com

# AlertManager
https://alerts.yourdomain.com
```

### Key Metrics Monitored

- **System**: CPU, Memory, Disk, Network
- **Application**: Request rate, Error rate, Latency
- **Database**: Connections, Query performance, Replication lag
- **Cache**: Hit rate, Memory usage, Evictions

### Alerting Rules

Configured alerts:
- High CPU usage (>80% for 5 min)
- High memory usage (>90%)
- Disk space low (<10%)
- Application errors (>1% error rate)
- Database replication lag (>100MB)
- SSL certificate expiration (<30 days)

## 🔐 Security

### Security Hardening Applied

✅ SSH hardening (key-based auth only)
✅ Firewall configuration (UFW)
✅ Fail2ban for brute-force protection
✅ Automatic security updates
✅ SELinux/AppArmor enabled
✅ Audit logging (auditd)
✅ CIS Benchmark compliance
✅ Regular security scanning

### Compliance

- **PCI DSS**: Database encryption, access controls
- **GDPR**: Data encryption, audit logs
- **HIPAA**: Access controls, audit trails
- **SOC 2**: Monitoring, logging, access management

### Security Scanning

```bash
# Run security audit
ansible-playbook -i inventory/production/hosts.yml playbooks/security-audit.yml

# Check for CVEs
ansible-playbook -i inventory/production/hosts.yml playbooks/vulnerability-scan.yml
```

## 🐛 Troubleshooting

### Common Issues

#### Connection Issues
```bash
# Test connectivity
ansible all -i inventory/production/hosts.yml -m ping

# Check SSH config
ssh -vvv user@host

# Verify SSH key
ansible all -i inventory/production/hosts.yml -m shell -a "echo success"
```

#### Deployment Failures
```bash
# Run in verbose mode
ansible-playbook -i inventory/production/hosts.yml infrastructure.yml -vvv

# Check specific host
ansible-playbook -i inventory/production/hosts.yml infrastructure.yml --limit web-01

# Dry run
ansible-playbook -i inventory/production/hosts.yml infrastructure.yml --check
```

#### Rollback
```bash
# Automatic rollback (built into advanced_deployment.yml)
ansible-playbook -i inventory/production/hosts.yml playbooks/rollback.yml \
  -e "target_version=v1.9.0"
```

### Logs

```bash
# Ansible logs
tail -f /var/log/ansible.log

# Application logs
ansible web -i inventory/production/hosts.yml -m shell \
  -a "tail -100 /var/log/app/application.log"

# System logs
ansible all -i inventory/production/hosts.yml -m shell \
  -a "journalctl -u nginx -n 50"
```

## 🧪 Testing

### Syntax Check
```bash
ansible-playbook infrastructure.yml --syntax-check
```

### Lint
```bash
ansible-lint infrastructure.yml
yamllint infrastructure.yml
```

### Molecule Testing
```bash
cd roles/web
molecule test
```

### Integration Tests
```bash
ansible-playbook tests/integration/test-full-stack.yml
```

## 📚 Advanced Usage

### Custom Fact Gathering
```bash
ansible all -i inventory/production/hosts.yml -m setup \
  -a "filter=ansible_distribution*"
```

### Ad-hoc Commands
```bash
# Check disk space on all servers
ansible all -i inventory/production/hosts.yml \
  -m shell -a "df -h"

# Restart service on specific group
ansible web -i inventory/production/hosts.yml \
  -m systemd -a "name=nginx state=restarted"

# Update packages
ansible all -i inventory/production/hosts.yml \
  -m apt -a "upgrade=dist" --become
```

### Dynamic Inventory

```bash
# AWS EC2
ansible-playbook -i aws_ec2.yml infrastructure.yml

# Azure
ansible-playbook -i azure_rm.yml infrastructure.yml

# GCP
ansible-playbook -i gcp_compute.yml infrastructure.yml
```

## 🎓 Best Practices

1. **Always use version control** for your Ansible code
2. **Test in staging** before production deployment
3. **Use Ansible Vault** for sensitive data
4. **Implement idempotency** in all tasks
5. **Document everything** in comments and README
6. **Use roles** for reusability
7. **Tag your playbooks** for selective execution
8. **Monitor your deployments** with metrics
9. **Implement rollback** procedures
10. **Regular backups** and disaster recovery testing

## 🤝 Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## 📄 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

## 👥 Authors

- DevOps Team - *Initial work*

## 🙏 Acknowledgments

- Ansible community
- DevOps best practices
- Cloud provider documentation

## 📞 Support

- **Documentation**: [docs/](docs/)
- **Issues**: GitHub Issues
- **Slack**: #devops channel
- **Email**: devops@example.com

---

**Made with ❤️ by DevOps Team**
