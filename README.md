# Anchore On-Prem Feed Service

## Background

Our open source Anchore Engine uses security vulnerability and package from the following sources:

- Linux Distributions
    - Alpine Linux
    - CentOS
    - Debian
    - Oracle Linux
    - Red Hat Enterprise Linux
    - Ubuntu

- Software Package Repositories
    - NpmJS.org
    - RubyGems.org

- NIST National Vulnerability Database (NVD)

The Anchore Feed Service collects vulnerability and package data from upstream sources and normalizes this data to be published as feeds that the Anchore Engine can subscribe to. 

Anchore Engine polls the feed service at a user defined interval (default is six hours), and will download feed data updated since the last sync.

The Anchore Enterprise On-Premise Feed Service allows for our customers to sync their existing Anchore Engine deployment with a locally deployed feed service, without any reliance on our cloud feed service. 

## Requirements

The Anchore Enterprise Feed Service is delivered as a Docker container that can be run on any compatible runtime.

- Runtime
    - Docker compatible runtime (version 1.13 or higher)

- Database 
    - PostgreSQL version 9.6 or higher database
      This database can be run in a container, as configured in the example Docker Compose file, or can be provided as an external service to the Anchore Feed Service.
      Amazon RDS for PostgreSQL and similar technologies can be used to high available cloud deployments.
      Requirement: 20 GB storage.

- Memory
    - The Anchore Feed Service Module should be allocated a minimum 4 GB of memory.

- Storage
    - Configuration volume
      This volume is used to provide persistant storage to the container from which it will read its configuration files and optionally add certificates.
      Requirement: Less than 1 MB.

    - Data volume
      This volume is used to store nomalized feed data.
      Requirement: 10 GB.

- Network
    - Ingress
        - The Anchore Feed Service module exposes a RESTful API by default on port 8448 however this port can be remapped.

    - Egress
        - The Anchore Feed Service module requires access to the upstream data feeds from supported Linux distributions and package registries.

    | Host | Port | Description |
    | --- | --- |
    | linux.oracle.com | 443  | Oracle Linux Security Feed | 
    | git.alpinelinux.com | 443 | Alpine Linux Security database |