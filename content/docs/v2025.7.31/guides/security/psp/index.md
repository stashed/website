---
title: PSP | Stash
description: Using Stash in PSP enabled cluster
menu:
  docs_v2025.7.31:
    identifier: security-psp
    name: PSP
    parent: security
    weight: 20
product_name: stash
menu_name: docs_v2025.7.31
section_menu_id: guides
info:
  cli: v0.41.0
  community: v0.41.0
  elasticsearch:
  - 5.6.4-v37
  - 6.2.4-v37
  - 6.3.0-v37
  - 6.4.0-v37
  - 6.5.3-v37
  - 6.8.0-v37
  - 7.14.0-v23
  - 7.2.0-v37
  - 7.3.2-v37
  - 8.2.0-v20
  enterprise: v0.41.0
  etcd:
  - 3.5.0-v24
  installer: v2025.7.31
  kubedump:
  - 0.2.0-v5
  mariadb:
  - 10.5.8-v31
  mongodb:
  - 3.4.17-v38
  - 3.4.22-v38
  - 3.6.13-v38
  - 3.6.8-v38
  - 4.0.11-v38
  - 4.0.3-v38
  - 4.0.5-v38
  - 4.1.13-v38
  - 4.1.4-v38
  - 4.1.7-v38
  - 4.2.3-v38
  - 4.4.6-v29
  - 5.0.15-v11
  - 5.0.3-v26
  - 6.0.5-v14
  mysql:
  - 5.7.25-v38
  - 8.0.14-v37
  - 8.0.21-v31
  - 8.0.3-v37
  nats:
  - 2.6.1-v25
  - 2.8.2-v20
  percona-xtradb:
  - 5.7-v32
  postgres:
  - 10.14-v36
  - 11.9-v36
  - 12.4-v36
  - 13.1-v33
  - 14.0-v25
  - 15.1-v17
  - 16.1-v6
  - 17.2-v4
  - 9.6.19-v36
  redis:
  - 5.0.13-v25
  - 6.2.5-v25
  - 7.0.5-v18
  ui-server: v0.22.0
  vault:
  - 1.10.3-v17
  version: v2025.7.31
---

# Stash with PSP Enabled Cluster

Stash comes with built-in support for [Pod Security Policy (PSP)](https://kubernetes.io/docs/concepts/policy/pod-security-policy/) enabled cluster. Stash may use two different [Kubernets recommended PSP](https://kubernetes.io/docs/concepts/security/pod-security-standards) based on your setup.

### Baseline PSP

 By default Stash uses minimally restrictive [baseline](https://kubernetes.io/docs/concepts/security/pod-security-standards/#baseline-default) PSP. Stash uses baseline PSP. Here, is the YAML of the baseline PSP that uses by Stash operator.

```yaml
# ref: https://kubernetes.io/docs/concepts/security/pod-security-standards/#policy-instantiation
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: baseline
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade
    "helm.sh/hook-delete-policy": before-hook-creation
  {{- if .Values.security.apparmor.enabled }}
    # Optional: Allow the default AppArmor profile, requires setting the default.
    apparmor.security.beta.kubernetes.io/allowedProfileNames: 'runtime/default'
    apparmor.security.beta.kubernetes.io/defaultProfileName:  'runtime/default'
  {{- end }}
  {{- if .Values.security.seccomp.enabled }}
    # Optional: Allow the default seccomp profile, requires setting the default.
    seccomp.security.alpha.kubernetes.io/allowedProfileNames: 'docker/default,runtime/default,unconfined'
    seccomp.security.alpha.kubernetes.io/defaultProfileName:  'unconfined'
  {{- end }}
spec:
  privileged: false
  # The moby default capability set, defined here:
  # https://github.com/moby/moby/blob/0a5cec2833f82a6ad797d70acbf9cbbaf8956017/oci/caps/defaults.go#L6-L19
  allowedCapabilities:
    - 'CHOWN'
    - 'DAC_OVERRIDE'
    - 'FSETID'
    - 'FOWNER'
    - 'MKNOD'
    - 'NET_RAW'
    - 'SETGID'
    - 'SETUID'
    - 'SETFCAP'
    - 'SETPCAP'
    - 'NET_BIND_SERVICE'
    - 'SYS_CHROOT'
    - 'KILL'
    - 'AUDIT_WRITE'
  # Allow all volume types except hostpath
  volumes:
    # 'core' volume types
    - 'configMap'
    - 'emptyDir'
    - 'projected'
    - 'secret'
    - 'downwardAPI'
    # Assume that persistentVolumes set up by the cluster admin are safe to use.
    - 'persistentVolumeClaim'
    # Allow all other non-hostpath volume types.
    - 'awsElasticBlockStore'
    - 'azureDisk'
    - 'azureFile'
    - 'cephFS'
    - 'cinder'
    - 'csi'
    - 'fc'
    - 'flexVolume'
    - 'flocker'
    - 'gcePersistentDisk'
    - 'gitRepo'
    - 'glusterfs'
    - 'iscsi'
    - 'nfs'
    - 'photonPersistentDisk'
    - 'portworxVolume'
    - 'quobyte'
    - 'rbd'
    - 'scaleIO'
    - 'storageos'
    - 'vsphereVolume'
  hostNetwork: false
  hostIPC: false
  hostPID: false
  readOnlyRootFilesystem: false
  runAsUser:
    rule: 'RunAsAny'
  seLinux:
    rule: 'RunAsAny'
  supplementalGroups:
    rule: 'RunAsAny'
  fsGroup:
    rule: 'RunAsAny'
```

### Privileged PSP

If you are using an NFS server as backend with Stash, you may need to give Stash operator privileged permission. In this case, Stash will use [privileged](https://kubernetes.io/docs/concepts/security/pod-security-standards/#privileged) PSP. Here, is the YAML of privileged PSP that is used by Stash when you uses NFS server as backend,

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: privileged
  annotations:
    seccomp.security.alpha.kubernetes.io/allowedProfileNames: '*'
spec:
  privileged: true
  allowPrivilegeEscalation: true
  allowedCapabilities:
  - '*'
  volumes:
  - '*'
  hostNetwork: true
  hostPorts:
  - min: 0
    max: 65535
  hostIPC: true
  hostPID: true
  runAsUser:
    rule: 'RunAsAny'
  seLinux:
    rule: 'RunAsAny'
  supplementalGroups:
    rule: 'RunAsAny'
  fsGroup:
    rule: 'RunAsAny'
```

You can use your own PodSecurityPolicy with Stash. In this case, you have to create the PSP manually and provide the PSP names during installation. You can provide the custom PSP names during installation as below,

```bash
$ helm install stash oci://ghcr.io/appscode-charts/stash \
  --version {{< param "info.version" >}} \
  --namespace stash --create-namespace \
  --set features.enterprise=true \
  --set podSecurityPolicies[0]=abc \
  --set podSecurityPolicies[1]=xyz
```
