# Interface: QueryMeta

> For AI agents: see [llms.txt](/llms.txt) for the complete documentation index. Markdown versions are available by adding .md to a page URL or requesting Accept: text/markdown.

[server](/api/modules/server.md).QueryMeta

Extra context available in Convex query functions.

## Hierarchy[​](#hierarchy "Direct link to Hierarchy")

* **`QueryMeta`**

  ↳ [`MutationMeta`](/api/interfaces/server.MutationMeta.md)

## Methods[​](#methods "Direct link to Methods")

### getFunctionMetadata[​](#getfunctionmetadata "Direct link to getFunctionMetadata")

▸ **getFunctionMetadata**(): `Promise`<[`FunctionMetadata`](/api/modules/server.md#functionmetadata)>

#### Returns[​](#returns "Direct link to Returns")

`Promise`<[`FunctionMetadata`](/api/modules/server.md#functionmetadata)>

#### Defined in[​](#defined-in "Direct link to Defined in")

[server/meta.ts:133](https://github.com/get-convex/convex-js/blob/main/src/server/meta.ts#L133)

***

### getTransactionMetrics[​](#gettransactionmetrics "Direct link to getTransactionMetrics")

▸ **getTransactionMetrics**(): `Promise`<[`TransactionMetrics`](/api/modules/server.md#transactionmetrics)>

#### Returns[​](#returns-1 "Direct link to Returns")

`Promise`<[`TransactionMetrics`](/api/modules/server.md#transactionmetrics)>

#### Defined in[​](#defined-in-1 "Direct link to Defined in")

[server/meta.ts:134](https://github.com/get-convex/convex-js/blob/main/src/server/meta.ts#L134)

***

### getDeploymentMetadata[​](#getdeploymentmetadata "Direct link to getDeploymentMetadata")

▸ **getDeploymentMetadata**(): `Promise`<[`DeploymentMetadata`](/api/modules/server.md#deploymentmetadata)>

#### Returns[​](#returns-2 "Direct link to Returns")

`Promise`<[`DeploymentMetadata`](/api/modules/server.md#deploymentmetadata)>

#### Defined in[​](#defined-in-2 "Direct link to Defined in")

[server/meta.ts:135](https://github.com/get-convex/convex-js/blob/main/src/server/meta.ts#L135)
