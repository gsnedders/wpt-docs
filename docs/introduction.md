---
layout: page
---

web-platform-tests is a W3C-coordinated effort to build a
cross-browser testsuite for the majority of
the [web platform][web-platform]; it excludes only CSS (whose
testsuite lives in [csswg-test][csswg-test]), ECMAScript (whose
testsuite lives in [test262][test262]), and WebGL (whose testsuite
lives in [WebGL][WebGL]).

That said, csswg-test follows a superset of the policies of
web-platform-tests, so the documentation provided here should
similarly apply to it. Where extra policies apply, they are provided
in separate documents and linked from the appropiate section.


## Testsuite Design

The vast majority of the testsuite is formed of HTML pages, which can
be loaded in a browser and either programmatically provide a result or
provide a set of steps to run the test and obtain the result.

The tests are, in general, short, cross-platform, and self-contained,
and should be easy to run in any browser.


## Test Layout

Each top level directory in the repository corresponds to tests for a
single specification. For W3C specs, these directories are typically
named after the shortname of the spec (i.e. the name used for snapshot
publications under `/TR/`); for WHATWG specs, they are typically named
after the subdomain of the spec (i.e. trimming `.spec.whatwg.org` from
the URL); for other specs, something deemed sensible is used. In all
cases, there are occasional exceptions for historic reasons.

Within the specification-specific directory there are two common ways
of laying out tests. The first is a flat structure which is sometimes
adopted for very short specifications. The alternative is a nested
structure with each subdirectory corresponding to the id of a heading
in the specification. This layout provides some implicit metadata
about the part of a specification being tested according to its
location in the filesystem, and is preferred for larger
specifications.


## Test Types

Tests are generally written using the mechanism that is most easily
reliably automatable. In general the following order of preference holds:

* [testharness.js](testharness.html) tests — for any test that can be
  written using script alone.

* [Reftests][reftests] — for most tests of rendering.

* WebDriver tests — for testing the WebDriver protocol itself.

* [Manual tests][manual-tests] — as a last resort for anything that can't be tested
  using one of the above techniques.

Some scenarios demand certain test types. For example:

* Tests for layout will generally be reftests. In some cases it will
  not be possible to construct a reference and a test that will always
  render the same, in which case a manual test, accompanied by
  testharness tests that inspect the layout via the DOM must be
  written.

* Features that require human interaction for security reasons
  (e.g. to pick a file from the local filesystem) typically have to be
  manual tests.


[reftests]: ./reftests.html
[manual-tests]: ./manual-test.html
[testharness-documentation]: ./testharness-documentation.html
[web-platform]: https://platform.html5.org
[test262]: https://github.com/tc39/test262
[csswg-test]: https://github.com/w3c/csswg-test
[webgl]: https://github.com/KhronosGroup/WebGL
