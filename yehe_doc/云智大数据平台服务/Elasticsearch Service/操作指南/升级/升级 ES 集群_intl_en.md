Tencent Cloud ES offers an upgrade feature that allows you to upgrade ES and X-Pack. You can upgrade your cluster based on your business needs to achieve seamless business transition.

## Supported Upgrade Types
ES supports the following two upgrade types.
1. Upgrade of the Elasticsearch version
<table style="width:350px !important;">
  <tr>
    <th>Source Elasticsearch Version</th>
    <th>Target Elasticsearch Version</th>
  </tr>
  <tr>
    <td>Low version (such as v5.6)</td>
    <td>High version (such as v6.8)</td>
  </tr>
</table>
2. Upgrade of X-Pack
<table style="width:350px !important;">
  <tr>
    <th>Source X-Pack Edition</th>
    <th>Target X-Pack Edition</th>
  </tr>
  <tr>
    <td>Open Source Edition</td>
    <td>Basic or Platinum Edition</td>
  </tr>
	 <tr>
    <td>Basic Edition</td>
    <td>Platinum Edition</td>
  </tr>
</table>

 **X-Pack description:**
The Basic Edition and Platinum Edition integrate Elasticsearch's official X-Pack plugin which including features such as security, SQL, machine learning, and monitoring. The former only comes with certain SQL features and monitoring, while the latter has all X-Pack features. For more information, please see [X-Pack](https://intl.cloud.tencent.com/document/product/845/30943).
>
>- You can perform only one of the two types of upgrade above at a time. When upgrading from a lower version of the Open Source Edition, you can choose to upgrade to the Basic Edition at the same time.
>- v5.x is available only in the Open Source but not the Basic or Platinum Edition.

## Restart After Upgrade

1. As far as restart is concerned, when an upgrade is performed, the cluster may:
  - Not restart
  - Restart on a rolling basis (nodes are upgraded on by one, and the service can be accessed normally during upgrade, but the performance may be affected; therefore, you are recommended to upgrade during off-peak hours)
  - Fully restart (the cluster will restart after all nodes are closed, and the service is inaccessible during upgrade. This option should be used with caution)

  According to your choice, the upgrade page will display prompts for these cases.

