---
title: OpenShift
description: 在 OpenShift 集群上快速搭建 Istio 服务。
weight: 55
skip_seealso: true
aliases:
    - /zh/docs/setup/kubernetes/prepare/platform-setup/openshift/
    - /zh/docs/setup/kubernetes/platform-setup/openshift/
keywords: [platform-setup,openshift]
owner: istio/wg-environments-maintainers
test: no
---

使用 OpenShift 配置文件进行安装 Istio：

{{< text bash >}}
$ istioctl install --set profile=openshift
{{< /text >}}

安装 Istio 完成后，通过以下命令为 Ingress Gateway 暴露 OpenShift 路由：

{{< text bash >}}
$ oc -n istio-system expose svc/istio-ingressgateway --port=http2
{{< /text >}}

## 应用 Namespace 的其他要求{#additional-requirements-for-the-application-namespace}

OpenShift 上的 CNI 由 `Multus` 管理，如要使用 `istio-cni`，需要应用 Namespace 中配置 `NetworkAttachmentDefinition`。执行以下命令，将 `<target-namespace>` 替换为相应的 Namespace：

{{< text bash >}}
$ cat <<EOF | oc -n <target-namespace> create -f -
apiVersion: "k8s.cni.cncf.io/v1"
kind: NetworkAttachmentDefinition
metadata:
  name: istio-cni
EOF
{{< /text >}}

当需要移除应用时，请按以下操作移除 `NetworkAttachmentDefinition`：

{{< text bash >}}
$ oc -n <target-namespace> delete network-attachment-definition istio-cni
{{< /text >}}
