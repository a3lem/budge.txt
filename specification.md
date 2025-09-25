# Budge Specification

**Version 0.1**

Budge is a plain-text file format for digital bullet journaling.

The name is supposed to be pronounced like the English word "exit".

## Preface

The keywords "MUST", "MUST NOT", "SHOULD", "SHOULD NOT", and "MAY"
in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

## File Format

The file extension SHOULD be `.buj`.
The file encoding MUST be UTF-8.

## Document Structure

A Budge document consists of zero or more sections. Each section MUST have a heading or a body or both.

### Sections

A section is the primary organizational unit in a Budge document. Sections can contain:
- An optional section heading
- A section body containing items, item separators, and footnotes

> #### Example
>
> ```
> == 2024-12-15/Sun
>
> . Write documentation
> x Review pull request
> ```

### Section Headings

Section headings are prefixed with `==` followed by one or more spaces and the heading content.

> #### Example
>
> ```
> == Project Planning
> == 2024-12-15/Sun
> == Q4 Goals
> ```

## Items

Items are the core content unit in Budge. Each item MUST begin with a bullet character, followed by a space, followed by content.

### Bullet Types

Budge supports seven bullet types, each representing a different item state or type:

| Bullet | Type | Description |
|--------|------|-------------|
| `.` | Todo | A task that needs to be done |
| `x` | Done | A completed task |
| `/` | Started | A task in progress |
| `>` | Migrated | A task moved to another time/location |
| `~` | Canceled | A task that won't be done |
| `-` | Note | Information or observation |
| `o` | Event | Something that happened |

> #### Example
>
> ```
> . Write specification document
> x Complete code review
> / Working on bug fix
> > Move meeting to next week
> ~ Cancel outdated task
> - Remember to update dependencies
> o Team standup at 10:00
> ```

### Hierarchical Structure (Sub-items)

Items can have sub-items through indentation. Sub-items MUST be indented with spaces relative to their parent item. Each level of indentation represents a deeper level in the hierarchy.

Indentation rules:
- Spaces count as 1 unit of indentation
- Tabs count as 8 units (aligned to 8-space boundaries)
- Sub-items maintain their own bullet types independent of their parent

> #### Example
>
> ```
> . Main project task
>   . Research phase
>     - Look into existing solutions
>     - Review documentation
>   . Implementation phase
>     x Set up development environment
>     / Write core functionality
>   . Testing phase
> ```

## Signifiers

Signifiers are single-character symbols that appear before item content to indicate priority, status, or importance. Multiple signifiers of the same type can be repeated for emphasis.

| Signifier | Meaning | Repeatable | Description |
|-----------|---------|------------|-------------|
| `!` | Importance | Yes | Priority or importance level |
| `?` | Curiosity | Yes | Questions or items needing investigation |
| `@` | Active Focus | Yes | Items requiring immediate attention |
| `~` | Uncertainty | Yes | Items with uncertain status |
| `*` | Pinned | Yes | Items to keep visible/important |

Signifiers MUST appear after the bullet and space, but before any other content.

> #### Example
>
> ```
> . ! Important deadline tomorrow
> . !! Very urgent task
> . ? Research this topic
> . @ Focus on this today
> . @@ High priority focus item
> . ~ Maybe implement this feature
> . * Keep this in mind
> ```

## Item Separators

The sequence `--` on its own line creates a visual separator between groups of items within a section.

> #### Example
>
> ```
> . Morning tasks
> x Check emails
> --
> . Afternoon tasks
> o 14:00 Team meeting
> ```

## Temporal System

Budge provides comprehensive support for dates, times, and scheduling.

### Time Expressions

Time can be expressed in 24-hour format (`HH:MM`) at the beginning of item content.

> #### Example
>
> ```
> o 09:30 Morning standup
> . 14:00 Review documents
> x 16:45 Submitted report
> ```

### Schedule Tags

Schedule tags use `->` followed by a date/time expression to indicate when something is scheduled.

Supported formats:
- Relative date words: `tomorrow`, `tmw`, `yesterday`, `yst`, `week`, `month`, `today`, etc.
- Weekday names: `monday`, `mon`, `tuesday`, `tue`, etc.
- Month names with optional day: `january`, `jan`, `feb5`, `oct12`, etc.
- ISO dates: `2024-12-15`, `2024/12/15`
- Times: `14:30`

> #### Example
>
> ```
> . -> tomorrow Prepare presentation
> . -> fri Submit weekly report
> . -> 2024-12-20 Project deadline
> . -> week Plan next sprint
> . -> jan15 Annual review
> ```

### Due Tags (Deadlines)

Due tags use `-!` to indicate a hard deadline, followed by a date/time expression.

> #### Example
>
> ```
> . -! fri Submit proposal
> . -! 2024-12-31 Year-end report
> . -! tomorrow Critical fix
> ```

### Schedule Ranges

Schedule ranges use `-> [` followed by a range expression and `]`. Ranges support:
- Start and end: `[start .. end]`
- Open start: `[.. end]`
- Open end: `[start ..]`
- Hard deadline end: `[start .. !end]`

> #### Example
>
> ```
> . -> [tomorrow .. friday] Work on feature
> . -> [.. month] Complete by month end
> . -> [week ..] Starting next week
> . -> [week .. !month] Project with hard deadline
> ```

