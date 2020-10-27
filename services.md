# An overview of GCP Services

## Terms

- Zonal: A particular section of a region (zone a)
- Region: A physical part of the world. (central)
- Multi Region: Multiple regions all connected together through network
- Global: Google's entire network

## Compute

### Compute Engine (GCE) [Zonal]

- Fast booting VMs you can rent on demand. You can do pretty much anything.
- IaaS
- Pick machine type, ram and cpus
- Premptable (way cheaper)
- Can add GPUs to improve performance
- Google will live migrate your instances to new vms seamlessly

### Google Kubernetes Engine (GKE) [Regional]

- Managed Kubernetes cluster for running docker container
- How it works...you create a GKE cluster in a region & google creates GCE instances to manage everything.
- Autoscaling
- Kubernetes DNS on by default
- No IAM integration by default. You need to add these manually
- Integrates with Persistent Disk for storage
- Pay for underlying GCE instances

### Google App Engine (GAE) [Regional]

- Platform as a Service that takes your code and runs it.
- Flex Mode, can run any container & access VPC.
- Auto Scale to handle load

### Cloud Functions (GCF) [Regional]

- Runs code when an event fires
- Functions as a Service - "serverless"
- Each function automaically gets an HTTP endpoint
- Can be triggered by many things
- Massively scalable

## Storage

Encrypted at rest

### Local SSD [Zonal]

- DAS
- Very fast 375 ssd physically attached to the server
- Can stripe across eight of them for better performance
- Deleted on instance shutdown
- Pay by Gb/month

### Persistent Disk Storage (PD) [Zonal]

- SAN
- Flexible block based network attached storage.
- Boot disk for the GCE instance
- Performance is lower than local ssd but still pretty quick
- Can resize while in use
- Snapshot and make machine images
- You can mount a single PD to multiple instances if all the instances are read only

### Cloud Filestore [Zonal]

- NAS
- Fully managed file based storage system
- More like network attached storage
- Predicably fast performance for workloads
- Accessible to GCE and GKE through VPC
- Primary use case is application migration to the cloud
- Does not do backups (for now)
- Pay for provisioned amount, not use

### Cloud Storage [Regional/Multi-Regional]

- Infinitely scalable, fully managed, versioned and durable object storage (s3 like)
- Can emulate a file system
- Designed for 99.999999999% of durability
- Strongly consistent
- Integrated site hosting and CDN functionality
- Lifecycle transitions across classes:
  - Multi-Regional: Data that needs to be spread out due to requestors or redundency
  - Regional: Faster access in particular location
  - Nearline: Data accessed less than once a month
  - Coldline: Data accessed less than once a year
- Pay for each operation

## Databases

### Cloud SQL [Regional]

- Fully managed reliable MySQL & PostgreSQL
- Supports automatic replication, backup, failover
- Scaling is manual
- Effectly pay for underlying GCE Instances and PDs

### Cloud Spanner [Regional Multi-Regional Global]

- Horizontally scalable, strongly consistent, relational database service
- Scales from 1 - 1000's of nodes
- Mega database scaling.
- Can sacrifice consistency for speed by allowing stale requests
- \$1000's of dollars a month

### BigQuery [Multi-Regional]

- Serverles column-store data warehouse for analytics using SQL
- Scales interally so it can handle massive queries
- Can scan TB's in seconds and PB's in minutes
- Pay for data BigQuery has to scans
- Pay for data stored over time
- Pay for Streaming inserts

### Cloud BigTable [Zonal]

- Low latency & high throughput NoSQL DB for large operation & analytical apps
- Supports open-source HBase API
- Integrates Hadoop, Dataflow, Dataproc
- Scales seamlessly & unlimitedly
- Pay for processing node hours
- pay for GB-hours used for storage

### Cloud Datastore [Regional Multi-Regional]

- Managed & autoscaled NoSQL DB with indexes, queries & ACID transaction support

### Firebase Realtime DB [Zonal]

- NoSQL document stores with ~real-time client updates via websockets
- Mega huge single JSON doc only located in the central US
- Pricing:
  - Spark: free
  - Flame: Flat rate
  - Blaze: Usage based

### Cloud Firestore [Multi-Regional]

- NoSQL document stores with ~real-time client updates via websockets
- More simliar to mongo with documents, collections and contained data
- Pay less for storage and transfer

## Data Transfer

### Data Transfer Appliance

- Rackable high capacity storage server to physically ship to GCS (Similar to AWS Snowball)
- Ingest only
- 100TB - 480TB Capacity
- Faster than 6gb/s link

### Storage Transfer Service

- Copy objects from other services (like s3)
- Copy from other sources
- Free to use, but pay for actions

## External Networking

Private and Public networking is available

### Google Domains [Global]

- Google's registrar for domains
- Private Whois records
- Built-in DNS
- Supports DNSEC
- Email forwarding

### Cloud DNS [Global]