2. In case of a full restart of the cluster, as the service will become inaccessible, please note the following:
  The full restart of the cluster is subject to the status change of the [ES cluster user authentication](https://intl.cloud.tencent.com/document/product/845/35275) feature from off to on (which can greatly improve the access security of the cluster). When you enable this feature for your cluster, according to the official design requirements of Elasticsearch, the cluster needs to be fully restarted, during which it will be inaccessible; therefore, you are recommended to enable this feature during off-peak hours.
> To enable ES cluster user authentication, you need to modify your business code in advance, so that the username and password can be passed in when relevant APIs/SDKs are called during [accessing the cluster through API](https://intl.cloud.tencent.com/document/product/845/19540); otherwise, the cluster will be inaccessible after the upgrade.
   
3. ES cluster user authentication is supported as follows:

 - Unavailable in the Open Source Edition
 - Supported in v6.8 or higher of the Basic Edition (you can choose to enable or disable it).
 - Enabled in the Platinum Edition by default

  Example:
 - For upgrade from the Open Source Edition v6.4 (with this feature unavailable) to the Basic Edition v6.8 (with this feature not enabled), no full restart of the cluster is needed.
 - For upgrade from the Open Source Edition v6.4 (with this feature unavailable) to the Basic Edition v6.8 (with user authentication enabled), full restart of the cluster is needed.
 - For upgrade from the Basic Edition v6.8 (with this feature not enabled) to the Platinum Edition v6.8 (with this feature enabled by default), full restart of the cluster is needed.
 - For upgrade from the Basic Edition v6.8 (with this feature enabled) to the Platinum Edition v6.8 (with this feature enabled by default), no full restart of the cluster is needed.


## Notes on Upgrade

### Compatibility and use of Elasticsearch 5.x and 6.x
1. Multi-type index
Starting from 6.x, Elasticsearch no longer supports multiple types within a single index. After you upgrade from v5.x to v6.x, creating a multi-type index will cause an error. Multi-type indices previously created on v5.x will not be affected, and data can be written to them normally.
2. Access to cluster with `curl`    
When accessing a cluster by using `curl`, you need to add the `header -H 'Content-Type: application/json'` request.
```
	curl -XPUT http://10.0.0.2:9200/china/city/beijing -H 'Content-Type: application/json' -d'
	{
		"name":"Beijing",
		"province":"Beijing",
		"lat":39.9031324643,
		"lon":116.4010433787,
		"x":6763,
		"level.range":4,
		"level.level":1,
		"level.name":"Tier-1 city",
		"y":6381,
		"cityNo":1
	}
```
3. Compatibility of configuration items   
 Different versions of Elasticsearch have certain incompatible configuration items, and if you have set such items, an upgrade may affect the use of your cluster. The upgrade feature of ES comes with a process to check configuration items as well as instructions for adjustment, as described below in <a href="#update_check">Upgrade check</a>.
4. For more information, please see [Breaking changes in 6.0](https://www.elastic.co/guide/en/elasticsearch/reference/6.4/breaking-changes-6.0.html).

    
## Upgrade Process
To upgrade the Elasticsearch version, you need to complete upgrade check and data backup first. The upgrade process can be started only after those two steps are successfully completed.
1. <a id="update_check">Upgrade check</a>
  >  Only available to upgrade of the Elasticsearch version.
  >
Check whether the two versions have any incompatible configuration items. If the check fails, the process will terminate, and you can view the specific check items and corresponding solutions. For more information, please see [Upgrade Check](https://intl.cloud.tencent.com/document/product/845/32599). You can also choose to only perform an upgrade check before upgrade so as to see whether your cluster meets the conditions for upgrade.
2. Snapshot backup
  > Only available to upgrade of the Elasticsearch version.
  >
 Before the upgrade, ES will perform snapshot backup of your cluster, so that you can restore it from the snapshot in case of an upgrade failure. Therefore, if the snapshot operation fails, the upgrade process will also terminate. **The time it takes to complete the snapshot backup depends on the amount of data in your cluster. If automatic snapshot backup is not enabled for your cluster, and the data volume is high, it will take longer to create a snapshot for the first time.**
3. Upgrade process and cluster restart  
For a cluster on v6.x or higher, you can upgrade X-Pack (i.e., from the Open Source Edition to the Basic or Platinum Edition). During the upgrade, the service needs to be restarted as follows:
  - If no status change of the ES cluster user authentication feature from off to on is involved during the upgrade, your cluster must be restarted on a rolling basis, its health status must be green, and there can be no single-replica or closed indices in it. During the upgrade, the service is accessible, but its performance will be partially affected. As a result, you are recommended to perform the upgrade during off-peak hours.
  - If status change of the ES cluster user authentication feature from off to on is involved during the upgrade, your cluster must be fully restarted. During the full restart, your cluster will be inaccessible until the upgrade is completed; therefore, this option should be used with caution.

## How to Upgrade a Cluster
1. Log in to the [ES Console](https://console.cloud.tencent.com/es), enter the cluster details page, and click **Upgrade** in the top-right corner.
![](https://main.qcloudimg.com/raw/12b43d50ad32df9e6f18e85867ee6a6d.jpg)
2. Choose to upgrade Elasticsearch or X-Pack in the upgrade dialog box.

### Upgrading the Elasticsearch version
1. Select **Upgrade the Elasticsearch version** as the **Upgrade Type** in the upgrade dialog box.
2. Select the version that you want to upgrade to in the **Elasticsearch Version** drop-down list.
> 
>- If you do not choose to upgrade X-Pack at the same time, the original X-Pack Edition will be retained when upgrading to a higher version by default.
>- Note: when upgrading the major version of Elasticsearch, such as from v5.x to v6.x, you can upgrade X-Pack from the Open Source Edition to the Basic Edition at the same time. You are recommended to select X-Pack **Basic Edition**, which contains features such as monitoring and SQL. When upgrading to the Basic Edition v6.8 or higher, you can also choose to enable ES cluster user authentication; in this case, the cluster needs to be fully restarted, during which your cluster will be inaccessible; therefore, this option should be used with caution.
3. Before the upgrade, the cluster status and configuration will be checked first to determine whether your cluster can be upgraded. You can select **Upgrade Check Only**. After you click **OK**, only an upgrade check will be performed, and the upgrade command will not be executed. You can view the check result in the **Cluster Change History** on the details page.
> If you want to perform a major version upgrade for Elasticsearch (e.g., from v5.x to v6.x), as some configuration items at the cluster or index level are not compatible, you need to perform an **upgrade check** to determine whether your cluster can be upgraded. Incorrect configuration items that trigger alarms need to be adjusted as appropriate. For more information, please see [ES Version Upgrade Check](https://intl.cloud.tencent.com/document/product/845/32599)
4. Read and indicate your consent to the upgrade notice and click **OK** to start the upgrade (if you select **Upgrade Check Only**, an upgrade check will be started).
![](https://main.qcloudimg.com/raw/b1f8111f2fbcda531fe5804ee1840215.jpg)

### Upgrading X-Pack
1. Select **Upgrade Type** > **Upgrade [X-Pack] Edition** in the upgrade dialog box.
2. Select the X-Pack edition that you want to upgrade to in **X-Pack**.
3. Click **OK** to start the upgrade. 
> 
>- Note on upgrade of X-Pack:
>- Currently, X-Pack can be upgraded for v6.x or higher but not v5.x (v5.x is available only in the Open Source but not the Basic or Platinum Edition).    
> - The upgrade process varies by edition as shown below:
- If no status change of the ES cluster user authentication feature from off to on is involved during the upgrade, your cluster needs to be restarted on a rolling basis, during which the service will be affected momentarily; therefore, you are recommended to perform the upgrade during off-peak hours.
- If status change of the ES cluster user authentication feature from off to on is involved during the upgrade, your cluster must be fully restarted. During the full restart, your cluster will be inaccessible; therefore, this option should be used with caution.
>  
 
 ![](https://main.qcloudimg.com/raw/c94e663d7839ace50c4ced2d43fcaa53.jpg)

4. After the upgrade starts, you can check the upgrade progress in the **Cluster Change History** on the cluster details page.
    ![](https://main.qcloudimg.com/raw/9c454150fbeb3f7396269b1dc9da8813.jpg)
