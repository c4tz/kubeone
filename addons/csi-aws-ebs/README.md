# AWS EBS CSI driver chart

The AWS EBS CSI manifest is generated from the [official Helm chart][helm-chart].

```shell
helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver
helm repo update

helm template \
    --namespace="kube-system" \
    --values="generated-values-csi" \
    --skip-tests \
    aws-ebs-csi-driver aws-ebs-csi-driver/aws-ebs-csi-driver > csi-aws-ebs.yaml
```

Required manual modifications include:

* Images must be changed to `{{ .InternalImages.Get "..." }}` as appropriate
  * Make sure that you update the appropriate entries in images list
* Remove `AWS_` environment variables in controller Deployment and node DaemonSet
* Make sure to update [External Snapshotter][snapshotter] (including CRDs) to match the version used by AWS CSI driver

[helm-chart]: https://github.com/kubernetes-sigs/aws-ebs-csi-driver/tree/master/charts/aws-ebs-csi-driver
[snapshotter]: https://github.com/kubernetes-csi/external-snapshotter