- Scalable reliable & managed authoritative DNS
- 100% uptime guarentee
- Public and private managed zones (split horizon available)
- Supports DNSSEC
- Manage via UI, CLI or API
- Charged for DNS Lookups

### Static IP [Regional & Global]

- Reserve static IP addresses in projects
- Regional IPs used for GCE instances & load balancers
- Global IPs used for Global load balancers
- Pay for reserved IPs that are not in use to discourage wasting them (IPv4)

### Load Balancing [Regional & Global]

- High performances, calable traffic distribution integrated with autoscaling & cloud cdn.
- SDN naturally handles spikes without any prewarming; no instances or devices
- Regional Network LB: heath checks, round robin, session affinity
- Global loLB with multi region failover for HTTP(s), SSL proxy, & TCP proxy
- Pay for ingress and egress traffic

### Cloud CDN [Global]

- CDN
- HTTP/2 & HTTPS
- Checkbox to setup
- Cache data and server really fast
- If data not found, cache fill called to Point of Presence resulting in some network charges
- Pay for volume too
- Pay for cache invalidation

## Interal Networking

Send data around withing your system

### VPC [Regional & Global]

- Global IPv5 unicast Software-Defined Network (SDN)
- Automatic mode is easy; custom mode gives control
- Configure subnets, routes, firewalls, VPNs, BGP, etc.
- VPC is Global and subnets are Regional
- Can be shared across multiple projects in the same org and peered with other VPCs
- Pay to use certain services (e.g. VPN) and for network egress

### Cloud Interconnect [Regional Multi-Regional]

- Options for connecting external networks to Google's network
- Private connections to VPC via Cloud VPN
- Public Google services accessible via external peering
  - Direct peering for high volume
  - Carrier peering

### Cloud VPN [Regional]

- IPsec VPN to connect to VPC via public internet for low-volume data connections
- For persistent, static connections
- Encrypted link to VPC
- Supports both static and dynamic routing
- 99% availability
- Normal traffic charges apply

### Dedicated interconnect [Regional Multi-Regional]

- Direct physical link between VPC and on-prem for high-volume data connections
- VLAN attachment is private to VPC in one region
- Links are private but not encrypted
- Pay a fee per 10Gbps link

### Cloud Router [Regional]

- Dynamic Routing
- Works with Cloud VPN and Dedicated interconnect
- Pay for egress

### CDN Interconnect [Regional Multi-Regional]

- Direct, low-latency connectivity to certain CDN providers with cheaper egress
- For external CDNs, not GCP CDN service
- Free to enable

## Machine Learning & AI

### Cloud ML Engine

- Massively scalable managed service for training ML models & predictions
- Enables apps/devs to use TensorFlow on datasets of any size
- Integrates: GCS/BQ (storage), Cloud Datalab (dev), Cloud Dataflow (preprocessing)
- HyperTune automatically tunes mdel hyperparameters to avoid manual tweaking
- Training: Pay for hour
- Prediction: Pay for provisioned mode

### Cloud Vision API [Global]

- Classifies images into categories, detects objects/faes & find/read text
- Pre-trained ML model
- Pay per image based on detection features

### Cloud Speech API [Global]

- Classify speech realtime or recorded
- Handles noisy source audio
- Accepts context words
- Pay per 15s of audio processing

### Cloud Natural language API [Global]

- Analyzes text for sentiment, intent, & classifcation.
- Pre-trained
- Charged per 1000 chars

### Cloud Translation API [Global]

- Pre-trained
- Translate stuff
- send plain text or html
- pay per char

### Dialogflow [Global]

- Chatbot
- Add code for more use
- Train using your own dataset
- Supports different languages and platforms
- Free plan & paid plan

### Cloud Video Intellegence API [Regional Global]

- Pre-trained
- Analyze videos like you would text files
- Label detection
- Shot change detection
- SafeSearch Detection
- Pay per minute

### Cloud Job Discovery [Global]

- Handles jargon
- Help match jobs to searchers
- Shows commute distance and stuff
- Handles natural sentences

## Big Data and IOT

### Cloud IOT Core [Global]

- Connect manage and ingest data
- Integrates other services
- Accepts CA certs
- Two way device communication

### Cloud Pub/Sub [Global]

- at least once messaging service
- Data stream for many devices
- Infinitly scalable
- Does not support native reread of messages
- 10MB message size
- Pay for data volume

### Cloud Dataprep [Global]

- Visually explore and clean data
- More data wrangling, less ETL
- Managed version of Trifacta Wrangler (not google)
- Automatically detects schemas
- Pay for underlying dataflow
- Pay for any services used

### Cloud Dataproc [Zonal]

- Batched map reduce
- Handles being told to scale
- Integrated with Cloud Storage, BigQuery, Bigtable and some stackdriver services
- Best for moving to GCP

### Cloud Dataflow [Zonal]

- Fully managed batch or stream processing
- Released as open-source apache beam
- Autoscales
- Dataflow shuffle service for batch offloads.
- Pay for second for vCPUs, RAM GBs

### Cloud Datalab [Regional]

