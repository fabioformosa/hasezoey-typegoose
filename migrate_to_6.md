# How to migrate from ~5.9 to 6.0.x

## Important, Read this first

To migrate "seamless" you should be on the latest Typegoose Version of 5.x (Currently 5.9.0) and then to migrate, upgrade to 6.0.x (not 6.x)
because in version 6.1+ the deprecated functions will get removed!

## InstanceType changed

`InstanceType<T>` got renamed to `DocumentType<T>`

## `getModelForClass`, `setModelForClass`, `buildSchema`

they are not in the Typegoose class anymore, they are now outsourced, which means the new syntax is the following
(for "seamless" migration the Typegoose Class still exists and has the functions, but with deprecation)

```ts
import { getModelForClass } from 'typegoose';
class Name {}

const NameModel = getModelForClass(Name);
```

Note: Typegoose is an empty class now, it is kept for future use, and "style"

## (ic) data.ts collections are now Map<T, S>

data.ts's collections got refactored to use ES6 Maps

## ModelOptions

- `getModelForClass(class, options)`' options got removed
- `setModelForClass(class, options)`' options got removed
- `buildSchema(class, options)`' options got removed

use the following decorator now

```ts
@modelOptions({ schemaOptions: {} })
class Name {}
```

## Hooks

Hooks got (in 6.0.0-13) a change for the types to comply with the latest mongoose (5.6.8)
-> no workarounds or typedefs required anymore

## Methods (staticMethod, instanceMethod, virtuals)

`@staticMethod` & `@instanceMethod` got deprecated in favor of `schema.loadClass()`
(means these decorators are no more needed, so they are auto-detected)

for virtual-populates use `@prop({ localField, foreignField })` and no more `overwrite` option is needed, it will auto detect if one of these values is included
for normal virtuals, just use `get somevalue() { return ''; }` and `set somevalue(val: string) { }` (no more `@prop` needed)

## setModelForClass got deprecated

`setModelForClass()` got deprecated, because mongoose would throw an OverwriteModelError if attempted to overwrite a model
-> use `getModelForClass()`

## Notes

* (ic) The internal handling of schema creation has changed a bit but tried to keep the inputs & outputs the same, means in some edge-cases it can happen to not work anymore

---

*`ic` means `internal change`*
