# check-proper-type-of

An element in a `type of` expression doesn’t have proper type information.

The message's severity is `Info` but may be raised to `Error` in the SQL,
SAP HANA, and OData backends.  These backends require elements to have a type.
Otherwise they aren’t able to render elements (for example, to SQL columns).

## Example

Erroneous code example:

```cdl
entity Foo {
  key id : Integer;
};
view ViewFoo as select from Foo {
  1+1 as calculatedField @(anno)
};
entity Bar {
  // `e` has no proper type but has the annotation `@anno`.
  e : type of ViewFoo:calculatedField;
};
```

`ViewFoo:calculatedField` is a calculated field without an explicit type.

`type of` is used in `E:e`'s type specification.  You would expect the element
to have a proper type.  However, because the referenced element is calculated,
the compiler isn’t able to determine the correct type.
The element still inherits `ViewFoo:calculatedField`'s annotations and other
properties but won’t have a proper type, which is required by some backends.

## How to Fix

To fix the issue, assign an explicit type to `ViewFoo:calculatedField`.

```cdl
view ViewFoo as select from Foo {
  1+1 as calculatedField @(anno) : Integer
};
```

## Related Messages

- `check-proper-type`
