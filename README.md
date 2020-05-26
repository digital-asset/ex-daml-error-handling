# Sample code

**This repo contains sample code to help you get started with DAML. Please bear in mind that it is provided for illustrative purposes only, and as such may not be production quality and/or may not fit your use-cases. You may use the contents of this repo in parts or in whole according to the BSD0 license:**

> Copyright © 2020 Digital Asset (Switzerland) GmbH and/or its affiliates
>
> Permission to use, copy, modify, and/or distribute this software for any purpose with or without fee is hereby granted.
>
> THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

# Error Handling in DAML

This repository walks you through how you can handle
errors in batch transactions such that the transaction
will run through partially and not fail completely if
an error arises in some part.

This is best read in the following order:
1. [Coin](src/Coin.daml) contains the `Coin` contract
   used throughout all examples.
2. [AllOrNothing](src/M0_AllOrNothing.daml) demonstrates
   the issue with aborting the whole batch transaction.
3. [PartialManual](src/M1_PartialManual.daml) fixes the
   previous example in a rather manual and tedius way.
4. [UseAssert](src/M2_UseAssert.daml) modifies the
   example from step 3 such that we can simply use `assertMsg` again.
5. [OrderSensitivity](src/M3_OrderSensitivity.daml)
   demonstrates an important difference between
   aborting a transaction in `EitherT` and doing so directly
   in `Update`. In `EitherT`, effects that happened before, e.g., in this example creation of a contract, will persist. A common pattern to avoid this issue is to structure the transaction into 3 parts:
   1. Read all your data, e.g., perform `fetch`es, `lookupByKey`, ….
   2. Validate the data using `assert`, `abort`, ….
      There is nothing to roll back at this point so
      `EitherT` shortcircuiting is sufficient.
   3. Write data, e.g., create contracts,
      exercise consuming choices, ….
