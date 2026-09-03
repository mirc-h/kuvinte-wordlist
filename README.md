# kuvinte-wordlist

The derived Romanian wordlist used by the **Kuvinte** word game, published here under the
**Mozilla Public License 1.1** — the licence leg its upstream dictionary is offered under.

This repository exists to satisfy that licence: hunspell-ro is distributed under an
MPL-1.1 leg, and a derivative of MPL-licensed source must be made available under the same
terms. `wordlist-published.mpl.txt` is that derivative.

## What is in here

| file | what it is |
|---|---|
| [`wordlist-published.mpl.txt`](wordlist-published.mpl.txt) | 1,361,302 Romanian word forms, one per line, sorted. Carries the filled-in MPL 1.1 Exhibit A notice in its own header. |
| [`LICENSE`](LICENSE) | The full text of the Mozilla Public License 1.1. |

## Provenance

**Original Code:** the Romanian Hunspell dictionary — "hunspell-ro" / "rospell",
`ro_RO.dic` + `ro_RO.aff`, **version 3.3.10** (released 2013-11-12),
<http://rospell.sourceforge.net>.

- Initial Developer: the Rospell Team. Portions created by the Rospell Team are
  Copyright (C) 2005–2013 Rospell Team. All Rights Reserved.
- Contributors, per the pinned distribution's own README: Lucian Constantin, Andrei Cipu,
  Sorin Sbarnea, Alexandru Szasz, Ionut Paduraru, Adrian Stoica, Nicu Buculei, Catalin
  Francu, Ionel Mugurel Ciobica, Mihai Budiu.
- Upstream is **tri-licensed** GPL-2.0 / LGPL-2.1 / MPL-1.1. The **MPL-1.1 leg is the one
  elected here**, and it is the leg this derivative is published under.

The exact upstream bytes this was built from are pinned by sha256, never taken from a mirror:

```
ro_RO.3.3.10.zip  7f128d64ea06c9e6711c30b118c0afeefb014d8f33c92daccdf455aba2d04519
ro_RO.dic         c26a9356f598a0ae89e7be650f6bdd9ba70acce66b41d7ab14c0c68639b6ed33   (181,357 entries)
ro_RO.aff         0c83a02f0ac5202c068e60e1aef5ce99e13d7f6c92ae8e68ca8b9e06829edfd1
```

## How the 181,357 dictionary entries became 1,361,302 forms

1. **Affix expansion.** Every `.dic` stem is expanded against its `.aff` affix flags, so
   inflected forms — Romanian is heavily inflected — are present as playable words rather
   than only their lemmas.
2. **Structural filtering.** Forms that cannot be traced on a letter grid are dropped by a
   set of 7 structural rules. Among them: **any form longer than 16 letters is excluded**,
   since a 4x4 board cannot produce it.
3. **Diacritic folding into classes.** Romanian uses ă â î ș ț, and real-world Romanian text
   is inconsistent about them — in particular ș/ț are correctly **S/T with comma below**
   (U+0219 / U+021B), but a great deal of text on the internet uses the visually similar
   Turkish **cedilla** forms ş/ţ (U+015F / U+0163) instead. Forms are folded into equivalence
   classes so that spelling variation does not change what counts as a word.
4. **Canonical form election.** One display form is elected per fold class. This election is
   **frequency-free**: no corpus data of any kind is consulted.

## Licence cleanliness — why only MPL applies

The game itself also uses a word-frequency list (Hermit Dave's *FrequencyWords*, derived from
OPUS OpenSubtitles2018, CC BY-SA 4.0) to decide which words to *suggest* at the end of a round.
**None of that data is in this file, and none of it influenced this file's derivation** — no
corpus input is consulted anywhere in the pipeline that produces it. `wordlist-published.mpl.txt`
is a pure hunspell-ro derivative, so MPL 1.1 is the only licence that attaches to it.

## Caveats for anyone reusing this

- **It is a game's wordlist, not a reference dictionary.** The >16-letter cut and the
  structural drops exist to serve a 4x4 grid; they are not lexicographic judgments.
- **Membership is deliberately cutoff-free.** Every form that survives the structural filter
  is present. Rarity was not used to prune the list, so it includes plenty of forms a native
  speaker would never use in conversation.
- **Vulgar words are present.** They are real dictionary words and removing them would make
  the game reject valid Romanian. The game handles them at the presentation layer, not by
  deleting them from the list.
- **It is a snapshot.** It tracks hunspell-ro 3.3.10 and changes only when that pin moves.

## Regenerating this file

The derivation is reproducible from the pinned inputs above by the Kuvinte project's own
pipeline. Given the same `ro_RO.dic`/`ro_RO.aff` bytes, it produces this file byte-for-byte.
