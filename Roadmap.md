Pandoc doesn't have a strict roadmap but this page should serve as an overview about major changes to pandoc that are under consideration.

## AST changes

- [Attributes for all elements](https://github.com/jgm/pandoc/issues/684)
- [a Figure block element](https://github.com/jgm/pandoc/issues/3177), capable of containing multiple images 
- [short and long captions for tables](https://github.com/jgm/pandoc/issues/2978), figures, [headers](https://github.com/jgm/pandoc/issues/4409)
- [internal links/references to tables and figures](https://github.com/jgm/pandoc/issues/813)
- [PageBreak element](https://github.com/jgm/pandoc/issues/1934)
- [Handle insertions and deletions (`<ins>` and `<del>`) consistently across readers/writers](https://github.com/jgm/pandoc/issues/1560)
- [change Format from String to a sum type](https://github.com/jgm/pandoc/issues/547)
- [Endnotes](https://github.com/jgm/pandoc/pull/4042) and [reusing the same Note](https://github.com/jgm/pandoc/issues/1603)
- [Comment AST element](https://github.com/jgm/pandoc/issues/1926)

## Filters/JSON

- changing JSON serialization format to be more human-readable, less Haskell-ADT-oriented, [see this comment](https://github.com/jgm/pandoc/issues/3211#issuecomment-258783108)

## YAML/command-line metadata handling

- [Specify command-line options using YAML metadata](https://github.com/jgm/pandoc/issues/4627)
- [Command-line options --css and --include-in-header override corresponding metadata fields instead of accumulating](https://github.com/jgm/pandoc/issues/3139)
- [Change merge-behaviour of two metadata blocks](https://github.com/jgm/pandoc/issues/4057)

## Pandoc's Markdown Transition to CommonMark

John has indicated that the default input reader for pandoc will eventually transition from `markdown` to `commonmark_x`. One important step along that path will be implementing a sufficient set of markdown extensions in CommonMark. The table below lists the extensions that are available to the `markdown` reader (plus a couple new extensions exclusive to CommonMark) and reports on their status in the `commonmark` reader. Extensions have one of a few possible statuses:

- **Available:** implemented as an extension to CommonMark
- **Core CommonMark:** implemented in the core spec, cannot be disabled
- **Incompatible with CommonMark:** violates the spec, will never be implemented
- **Planned:** likely to be implemented as extensions at some future date
- **Unlikely to be implemented:** the feature will not be supported under `commonmark`, for a variety of reasons
- **Not Currently Implemented:** may or may not be supported under `commonmark`—to be determined
- **Abandoned in favor of…/subsumed by…:** Extension behavior implemented in a different extension or in core syntax

| Pandoc's Markdown Extensions      | Status in commonmark                          |
|-----------------------------------|-----------------------------------------------|
| abbreviations                     | not currently implemented                     |
| all_symbols_escapable             | core commonmark                               |
| angle_brackets_escapable          | core commonmark                               |
| ascii_identifiers                 | available                                     |
| attributes                        | available                                     |
| auto_identifiers                  | abandoned in favor of gfm_auto_identifiers    |
| autolink_bare_uris                | available                                     |
| backtick_code_blocks              | core commonmark                               |
| blank_before_blockquote           | incompatible with commonmark                  |
| blank_before_header               | incompatible with commonmark                  |
| bracketed_spans                   | available                                     |
| citations                         | planned                                       |
| compact_definition_lists          | unlikely to be implemented                    |
| definition_lists                  | available                                     |
| east_asian_line_breaks            | available                                     |
| emoji                             | available                                     |
| escaped_line_breaks               | core commonmark                               |
| example_lists                     | planned                                       |
| fancy_lists                       | available                                     |
| fenced_code_attributes            | subsumed by attributes extension              |
| fenced_code_blocks                | core commonmark                               |
| fenced_divs                       | available                                     |
| footnotes                         | available                                     |
| four_space_rule                   | incompatible with commonmark                  |
| gfm_auto_identifiers              | available                                     |
| grid_tables                       | planned                                       |
| gutenberg                         | not currently implemented                     |
| hard_line_breaks                  | available                                     |
| header_attributes                 | subsumed by attributes extension              |
| ignore_line_breaks                | not currently implemented                     |
| implicit_figures                  | available                                     |
| implicit_header_references        | available                                     |
| inline_code_attributes            | subsumed by attributes extension              |
| inline_notes                      | planned                                       |
| intraword_underscores             | core commonmark                               |
| latex_macros                      | not currently implemented                     |
| line_blocks                       | planned                                       |
| link_attributes                   | subsumed by attributes extension              |
| lists_without_preceding_blankline | core commonmark                               |
| literate_haskell                  | not currently implemented                     |
| markdown_attribute                | unlikely to be implemented                    |
| markdown_in_html_blocks           | abandoned in favor of core commonmark syntax  |
| mmd_header_identifiers            | not currently implemented                     |
| mmd_link_attributes               | not currently implemented                     |
| mmd_title_block                   | not currently implemented                     |
| multiline_tables                  | planned                                       |
| native_divs                       | unlikely to be implemented                    |
| native_spans                      | unlikely to be implemented                    |
| old_dashes                        | unlikely to be implemented                    |
| pandoc_title_block                | not currently implemented                     |
| pipe_tables                       | available                                     |
| raw_attribute                     | available                                     |
| raw_html                          | available                                     |
| raw_tex                           | incompatible with commonmark                  |
| rebase_relative_paths             | available                                     |
| short_subsuperscript              | Unlikely to be implemented                    |
| shortcut_reference_links          | core commonmark                               |
| simple_tables                     | planned                                       |
| smart                             | available                                     |
| sourcepos                         | available                                     |
| space_in_atx_header               | core commonmark                               |
| spaced_reference_links            | incompatible with commonmark                  |
| startnum                          | core commonmark                               |
| strikeout                         | available                                     |
| subscript                         | available                                     |
| superscript                       | available                                     |
| table_captions                    | planned                                       |
| task_lists                        | available                                     |
| tex_math_dollars                  | available                                     |
| tex_math_double_backslash         | not currently implemented                     |
| tex_math_single_backslash         | incompatible with commonmark                  |
| yaml_metadata_block               | available                                     |

A brief description of John's reasoning for the extensions, see [this thread](https://groups.google.com/g/pandoc-discuss/c/cUEH93noFHE/m/BfmeyDiLAwAJ) in Pandoc-discuss. 

## Other

- [Custom styles in ODT, ICML (and maybe LaTeX and RST) writers](https://github.com/jgm/pandoc/issues/2106) (maybe also [in Docbook](https://github.com/jgm/pandoc/issues/3657))
- [Image alignment in more readers and writers](https://github.com/jgm/pandoc/issues/4542)
- [Include files](https://github.com/jgm/pandoc/issues/553)