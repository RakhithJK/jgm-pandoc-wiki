|                            Extension| markdown (Pandoc) | markdown\_phpextra | markdown\_github | markdown\_mmd | markdown\_strict |
|------------------------------------:|:-----------------:|:------------------:|:----------------:|:-------------:|:----------------:|
|                        abbreviations|         no        |         yes        |        no        |       no      |        no        |
|              all\_symbols\_escapable|        yes        |         no         |        no        |      yes      |        no        |
|                   ascii\_identifiers|         no        |         no         |        yes       |       no      |        no        |
|                    auto\_identifiers|        yes        |         no         |        yes       |      yes      |        no        |
|                 autolink\_bare\_uris|         no        |         no         |        yes       |       no      |        no        |
|               backtick\_code\_blocks|        yes        |         no         |        yes       |       no      |        no        |
|            blank\_before\_blockquote|        yes!       |         no         |        no        |       no      |        no        |
|                blank\_before\_header|        yes!       |         no         |        no        |       no      |        no        |
|                            citations|        yes        |         no         |        no        |       no      |        no        |
|           compact\_definition\_lists|         no        |         no         |        no        |       no      |        no        |
|                    definition\_lists|        yes        |         yes        |        no        |      yes      |        no        |
|           epub\_html\_exts (LOOKUP!)|        yes        |         no         |        no        |       no      |        no        |
|                escaped\_line\_breaks|        yes        |         no         |        no        |       no      |        no        |
|                       example\_lists|        yes        |         no         |        no        |       no      |        no        |
|                         fancy\_lists|        yes        |         no         |        no        |       no      |        no        |
|             fenced\_code\_attributes|        yes        |         no         |        no        |       no      |        no        |
|                 fenced\_code\_blocks|        yes        |         yes        |        yes       |       no      |        no        |
|                            footnotes|        yes        |         yes        |        no        |      yes      |        no        |
|                         grid\_tables|        yes        |         no         |        no        |       no      |        no        |
|                   hard\_line\_breaks|         no        |         no         |        yes       |       no      |        no        |
|                   header\_attributes|        yes        |         yes        |        no        |       no      |        no        |
|                 ignore\_line\_breaks|         no        |         no         |        no        |       no      |        no        |
|                    implicit\_figures|        yes        |         no         |        no        |       no      |        no        |
|         implicit\_header\_references|        yes        |         no         |        no        |      yes      |        no        |
|             inline\_code\_attributes|        yes        |         no         |        no        |       no      |        no        |
|                        inline\_notes|        yes        |         no         |        no        |       no      |        no        |
|               intraword\_underscores|        yes        |         yes        |        yes       |      yes      |        no        |
|                        latex\_macros|        yes        |         no         |        no        |       no      |        no        |
|                         line\_blocks|        yes        |         no         |        no        |       no      |        no        |
|                     link\_attributes|         no        |         no         |        no        |      yes      |        no        |
| lists\_without\_preceding\_blankline|         no        |         no         |        yes       |       no      |        no        |
|              literate\_haskell / lhs|        yes        |         no         |        no        |       no      |        no        |
|                  markdown\_attribute|         no        |         yes        |        no        |      yes      |        no        |
|             mmd\_header\_identifiers|         no        |         no         |        no        |      yes      |        no        |
|                    mmd\_title\_block|         no        |         no         |        no        |      yes      |        no        |
|                    multiline\_tables|        yes        |         no         |        no        |       no      |        no        |
|                         native\_divs|        yes        |         no         |        no        |       no      |        no        |
|                        native\_spans|        yes        |         no         |        no        |       no      |        no        |
|                 pandoc\_title\_block|        yes        |         no         |        no        |       no      |        no        |
|                         pipe\_tables|        yes        |         yes        |        yes       |      yes      |        no        |
|                            raw\_html|        yes        |         yes        |        yes       |      yes      |        yes       |
|                             raw\_tex|        yes        |         no         |        no        |      yes      |        no        |
|           shortcut\_reference\_links|        yes        |         yes        |        yes       |       no      |        yes       |
|                       simple\_tables|        yes        |         no         |        no        |       no      |        no        |
|                             startnum|        yes        |         no         |        no        |       no      |        no        |
|                            strikeout|        yes        |         no         |        yes       |       no      |        no        |
|                       table\_caption|        yes        |         no         |        no        |       no      |        no        |
|                   tex\_math\_dollars|        yes        |         no         |        no        |       no      |        no        |
|         tex\_math\_double\_backslash|         no        |         no         |        no        |      yes      |        no        |
|         tex\_math\_single\_backslash|         no        |         no         |        yes       |       no      |        no        |
|                yaml\_metadata\_block|        yes        |         no         |        no        |       no      |        no        |


**Notes:**

1. Each extension which is ***en***abled by default can be disabled by attaching the `-EXTENSIONNAME` to the markdown format specifier.
1. Each extension which is ***dis***abled by default can be enabled by attaching the `+EXTENSIONNAME` to the markdown format specifier.

**Examples:**

1. Use `-t markdown_mmd-pipe_tables+grid_tables` if you want to generate Markdown with `grid_tables`.
1. Another way for same purpose: Use `-t markdown-multiline_tables-simple_tables-pipe_tables` if you want to generate Markdown with `grid_tables`.
1. There are more ways…

----

<sub>*(Table compiled by evaluating the most recent Pandoc man page for v1.15.0.6)*</sub>