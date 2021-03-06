## Prerequisites
Before using Elastic Kubernetes Service (EKS), you must complete identity verification for your Tencent Cloud account. For more information on identity verification, see [Introduction to Identity Verification](https://intl.cloud.tencent.com/document/product/378/3629).


## Supported Regions
For more information on the regions and resource specifications supported by EKS, see [Resource Specifications](https://intl.cloud.tencent.com/document/product/457/34057).

## Resource Quotas
| Resource | Limit | Description |
| --------- | --------- | --------- |
| Clusters in one region | 5 | Includes clusters that are being created and running. |
| Pods in one cluster | 50 | Includes all namespaces, workloads, and stateless and stateful pods. |
| Pod replicas for one workload | 50 | Includes all stateless and stateful pods in the workload. |

If the number of required resources exceeds the quota limit shown in the preceding table, you can submit a ticket to apply for a higher quota. Tencent Cloud will assess your actual needs and increase your quota as appropriate.

#### Applying for a higher quota
1. Log in to the [ticket system console](https://console.cloud.tencent.com/workorder/category?level1_id=6&level2_id=350&source=0&data_title=%E5%AE%B9%E5%99%A8%E6%9C%8D%E5%8A%A1TKE&step=1). On the **Submit ticket** page, select **Other Problems** and then click **Create Now** to go to the page for creating a ticket.
2. In the **Problem description** field, enter a description such as "I want to apply for a higher quota for the elastic cluster." Then, enter the region where the elastic cluster is located and your desired quota. Finally, enter your mobile number and other information as instructed.
3. After providing all the necessary information, click **Submit ticket** to submit the ticket.