### Time Estimates

Time estimates use `-@` followed by an optional number and a time unit.

Supported units:
- `m`, `min`, `minute` - minutes
- `h`, `hr`, `hour` - hours
- `d`, `day` - days
- `w`, `wk`, `week` - weeks
- `mo`, `month` - months
- `yr`, `year` - years

> #### Example
>
> ```
> . Review document -@ 30min
> . Complex feature -@ 2d
> . Research task -@ 1.5h
> . Long-term project -@ 3mo
> ```

## Tags and Mentions

### Tags

Tags use `#` followed by alphanumeric characters and optional hierarchy using `/`. Tags can also have values using `=`.

Tag formats:
- Simple: `#tag`
- Hierarchical: `#project/frontend/ui`
- With value: `#priority=high`
- With quoted value: `#status="in progress"`

> #### Example
>
> ```
> . Task #work #priority=high
> x Bug fix #project/backend/auth
> - Meeting notes #team/standup
> . Review #status="under review"
> ```

### Mentions

Mentions use `@` to reference people, teams, or entities. They support hierarchy using `/`.

> #### Example
>
> ```
> . Discuss with @alice about design
> x Email sent to @team/management
> - Follow up with @client/support
> ```

## References and Links

### File Links

File links use wiki-style double brackets `[[filename]]` to reference files.

> #### Example
>
> ```
> . Review [[project-plan.md]]
> - See [[docs/architecture.md]] for details
> x Updated [[config.json]]
> ```

### Fish Links

Fish links provide contextual references within and across documents:
- `>(reference)` - Reference within the same file
- `>>(reference)` - Global reference across all files

> #### Example
>
> ```
> . Follow up on >(meeting notes)
> x Fixed issue mentioned in >>(bug report)
> - See >(previous discussion) for context
> ```

### Anchors

Anchors use `=(name)` to create named reference points in the document.

> #### Example
>
> ```
> . Implement feature =(new-feature-v2)
> x Database migration =(db-migration-2024)
> - Architecture decision =(arch-decision-001)
> ```

### Footnotes

Footnotes consist of references and definitions:
- Reference: `[^name]` within item content
- Definition: `[^name]: content` on its own line

> #### Example
>
> ```
> . Research topic [^1]
> x Implement algorithm [^smith2020]
>
> [^1]: See research paper for details
> [^smith2020]: Smith et al. 2020, "Algorithm Design"
> ```

## Formatting

### Strikethrough

Text can be struck through using `~~text~~`. The strikethrough MUST be complete (both opening and closing `~~`).

> #### Example
>
> ```
> . ~~Old approach~~ New implementation
> x Fixed ~~bug~~ feature request
> - Note: ~~deprecated method~~ use new API
> ```

### Update Markers

The sequence `//` indicates progressive updates or additional information added to an item over time.

> #### Example
>
> ```
> . Contact vendor // Awaiting response
> x Submit report // Approved // Published
> - Project discussion // Decided on approach // Implementation started
> ```

## Content Rules

### Item Content

Item content consists of any combination of:
- Plain text (words)
- Time expressions (at the beginning)
- Signifiers (immediately after bullet)
- Tags and mentions
- Schedule tags and due tags
- Time estimates
- File links, fish links, and anchors
- Footnote references
- Strikethrough text
- Update markers

The order of elements is flexible, except:
1. Signifiers MUST come first (after bullet and space)
2. Time expressions SHOULD come at the beginning of content (after signifiers)

### Whitespace

- Leading spaces are significant for indentation
- Trailing spaces SHOULD be ignored
- Empty lines between items are allowed
- Multiple consecutive empty lines are treated as one

### Special Characters

The following characters have special meaning and MUST be escaped or used in their proper context:
- `.` `/` `x` `>` `~` `-` `o` - When starting a line (bullets)
- `#` - Tags
- `@` - Mentions or signifiers
- `!` `?` `*` - Signifiers
- `[[` `]]` - File links
- `>(` `>>(` `)` - Fish links
- `=(` `)` - Anchors
- `[^` `]` - Footnotes
- `~~` - Strikethrough
- `//` - Update markers
- `--` - Item separator (on its own line)
- `->` - Schedule tags
- `-!` - Due tags
- `-@` - Time estimates

## Grammar and Parsing

### Lexical Analysis

The Budge parser uses a custom scanner for handling indentation-sensitive parsing:
- Tracks indentation levels using a stack
- Generates INDENT tokens when indentation increases
- Generates DEDENT tokens when indentation decreases
- Handles both spaces and tabs (tabs count as 8 spaces)

### Parsing Rules

1. A document is a sequence of sections
2. Sections may have headings and bodies
3. Section bodies contain items, separators, and footnotes
4. Items can have sub-items through indentation
5. All inline elements (tags, mentions, etc.) are parsed within item content

## Future Considerations

This specification may be extended in future versions to include:
- Additional bullet types
- Extended formatting options
- Metadata headers
- Cross-file transclusion
- Query and filtering syntax
- Export formats

## Conclusion

Budge provides a flexible, plain-text format for bullet journaling that combines the simplicity of bullet journal notation with the power of digital text processing. The format is designed to be human-readable and writable while maintaining enough structure for parsing and tooling support.
