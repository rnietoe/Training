# Databases

## DynamoDB

With Amazon DynamoDB **transactions**, you can group multiple actions together and submit them as a single all-or-nothing `TransactWriteItems` or `TransactGetItems` operation.

* `TransactWriteItems` is a synchronous and idempotent write operation that groups up to 25 write actions in a single all-or-nothing operation. These actions can target up to 25 distinct items in one or more DynamoDB tables within the same AWS account and in the same Region. The aggregate size of the items in the transaction cannot exceed 4 MB. The actions are completed atomically so that either all of them succeed or none of them succeeds.

To make the most of the table’s provisioned throughput, the Developer can use the **Parallel Scan API operation** so that the scan is distributed across the table’s partitions. This will help to optimize the scan to complete in the fastest possible time. However, the Developer will also need to apply rate limiting to ensure that the scan does not affect normal workloads.

**DynamoDB Streams** captures a time-ordered sequence of item-level modifications in any DynamoDB table and stores this information in a log for up to 24 hours. Applications can access this log and view the data items as they appeared before and after they were modified, in near-real time. The StreamSpecification parameter determines how the stream is configured:
* StreamEnabled — Specifies whether a stream is enabled (true) or disabled (false) for the table.
* StreamViewType — Specifies the information that will be written to the stream whenever data in the table is modified:
    * `KEYS_ONLY` — Only the key attributes of the modified item.
    * `NEW_IMAGE` — The entire item, as it appears after it was modified.
    * `OLD_IMAGE` — The entire item, as it appeared **before** it was modified.
    * `NEW_AND_OLD_IMAGES` — Both the new and the old images of the item.

## Amazon Aurora 

MySQL with operations made within a **transaction block**
