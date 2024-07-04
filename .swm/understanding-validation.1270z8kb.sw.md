---
title: Understanding Validation
---
Validation in DEMO-one-app refers to the process of checking the correctness and integrity of data before it is processed. It is implemented using the Joi library, which allows for a declarative approach to define the shape and constraints of data. The validation functions are primarily used to validate schemas, such as the Progressive Web App (PWA) configuration and web manifest. For instance, the `validateSchema` function takes a schema, a target for validation, and options, then uses the `validate` method from Joi to check if the target matches the schema. If there are any validation errors, they are thrown; otherwise, the validated value is returned.

<SwmSnippet path="/src/server/utils/validation/shared.js" line="19">

---

## Validation Schemas

Here, we define validation schemas using Joi. For example, `urlSchema` validates that a value is a string and a valid URL, and `colorSchema` validates that a value is a string or a hexadecimal color code.

```javascript
export const urlSchema = Joi.string().uri({
  allowRelative: true,
}).messages({
  'string.base': '{{#label}} must be a string',
  'string.uri': '{{#label}} must be a relative or absolute URL',
});

export const colorSchema = Joi.alternatives(
  Joi.string(),
  Joi.string().hex()
);
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/utils/validation/index.js" line="19">

---

## Validation Function

The `validateSchema` function is used to validate a target value against a schema. If the validation fails, it throws an error. Otherwise, it returns the validated value.

```javascript
export function validateSchema(schema, validationTarget, options) {
  const { error, value } = schema.validate(
    validationTarget,
    { ...options, abortEarly: false }
  );
  if (error) throw error;
  return value;
}
```

---

</SwmSnippet>

<SwmSnippet path="/src/server/utils/validation/index.js" line="28">

---

## Validation Usage

Here's an example of how to use the `validateSchema` function. In this case, we're validating a `pwaConfig` object against the `pwaSchema`.

```javascript
export function validatePWAConfig(pwaConfig, context) {
  return validateSchema(pwaSchema, pwaConfig, { context });
}
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