- Interactive tool for data exploration, visualizations and machine learning
- uses Jupyter Notebook
- Support iterative development
- Pay for GCE/GAE instance
- Pay for accessed resource

### Cloud Data Studio [Global]

- Big Data visualization tool
- Meaning data stories
- Familiar G Suite sharing and real time collab

### Cloud Genomics [Global]

- Store and process genomes and related experiments
- Process many genomes in parallel
- Support "Requestor Pays" sharing

## Identiy & Access (Core Security)

### Roles [Global]

- Collections of permissions
- Service.Resource.Verb
- Primitive Roles: Owner, editor, viewer
- Predefined Roles: Give granular control to some resources
- Custom Roles: custom super granular permissions

### Cloud IAM [Global]

- Controls access to GCP resources (more authorization)
- Member is a user, group, domain, service account or public
- Use groups
- Policy bind members to roles at hierarchy level

### Service Accounts [Global]

- Represents an application not a user
- Assumed by applications or users when authorized
- Generate and download private keys for service account
- You should use Cloud-Platform-managed keys

### Cloud Identity [Global]

- Identity as a Service
- Free google accounts for no-G-suite users
- Centrally manage all users in google admin consol
- 2FA
- Sync from LDAP or AD
- Identites work with other services
- Free

### Security Key Enforcement [Global]

- USB or 2fa devices
- Verifies target service
- Eliminates man in the middle attacks

### Resource manager [Global]

- Centrally manage and secure organization projects
- Organize resources
- Tied 1:1 to cloud identity
- Recycle bin
- Define custom AIM roles

### Cloud IAP [Global]

- Guards apps
- Based on CLB & IAM, to only pass authed requests
- Pay for load balancing

### Cloud Audit Logging [Global]

- Answers 'who did what'
- Maintains non-tamperable audit logs for each project and organization
- Data Access logs priced through Stackdriver Logging; rest are free

## Security Management - Monitoring & Response

### Cloud Armor [Global]

- Edge-level protection from DDoS
- Only available with Global load balancer
- Monitor
- Manage IPs with CIDR-based allow/block lists
- Intelligent rules
- Pay per policy and rule configured

### Security Scanner [Global]

- Free but limited GAE app vulnerability scanner
- Protect against XSS

### Cloud DLP API [Global]

- Finds and redacts sensitive data in unstructured data
- Can integrate with other GCP storage stuff
- Cheaper the more you use it
- Pay for data processed

### Event Threat Detection [Global]

- Scan your Stackdriver activity for bad stuff
- Quickly detects threats
- can detect many attacks

### Cloud SCC [Global]

- Data Risk Platform
- Security Information and Event Management software
- Ties a bunch of security stuff together

## Encryption Keys

### Cloud KMS [Regional Multi-Regional Global]

- Latency to manage cryptographic keys
- Support symemetric and asymetric
- Move secrets out of code and into environment
- Rotate keys
- Can track key usage
- Pay for active key versions stored

### Cloud HSM [Regional Multi-Regional Global]

- Cloud KMS managed by FIPS 140-2 Level 3 HSM
- Device hosts encryption keys
- Enables you to meet compliance
- Integrated with Cloud KMS

## Operations and Management

### Stackdriver [Global]

- Family of services like logging and monitoring
- Integrates all the things
- Pay for usage

### Stackdriver Monitoring [Global]

- Monitoring service
- Built in metrics
- Alert policy
- Cross Cloud
- Free

### Stackdriver Logging [Global]

- Logs all the things
- Built in to many GCP services
- You can log through stack driver API
- mostly free

### Stackdriver Error Reporting [Global]

- Stacks errors into groups
- Alerts

### Stackdriver Trace [Global]

- Tracks and displays call trees
- View aggregate app latency
- Pay for ingesting and retrieving trace

### Stackdriver Debugger [Global]

- Grabs program state (callstack, variables ....) in live deploys
- Good for debugging
- Supports many languages
- Free

### Stackdriver Profiler [Global]

- Continuous CPU and memory profiling
- Low overhead
- Free

### Deployment Manager [Global]

- Create manage resourse via declarative templates IaC
- Create and update deployment previews

### Cloud Billing API [Global]

- Billing config
- Billing accounts
- Programatically control billing

## Development and APIs

### Cloud Source Repositories [Global]

- You can setup to sync from github /bitbucket
- No pull requests
- Help you live debug deployed apps

### Cloud Build [Global]

- CI/CD service

### Container Registry [Regional Multi-Regional]

- Just a container registry
- Translater for GCS
- Native docker CLI support

### Cloud Endpoints [Global]

- Auth, monitoring, logging, API keys, APIs backed by GCP
- Proxy instances are distributed
- Super fast
- Uses JWT

### Apigee [Global]

- Translate SOAP to REST and more
- Enterprise scale API Management

### Test lab for Android [Global]

- Cloud infrastructure for running test matrix across variety of android devices
- Production grade
- Physical and virtual devices available
