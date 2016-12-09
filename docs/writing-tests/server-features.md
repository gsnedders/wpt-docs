---
layout: page
---

## Advanced Testing Features

Certain test scenarios require more than just static HTML
generation. This is supported through the
[wptserve](http://github.com/w3c/wptserve) server. Several scenarios
in particular are common:

### Standalone workers tests

Tests that only require assertions in a dedicated worker scope can use
standalone workers tests. In this case, the test is a JavaScript file
with extension `.worker.js` that imports `testharness.js`. The test can
then use all the usual APIs, and can be run from the path to the
JavaScript file with the `.js` removed.

For example, one could write a test for the `FileReaderSync` API by
creating a `FileAPI/FileReaderSync.worker.js` as follows:

    importScripts("/resources/testharness.js");
    test(function () {
      var blob = new Blob(["Hello"]);
      var fr = new FileReaderSync();
      assert_equals(fr.readAsText(blob), "Hello");
    }, "FileReaderSync#readAsText.");
    done();

This test could then be run from `FileAPI/FileReaderSync.worker`.

### Multi-global tests

Tests for features that exist in multiple global scopes can be written in a way
that they are automatically run in a window scope as well as a dedicated worker
scope.
In this case, the test is a JavaScript file with extension `.any.js`.
The test can then use all the usual APIs, and can be run from the path to the
JavaScript file with the `.js` replaced by `.worker` or `.html`.

For example, one could write a test for the `Blob` constructor by
creating a `FileAPI/Blob-constructor.any.js` as follows:

    test(function () {
      var blob = new Blob();
      assert_equals(blob.size, 0);
      assert_equals(blob.type, "");
      assert_false(blob.isClosed);
    }, "The Blob constructor.");

This test could then be run from `FileAPI/Blob-constructor.any.worker` as well
as `FileAPI/Blob-constructor.any.html`.

### Tests Involving Multiple Origins

In the test environment, five subdomains are available; `www`, `www1`,
`www2`, `天気の良い日` and `élève`. These must be used for
cross-origin tests. In addition two ports are available for http and
one for websockets. Tests must not hardcode the hostname of the server
that they expect to be running on or the port numbers, as these are
not guaranteed by the test environment. Instead tests can get this
information in one of two ways:

* From script, using the `location` API.

* By using a textual substitution feature of the server.

In order for the latter to work, a file must either have a name of the
form `{name}.sub.{ext}` e.g. `example-test.sub.html` or be referenced
through a URL containing `pipe=sub` in the query string
e.g. `example-test.html?pipe=sub`. The substitution syntax uses `{% raw %}{{ }}{% endraw %}`
to delimit items for substitution. For example to substitute in
the host name on which the tests are running, one would write:

```text
{% raw %}{{host}}{% endraw %}
```

As well as the host, one can get full domains, including subdomains
using the `domains` dictionary. For example:

```text
{% raw %}{{domains[www]}}{% endraw %}
```

would be replaced by the fully qualified domain name of the `www`
subdomain. Ports are also available on a per-protocol basis e.g.

```text
{% raw %}{{ports[ws][0]}}{% endraw %}
```

is replaced with the first (and only) websockets port, whilst

```text
{% raw %}{{ports[http][1]}}{% endraw %}
```

is replaced with the second HTTP port.

The request URL itself can be used as part of the substitution using
the `location` dictionary, which has entries matching the
`window.location` API. For example

```text
{% raw %}{{location[host]}}{% endraw %}
```

is replaced by `hostname:port` for the current request.

### Tests Requiring Special Headers

For tests requiring that a certain HTTP header is set to some static
value, a file with the same path as the test file except for an an
additional `.headers` suffix may be created. For example for
`/example/test.html`, the headers file would be
`/example/test.html.headers`. This file consists of lines of the form

    header-name: header-value

For example

    Content-Type: text/html; charset=big5

To apply the same headers to all files in a directory use a
`__dir__.headers` file. This will only apply to the immediate
directory and not subdirectories.

Headers files may be used in combination with substitutions by naming
the file e.g. `test.html.sub.headers`.

### Tests Requiring Full Control Over The HTTP Response

For full control over the request and response the server provides the
ability to write `.asis` files; these are served as literal HTTP
responses. It also provides the ability to write python scripts that
have access to request data and can manipulate the content and timing
of the response. For details see the
[wptserve documentation](http://wptserve.readthedocs.org).
