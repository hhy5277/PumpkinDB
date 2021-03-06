# CURSOR/SEEK?

{% method -%}

Sets the cursor at the key value pair with a greater or equal key

Input stack: `cursor key`

Output stack: `b`

If there is a key/value pair in the database that has a key
that is greater or equal to `key`, `1` will be pushed onto the stack.
Otherwise, `0` will be pushed and the cursor will be moved.

Useful in conjunction with [CURSOR/CUR](../QCURSOR/CUR.md)

{% common -%}

```
PumpkinDB> ["3" "3" ASSOC COMMIT] WRITE [CURSOR 'c SET c "2" CURSOR/SEEK?] READ
1
```

{% endmethod %}

## Allocation

Allocates for values to be put onto the stack

## Errors

NoTransaction error if there's no current write transaction

InvalidValue error if the cursor identifier is incorrect or expired

## Tests

```test
works : ["3" "3" ASSOC COMMIT] WRITE [CURSOR 'c SET c "2" CURSOR/SEEK?] READ.
requires_txn : ["1" "1" CURSOR/SEEK?] TRY UNWRAP 0x08 EQUAL?.
empty_stack : [CURSOR/SEEK?] TRY UNWRAP 0x04 EQUAL?.
empty_stack_1 : ["a" CURSOR/SEEK?] TRY UNWRAP 0x04 EQUAL?.
invalid_cursor : [["1" "A" CURSOR/SEEK?] READ] TRY UNWRAP 0x03 EQUAL?.
```
