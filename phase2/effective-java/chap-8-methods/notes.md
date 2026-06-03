# Methods

## Item 49: check params for validity 

1. Always document and enforce parameter restrictions at the start of methods/constructors to fail fast with clear exceptions.
2. Use Javadoc `@throws` for public APIs and leverage `Objects.requireNonNull()` or Java 9's range-check methods for validation.
3. For private methods, assertions may suffice since you, as the package author, control the call sites.
4. Crucially validate parameters stored for later use to preserve failure atomicity and ease future debugging.
5. Skip explicit checks only when validation is costly and occurs implicitly during computation (e.g., `Collections.sort`).
6. Ensure any implicit validation throws the documented exception; use exception translation if needed (Item 73).
7. Design methods to accept the broadest reasonable parameter range, enforcing only intrinsic constraints—this habit prevents subtle bugs.
