Example of deploying openshift-gitops via a policy along with a Argo CD App of Apps cluster configuration. The Argo CD App of App leverages my cluster-config repository to configure a AWS remote cluster.

When I use this repo the following steps are required:

0. Create namespace `open-cluster-management-policies` and `openshift-gitops` on ACM Hub Cluster
1. Add gitops-operator and sealed-secrets-seed policies to ACM. The sealed-secrets-seed policy deploys the `sealed-secrets` namespace along with a default private key for decrypting secrets.
2. Import a new AWS cluster into ACM and set labels `cluster=aws.cluster` and `gitops=true`
    * `kustomize build components/policies/gitops-operator/base | oc apply -f -`
3. Policy will automatically deploy artifacts, wait for it to finish
4. Deploy the cluster-config subscription:
    * `kustomize build clusters/aws.cluster/acm/overlays/subscription | oc apply -f -`

Thanks to Andrew Block for the original version of the policy at https://github.com/sabre1041/rhacm-argocd/tree/main/rhacm-managed-argocd/argocd-cluster/policies