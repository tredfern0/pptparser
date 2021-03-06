# pptparser
Parse ProPokerTools style Pot-Limit Omaha hand range syntax

### Installation

Numpy must be installed.  The program has only been tested in Python 3.5 and 3.6.

### Use

Call the 'evaluate' function in the RangeParserMasks script with a range of hands  in the style of the syntax used by the <a href="http://www.propokertools.com/">ProPokerTools software</a>, and optionally a board (Example: AdKhQc), or a numpy mask of impossible hands from a previously generated hand range.

The function will return an array of shape (n,4), with each row corresponding to a unique hand in this range.

The cards are represented with integers 1-52, corresponding to their index+1 in the following array

```
cardIndexes = ['2H','3H','4H','5H','6H','7H','8H','9H','TH','JH','QH','KH','AH',
               '2D','3D','4D','5D','6D','7D','8D','9D','TD','JD','QD','KD','AD',
               '2C','3C','4C','5C','6C','7C','8C','9C','TC','JC','QC','KC','AC',
               '2S','3S','4S','5S','6S','7S','8S','9S','TS','JS','QS','KS','AS']
```

The function will also return a mask of the hands that were found, hands are matched against the pptRankedHUnums.npy file.  This mask enables quicker calculations of subranges, for example instead of typing AA:xxyz, if you already had the mask for xxyz saved in the variable 'myMask', you could call `evaluate('AA',None,myMask)`, and this use the known information about the 'xxyz' range and calculate 'AA:xxyz'

### Inconsistencies with ProPokerTools ranges

In rare cases where I believe ProPokerTools is inconsistent with their syntax, different ranges will be returned by this program.

Two examples:
`[2c,3c,4c,5c,6c]xxx` will return AcKcQc2c, while `[2c-6c]xxx` will not include that hand.  Given that the general case is for suit variables to match with bracketed ranges, in both situations AcKcQc2c will not be included here.

`[33-]RR` will include 2222, while `[22-]RR` will not include that hand.  The general behavior is for rank variables to not match with bracketed ranges, so in both cases 2222 would be returned here.

### License

Released under the MIT license.
