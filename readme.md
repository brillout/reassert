Minimalistic & simple assertions for library authors.

Designed so that you can create all kinds of assertion types.

For example:

~~~ts
import { createError } from "@brillout/libassert";

export { assert, assertUsage, assertWarning };

const libName = "Awesome Library";

function assert(condition: unknown): asserts condition {
  if (condition) {
    return;
  }

  const prefix =
  `[${libName}][Internal Error] Something unexpected happened, `+
  `please open a GitHub issue.`;
  const err = createError({ prefix });

  throw err;
}

function assertUsage(condition: unknown, errorMessage: string): asserts condition {
  if (condition) {
    return;
  }

  const err = createError({
    prefix: `[${libName}][Wrong Usage]`,
    errorMessage,
  });

  throw err;
}

function assertWarning(condition: unknown, errorMessage: string): void {
  if (condition) {
    return;
  }

  const err = createError({
    prefix: `[${libName}][Warning]`,
    errorMessage,
  });

  console.warn(err);
}
~~~

The `createError(errorMessage)` is the same than `new Error(${prefix} ${errorMessage})` except that:
 - `prefix` and `errorMessage` are forbidden to contain new lines.
 - The stack trace is complete but also cleaned to remove useless information.
