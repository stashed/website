---
title: PSP | Stash
description: Using Stash in PSP enabled cluster
menu:
  docs_v2022.06.27:
    identifier: security-psp
    name: PSP
    parent: security
    weight: 20
product_name: stash
menu_name: docs_v2022.06.27
section_menu_id: guides
info:
  cli: v0.21.0
  community: v0.21.0
  elasticsearch:
  - 5.6.4-v18
  - 6.2.4-v18
  - 6.3.0-v18
  - 6.4.0-v18
  - 6.5.3-v18
  - 6.8.0-v18
  - 7.14.0-v4
  - 7.2.0-v18
  - 7.3.2-v18
  - 8.2.0-v1
  enterprise: v0.21.0
  etcd:
  - 3.5.0-v5
  installer: v2022.06.27
  kubedump:
  - 0.1.0-v1
  mariadb:
  - 10.5.8-v11
  mongodb:
  - 3.4.17-v18
  - 3.4.22-v18
  - 3.6.13-v18
  - 3.6.8-v18
  - 4.0.11-v18
  - 4.0.3-v18
  - 4.0.5-v18
  - 4.1.13-v18
  - 4.1.4-v18
  - 4.1.7-v18
  - 4.2.3-v18
  - 4.4.6-v9
  - 5.0.3-v6
  mysql:
  - 5.7.25-v18
  - 8.0.14-v18
  - 8.0.21-v12
  - 8.0.3-v18
  nats:
  - 2.6.1-v5
  - 2.8.2-v1
  percona-xtradb:
  - 5.7-v13
  postgres:
  - 10.14-v17
  - 11.9-v17
  - 12.4-v17
  - 13.1-v14
  - 14.0-v6
  - 9.6.19-v17
  redis:
  - 5.0.13-v6
  - 6.2.5-v6
  ui-server: v0.3.0
  version: v2022.06.27
---

# Stash with PSP Enabled Cluster

Stash comes with built-in support for [Pod Security Policy (PSP)](https://kubernetes.io/docs/concepts/policy/pod-security-policy/) enabled cluster. Stash may use two different [Kubernets recommended PSP](https://kubernetes.io/docs/concepts/security/pod-security-standards) based on your setup.

### Baseline PSP

 By default Stash uses minimally restrictive [baseline](https://kubernetes.io/docs/concepts/security/pod-security-standards/#baseline-default) PSP. Both Stash Community Edition and Stash Enterprise Edition uses baseline PSP. Here, is the YAML of the baseline PSP that uses by Stash operator.

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

If you are using an NFS server as backend with Stash Enterprise Edition, you may need to give Stash operator privileged permission. In this case, Stash will use [privileged](https://kubernetes.io/docs/concepts/security/pod-security-standards/#privileged) PSP. Here, is the YAML of privileged PSP that is used by Stash Enterprise Edition when you uses NFS server as backend,

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
$ helm install stash appscode/stash-community   \
    -n kube-system                    \
    --set podSecurityPolicies[0]=abc  \
    --set podSecurityPolicies[1]=xyz
```
