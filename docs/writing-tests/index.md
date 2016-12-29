---
layout: page
title: Writing Tests
---

If you haven't already, it's strongly recommended to read
the [introduction](../introduction) first, as it introduces the
various test types.

## General Test Requirements

### File Paths and Names

When chosing where in the directory structure to put the tests, try to
follow the structure of existing tests for that specification; if
there are no existing tests, it is generally recommended to create
subdirectories for each section.

Due to path length limitations on Windows, test paths must be less
that 150 characters relative to the test root directory (this gives
vendors just over 100 characters for their own paths when running in
automation).

File names should generally be somewhat descriptive of what is being
tested; very generic names like `001.html` are discouraged. A common
format is `test-topic-001.html`, where `test-topic` is a short
identifier that describes the test. It should avoid conjunctions,
articles, and prepositions as it is a file name, not an English
phrase, hence it should be as concise as possible. The integer that
follows is normally just increased incrementally, and padded to three
digits. (If you'd end up with more than 999 tests, your `test-topic`
is probably too broad!)

csswg-test requires a [naming scheme](css-naming) that inspired this.

#### Support Files

Various support files are available in in the `/common/` and `/media/`
directories (web-platform-tests) and `/support/` (csswg-test). Reusing
existing resources is encouraged where possible, as is adding
generally useful files to these common areas rather than to specific
testsuites.


#### Tools

Sometimes you may want to add a script to the repository that's meant
to be used from the command line, not from a browser (e.g., a script
for generating test files). If you want to ensure (e.g., or security
reasons) that such scripts won't be handled by the HTTP server, but
will instead only be usable from the command line, then place them
in a `tools` subdirectory at the appropriate level.

Any files in those `tools` directories won't be handled by the HTTP
server; instead the server will return a 404 if a user navigates to
the URL for a file within them.

For example, if you wanted to add a script for use with tests in the
`notifications` directory, create the `notifications/tools` subdir
and put your script there.


### File Formats

Tests must be HTML or XML (inc. XHTML and SVG) files. Some
testharness.js tests are auto-generated from JS files. [XXX: LINKME]

*Note: For CSS tests, the test source will be parsed and
re-serialized. This re-serialization will cause minor changes to the
test file, notably: attribute values will always be quoted, whitespace
between attributes will be collapsed to a single space, duplicate
attributes will be removed, optional closing tags will be inserted,
and invalid markup will be normalized.  If these changes should make
the test inoperable, for example if the test is testing markup error
recovery, add the [flag][requirement-flags] `asis` to prevent
re-serialization. This flag will also prevent format conversions so it
may be necessary to provide alternate versions of the test in other
formats (XHTML, HTML, etc.)*

### Character Encoding

Except when specifically testing encoding, files must be encoded in
UTF-8. In file formats where UTF-8 is not the default encoding, they
must contain metadata to mark them as such (e.g., `<meta
charset=utf-8>` in HTML files) or be pure ASCII.

### Be Short

Tests should be as short as possible. For reftests in particular
scrollbars at 800&#xD7;600px window size must be avoided unless scrolling
behaviour is specifically being tested. For all tests extraneous
elements on the page should be avoided so it is clear what is part of
the test (for a typical testharness test, the only content on the page
will be rendered by the harness itself).

### Be Minimal

Tests should generally avoid depending on edge case behaviour of
features that they don't explicitly intend to test. For example,
except where testing parsing, tests should contain no
[parse errors][validator].

This said, tests which intentionally address the interactions between
multiple platform features are not only acceptable but encouraged.

### Be Cross-Platform

Tests should be as cross-platform as reasonably possible, working
across different devices, screen resolutions, paper sizes, etc. The
assumptions that can be relied on are documented [here](assumptions).

Tests that rely on anything else should be manual tests that document
their assumptions.

### Be Self-Contained

Tests must not depend on external network resources, including
w3c-test.org. When these tests are run on CI systems they are
typically configured with access to external resources disabled, so
tests that try to access them will fail. Where tests want to use
multiple hosts this is possible through a known set of subdomains and
[features of wptserve](server-features).

### Be Self-Describing

Tests should make it obvious when they pass and when they fail. It
shouldn't be necessary to consult the specification to figure out
whether a test has passed of failed.

### Style Rules

A number of style rules should be applied to the test file. These are
not uniformly enforced throughout the existing tests, but will be for
new tests. Any of these rules may be broken if the test demands it:

 * No trailing whitespace

 * Use spaces rather than tabs for indentation

 * Use UNIX-style line endings (i.e. no CR characters at EOL).
 
We have a lint tool for catching these and other common mistakes. You
can run it manually by starting the `lint` executable from the root of
your local web-platform-tests working directory like this:

```
./lint
```

The lint tool is also run automatically for every submitted pull request,
and reviewers will not merge branches with tests that have lint errors, so
you must fix any errors the lint tool reports. For details on doing that,
see the [lint-tool documentation][lint-tool].

But in the unusual case of error reports for things essential to a certain
test or that for other exceptional reasons shouldn't prevent a merge of a
test, update and commit the `lint.whitelist` file in the web-platform-tests
root directory to suppress the error reports. For details on doing that,
see the [lint-tool documentation][lint-tool].


## CSS-Specific Requirements

Tests for csswg-test have some additional requirements that have to be
met in order to be included in an official specification testsuite:

* [User style sheets](css-user-styles.html)

* [Metadata](css-metadata.html)


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
