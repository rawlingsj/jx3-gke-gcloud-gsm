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

2. Get the latest jx3 CLI https://github.com/jenkins-x/jx-cli/releases/latest


3. Upgrade the CLI plugins

```bash
jx upgrade plugins
```

4. Create the boot git repository

```bash
jx admin create -b --initial-git-url https://github.com/rawlingsj/jx3-gke-gcloud-gsm --env dev --env-git-owner=$ENV_GIT_OWNER --repo env-$CLUSTER_NAME-dev --no-operator
```

5. Clone the newly created boot git repository

```bash
git clone https://github.com/$ENV_GIT_OWNER/env-$CLUSTER_NAME-dev.git
cd env-$CLUSTER_NAME-dev
```

6. Set environment variables

```bash
./bin/configure.sh
```

7. Create the cluster

```bash
./bin/create.sh
```

8. Commit any local changes and push

```bash
# lets add / commit any cloud resource specific changes
git add --all
git commit -a -m "chore: cluster configuration changes"
git push
```

9. Install the gitops operator

This will install a Kubernetes job that periodically syncronises the git repository with your Kubernetes cluster

```bash
# --username is found from $GIT_USERNAME or git clone URL
# --token is found from $GIT_TOKEN or git clone URL
jx admin operator
```