---
layout: page
title: Visual Tests
---

Visual tests are typically used when testing rendering of things that
cannot be tested with [reftests](reftests).

## Writing a Visual Test

Visuals tests are test files which have `-visual` at the end of their
filename, before the extension. There is nothing needed in them to
make them work. [XXX: do we want to add reftest-wait support?]

They should follow the [general test guidelines](general-guidelines),
especially noting the requirement to be self-describing (i.e., they
must give a clear pass condition in their rendering).

In addition to this, there's a number of techniques that are useful to
follow to avoid those running the tests incorrectly determining their
result:

### Indicating success

Success is largely indicated by the colour green; typically in one of
two ways:

 * **The green paragraph**: arguably the simplest form of test, this
   typically consists of single line of text with a pass condition of,
   "This text should be green". A variant of this is using the
   background instead, with a pass condition of, "This should have a
   green background".
   
 * **The green square**: applicable to many block layout tests, the
   test renders a green square when it passes. This green square is
   often rendered over a red square, such that when the test fails
   there is red visible on the page.

More occasionally, the entire canvas is rendered green, typically when
testing parts of CSS that affect the entire page. Care has to be taken
when writing tests like this that the test will not result in a single
green paragraph if it fails. This is usually done by forcing the short
descriptive paragraph to have a neutral color (e.g., white).

Sometimes instead of a green square, a white square is used to ensure
any red is obvious. To ensure the stylesheet has loaded, it is
recommended to make the pass condition paragraph green and require
that in addition to there being no red on the page.

### Indicating failure

In addition to having clearly defined characteristics when
they pass, well designed tests should have some clear signs when
they fail. It can sometimes be hard to make a test do something only
when the test fails, because it is very hard to predict how user
agents will fail! Furthermore, in a rather ironic twist, the best
tests are those that catch the most unpredictable failures!

Having said that, here are the best ways to indicate failures:

 * Using the color red is probably the best way of highlighting
   failures. Tests should be designed so that if the rendering is a
   few pixels off some red is uncovered or otherwise rendered on the
   page.

 * Tests of the `line-height`, `font-size` and similar properties can
   sometimes be devised in such a way that a failure will result in
   the text overlapping.

 * Some properties lend themselves well to making "FAIL" render in the
   case of something going wrong, for example `quotes` and
   `content`.

### Other Colors

Aside from green and red, other colors are generally used in specific
ways:

 * Black is typically used for descriptive text,
 
 * Blue is frequently used as an obvious color for tests with complex
   pass conditions,
 
 * Fuchsia, yellow, teal, and orange are typically used when multiple
   colors are needed,

 * Dark gray is often used for descriptive lines, and

 * Silver or light gray is often used for irrelevant content, such as
   filler text.

Obviously, none of this can be taken as absolute rules, as testing of
color-related functionality will necessitate using some of these
colors!
