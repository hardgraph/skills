# Class: SchemaDefinition\<Schema, StrictTableTypes>

> For AI agents: see [llms.txt](/llms.txt) for the complete documentation index. Markdown versions are available by adding .md to a page URL or requesting Accept: text/markdown.

[server](/api/modules/server.md).SchemaDefinition

The definition of a Convex project schema.

This should be produced by using [defineSchema](/api/modules/server.md#defineschema).

## Type parameters[​](#type-parameters "Direct link to Type parameters")

| Name               | Type                                                            |
| ------------------ | --------------------------------------------------------------- |
| `Schema`           | extends [`GenericSchema`](/api/modules/server.md#genericschema) |
| `StrictTableTypes` | extends `boolean`                                               |

## Properties[​](#properties "Direct link to Properties")

### tables[​](#tables "Direct link to tables")

• **tables**: `Schema`

#### Defined in[​](#defined-in "Direct link to Defined in")

[server/schema.ts:680](https://github.com/get-convex/convex-js/blob/main/src/server/schema.ts#L680)

***

### strictTableNameTypes[​](#stricttablenametypes "Direct link to strictTableNameTypes")

• **strictTableNameTypes**: `StrictTableTypes`

#### Defined in[​](#defined-in-1 "Direct link to Defined in")

[server/schema.ts:681](https://github.com/get-convex/convex-js/blob/main/src/server/schema.ts#L681)

***

### schemaValidation[​](#schemavalidation "Direct link to schemaValidation")

• `Readonly` **schemaValidation**: `boolean`

#### Defined in[​](#defined-in-2 "Direct link to Defined in")

[server/schema.ts:682](https://github.com/get-convex/convex-js/blob/main/src/server/schema.ts#L682)
