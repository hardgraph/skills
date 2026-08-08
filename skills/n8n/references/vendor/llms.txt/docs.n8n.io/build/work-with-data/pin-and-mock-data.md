> For the complete documentation index, see [llms.txt](https://docs.n8n.io/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://docs.n8n.io/build/work-with-data/pin-and-mock-data.md).

# Pin and mock data

When developing workflows, you might want to test your logic without repeatedly calling external systems or working with live data. n8n provides two related features to help with this:

* **Data mocking**: Create or simulate test data without connecting to real data sources
* **Data pinning**: Save test data (mocked or real) and reuse it in future workflow executions instead of fetching fresh data

Both approaches save time and resources during development, help you work with consistent datasets, and protect live systems from repeated test calls.

{% hint style="info" %}
**For development only**

Data pinning and mocking are features to help test workflows during development. Data pinning isn't available for production workflow executions.
{% endhint %}

## Data mocking approaches <a href="#data-mocking-approaches" id="data-mocking-approaches"></a>

Create test data to work with during development. You can create mock data in several ways:

### Generate custom data using the Code or Edit Fields nodes <a href="#generate-custom-data-using-the-code-or-edit-fields-nodes" id="generate-custom-data-using-the-code-or-edit-fields-nodes"></a>

You can create a custom dataset in your workflow using either the [Code node](/integrations/builtin/core-nodes/n8n-nodes-base.code.md) or the [Edit Fields (Set) node](/integrations/builtin/core-nodes/n8n-nodes-base.set.md).

In the Code node, you can create any data set you want, and return it as the node output. In the Edit Fields node, select **Add fields** to add your custom data.

The Edit Fields node is a good choice for small tests. To create more complex datasets, use the Code node.

**Use this approach when**: You need complete control over your test data structure and values, or when you want to test edge cases with specific data patterns.

### Output a sample data set from the Customer Datastore node <a href="#output-a-sample-data-set-from-the-customer-datastore-node" id="output-a-sample-data-set-from-the-customer-datastore-node"></a>

The Customer Datastore node provides a fake dataset to work with. Add and execute the node to explore the data.

**Use this approach when**: You need some test data when exploring n8n, and you don't have a real use-case to work with.

Once you've created or obtained test data you want to reuse across multiple workflow executions, use [Data pinning](#data-pinning) to save it for consistent testing.

## Data pinning <a href="#data-pinning" id="data-pinning"></a>

You can 'pin' data during workflow development. Data pinning means saving the output data of a node and using the saved data instead of fetching fresh data in future workflow executions.

You can use this when working with data from external sources to avoid having to repeat requests to the external system. This can save time and resources:

* If your workflow relies on an external system to trigger it, such as a webhook call, being able to pin data means you don't need to use the external system every time you test the workflow.
* If the external resource has data or usage limits, pinning data during tests avoids consuming your resource limits.
* You can fetch and pin the data you want to test, then have confidence that the data is consistent in all your workflow tests.
* You can mock test data (using the approaches above), then pin it for reuse across executions.

You can only pin data for nodes that have a single main output ("error" outputs don't count for this purpose).

### Pin data <a href="#pin-data" id="pin-data"></a>

To pin data in a node:

1. Run the node to load data.
2. In the **OUTPUT** view, select **Pin data** <img src="/spaces/GixZThfitWP21x2gQFpD/files/WsEGdGRMGI30vSkTmMHD" alt="Pin data icon" data-size="line">. When data pinning is active, the button is disabled and a "This data is pinned" banner is displayed in the **OUTPUT** view.

{% hint style="info" %}
**Nodes that output binary data**

You can't pin data if the output data includes binary data.
{% endhint %}

### Unpin data <a href="#unpin-data" id="unpin-data"></a>

When data pinning is active, a banner appears at the top of the node's output panel indicating that n8n has pinned the data. To unpin data and fetch fresh data on the next execution, select the **Unpin** link in the banner.

### Edit pinned data <a href="#edit-pinned-data" id="edit-pinned-data"></a>

n8n allows you to edit pinned data. This means you can check different scenarios without setting up each scenario and sending the relevant data from your external system. It makes it easier to test edge cases.

{% hint style="info" %}
**For development only**

Data editing isn't available for production workflow executions. It's a feature to help test workflows during development.
{% endhint %}

#### Edit output data <a href="#edit-output-data" id="edit-output-data"></a>

To edit output data:

1. Run the node to load data.
2. In the **OUTPUT** view, select **JSON** to switch to JSON view.
3. Select **Edit** <img src="/files/Nxv7jZixzRfyQDYEZxqu" alt="Edit data icon" data-size="line">.
4. Edit your data.
5. Select **Save**. n8n saves your data changes and pins your data.

#### Use data from previous executions <a href="#use-data-from-previous-executions" id="use-data-from-previous-executions"></a>

You can copy data from nodes in previous workflow executions:

1. Open the left menu.
2. Select **Executions**.
3. Browse the workflow executions list to find the one with the data you want to copy.
4. Select **Open Past Execution** <img src="/files/L8sKNoHmgiGxccol0tGk" alt="Open past execution icon" data-size="line">.
5. Double click the node whose data you want to copy.
6. If it's table layout, select **JSON** to switch to JSON view.
7. There are two ways to copy the JSON:
8. Select the JSON you want by highlighting it, like selecting text. Then use `ctrl` + `c` to copy it.
9. Select the JSON you want to copy by clicking on a parameter. Then: 1. Hover over the JSON. n8n displays the **Copy** <img src="/files/ZCg9FIST322yj9yt4Sr4" alt="Copy data icon" data-size="line"> button. 2. Select **Copy** <img src="/files/ZCg9FIST322yj9yt4Sr4" alt="Copy data icon" data-size="line">. 3. You can choose what to copy:
   * **Copy Item Path** and **Copy Parameter Path** give you expressions that access parts of the JSON.
   * **Copy Value**: copies the entire selected JSON.
10. Return to the workflow you're working on:
    1. Open the left menu.
    2. Select **Workflows**.
    3. Select **Open**.
    4. Select the workflow you want to open.
11. Open the node where you want to use the copied data.
12. If there is no data, run the node to load data.
13. In the **OUTPUT** view, select **JSON** to switch to JSON view.
14. Select **Edit** <img src="/files/Nxv7jZixzRfyQDYEZxqu" alt="Edit data icon" data-size="line">.
15. Paste in the data from the previous execution.
16. Select **Save**. n8n saves your data changes and pins your data.

### Combine mocking with pinning <a href="#combine-mocking-with-pinning" id="combine-mocking-with-pinning"></a>

For the most realistic testing experience, you can combine mocking and pinning approaches:

1. Create test data using one of the mocking approaches (Code node, Edit Fields node, or Customer Datastore)
2. Edit the test data to create specific test scenarios or edge cases
3. Pin the edited data for reuse across multiple workflow executions
4. Continue developing with this edited, pinned dataset

This approach gives you complete control over your test data while ensuring consistent testing across multiple runs.
