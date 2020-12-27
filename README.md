# What is this?
This is a simple repo that is parte of a guided exercise for the Red Hat course 
DO380 Red Hat OpenShift Administration III: Scaling Kubernetes Deployments in the Enterprise

If nothing else, this can be used as an example of things used in a GitOps Jenkins deployment

# What you need to do
In OpenShift:

`oc new-project gitops-deploy`

Get a list of Jenkins templates:

`oc get template -n openshift | grep jenkins`

Pick one and create a new app for it:

`oc new-app --template jenkins-persistent`

As an admin user in OpenShift, grant the Jenkins service account permissions to create projects:

`oc adm policy add-cluster-role-to-user self-provisioner -z jenkins -n gitops-deploy

(This creates a cluster role binding resource, which is NOT namespaced, and references a service account
which IS namespaced.  Thus the need for `-n`

This should create a Jenkins pod and you will need to log into the Jenkins pod and set up your pipeline.

When logged into Jenkins, Click `New Item` on the Dashboard page, give it a name and select
`Multibranch Pipeline`  Under the `Branch Sources heading, slect `Add Source -> Git` and put in the
repo address you are using in the `Project Repository` field.  `Save` and move on.

This should go through the pipeline and then hang on `test` because the pipeline is waiting for verification.
