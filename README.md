# jx3-gke-gcloud-vault
Jenkins X 3.x GitOps repository using Google Cloud with gcloud for cloud infrastructure creation and vault for secret management

## Prerequisites

Before you begin you need to install gcloud

You may want to upgrade to the latest gcloud:

gcloud components update

1. Define these environment variables:

- PROJECT_ID the GCP project the cluster and other cloud resources will be created into
- CLUSTER_NAME provide a unique cluster name for the GCP project
- ZONE the GCP zone to create the new cluster, e.g. europe-west1-b
- ENV_GIT_OWNER the GitHub organisation the GitOps environments are created, these are the repos that contain the meta data for each Jenkins X environment. Note the pipline user env vars below must have permission to create repos in the GitHub organisation
- LABELS and labels to be added to the GCP nodes

e.g.

```bash
export CLUSTER_NAME=my-cool-project
export PROJECT_ID=my-gcp-project
export ZONE=europe-west1-b
export ENV_GIT_OWNER=jenkins-x
export LABELS=""
```

2. Get the latest jx CLI https://github.com/jenkins-x/jx-cli/releases/latest


3. Upgrade the CLI plugins

```bash
jx upgrade plugins
```

4. Create the boot git repository to mimic creating the git repository via the github create repository wizard

```bash
jx admin create -b --initial-git-url https://github.com/jx3-gitops-repositories/jx3-gke-gcloud-gsm --env dev --env-git-owner=$GH_OWNER --repo env-$CLUSTER_NAME-dev --no-operator
```