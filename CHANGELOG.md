# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v1.0.0] - 2025-11-24

### Added

#### Core Instance Management
- Primary Cloud SQL PostgreSQL instance provisioning with comprehensive configuration options
- Support for PostgreSQL versions 9.6, 14, 15, 16, and 17
- Instance type support: `CLOUD_SQL_INSTANCE`, `ON_PREMISES_INSTANCE`, and `READ_REPLICA_INSTANCE`
- Random instance name suffix generation for unique naming
- Configurable deletion protection for instance and databases
- Root password configuration during instance creation

#### High Availability & Replication
- Regional high availability configuration with `REGIONAL` availability type
- Read replica support with flexible configuration per replica
- Master-replica relationship management for failover scenarios
- Disaster recovery (DR) replica configuration with `failover_dr_replica_name`
- Cross-region read replica support with encryption key requirements
- Automated point-in-time recovery for regional instances

#### Backup & Recovery
- Automated backup configuration with customizable start time
- Point-in-time recovery (PITR) enablement
- Transaction log retention settings (1-7 days)
- Backup retention policy with configurable retained backups and retention unit
- Location-specific backup storage
- Retain backups on instance deletion option

#### Storage & Performance
- Disk autoresize with configurable limits
- Disk type selection: `PD_SSD`, `PD_HDD`
- Initial disk size configuration (minimum 10GB)
- Data cache support for ENTERPRISE_PLUS edition
- Performance optimization flags and database-specific configurations

#### Networking & Security
- Private network connectivity with VPC integration
- Private Service Connect (PSC) configuration with allowed consumer projects
- IPv4 public IP enablement/disablement
- SSL/TLS mode configuration with server CA management
- Authorized networks with expiration time support
- Private path for Google Cloud services
- Custom subject alternative names for certificates
- Connector enforcement for Cloud SQL Auth Proxy/Connector

#### Database & User Management
- Default database creation with charset and collation support
- Multiple additional databases with individual configurations
- Default user creation with auto-generated secure passwords
- Additional users with random or custom password options
- IAM-based authentication support for users, service accounts, and groups
- Password validation policies with complexity requirements
- User password policies with expiration and failed attempts tracking
- Database and user deletion policies (ABANDON option)

#### Encryption & Key Management
- Customer-managed encryption keys (CMEK) support
- Google Cloud KMS Autokey integration
- KMS key handle management for automatic encryption
- Cross-region encryption key support for replicas

#### Monitoring & Observability
- Query Insights configuration with customizable settings
- Query plans per minute tracking
- Query string length configuration
- Application tags and client address recording
- Maintenance window scheduling with update track selection
- Deny maintenance period configuration

#### Integration & Advanced Features
- Google ML integration enablement
- Dataplex integration support
- Database integration roles for GCP service connectivity
- Edition support: `ENTERPRISE` and `ENTERPRISE_PLUS`
- Activation policy: `ALWAYS`, `NEVER`, `ON_DEMAND`
- Pricing plan configuration
- User labels for resource organization
- Location preference with zone and secondary zone
- Google App Engine application follow support

### Outputs

#### Primary Instance Outputs
- `instance_name` - Primary instance name
- `instance_connection_name` - Connection string for Cloud SQL Proxy
- `public_ip_address` - First public IPv4 address
- `private_ip_address` - First private IPv4 address
- `instance_ip_address` - All IP addresses
- `instance_first_ip_address` - First assigned IP address
- `instance_self_link` - Instance URI
- `instance_server_ca_cert` - CA certificate for SSL connections (sensitive)
- `instance_service_account_email_address` - Service account email
- `instance_psc_attachment` - PSC service attachment link
- `dns_name` - DNS name of instance endpoint

#### Read Replica Outputs
- `read_replica_instance_names` - List of replica instance names
- `replicas_instance_connection_names` - Replica connection strings
- `replicas_instance_first_ip_addresses` - Replica IP addresses
- `replicas_instance_self_links` - Replica URIs
- `replicas_instance_server_ca_certs` - Replica CA certificates (sensitive)
- `replicas_instance_service_account_email_addresses` - Replica service accounts
- `replicas_instance_psc_attachments` - Replica PSC attachment links

#### User & Database Outputs
- `generated_user_password` - Auto-generated default user password (sensitive)
- `additional_users` - List of additional users with passwords (sensitive)
- `additional_user_passwords_map` - Map of username to password (sensitive)
- `iam_users` - List of IAM users with access

#### Resource Outputs
- `primary` - Complete primary instance resource (sensitive)
- `replicas` - List of replica instance resources (sensitive)
- `instances` - All instance resources combined (sensitive)

#### Environment & Integration Outputs
- `env_vars` - Exported environment variables for application configuration
- `apphub_service_uri` - Service URI for AppHub integration

### Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.5.0 |
| google | >= 7.0.0, < 8.0.0 |
| google-beta | >= 7.0.0, < 8.0.0 |

### Notes
- Default tier is `db-f1-micro` for cost-effective development environments
- REGIONAL availability type automatically enables PITR and backups
- Password complexity follows security best practices with configurable policies
- Read replicas in different regions require encryption keys
- KMS key handles cannot be deleted from GCP (removed from state only)
- Disk size changes are ignored in lifecycle to prevent disruptions