# cephfs-provisioner

## 用于提供kubernetes使用外部存储cephfs作为动态pv为集群提供持久化存储。

## 快速配置使用

**注意:** 需要提前部署ceph集群，并创建好存储池，快速部署ceph可以参考[官方文档](https://docs.ceph.com/docs/master/cephadm/install/)

1. 添加ceph monitor
   ```
   比如:  10.16.18.176:6789,10.16.18.177:6789,10.16.18.178:6789
   ```

2. 获取ceph用户信息和认证key, 这里以默认admin为例子.
   ```
   $ sudo ceph auth get-key client.admin
   ```

3. 将获取到用户信息认证key填入.

4. 点击启动部署，等待片刻运行成功后，可以创建一个pv测试.