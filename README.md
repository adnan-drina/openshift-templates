*For other versions of OpenShift, follow the instructions in the corresponding branch e.g. ocp-3.6, ocp-3.5, etc at https://github.com/OpenShiftDemos/openshift-cd-demo*

# CI/CD Demo - OpenShift Container Platform 3.6

This repository includes the infrastructure and pipeline definition for continuous delivery using Jenkins, Nexus and SonarQube on OpenShift.

## Create Project structure for CI/CD Pipeline (script)

You can use the `scripts/provision.sh` script provided to deploy the entire demo:

  ```
  ./provision.sh --help
  ./provision.sh deploy --project-suffix msa
  ./provision.sh delete --project-suffix msa
  ```

## Deploy on OpenShift (Manual)

  ```shell
  # Create Projects
  oc new-project dev --display-name="Tasks - Dev"
  oc new-project stage --display-name="Tasks - Stage"
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
