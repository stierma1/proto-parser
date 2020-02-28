# proto-parser [![npm version](https://badgen.net/badge/npm/v0.0.x/red)](https://www.npmjs.com/package/proto-parser)

A parser for [proto](https://developers.google.com/protocol-buffers/docs/proto3) files.

## Table of Contents

* [Introduction](#introduction)
* [Usage](#usage)
  * [Install](#install)
  * [Coding](#coding)
* [API](#api)
  * [parse](#parse)

## Introduction

The target of proto-parser is to generate an AST for a proto file, regardless of injecting upper functions into the AST structure, such as encoding/decoding.

### vs protobuf.js

Proto-parser is developed based on [protobuf.js](https://github.com/protobufjs/protobuf.js), and uses some codes of protobuf.js。

Compared with protobuf.js, proto-parser is more focused on parsing. so, proto-parser gains some advantages:

- with less code and higher performance
- output more pure AST structure
- support TypeScript

## Usage

### Install

```
npm install proto-parser
```

### Coding

A code example:

```ts
import * as t from 'proto-parser';

const content = `
syntax = 'proto3';
message Foo {
  string key = 1;
}
`;

const protoDocument = t.parse(content) as t.ProtoDocument;
console.log(JSON.stringify(protoDocument, null, 2));
```

The resulted AST:

```json
{
  "syntax": "proto3",
  "root": {
    "name": "",
    "fullName": "",
    "syntaxType": "ProtoRoot",
    "nested": {
      "Foo": {
        "name": "Foo",
        "fullName": ".Foo",
        "comment": null,
        "syntaxType": "MessageDefinition",
        "fields": {
          "key": {
            "name": "key",
            "fullName": ".Foo.key",
            "comment": null,
            "type": {
              "value": "string",
              "syntaxType": "BaseType"
            },
            "id": 1,
            "required": false,
            "optional": true,
            "repeated": false,
            "map": false
          }
        }
      }
    }
  },
  "syntaxType": "ProtoDocument"
}
```

## API

### <a name="parse"></a> `> parse(source: string, option?: ParseOption): ProtoDocument | ProtoError`

Parse the content of a proto file to an AST.

The definition of ParseOption:

```ts
interface ParseOption {
  // Set whethe to keep the origin case. The default value is true.
  keepCase?: boolean

  // Set whethe to enable alternateCommentMode. The default value is true.
  alternateCommentMode?: boolean 

  // Set whethe to resolve types. The default value is true.
  resolve?: boolean

  // Set whethe to return pure json Object. The default value is true.
  toJson?: boolean
}
```

## License

[MIT](https://github.com/microsoft/vscode/blob/master/LICENSE.txt)