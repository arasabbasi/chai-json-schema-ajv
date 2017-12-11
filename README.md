# chai-json-schema-ajv

[![Build Status](https://travis-ci.org/up9cloud/chai-json-schema-ajv.svg?branch=master)](https://travis-ci.org/up9cloud/chai-json-schema-ajv)
[![Coverage Status](https://coveralls.io/repos/github/up9cloud/chai-json-schema-ajv/badge.svg?branch=master)](https://coveralls.io/github/up9cloud/chai-json-schema-ajv?branch=master)

A chai plugin for validate json schema.

This is based on [ajv](https://github.com/epoberezkin/ajv), a JSON schema Validator fully support [JSON Schema draft 4](http://json-schema.org/)

## Usage

```sh
npm install chai-json-schema-ajv@1
```

### Basic (jsonSchema)

```js
const chai = require('chai')
chai.use(require('chai-json-schema-ajv'))
const expect = chai.expect

let apple = {
  name: 'foo',
  color: ['red', 'green', 'yellow'],
  value: 10
}
let schema = {
  title: 'fruit schema v0.1',
  type: 'object',
  required: ['name', 'color', 'value'],
  properties: {
    name: {
      type: 'string',
      minLength: 3
    },
    color: {
      type: 'array',
      minItems: 1,
      uniqueItems: true,
      items: {
        type: 'string'
      }
    },
    value: {
      type: 'integer',
      minimum: 5
    }
  }
}

expect(apple).to.be.jsonSchema(schema)
```

### Basic (validJsonSchema)

```js
const chai = require('chai')
chai.use(require('chai-json-schema-ajv'))
const expect = chai.expect

let schema = {
  title: 'valid schema',
  type: 'object',
  required: ['name'],
  properties: {
    name: {
      type: 'string',
      minLength: 3
    }
  }
}

expect(schema).to.be.validJsonSchema
```

### With custom ajv options

```js
const options = { ... }
chai.use(require('chai-json-schema-ajv').withOptions(options))

...
```

- options will be send to [ajv](https://github.com/epoberezkin/ajv#options)

## Verbose

Default error message is parsed by `ajv.errorsText`.

If we set `withOptions({verbose: true})`, it will print full errors.

```js
...
chai.use(require('chai-json-schema-ajv')
...
 
//expected value not match the json-schema
//data.value should be integer
```

```js
...
chai.use(require('chai-json-schema-ajv').withOptions({ verbose: true }))
...

//expected value not match the json-schema
//[
//  {
//    "keyword": "type",
//    "dataPath": ".value",
//    "schemaPath": "#/properties/value/type",
//    "params": {
//      "type": "integer"
//    },
//    "message": "should be integer",
//    "schema": "integer",
//    "parentSchema": {
//      "type": "integer"
//    },
//    "data": 1.1
//  }
//]
```

## TODO

- support browser side
- move to es2017 async/await (v2)
- ~~add lint~~
- ~~send option to ajv~~ (thanks @dimac)

## License

MIT
