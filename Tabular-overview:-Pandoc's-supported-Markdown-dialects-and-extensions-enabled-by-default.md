|                            Extension| markdown (Pandoc) | markdown\_phpextra | markdown\_github | markdown\_mmd |
|------------------------------------:|:-----------------:|:------------------:|:----------------:|:-------------:|
|                        abbreviations|         no        |         yes        |        no        |       no      |
|              all\_symbols\_escapable|        yes        |         no         |        no        |      yes      |
|                   ascii\_identifiers|         no        |         no         |        yes       |       no      |
|                    auto\_identifiers|        yes        |         no         |        yes       |      yes      |
|                 autolink\_bare\_uris|         no        |         no         |        yes       |       no      |
|               backtick\_code\_blocks|        yes        |         yes        |        no        |       no      |
|           compact\_definition\_lists|         no        |         no         |        no        |       no      |
|                    definition\_lists|        yes        |         yes        |        no        |      yes      |
|                 fenced\_code\_blocks|        yes        |         yes        |        yes       |       no      |
|                            footnotes|        yes        |         yes        |        no        |      yes      |
|                         grid\_tables|        yes        |         no         |        no        |       no      |
|                   hard\_line\_breaks|         no        |         no         |        yes       |       no      |
|                   header\_attributes|        yes        |         yes        |        no        |       no      |
|                 ignore\_line\_breaks|         no        |         no         |        no        |       no      |
|         implicit\_header\_references|        yes        |         no         |        no        |      yes      |
|               intraword\_underscores|        yes        |         yes        |        yes       |      yes      |
|                     link\_attributes|         no        |         no         |        no        |      yes      |
| lists\_without\_preceding\_blankline|         no        |         no         |        no        |       no      |
|                  markdown\_attribute|         no        |         yes        |        no        |      yes      |
|             mmd\_header\_identifiers|         no        |         no         |        no        |      yes      |
|                    mmd\_title\_block|         no        |         no         |        no        |      yes      |
|                    multiline\_tables|        yes        |         no         |        no        |       no      |
|                         pipe\_tables|        yes        |         yes        |        yes       |      yes      |
|                            raw\_html|        yes        |         yes        |        yes       |      yes      |
|                             raw\_tex|        yes        |         no         |        no        |      yes      |
|           shortcut\_reference\_links|        yes        |         yes        |        yes       |      yes      |
|                       simple\_tables|        yes        |         no         |        no        |       no      |
|                            strikeout|        yes        |         no         |        yes       |       no      |
|                       table\_caption|        yes        |         no         |        no        |       no      |
|         tex\_math\_double\_backslash|         no        |         no         |        no        |      yes      |
|         tex\_math\_single\_backslash|         no        |         no         |        yes       |       no      |


**Notes:**

1. Each extension which is ***en***abled by default can be disabled by it attaching the `-EXTENSIONNAME` to the format.
1. Each extension which is ***dis***abled by default can be enabled by it attaching the `+EXTENSIONNAME` to the format.
1. Pandoc also supports `markdown_strict`. Here the only extension enabled by default is `raw_html`.

----

<sub>*(Table compiled by evaluating the most recent Pandoc man page for v1.15.0.6)*</sub>