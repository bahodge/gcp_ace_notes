# Review

These are questions I got wrong on the GCP Udemy test

## Projects

### Describe New projects:

ANSWER: Free tier does not last a year, free tier does not last 30 days, big query is not enabled by default

### How to connect an external service account to your project

ANSWER: Use the service account's email or the external project's project number

## Logging

### Containerized Java Application Running on GKE

ANSWER: Devs should write to STDOUT & STDERR

### Containerized Java Application Running on GAE Flex

ANSWER: Devs should write to STDOUT & STDERR

### Non-Containerized Java Application Running on GAE

ANSWER: Have developers write logs using the App Engine Java SDK

### Non-Containerized Java Application Running on GCE

ANSWER: Write log lines to a file named application.log & install stack driver agent on VMs. Configure Stackdriver agent to monitor and push application.log

## Storage

### What roles would allow you to read a newly-created GCS Bucket

ANSWER: roles/owner, roles/storage.legacyBucketReader

### Have GCE instance in project-a read from bucket in project-b

ANSWER: Don't change the default service account. In project-b, grant bucket read access to project-a's default compute service account

### Store 2TB objects for one month

ANSWER: Nearline Cloud Storage Bucket

## Pricing Calculator

### Estimate cost of hosting a system on GKE and exposing 2 services externally

ANSWER: All must be done on the GKE tab of the calculator. There is no load balanceing in the the Networking Tab of the calculator.

## GKE

### After you create a node -> Activity Log -> Filter for "GCE VM Instance"

ANSWER: You will see a message in the passive voice "GKE_NODE_INSTANCE_NAME was created" because it was something that GCP did.

### What happens if a node dies

ANSWER: GKE will automatically restart the node on an available GCE host

## Billing

### How to link billing account to a new project

ANSWER: Projects created in the console are always linked automatically. You can use the `gcloud beta billing` command to manually link.

## GCE

### What is not a part of having a Java program running on a GCE instance access the Cloud Tasks API in a Google-recommended way?

ANSWER: The program should pass "Metadata-Flavor: Google" to the SDK. The SDK already takes care of the Metadata stuff

## Security

### If a service account is compromised

ANSWER: The attacker won't be able to do shit outside of GCP
