# Cato Sans Linear B

This is a fork of [Noto Sans Linear B](https://github.com/notofonts/linear-b), released by the Google Noto project under SIL OFL 1.1, edited with Fontforge starting from its TTF release. It tries to fix some of the font's inaccuracies. Hopefully all of the fixes will eventually be implemented in the main Noto font, so that people won't have to install this one instead.

## Differences from Noto

### Adjuncts

I encoded the adjuncts using `rlig`, even though I assume some other rule should be used. Temporarily, the joiner character is the the plus sign `+`, following the scheme used at the English Wiktionary, so that it remains visibly contrastive even with other fonts. An upcoming Unicode proposal will hopefully clarify whether the zero-width joiner should be used or new control characters should be created. The adjuncts currentl supported are the following:

* OVIS `U+10025` + *TA* `U+10032`
* SUS `U+10042` + *KA* `U+1000F`
* SUS `U+10042` + *SI* `U+1002F`
* BOS `U+10018` + *SI* `U+1002F`

### Normalise margins of numbers

Numbers being all of the same width makes little sense. It is anacronistic, and makes it appear as if there are spaces where there are none. After selecting all the Aegean numbers, I ran the following script.

```py
font = fontforge.activeFont()
for glyph in font.selection.byGlyphs:
	glyph.left_side_bearing = 100
	glyph.right_side_bearing = 100
```

### Increase space width

In the tablets spacing is very evident, but in the font it is barely visible. I increased the width of the space all the way to 500px.

### Square brackets

I increased the height of the square brackets, so that they are 86px above the signs just like they were already 86px below the sign. I also added glyphs for the double square brackets ⟦ `U+27E6` and ⟧ `U+27E7`, by meshing two single square brackets together. I also set up the same `rlig` table as above so that a *[[ ]]* sequence would turn into *⟦ ⟧*.

### Dot below

Dot below is used to indicate damaged portions of text. I created the combining dot below glyph `U+0323` by reusing the shape of the dot above, setting a new `bottom` mark anchor and placing the base glyph anchor on all characters automatically with the following script:

```py
font = fontforge.activeFont()
for glyph in font.selection.byGlyphs:
    xmin, ymin, xmax, ymax = glyph.boundingBox()
    x = (xmin + xmax) / 2
    y = -100
    glyph.addAnchorPoint("bottom", "base", x, y)
```