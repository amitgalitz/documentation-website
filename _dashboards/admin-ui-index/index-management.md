---
layout: default
title: Index management
parent: Index and snapshot management in OpenSearch Dashboards
nav_order: 16
---

# Index management
Introduced 2.5
{: .label .label-purple }

In the **Index Management** section, you can perform the operations available in the [Index API]({{site.url}}{{site.baseurl}}/api-reference/index-apis/index/).

## Index policies

[Policies]({{site.url}}{{site.baseurl}}/im-plugin/ism/policies/) are configurations that define the possible states of an index, the actions to perform when an index enters a state, and the conditions that must be met to transition between states:

1. **States**: The possible states of an index, including the default state for new indexes. For example, you might name your states `hot`, `warm`, or `delete`. For more information, see [States]({{site.url}}{{site.baseurl}}/im-plugin/ism/policies/#states).
2. **Actions**: Any actions that you want the plugin to take when an index enters a state, such as performing a rollover. For more information, see [Actions]({{site.url}}{{site.baseurl}}/im-plugin/ism/policies/#actions).
3. **Transitions**: The conditions that must be met for an index to move into a new state. For example, if an index is more than eight weeks old, you might want to move it to the `delete` state. For more information, see [Transitions]({{site.url}}{{site.baseurl}}/im-plugin/ism/policies/#transitions).

You can also upload a JSON document to specify an index policy.
{: .note}

You have complete flexibility in the way you can design your policies. You can create any state, transition to any other state, and specify any number of actions in each state.

To attach policies to indices, perform the following steps:

1. Under **Index Management**, select **Index policies**.
2. Choose the index or indexes to which you want to attach your policy.
3. Choose the **Apply policy** button.
4. From the **Policy ID** menu, choose the policy that you created.
    View the preview of your policy.
5. (Optional): Specify a rollover alias if your policy includes a rollover operation. Make sure that the alias that you enter already exists. For more information about the rollover operation, see [rollover]({{site.url}}{{site.baseurl}}/im-plugin/ism/policies#rollover).
6. Choose the **Apply** button.
After you attach a policy to an index, ISM creates a job that runs every 5 minutes by default to perform policy actions, check conditions, and transition the index into different states. To change the default time interval for this job, see Settings.

Policy jobs don't run if the cluster state is red.
{: .note}

## Managed indexes

To attach policies to indexes, perform the following steps:

1. Under **Index Management**, select **Manage Indices**.
2. Choose the index or indexes to which you want to attach your policy.
3. Select the **Change policy** button.
4. Choose the **Apply policy** button.

## Indexes

The **Indices** section displays a list of indexes in your OpenSearch cluster. For each index included, you can see its health status (`green`, `yellow`, or `red`), policy (if the index is managed by a policy), status, total size, size of primaries, total documents, deleted documents, primaries, and replicas.

The following are the three index health statuses:

- Green: All primary and replica shards are assigned.
- Yellow: At least one replica shard is not assigned.
- Red: At least one primary shard is not assigned.

### Create index

While you can [create an index]({{site.url}}{{site.baseurl}}/api-reference/index-apis/create-index/) by using a document as a base, you can also create an empty index for later use. 

To create an index, select the **Create Index** button located under the **Indices** section of **Index Management**. Then define the index by setting the following parameters:

- Index name
- Number of primary shards
- Number of replicas
- Refresh interval

You can also add fields and objects using either the visual editor or the JSON editor.

**Advanced settings** allows you to upload a JSON configuration.

### Apply policy

If you analyze time series data, you likely want to prioritize new data over old data. You might periodically perform certain operations on older indexes, such as reducing replica count or deleting them.

[Index State Management]({{site.url}}{{site.baseurl}}/im-plugin/ism/index/) (ISM) is a plugin that lets you automate these periodic administrative operations by triggering them based on changes in the index age, index size, or number of documents. You can define policies that automatically handle index rollovers or deletions to fit your use case.

For example, you can define a policy that moves your index into a **read_only** state after 30 days and then deletes it after a set period of 90 days. You can also set up the policy to send you a notification message when the index is deleted.

You might want to perform an index rollover after a certain amount of time or run a **force_merge** operation on an index during off-peak hours to improve search performance during peak hours.

To apply a policy, select the index to which you want to apply the policy in the **Indices** list under **Index Management**. Then select the **Actions** button, and select **Apply policy** from the dropdown list.

<img src="{{site.url}}{{site.baseurl}}/images/admin-ui-index/apply-policy.PNG" alt="User interface showing apply policy prompt">

### Close index

The [close index]({{site.url}}{{site.baseurl}}/api-reference/index-apis/close-index/) operation closes an index. Once an index is closed, you cannot add data to it or search for any data within the index.

To close an index, select the index you want to close in the **Indices** list under **Index Management**. Then select the **Actions** button, and select **Close** from the dropdown list.

### Open index

The [open index]({{site.url}}{{site.baseurl}}/api-reference/index-apis/open-index/) operation opens a closed index, letting you add or search for data within the index.

To open an index, select the index you want to open in the **Indices** list under **Index Management**. Then select the **Actions** button, and select **Open** from the dropdown list.

### Reindex

The [reindex]({{site.url}}{{site.baseurl}}/api-reference/document-apis/reindex/) operation lets you copy all your data or a subset of data from a source index into a destination index.

To reindex an index, select the index in the **Indices** list under **Index Management**. Then select the **Actions** button, and select **Reindex** from the dropdown list.

<img src="{{site.url}}{{site.baseurl}}/images/admin-ui-index/reindex-expanded.png" alt="User interface showing reindex prompt">

### Shrink index

The [shrink]({{site.url}}{{site.baseurl}}/api-reference/index-apis/shrink-index/) index operation copies all of the data in an existing index into a new index with fewer primary shards.

To shrink an index, select the index you want to shrink in the **Indices** list under **Index Management**. Then select the **Actions** button, and select **Shrink** from the dropdown list.

<img src="{{site.url}}{{site.baseurl}}/images/admin-ui-index/shrink.png" alt="User interface showing shrink prompt">

### Split index

The [split index]({{site.url}}{{site.baseurl}}/api-reference/index-apis/split/) operation splits an existing read-only index into a new index, splitting each primary shard into a number of primary shards in the new index.

To split an index, select the index you want to split in the **Indices** list under **Index Management**. Then select the **Actions** button, and select **Split** from the dropdown list.

<img src="{{site.url}}{{site.baseurl}}/images/admin-ui-index/split-expanded.png" alt="User interface showing split page">

### Delete index

If you no longer need an index, you can use the [delete index]({{site.url}}{{site.baseurl}}/api-reference/index-apis/delete-index/) operation to delete it.

To delete an index, select the index you want to delete in the **Indices** list under **Index Management**. Then select the **Actions** button, and select **Delete** from the dropdown list.

## Templates

[Index templates]({{site.url}}{{site.baseurl}}/opensearch/index-templates/) let you initialize new indexes with predefined mappings and settings. For example, if you continuously index log data, you can define an index template so that all of these indexes have the same number of shards and replicas.

<img src="{{site.url}}{{site.baseurl}}/images/admin-ui-index/templates.PNG" alt="User interface showing Templates page">

### Creating a template

To create a template, select the **Create template** button on the **Templates** page under **Index Management**.

Next, define the template:

1. Enter the template name.
1. Select the template type.
1. Specify any index patterns you would like to use.
1. Set the priority of the template.
1. Select an index alias.
1. Set the number of primary shards.
1. Set the number of replicas.
1. Set the refresh intervals.
1. Add fields and objects for your index mapping using either the visual editor or the JSON editor.
1. Under **Advanced Settings** you can specify advanced index settings with a comma-delimited list.

<img src="{{site.url}}{{site.baseurl}}/images/admin-ui-index/create-template-expanded.png" alt="User interface showing Create Template page">

### Editing a template

To edit a template, select the template you want to edit from the list of templates. Next, select the **Actions** dropdown list and select the **Edit** option.

### Deleting a template

To delete a template, select the template you want to delete from the list of templates. Next, select the **Actions** dropdown list and select the **Delete** option.

## Aliases

An alias is a virtual index name that can point to one or more indexes. If your data is spread across multiple indexes, rather than keeping track of which indexes to query, you can create an alias and query it instead.

<img src="{{site.url}}{{site.baseurl}}/images/admin-ui-index/aliases.PNG" alt="User interface showing Alias page">

To create an alias, perform the following steps:

1. Select the **Create Alias** button on the **Aliases** page under **Index Management**.
2. Specify the alias name.
3. Enter the index, or index patterns, to be part of the alias.
4. Select **Create alias**.

<img src="{{site.url}}{{site.baseurl}}/images/admin-ui-index/create-alias.PNG" alt="User interface showing creat Alias page">

To edit an alias, perform the following steps:

1. Select the alias you want to edit.
2. Select the **Actions** button.
3. Select **Edit** from the dropdown list.

To delete an alias, perform the following steps:

1. Select the alias you want to edit.
2. Select the **Actions** button.
3. Select **Delete** from the dropdown list.

## Rollup jobs

The **Rollup Jobs** section under **Index Management** allows you to create or update index rollup jobs.

To create a rollup job, perform the following steps:

1. Select the **Create rollup job** button on the **Rollup Jobs** page under **Index Management**.
2. Set the name, source index, and target index.
3. Select **Next**.
4. Set the timestamp field and interval type.
5. Optionally, set additional aggregations and additional metrics.
6. Select **Next**.
7. Under **Schedule**, check or uncheck **Enable job by default**.
8. Set the **Continuous**, **Execution frequency**, **Rollup interval**, and **Pages per execution** settings.
9. Additionally, you can set an execution delay.
10. Select **Next**.
11. Review the settings for the rollup job and select **Create**.

You can also disable and enable rollup jobs by selecting the corresponding buttons on the **Rollup Jobs** page.

## Transform jobs

You can create, start, stop, and complete operations with [transform]({{site.url}}{{site.baseurl}}/im-plugin/index-transforms/transforms-apis/) jobs.

To create a transform job, perform the following steps:

1. Select the **Create transform job** button on the **Transform Jobs** page under **Index Management**.
2. Set the name, source index, and target index.
3. Select **Next**.
4. Select the fields to transform. From the table above, select a field you want to transform by clicking **+** next to the field name.
5. Select **Next**.
6. Check or uncheck **Job enabled by default**.
7. Set whether the schedule is continuous and the transform execution interval.
8. Optionally, set pages per execution under the **Advanced** dropdown list.
9. Select **Next**.
10. Review the settings for the rollup job and select **Create**.

You can also disable and enable rollup jobs by selecting the corresponding buttons on the **Transform Jobs** page.

## Long running operation status check 

Certain index operations take time to complete (usually more than 30 seconds, up to tens of minutes or hours). This is tracked on the index status on **Indices** page.

You can check the status of the reindex, shrink, and split operations because they are one-time non-recursive operations.

## Security integration

  Permission control is managed with existing [permissions]({{site.url}}{{site.baseurl}}/security-plugin/access-control/permissions/) or action groups that are enforced at the API level. There is currently no UI-level permission control. Users with permissions to access the ISM plugin are able to view new pages. They can also make changes if they have permissions to run the related APIs.

## Error handling

Similar to API calls, if the operation fails immediately, you will be notified with an error message. However, if it is a long-running operation, you will be notified of the failure at the time of failure, or you can check the index status on the **Indices** page.
 