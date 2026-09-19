# Cato Sans Linear B

This is a fork of [Noto Sans Linear B](https://github.com/notofonts/linear-b), released by the Google Noto project under SIL OFL 1.1, edited with Fontforge starting from its TTF release. It tries to fix some of the font's inaccuracies. Hopefully all of the fixes will eventually be implemented in the main Noto font, so that people won't have to install this one instead.

## Differences from Noto

### Composed characters

I encoded the composed characters using `ccmp`. Temporarily, the joiner character is the the plus sign `+`, following the scheme used at the English Wiktionary, so that it remains visibly contrastive even with other fonts. An upcoming Unicode proposal will hopefully clarify whether the zero-width joiner should be used or new control characters should be created. The characters currently supported are the following:

* OVIS (\*106) `U+10025` + *TA* (\*59) `U+10032`
* SUS (\*108) `U+10042` + *SI* (\*41) `U+1002F`
* SUS (\*108) `U+10042` + *KA* (\*77) `U+1000F`
* BOS (\*109) `U+10018` + *SI* (\*41) `U+1002F`
* GRA (\*120) `U+1008E` + *PE* (\*72) `U+1001F`
* OLIV (\*122) `U+10090` + *A* (\*8) `U+10000`
* OLIV (\*122) `U+10090` + *TI* (\*37) `U+10034`
* AROM (\*123) `U+10091` + *KO* (\*70) `U+10012`
* CYP (\*125) `U+10092` + *PA* (\*3) `U+1001E`
* CYP (\*125) `U+10092` + *QA* (\*16) `U+10023`
* CYP (\*125) `U+10092` + *O* (\*61) `U+10003`
* CYP (\*125) `U+10092` + *KU* (\*81) `U+10013`
* OLE (\*130) `U+10095` + *PA* (\*3) `U+1001E`
* OLE (\*130) `U+10095` + *A* (\*8) `U+10000`
* OLE (\*130) `U+10095` + *WE* (\*75) `U+10038`
* \*155<sup>VAS</sup> `U+100DE` + *DI* (\*7) `U+10007`
* \*155<sup>VAS</sup> `U+100DE` + *NI* (\*30) `U+1001B`
* TELA (\*159) `U+100A7` + *PA* (\*3) `U+1001E`
* TELA (\*159) `U+100A7` + *TE* (\*4) `U+10033`
* TELA (\*159) `U+100A7` + *PO* (\*11) `U+10021`
* TELA (\*159) `U+100A7` + *ZO* (\*20) `U+1003F`
* TELA (\*159) `U+100A7` + *PO* (\*50) `U+10022`
* TELA (\*159) `U+100A7` + *KU* (\*81) `U+10013`
* TUN (\*162) `U+100AA` + *RI* (\*53) `U+1002A`
* TUN (\*162) `U+100AA` + *KI* (\*66) `U+10011`
* TUN (\*162) `U+100AA` + *QE* (\*78) `U+10024`
* \*166 `U+100AE` + *WE* (\*75) `U+10038`
* \*168 `U+100B0` + *SE* (\*9) `U+1002E`
* \*202<sup>VAS</sup> `U+100E1` + *DI* (\*7) `U+10007`
* \*209<sup>VAS</sup> `U+100E8` + *A* (\*8) + `U+10000`
* \*210<sup>VAS</sup> `U+100E9` + *KA* (\*77) `U+1000F`
* \*211<sup>VAS</sup> `U+100EA` + *PO* (\*11) `U+10021`
* \*212<sup>VAS</sup> `U+100EB` + *U* (\*10) `U+10004`
* \*214<sup>VAS</sup> `U+100ED` + *DI* (\*7) `U+10007`
* ROTA (\*243) `U+100CF` + *TE* (\*4) `U+10033`

### Clarification about \*211<sup>VAS</sup>

The ideogram \*211<sup>VAS</sup> is only ever attested in two Knossos tablets in the compound \*211<sup>VAS</sup> + *PO*. As such, the codepoint for the ideogram, `U+100EA`, was represented in the Unicode charts ([see PDF](https://www.unicode.org/charts/PDF/U10000.pdf)), and later in the fonts Aegean and Noto Sans Linear B, already with a precomposed *PO*. However I believe that the composed sign should be encoded as any other, i.e. as \*211<sup>VAS</sup> `U+100EA` + *PO* `U+10021`, so I reused the glyph for the compound term and stripped the syllable away from the bare ideogram codepoint.

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

I increased the height of the square brackets, so that they are 86px above the signs just like they were already 86px below the sign. I also added glyphs for the double square brackets ⟦ `U+27E6` and ⟧ `U+27E7`, by joining two single square brackets together.

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
