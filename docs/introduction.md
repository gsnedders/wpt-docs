---
layout: page
title: Introduction
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

The testsuite has a few types of tests, outlined below:

* [testharness.js](testharness.html) tests, which are run through a JS
  harness and report their result back with JS.

* [Reftests][reftests], which render two (or more) web pages and
  combine them with equality assertions about their rendering (e.g.,
  A.html and B.html must render identically).

* [Manual tests][manual-tests], which rely on a human to run them and
  determine their result.

* WebDriver tests, which are used for testing the WebDriver protocol
  itself.


## GitHub

GitHub is used both for issue tracking and test submissions; we
provide a limited introduction to both git and GitHub in our appendix
[XXX: add link].

Pull Requests are automatically labelled based on the directory the
files they change are in; there are also comments added automatically
to notify a number of people similarly based on the directories: this
list of people comes from the OWNERS files (note that these work
recursively: `a/OWNERS` will get notified for `a/foo.html` and
`a/b/bar.html`).

If you want to be notified about changes to tests in a directory, feel
free to add yourself to the OWNERS file: there's no requirement to own
anything as a result!


## Local Setup

[XXX: include the "Running the Tests" section from README.]


[reftests]: ./reftests.html
[manual-tests]: ./manual-test.html
[testharness-documentation]: ./testharness-documentation.html
[web-platform]: https://platform.html5.org
[test262]: https://github.com/tc39/test262
[csswg-test]: https://github.com/w3c/csswg-test
[webgl]: https://github.com/KhronosGroup/WebGL
