# Anchore On-Premise Feed Service

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
| :--- | :--- | :--- |
| linux.oracle.com | 443 | Oracle Linux Security Feed |
| git.alpinelinux.com | 443 | Alpine Linux Security database |
| github.com | 443 | Alpine Linux Security database |
| redhat.com | 443 | Red Hat Enterprise Linux security feed |
| security-tracker.debian.org | 443 | Debian security feed |
| salsa.debian.org | 443 | Debian security feed |
| replicate.npmjs.com | 443 | NPM Registry package data |
| s3-us-west-2.amazonaws.com | 443 | Ruby Gems data feed (stored in Amazon S3) |
| static.nvd.nist.gov | 443 | NVD Database |
| launchpad.net/ubuntu-cve-tracker | 443 | Ubuntu data |


## Feed Service Installation

Make sure you've satisfied all of the requirememnts first.

See the sample config files from this repository: https://github.com/valancej/anchore-feed-service

This sample installation uses Docker Compose to run the Anchore Enterprise Feed Service and PostgreSQL database. 

Be sure to update the volumes in the config file to match the correct directory referenced locally. 

### Start the Anchore Enterprise Feed Service

Run `docker-compose up -d` in the directory where the docker-compose.yaml file lives. 

### Stop the Anchore Enterprise Feed Service

Run `docker-compose down -v` in the directory where the docker-compose.yaml file lives. 


## Feed Service Configuration with Anchore Engine

By default, Anchore Engine is configured to use the public and free service maintained by Anchore at https://ancho.re/v1/service/feeds

As part our Anchore Enterprise offering, an on-premise feed service is available.

Configuration: 

    1. Update the top level feeds property in Anchore Engine's config.yaml to use the on-premise feed service endpoint. 

```
    feeds:
      selective_sync:
        enabled: True
        feeds:
          vulnerabilities: True
          packages: False
          nvd: False
    url: 'http://feedservice.example.com:8448/v1/feeds'
```

    2. Restart Anchore Engine (or just the Policy Engine component containers if you have split the services out into their own containers) for the configuration changes to take effect. If the service starts successfully then it was able to reach the feed service endpoint. 
