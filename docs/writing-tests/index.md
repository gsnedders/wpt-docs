---
layout: page
title: Writing Tests
---

If you haven't already, it's strongly recommended to read
the [introduction](../introduction) first, as it introduces the
various test types.

There's also a load of [general requirements](general-requirements)
that apply to all tests.

## Choosing the Test Type

Tests are generally written using the mechanism that is most easily
reliably automatable. In general the following order of preference holds:

* [testharness.js](testharness.html) tests should be used for testing
  anything about parsing or scipt APIs.

* [Reftests][reftests] should be used to test rendering.

* WebDriver tests are used exclusively for testing the WebDriver
  protocol itself.

* [Manual tests][manual-tests] are used as a last resort for anything
  that can't be tested using any of the above.

There are various reasons why one might end up writing a manual test:

* In some cases it will not be possible to construct a reference and a
  test that will always render the same, leaving no choice but to
  write a manual test.

* Features that require human interaction for security reasons
  (e.g. to pick a file from the local filesystem) have to be a manual
  test.


[lint-tool]: ./lint-tool.html
[reftests]: ./reftests.html
[manual-tests]: ./manual-test.html
[test-templates]: ./test-templates.html
[requirement-flags]: ./test-templates.html#requirement-flags
[testharness-documentation]: ./testharness-documentation.html
[validator]: http://validator.w3.org
