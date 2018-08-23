# CI/CD - OpenShift Container Platform 3.6

This repository includes the infrastructure and pipeline definition for continuous delivery using Jenkins on OpenShift.

## Create Project structure for CI/CD Pipeline (script)

You can use the `scripts/provision.sh` script provided to deploy:

  ```
  ./provision.sh --help
  ./provision.sh deploy --project-suffix msa
  ./provision.sh delete --project-suffix msa
  ```

## Deploy on OpenShift (Manual)

  ```shell
  # Create Projects
  oc new-project dev --display-name="Project - Dev"
  oc new-project stage --display-name="Project - Stage"
  oc new-project cicd --display-name="CI/CD"

  # Grant Jenkins Access to Projects
  oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n dev
  oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n stage
  ```
OpenShift 3.7 by default uses an older version of Jenkins. Import a newer Jenkins image in order to use with this demo:
  ```
  oc login -u system:admin
  oc import-image jenkins:v3.7 --from="registry.access.redhat.com/openshift3/jenkins-2-rhel7" --confirm -n openshift
  oc tag jenkins:v3.7 jenkins:latest -n openshift
