# CephFS Volume Provisioner for Kubernetes

When deploying an application that needs to retain data, you’ll need to create persistent storage. Persistent storage allows you to store application data external from the pod running your application. This storage practice allows you to maintain application data, even if the application’s pod fails.

**Note That:**

Currently, cephfs adaptation is not provided in Kubernetes, so external plug-in support is needed. For the list of persistent storage supported by Kubernetes, please refer to the [official document](https://kubernetes.io/docs/concepts/storage/storage-classes/).

This cephfs provisioner helm chart base on Kubernetes external-storage project, detail is [here](https://github.com/kubernetes-incubator/external-storage/tree/master/ceph/cephfs).

# Quick Start

## Requirements

- Kubernetes cluster v1.16+
- kubectl CLI
- Ceph cluster
- Helm CLI v3

1. Create new namespace in you Kubernetes cluster, e.g: cephfs
   ```
   $ kubectl create namespace cephfs
   ```

2. Get auth key from you Ceph cluster. 
   ```
   $ sudo ceph auth get-key client.admin
   ```

3. Get this Cephfs provisioner folder place in you workspace, assume in current directory, use below command to install.
   ```
   helm install cephfs-provisioner cephfs-provisioner -n cephfs - --set cephfs.monitors=YOU_CEPH_CLUSTER -set cephfs.authkey=YOU_AUTHKEY
   
   #for example:
   helm install cephfs-provisioner cephfs-provisioner -n cephfs --set cephfs.monitors=10.16.18.176:6789,10.16.18.177:6789,10.16.18.178:6789 --set cephfs.authkey=AQCQrsReQMMZExAADt3eAGJwICHJv2qb0ACmGA==
   ```

4. Confirm that Cephfs volume provisioner pod is running.
   ```
   $ kubectl get pods -l app=cephfs-provisioner -n cephfs
   ```

5. Create a test Claim on Kubernetes.
   ```
   $ vim cephfs-claim.yml
    ---
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
      name: cephfs-claim1
      namespace: cephfs
    spec:
      accessModes:
        - ReadWriteOnce
      storageClassName: cephfs
      resources:
        requests:
          storage: 1Gi
   ```
   Apply manifest file.

   ```
   $ kubectl  apply -f cephfs-claim.yml
   ```

6. If it was successful in binding, it should show `Bound` status.
   ```
   kubectl  get pvc -n cephfs
   NAME   STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
   cephfs-claim1   Bound    pvc-8ba46e58-7d08-4127-a773-22e1a38e28f5   1Gi        RWO            cephfs         56
   ```

   # Also

   This cephfs provisioner helm chart also can use for `Rancher Catalogs`. push this chart to you helm repository，and add this helm repository to you `Rancher Catalogs`.