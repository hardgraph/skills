---
modificationDate: July 01, 2026
title: Type generation reference
description: A reference for the expo-type-information package.
packageName: 'expo-type-information'
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/modules/type-generation-reference/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/modules/type-generation-reference/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > Expo Modules API > Reference
Pages in this section:
- [Module API](https://docs.expo.dev/modules/module-api.md)
- [Inline modules reference](https://docs.expo.dev/modules/inline-modules-reference.md)
- [Type generation reference](https://docs.expo.dev/modules/type-generation-reference.md) (this page)
- [Android lifecycle listeners](https://docs.expo.dev/modules/android-lifecycle-listeners.md)
- [iOS AppDelegate subscribers](https://docs.expo.dev/modules/appdelegate-subscribers.md)
- [Autolinking](https://docs.expo.dev/modules/autolinking.md)
- [Shared objects](https://docs.expo.dev/modules/shared-objects.md)
- [expo-module.config.json](https://docs.expo.dev/modules/module-config.md)
- [Mocking native calls](https://docs.expo.dev/modules/mocking.md)
- [Design considerations](https://docs.expo.dev/modules/design.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Type generation reference

A reference for the expo-type-information package.

> The `expo-type-information` library works only on macOS. Available for SDK 56 and later.

The `expo-type-information` package provides tools to automatically generate TypeScript interface for Swift modules. It consists of multiple parts:

-   Swift parser based on `sourcekitten`: a parser that can retrieve and structure the type information from a Swift Expo module.
-   Type Abstraction: an abstraction over type information relevant for Expo modules, the parser returns the information in this abstracted way.
-   TypeScript AST Emitter: a set of functions which help with generating TypeScript code.
-   CLI: a command-line tool, which integrates the above functionalities.

## Configuration

Install the `expo-type-information` library:

```sh
# npm
npx expo install expo-type-information

# yarn
yarn expo install expo-type-information

# pnpm
pnpm expo install expo-type-information

# bun
bun expo install expo-type-information
```

To use this package you also need to have `sourcekitten` installed. You can use macOS package manager like Homebrew.

```sh
brew install sourcekitten
```

## CLI reference

### Common CLI options

Every CLI command in this library uses the following common options for configuration. The only exception is the `inline-modules-interface` which has slightly different options.

| Flag | Description | Default |
| --- | --- | --- |
| `-i, --input-paths <filePaths. .>` | Paths to Swift files for some module, glob patterns are allowed. |  |
| `-m --module-path <modulePath>` | Path to Expo module root directory. |  |
| `-o, --output-path <filePath>` | Path to save the generated output. If this option is not provided the generated output is printed to console. |  |
| `-t, --type-inference <typeInference>` | Level of type inference: `NO_INFERENCE`, `SIMPLE_INFERENCE`, or `PREPROCESS_AND_INFERENCE`. Note that the `PREPROCESS_AND_INFERENCE` option can occasionally fail on some modules. If you encountered errors, fall back to `SIMPLE_INFERENCE` or `NO_INFERENCE`. | `PREPROCESS_AND_INFERENCE` |
| `-s, --skip-unicode-character-mapping` | Skip mapping all non-ASCII characters in a file to ASCII strings. By default this mapping is performed as SourceKitten is inconsistent when calculating offsets of non-ASCII characters. |  |
| `-w --watcher` | Starts a watcher that checks for changes in input-path file. |  |

### Main commands

#### `module-interface`

Generates a full TypeScript interface for a Swift module. It consists of:

-   **types.ts** file with all types defined in the module
-   **module.ts** with the native module definition
-   **view.tsx** for each view defined in the module
-   **index.ts** file which reexports some functions

> Accepts standard [Common CLI options](/modules/type-generation-reference.md#common-cli-options).

#### `inline-modules-interface`

Creates a TypeScript interface for every Swift inline module in the project. The interface consists of two files:

-   **Module.generated.ts**: This is regenerated with each run of the command
-   **Module.tsx**: This one is not regenerated if you change it

**Options:**

| Flag | Description | Default |
| --- | --- | --- |
| `-a --app-json <appJsonPath>` | A path to the [app config](/workflow/configuration.md) file where the `inline.modules.watchedDirectories` are defined. |  |
| `-w --watcher` | Starts a watcher that checks for changes in inline modules files. |  |
| `-t, --type-inference <typeInference>` | Level of type inference: `NO_INFERENCE`, `SIMPLE_INFERENCE`, or `PREPROCESS_AND_INFERENCE`. Note that the `PREPROCESS_AND_INFERENCE` option can occasionally fail on some modules. If you encountered errors, fall back to `SIMPLE_INFERENCE` or `NO_INFERENCE`. | `SIMPLE_INFERENCE` |

#### `short-module-interface`

Creates a short TypeScript interface for an Expo module. Overwrites **ModuleName.generated.ts** and creates **ModuleName.ts** if not present. Can be used with inline-modules.

> Accepts standard [Common CLI options](/modules/type-generation-reference.md#common-cli-options).

#### `generate-mocks-for-file`

Generates mocks for a given expo module.

> Accepts standard [Common CLI options](/modules/type-generation-reference.md#common-cli-options).

### Other commands

These commands are internal or very specific.

> All commands in this section accept the standard [Common CLI options](/modules/type-generation-reference.md#common-cli-options).

#### `other type-information`

Parses Swift module type information and outputs a `FileTypeInformation` JSON.

#### `other generate-module-types`

Generates a type declaration file content for a module.

#### `other generate-view-types`

Generates a type declaration file for a native View.

#### `other generate-jsx-intrinsics`

Generates a declaration file for a View, updates JSX intrinsics with the View props.

#### `other preprocess-file`

Print the preprocessed file(s) in the state right before parsing them using `sourcekitten`. It helps with checking how the `--module-path`, `--input-path`, and `--type-inference` options affect the parsed file.

## Type information abstraction

### `deserializeTypeInformation(fileTypeinformationSerialized)`

| Parameter | Type | Description |
| --- | --- | --- |
| `fileTypeinformationSerialized` | [FileTypeInformationSerialized](#filetypeinformationserialized) | `FileTypeInformationSerialized` object to deserialize. |

  

Used for testing purposes, maps Arrays to Sets and Maps depending on the field and returns `FileTypeInformation` object.

Returns: `FileTypeInformation`

`FileTypeInformation` object.

### `getFileTypeInformation(options)`

| Parameter | Type | Description |
| --- | --- | --- |
| `options` | [GetFileTypeInformationOptions](#getfiletypeinformationoptions) | Configuration object containing the input source (file or string) and the desired level of type inference. |

  

Reads and extracts `FileTypeInformation` from either a provided file path or a raw string of source code. If a raw string is provided, or if the `PREPROCESS_AND_INFERENCE` inference option is selected, the function will create a temporary file with the (optionally preprocessed) content to facilitate parsing.

Returns: `Promise<filetypeinformation>`

A promise that resolves to a `FileTypeInformation` object if the input was parsed successfully. Otherwise, it resolves to `null`.

### `serializeTypeInformation(fileTypeinformation)`

| Parameter | Type | Description |
| --- | --- | --- |
| `fileTypeinformation` | [FileTypeInformation](#filetypeinformation) | `FileTypeInformation` object to serialize. |

  

Used for testing purposes, maps Sets and Maps to Arrays and returns `FileTypeInformationSerialized` object which can be written to a JSON.

Returns: `FileTypeInformationSerialized`

a `FileTypeInformationSerialized` object.

## TypeScript generation

### `generateConciseTsInterface(fileTypeInformation)`

| Parameter | Type | Description |
| --- | --- | --- |
| `fileTypeInformation` | [FileTypeInformation](#filetypeinformation) | The abstracted type information of an Expo module. |

  

Generates a short TypeScript interface for an Expo module. This creates the content for two files: a volatile generated file containing raw type definitions, and a stable user-facing file that wraps and exports the native module methods in new functions.

Returns: `Promise<{ moduleTypescriptInterfaceFileContent: string, volatileGeneratedFileContent: string }>`

A promise that resolves to an object containing the string contents for both the volatile generated file and the stable TypeScript interface file.

### `generateFullTsInterface(fileTypeInformation)`

| Parameter | Type | Description |
| --- | --- | --- |
| `fileTypeInformation` | [FileTypeInformation](#filetypeinformation) | The abstracted type information of an Expo module. |

  

Generates a full, multi-file TypeScript interface for an Expo module. The generated interface is separated into a file with type definitions, a file which wraps the native module, a file for each view defined in a module and an index file which reexports all definitions from the other files.

Returns: `Promise<{ indexFile: OutputFile, moduleNativeFile: OutputFile, moduleTypesFile: OutputFile, moduleViewsFiles: OutputFile[] } | null>`

A promise that resolves to an object containing the string contents for all of the generated files or `null` if the generation has failed.

### `generateJSXIntrinsicsFileContent(fileTypeInformation)`

| Parameter | Type | Description |
| --- | --- | --- |
| `fileTypeInformation` | [FileTypeInformation](#filetypeinformation) | The abstracted type information of an Expo module. |

  

Generates the TypeScript string content for a native View's type declaration file which mounts the View props on the global `JSXIntrinsics`.

Returns: `Promise<string>`

A promise that resolves to a string containing the TypeScript declaration file content or `null` if the generation has failed.

### `generateModuleTypesFileContent(fileTypeInformation)`

| Parameter | Type | Description |
| --- | --- | --- |
| `fileTypeInformation` | [FileTypeInformation](#filetypeinformation) | The abstracted type information of an Expo module. |

  

Generates the TypeScript string content for a native module type declaration file.

Returns: `Promise<string>`

A promise that resolves to a string containing the TypeScript module declaration file content or `null` if the generation has failed.

### `generateViewTypesFileContent(fileTypeInformation)`

| Parameter | Type | Description |
| --- | --- | --- |
| `fileTypeInformation` | [FileTypeInformation](#filetypeinformation) | The abstracted type information of an Expo module. |

  

Generates the TypeScript string content for a native View's type declaration file.

Returns: `Promise<string>`

A promise that resolves to a string containing the TypeScript declaration file content or `null` if the generation has failed.

## Mock generation

### `generateMocks(files, outputLanguage)`

| Parameter | Type | Description |
| --- | --- | --- |
| `files` | [FileTypeInformation[]](#filetypeinformation) | A list of `FileTypeInformation` objects with generated type information for multiple modules |
| `outputLanguage`(optional) | `'typescript' | 'javascript'` | the language to emit the mocks in. Default: `'javascript'` |

  

This function generates JavaScript/TypeScript mocks for each provided `FileTypeInformation` object.

Returns: `Promise<void>`

nothing

### `getAllExpoModulesInWorkingDirectory()`

Returns: `Promise<filetypeinformation[]>`

## Component

### `withPreparedSingleFile`

Type: React.[Element](https://www.typescriptlang.org/docs/handbook/jsx.html#function-component)<[GetFileTypeInformationOptions](#getfiletypeinformationoptions)\>

## Types

### `AnonymousType`

Literal type: `union`

Represents an anonymous type, a one that is not named instead written directly in the code, such as inline generics, arrays, or optionals.

Acceptable values are: [ParametrizedType](#parametrizedtype) | [SumType](#sumtype) | [OptionalType](#optionaltype) | [DictionaryType](#dictionarytype) | [ArrayType](#arraytype)

### `Argument`

Represents an argument passed to a function or constructor.

| Property | Type | Description |
| --- | --- | --- |
| name | `string | undefined` | - |
| type | [Type](#type) | - |

### `ArrayType`

Type: [Type](#type)

Represents a list or array of a specific type.

> **Note:** The information that this type is array is implicit and exists only in the type system and on the parent type. There is no field on the `ArrayType` object that explicitly indicates that.

### `ClassDeclaration`

Represents a DSL native class declaration.

Type: [DefinitionOffset](#definitionoffset) extended by:

| Property | Type | Description |
| --- | --- | --- |
| asyncMethods | [FunctionDeclaration[]](#functiondeclaration) | - |
| constructor | [ConstructorDeclaration](#constructordeclaration) | null | - |
| methods | [FunctionDeclaration[]](#functiondeclaration) | - |
| name | `string` | - |
| properties | [PropertyDeclaration[]](#propertydeclaration) | - |

### `ConstantDeclaration`

Represents a DSL constant declaration.

Type: [DefinitionOffset](#definitionoffset) extended by:

| Property | Type | Description |
| --- | --- | --- |
| name | `string` | - |
| type | [Type](#type) | - |

### `ConstructorDeclaration`

Represents a DSL class constructor declaration.

Type: [DefinitionOffset](#definitionoffset) extended by:

| Property | Type | Description |
| --- | --- | --- |
| arguments | [Argument[]](#argument) | - |

### `DefinitionOffset`

Retains information of where the thing was defined in the file. As collecting type information is written in asynchronous way it is non-deterministic. To make it deterministic we just sort the declaration by the definitionOffset, maintaining the same ordering as in original file.

| Property | Type | Description |
| --- | --- | --- |
| definitionOffset | `number` | - |

### `DictionaryType`

Represents a dictionary type, defining the explicit types for its keys and values.

| Property | Type | Description |
| --- | --- | --- |
| key | [Type](#type) | - |
| value | [Type](#type) | - |

### `EnumCase`

Represents a single case inside an enum declaration.

Type: `string`

### `EnumType`

Represents an enum type, containing its name and all associated cases.

| Property | Type | Description |
| --- | --- | --- |
| cases | [EnumCase[]](#enumcase) | - |
| name | `string` | - |
| stringBacked | `boolean` | - |

### `EventDeclaration`

Represents a DSL event declaration.

Type: `string`

### `Field`

Type: [Argument](#argument)

Represents a single field within a record or a struct.

### `FileInputOption`

Defines an input option for extracting type information from a set of physical files.

| Property | Type | Description |
| --- | --- | --- |
| inputFileAbsolutePaths | `string[]` | - |
| type | `'file'` | - |

### `FileTypeInformation`

`FileTypeInformation` object abstracts over type related information in a file. The abstraction is closely related to Typescript and expo NativeModules (both to be independent of the actual native side and to give accurate information about what and how we can use the given module).

| Property | Type | Description |
| --- | --- | --- |
| declaredTypeIdentifiers | `Set<string>` | - |
| enums | [EnumType[]](#enumtype) | - |
| inferredTypeParametersCount | `Map<string, number>` | - |
| moduleClasses | [ModuleClassDeclaration[]](#moduleclassdeclaration) | - |
| records | [RecordType[]](#recordtype) | - |
| typeIdentifierDefinitionMap | [TypeIdentifierDefinitionMap](#typeidentifierdefinitionmap) | - |
| usedTypeIdentifiers | `Set<string>` | - |

### `FileTypeInformationSerialized`

Serialized version of the `FileTypeInformation`, suitable for JSON storage or testing environments.

| Property | Type | Description |
| --- | --- | --- |
| declaredTypeIdentifiersList | `string[]` | - |
| enums | [EnumType[]](#enumtype) | - |
| inferredTypeParametersCountList | `undefined` | - |
| moduleClasses | [ModuleClassDeclaration[]](#moduleclassdeclaration) | - |
| records | [RecordType[]](#recordtype) | - |
| typeIdentifierDefinitionList | [TypeIdentifierDefinitionList](#typeidentifierdefinitionlist) | - |
| usedTypeIdentifiersList | `string[]` | - |

### `FunctionDeclaration`

Represents a DSL function declaration.

Type: [DefinitionOffset](#definitionoffset) extended by:

| Property | Type | Description |
| --- | --- | --- |
| arguments | [Argument[]](#argument) | - |
| isStatic | `boolean` | - |
| name | `string` | - |
| parameters | [Type[]](#type) | - |
| returnType | [Type](#type) | - |

### `GetFileTypeInformationOptions`

Options specifying the input source and inference level for retrieving type information.

| Property | Type | Description |
| --- | --- | --- |
| input | [StringInputOption](#stringinputoption) | [FileInputOption](#fileinputoption) | The input source, provided either as a direct string or a file path. |
| mapUnicodeCharacters | `boolean` | An option to map unicode code points to ASCII strings to fix underlying SourceKit issue. |
| typeInference(optional) | [TypeInferenceOption](#typeinferenceoption) | The desired level of type inference. Defaults to PREPROCESS_AND_INFERENCE if omitted. |

### `IdentifierDefinition`

Represents a definition of an identifier.

| Property | Type | Description |
| --- | --- | --- |
| definition | string | [RecordType](#recordtype) | [EnumType](#enumtype) | [ClassDeclaration](#classdeclaration) | - |
| kind | [IdentifierKind](#identifierkind) | - |

### `ModuleClassDeclaration`

Represents a DSL module declaration.

Type: [DefinitionOffset](#definitionoffset) extended by:

| Property | Type | Description |
| --- | --- | --- |
| asyncFunctions | [FunctionDeclaration[]](#functiondeclaration) | - |
| classes | [ClassDeclaration[]](#classdeclaration) | - |
| constants | [ConstantDeclaration[]](#constantdeclaration) | - |
| constructor | [ConstructorDeclaration](#constructordeclaration) | null | - |
| events | [EventDeclaration[]](#eventdeclaration) | - |
| functions | [FunctionDeclaration[]](#functiondeclaration) | - |
| name | `string` | - |
| properties | [PropertyDeclaration[]](#propertydeclaration) | - |
| props | [PropDeclaration[]](#propdeclaration) | - |
| views | [ViewDeclaration[]](#viewdeclaration) | - |

### `OptionalType`

Type: [Type](#type)

Represents an optional type that can also resolve to null or undefined.

> **Note:** The information that this type is optional is implicit and exists only in the type system and on the parent type. There is no field on the `OptionalType` object that explicitly indicates that.

### `OutputFile`

A helper type which contains the generated file content and name.

| Property | Type | Description |
| --- | --- | --- |
| content | `string` | - |
| name | `string` | - |

### `ParametrizedType`

Represents a parametrized type, that is a generic type with specified parameters e.g. Map<string, number>.

| Property | Type | Description |
| --- | --- | --- |
| name | [TypeIdentifier](#typeidentifier) | - |
| types | [Type[]](#type) | - |

### `PropDeclaration`

Represents a DSL prop declaration.

Type: [DefinitionOffset](#definitionoffset) extended by:

| Property | Type | Description |
| --- | --- | --- |
| arguments | [Argument[]](#argument) | - |
| name | `string` | - |

### `PropertyDeclaration`

Type: [ConstantDeclaration](#constantdeclaration)

Represents a DSL property declaration.

### `RecordType`

Represents a struct or dictionary-like record consisting of named fields.

| Property | Type | Description |
| --- | --- | --- |
| fields | [Field[]](#field) | - |
| name | `string` | - |

### `StringInputOption`

Defines an input option for extracting type information directly from a raw string of source code.

| Property | Type | Description |
| --- | --- | --- |
| fileContent | `string` | - |
| language | `'Swift'` | - |
| type | `'string'` | - |

### `SumType`

Represents a union or a sum type where a value can be one of several different types.

| Property | Type | Description |
| --- | --- | --- |
| types | [Type[]](#type) | - |

### `Type`

Represents an abstract type node.

| Property | Type | Description |
| --- | --- | --- |
| kind | [TypeKind](#typekind) | - |
| type | [BasicType](#basictype) | [TypeIdentifier](#typeidentifier) | [AnonymousType](#anonymoustype) | - |

### `TypeIdentifier`

Represents a type identifier as a string reference.

Type: `string`

### `TypeIdentifierDefinitionList`

Type: `undefined`

Serialized version of the `TypeIdentifierDefinitionMap`.

### `TypeIdentifierDefinitionMap`

Type: Map<string, [IdentifierDefinition](#identifierdefinition)\>

Maps type identifier strings to their definition objects.

### `ViewDeclaration`

Type: [ModuleClassDeclaration](#moduleclassdeclaration)

Represents a DSL view declaration.

## Enums

### `BasicType`

Represents a basic type that is not user defined.

#### `ANY`

`BasicType.ANY = 0`

#### `STRING`

`BasicType.STRING = 1`

#### `NUMBER`

`BasicType.NUMBER = 2`

#### `BOOLEAN`

`BasicType.BOOLEAN = 3`

#### `VOID`

`BasicType.VOID = 4`

#### `UNDEFINED`

`BasicType.UNDEFINED = 5`

#### `UNRESOLVED`

`BasicType.UNRESOLVED = 6`

Represents a type that couldn't be resolved

### `IdentifierKind`

Represents the kind of a parsed identifier from a native file.

#### `BASIC`

`IdentifierKind.BASIC = 0`

#### `ENUM`

`IdentifierKind.ENUM = 1`

#### `RECORD`

`IdentifierKind.RECORD = 2`

#### `CLASS`

`IdentifierKind.CLASS = 3`

### `TypeInferenceOption`

Defines the level of type inference to apply when extracting type information.

> **Note:** In case where type inference is on, it may take more than twice the time to compute the type information.

#### `NO_INFERENCE`

`TypeInferenceOption.NO_INFERENCE = 0`

No type inference will be performed.

#### `SIMPLE_INFERENCE`

`TypeInferenceOption.SIMPLE_INFERENCE = 1`

Basic type inference will be applied.

#### `PREPROCESS_AND_INFERENCE`

`TypeInferenceOption.PREPROCESS_AND_INFERENCE = 2`

Preprocesses the file by injecting returns to extract more type info from sourcekitten.

### `TypeKind`

Categorizes the type node within the abstract syntax tree.

#### `BASIC`

`TypeKind.BASIC = 0`

#### `IDENTIFIER`

`TypeKind.IDENTIFIER = 1`

#### `SUM`

`TypeKind.SUM = 2`

#### `PARAMETRIZED`

`TypeKind.PARAMETRIZED = 3`

#### `OPTIONAL`

`TypeKind.OPTIONAL = 4`

#### `ARRAY`

`TypeKind.ARRAY = 5`

#### `DICTIONARY`

`TypeKind.DICTIONARY = 6`

## Swift parser limitations

`expo-type-information` library uses `sourcekitten` to parse the Swift file. Parsing a Swift file is a complex task and not everything is now implemented.

#### Known issues about the current version of parsing the Swift file

-   Nested classes will not be resolved fully.
    
    `Sourcekitten` has a limit on resolving nested closures, so classes defined in your module may not be fully resolved. This is the reason why if there is a DSL `Class` in a module, its methods will have their return types `unresolved`.
    
-   Return type resolution, `PREPROCESS_AND_INFERENCE` inference option.
    
    When the return type of a closure is not provided explicitly, the tool needs to infer it. One of the ways of doing it in `sourcekitten` is when there is a `return identifier` statement we can ask about the type of the `identifier`. The third inference option, `PREPROCESS_AND_INFERENCE`, rewrites the file such that for each `return expression` statement a new `let return_expression = expression; return return_expression;` is inserted so that we can ask about the identifier type. Rewriting is known to sometimes have issues (mostly because of strings and comments) so it may not always work. There is also currently no way to resolve a `return`-less return, when the return expression is a tail expression of the closure.
    
-   Unsupported Expo Module DSL declarations.
    
    Not all of the DSL declarations are parsed right now, and some are not parsed in every context. For example `Events` are parsed inside a `View` but not in a Module definition.
    
-   Using Unicode characters breaks `sourcekitten` offsets.
    

### Supported Expo modules DSL declarations

| Feature | Notes and limitations |
| --- | --- |
| **Expo DSL Declarations** | Supports parsing for: `AsyncFunction`, `Constant`, `Constructor`, `Events`, `Function`, `Name`, `Prop`, `Property`, and `View`. |
| **Swift `struct` & `class`** | Must conform to the `Record` protocol. Only properties marked with the `@Field` annotation are parsed. |
| **Swift `enum`** | Basic cases are supported. Values associated with enum cases are not currently parsed. |
