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

Anchore On-Premise Feed Service allows for our customers to sync their existing Anchore Engine deployment with a locally deployed feed service, without any reliance on our cloud feed service. 



