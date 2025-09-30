# Budge.txt

**Budge** is a plain text file format for bullet journaling. It translates the core concepts of rapid logging to the digital realm in a way that is designed to:

- Minimize typing, especially on a smartphone keyboard;
- Have an unambiguous specification, allowing for syntax highlighting;
- Feel more or less 'familiar' to bullet journalers and markdown writers.

Budge is best used together with a text editor that has builtin fuzzy search and Tree-sitter grammar support. My favorites: Helix (TUI) and Zed (GUI).

## Examples

### OG Rapid Logging

> (`# <-` is a comment prefix.)

```
== 2025-09-28/Sun        # <- Section/collection headings start with `==`

. Send email to confirm  # <- Open task (todo)
x Test in production     # <- Completed task (done)
/ Document changes       # <- Started task (doing)
> Write documentation    # <- 'Migrated' (i.e. moved to (newer) section)
< Bring cat to vet       # <- Scheduled (in external calendar)
o 17:30 pink sunset      # <- Event (something that has happened or will happen)
- Just a note
  - And a nested note    # <- All bullet types can be nested and mixed.
~ A canceled item        # <- In an analog bujo this is scribbled over.
. ! Very urgent task     # <- `!` is a signifier, augmenting the meaning of the bullet.
```

### Budge.txt innovations

```
== Dates and Times

. -> tmw,14:00 Submit report  # <- `-> tmw,14:00` is a schedule tag
. -> [sep .. dec] Start: September; complete: December # <- Schedule date ranges
. -! 2025-10-04 Release changes  # <- Deadline shorthand for `-> ! ...`
. -@ 2days Rewrite chapter  # <- Use `-@` for time estimates.
. 15:00 Take train  # <- 24h times don't need to be prefixed by `->`

== Fishy Links and Footnotes

- There are several kinds of links in Budge.
- You can use familiar wiki-style links to reference other files, e.g. [[specification.md]].
- An item can contain a markdown-style footnote [^1].
- And then there are what I call 'fish links': `>()` and `>>()`
  - One `>` (greater-than sign) means 'search in the current file'.
  - Two `>>` means 'search in any other file'.
  - The link body is usually a (fuzzy) substring of another item.
    - For example, >(take train) refers to the last item of the previous 'Scheduling Items' section.
    - Usually, the first match wins, be it located above or below.
  - Fish links are highly contextual. However, explicit link anchors can also be defined.
    - For example, =(sdsy.1) defines a link anchor ID for the current line.
      - To define a probably unique anchor ID, I:
        - 1. Read the words of the section heading in reverse
        - 2. Take the last letter of each word
        - 3. Add a <dot> + <number> in case of conflicts.
  - Finally, `<()` is also a valid fish link. The flipped inequality sign now means 'originating from'.
    - I use this with migrated items to indicate where it originally came from.

[^1]: e.g. for long links you don't want cluttering the text.


== Other

- There are #tags
- But also hierarchical tags, e.g. #project/budgetxt
- And #key=value or #key="value with spaces" tags!
- You can also mention @people. These an also be hierarchical, e.g. @client/person.
  - Really handy for building a knowledge base of facts you can search with grep!
x ? What if I want to update an item later on? // Use `//` as an 'edit' marker.
--     # <- separator
- The (optional) separator `--` above is an easy way to partition the day, e.g. morning (personal), day (work), evening (personal)
- Content can be ~~struck through~~  with `~~`.
- Markdown formatting like *italics* and **bold** and `code` is also valid.


== To-do

. Fenced code blocks for longer snippets (for this I just use regular .md for now).
```


Sources of inspiration:

- todo.txt
- Xit
- Markdown
- Obsidian

## Tips

- In your editor or in a separate app, create a text expansion macro for the current date.

## How I Designed Budge.txt

### Links within and without

### Differences with analog rapid logging

In bujo, signifiers appear to the left of the bullet. For example, `* .` could
mean 'important (`*`) open task (`.`)'. On paper this works well. All bullets
leave a bit of margin to left for possible signifier symbols. When translated to
plain text, however, the result is less practical. Lines that start with
signifiers have their bullets shifted to the right, whereas my personal
preference is for all bullets to appear in the same column, so that I can Tip:
quickly scan them. For the shortest of instants I contemplated left-padding all
entries, so that signifiers wouldn't have this off-setting effect. In the end,
though, just swapping the order of bullet and signifier around made more sense.
In Budge.txt, `* .` is written as `. *`.

## How I use Budge.txt

I mostly use a single file, `daily.buj.txt`.

Each new day gets a new section.

> [!TIP]
> Add new journal sections to the _top_ of the file rather than at the bottom. This will save you a lot of scrolling down on mobile.

## FAQs

> I want something nicer.

Check out Noteplan on macOS/iOS!

## Resources

[^1]: [Official _Get Started_ from bulletjournal.com (archived link, 2017)](https://web.archive.org/web/20171113192309/http://bulletjournal.com/get-started/#topics)
