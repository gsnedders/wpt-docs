---
layout: page
title: The Ahem Font
---

Todd Fahrner has developed a font called [Ahem][ahem-readme], which
consists of some very well defined glyphs of precise sizes and
shapes. This font is especially useful for testing font and text
properties. Without this font it would be very hard to use the
overlapping technique with text.

The font's em-square is exactly square. Its ascent and descent is
exactly the size of the em square. This means that the font's extent
is exactly the same as its line-height, meaning that it can be
exactly aligned with padding, borders, margins, and so forth.

The font's alphabetic baseline is 0.2em above its bottom, and 0.8em
below its top.

The font has four glyphs:

* X U+0058  A square exactly 1em in height and width.
* p U+0070  A rectangle exactly 0.2em high, 1em wide, and aligned so
that its top is flush with the baseline.
* É U+00C9  A rectangle exactly 0.8em high, 1em wide, and aligned so
that its bottom is flush with the baseline.
* U+0020  A transparent space exactly 1em high and wide.

Most other US-ASCII characters in the font have the same glyph as X.

## Usage
__If the test uses the Ahem font, make sure its computed font-size
is a multiple of 5px__, otherwise baseline alignment may be rendered
inconsistently (due to rounding errors introduced by certain
platforms' font APIs). We suggest to use a minimum computed font-
size of 20px.

E.g. Bad:

``` css
{font: 1in/1em Ahem;}  /* Computed font-size is 96px */
{font: 1in Ahem;}
{font: 1em/1em Ahem} /* with computed 1em font-size being 16px */
{font: 1em Ahem;} /* with computed 1em font-size being 16px */
```

E.g. Good:

``` css
{font: 100px/1 Ahem;}
{font: 1.25em/1 Ahem;} /* with computed 1.25em font-size being 20px
*/
```

__If the test uses the Ahem font, make sure the line-height on block
elements is specified; avoid `line-height: normal`__. Also, for
absolute reliability, the difference between computed line-height
and computed font-size should be divisible by 2.

E.g. Bad:

``` css
{font: 1.25em Ahem;} /* computed line-height value is 'normal' */
{font: 20px Ahem;} /* computed line-height value is 'normal' */
{font-size: 25px; line-height: 50px;} /* the difference between
computed line-height and computed font-size is not divisible by 2. */
```

E.g. Good:

``` css
{font-size: 25px; line-height: 51px;} /* the difference between
computed line-height and computed font-size is divisible by 2. */
```

[Example test using Ahem][ahem-example]

_View the page's source to see how the Ahem font is used._


## Installing Ahem

1. Download the [TrueType version of Ahem][download-ahem].
2. Open the folder where you downloaded the font file.
3. Right-click the downloaded font file and select "Install".

[ahem-example]: http://test.csswg.org/source/css21/positioning/absolute-non-replaced-width-001.xht
[ahem-readme]: http://www.w3.org/Style/CSS/Test/Fonts/Ahem/README
[download-ahem]: http://www.w3.org/Style/CSS/Test/Fonts/Ahem/AHEM____.TTF
