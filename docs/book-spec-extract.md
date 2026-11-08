<!-- Extract of specifications from Lion Kimbro, "How to Make a Complete Map of Every Thought You Think" (2003). Licensed CC BY-SA 1.0 (see ../book/LICENSE.html). Line numbers refer to book/book.txt. -->

# Kimbro "hybrid" program: spec extract for the builder

Source: book.txt (Lion Kimbro, CC BY-SA 1.0), 7141 lines. Line numbers = book.txt. Quotes verbatim and short.
Legend: [BOOK] stated in the book; [INFER] my reading; [GAP] builder must decide.

## 0. The program as Kimbro states it

### Vision (ch. VI, 6151-6165)
- Day: pan-subject speeds. "You don't number them, you just keep them." (6154-6155)
- Night: "quickly transcribe the pan-subject speeds ... into ASCII" (6157-6159); icons/images are lost ("there went our icons and images", 6159-6160).
- "You run the magic program, and bam! It spits out updated pages over your printer." (6160-6162)
- Output: "ULTRA-DENSE, but in a READABLE size (that you chose)... fitting *100 speeds* to the page" (6167-6169).
- Not only speeds: "Put your POI titles in there too! And put your REFs in there too! If there's a TOC for it, put it in there!" (6214-6218). Goal: "high quality laser printer ultra-info-density tables" (6220).
- "A programmer of 1 YEAR could do this!" (6202); "C# or Python" (6204); may drive Word: "using Word;" (6204).
- Why: tedious parts are "Maintaining TOC's. Transcribing Speeds. Redrawing Maps" (6138-6140); first two are ~90% of maintenance (6142).
- Maps are the second program (6338); out of scope.

### Change-file example: splitting a subject (6171-6189) [BOOK]
```
MTK
S
13 Eth
19 Spt
43 Eth
44 Eth
49 Imgn
51 Imgn
62 Eth
93 Eth
121 Spt
```
- Meaning (6186-6189): "In Mental Technique, regarding the Speed thoughts, send #13 to Ethics, #19 to Spirit, ..."
- Line 1 = source subject abbreviation (MTK = Mental Techniques, 7042). Line 2 = segment/list (S = Speed Thought, 1562). Then one `<speed#> <dest abbrev>` per line.
- Abbreviations: Eth=Ethics, Spt=Spirit, Imgn=Imagination (6176-6184; names at 5431-5433).
- [GAP] Kimbro's prose gloss skips 49/51 (Imgn); treat the list as authoritative.
- [GAP] No syntax for several blocks per file, comments, or other change types.

### Geek Block (6555-6645) essentials
- "I would code it in C# or Python." (6559)
- ASCII diagram (6562-6579): INPUT (by file | by GUI/CLI) -> Database Access (TXT XML YAML SQL) with 4 stores: Lists, Entrees, Subjects/IDs, Output Configuration; Output -> "changed pages" and "duplicates" -> Page Output System -> .doc and .txt; "system resources (printer)".
- "You can input by file (changes), or by GUI/CLI (invocation, diognostics, output requests)." (6582-6583)
- Change requests (6588-6593): add entries; change/update/annotate entries; move entries; mark to archive; new subjects.
- Output requests (6595-6597): "request duplicates of pages"; "request all pages updated since last request".
- Invocation (6599-6600): "update from the latest request, and return a receipt."
- Diagnostics (6602-6605): debug information; manual vision/manipulation of internal databases; database statistics.
- DATABASE (6608-6621): "Abstract the database system. May want to use pure interfaces, or a bridge pattern." (6610-6611). "I myself would go with YAML." (6616). Keeps "your lists (TOCs, speed lists), the entrees in the lists (Speeds, TOC entries; their contents, hints, and titles), the subjects & their abbreviations, and the present output configuration (txt? doc? rtf? font? size? color?...)" (6618-6621).
- OUTPUT (6624-6636): "START SIMPLE... dirt simple. For a word.doc output, just say '2 columns, point 8 text, tabbed, fit 70 to the page.'" (6626-6628). "Don't worry about making optimal use of space for now" since longer speeds roll text onto the next page "and you have to keep track of which pages need reprinting" (6629-6634). "Just keep it dirt simple for this first trial." (6636)
- "It's really a VERY simple program!" (6641)

### CLI mapping [INFER]
- `ingest <changefile>`; `update` (invocation: apply, print changed pages, emit receipt); `duplicates <page-ids>`; `since-last`; `stats`/`dump`/`validate`; `subject new`.
- Receipt: no format in book [GAP]; suggest timestamp, applied changes by type, pages emitted (IDs + files), warnings/errors.

## 1. Pan-subject speeds (transcribed input)

- Four partitions (940-945): "Transcription Checkoff / Subject / Hint / Content". Plus Psi: "Mine also has a place for a 'Psi' marker" (947-948). Plus page date: "a date marker at the top of the page, for the Chrono archives" (958-959). "the format is maleable" (957).
- Fields for the program: checkoff, subject, hint, content, psi (optional), date (page-level, not per row).
- "DO NOT PUT THOUGHT NUMBERS on the Pan-Subject Speeds page" (961); it is a "TEMPORARY placeholder" (962-963). Numbers are assigned only on landing in a subject list (2925-2928; 5235-5236).
- Checkoff (967-969; 2993-2995): "You check the box after you have moved the idea OUT to where it needs to go. Don't check it when you first put the thought in." Pan page: "you check off when you've transcribed the speed thought to the appropriate speed list."
- Subject (971-974): target subject. Hint (976-977): "a quick 1-2 word, maybe 3 word, description of how this thought fits into things"; "VERY IMPORTANT" (988); "EVEN MORE IMPORTANT THAN THE THOUGHT ITSELF" (1004). Subject-list version: "1-3 word description of the CONTEXT" (2939).
- Content (1011-1018; 2964-2970): terse, abbreviated, "Lean toward the terse"; speeds are "about 1-50 words. Generally around one line" (256-257); 1-5 lines tops (3025-3027), beyond that write a POI.
- Incomplete rows are legit: "Do you have to fill in the hint as well? And the subject? NO! Just wait for break." (1020-1026). Accept empty subject/hint.
- Psi (2947-2962; list 949-951, ~5880-5901): thought type (Principle, Observation, Warning, Possible Action, Goal, Problem, Starting Point, question, rule, name, see also, quotation, hazard...). "You can omit this column" (2948). [GAP] ASCII codes for glyphs.
- Inline one-line speed form appears at 3176: `"#43:blahblah:Remember to work on BlahBlah."` = `#number:hint:content` [INFER usable]; no file format for pan input is defined [GAP].
- Date style: "(Sunday) 25 May 2003" (1376-1377), weekday in kanji on paper; ASCII/ISO in program [GAP].
- Batch: "Transcribe pan-subj speed lists in batch." (1218); "check them all off onto the one subject speed list while you are there" (1195-1196). Prefer writing straight into the subject list when possible (1170-1177, 1200).
- Speeds can be to-dos: "Check them off in the first column when you complete them." (2972-2974); also "work requests" (3181-3182).

## 2. Subject speed lists: format, numbering, page IDs

### Page format (2895-2908) verbatim
```
        (Subject Name) S__-S__         ___________
V # Hint @ Content
...rows...
                    []      (Subject Name) S__-S__
```
- Header top-left "(Subject Name) S__-S__" = first/last speed number on page; top-right blank = date (1376). Footer bottom-right repeats it; "[]" = archive box.
- Columns: V = checked off ("mapped", 2912-2916); # = speed number; Hint; @ = psi; Content.
- Number only on the first row of a multi-row speed (2927-2928).
- Subject spelled in FULL on speed pages, not acronym: "just spell everything out on your speeds and on your P&P's" (1547-1548; reason 1537-1546, "Does MTK come before or after MP?").
- Page ID examples: "Notebook S27-S47" = "Speed Thoughts numbered 27-47" (1490-1497); last page "S127-S140" (1094-1096).
- Map refs: "`47" for a speed (2931-2932); "(5)", "P5", "REF5" (2664-2665, 3757-3759). Cross-subject refs vary: "ECollab S57" (5237-5238), "SYS-S4", "ACT2:S9" (5059-5086) [GAP: pick canonical `<Subj> S<n>`, accept others].

### Page ID structure (1518-1698) [BOOK]
- "Subject - Segment - Segment ID - Page ID" (1520). Subject optional (1526) but mandatory and spelled out on speeds and P&P (1537-1548).
- Segment signifiers (1552-1563): PJ Project; POI or P; RS Research; REF Reference; A/S; P&P; I Index; SMOC or M; S Speed Thought; Cht Cheat Sheet. "R" sometimes for REF (1576-1578). Experimental: X (unlinkable, 4103-4112), CEP, TD, DD, L/T (4121-4124). The book also writes "PR" for Project (2706).
- Segment ID number (1580-1626): TOC # (ordinality) or Version # (A/S2, M2-1, M3-1). Bare "A/S" or "M" = first/single (1603, 1613).
- Page suffix: "P27" = one-sheeter; "P27-1" = first of several (1416-1419); "P2-3" = page 3 of POI 2 (1508). Decimal insertion: "P7-4.3 and P7-4.7" (1639-1643). "There's no binder police."
- REF form (1675-1696; 3893-3905; 7096-7110): subject REF(ref#)-(book structure id)-(page), e.g. "Robots REF7-II.3-1", "GKI REF13-II.4.A-2". REF# comes from the references list, not reading order (7106-7110).
- Speeds are not paginated with "-n": ID = Subject S(first)-S(last).
- Sequence marks (1406-1445): arrow = continues; box = end; box with arrow through = extended; above the page ID. [GAP] ASCII rendering.
- Archival mark (1447-1482): LEFT of page ID, RED, kanji "Old". Meaning: "no longer required in the subject ... may still be the target of some links" (1467-1471). Procedure: red "SEE ALSO: (page id of more recent information)" plus glyph (1473-1476). Archival pages go to the back (1478). Speed lists: all rows red-checked then "check the archive box [] at the bottom of the page" (3061-3064).
- Out cards (1786-1830): moved page leaves a page with "the same PAGE ID as the old place, that points to the new page" (1787-1789); consolidate "POI3, Pages 4-7" (1803-1805); fix chains to the final target (1826-1830).

### Numbering rules for speeds
- New speed gets "the next available number" (5235-5236). "S74: Moving Entrees. Don't renumber if don't have to. Open spaces AOK." (5095). Gaps fine (5263). When renumbering, "CROSS OUT rather than WHITE OUT" (5269-5271). "just keep a record of next numbers" (7132-7133). [INFER] per-list next_number counter in YAML; never reuse.

### Moving speeds between subjects
- Misfiled thought (5230-5245): add to destination with next number; "cross it out of PFT, in red. Also in red, write ECollab S57" (5236-5238). Reason: "you might have links to the original" (5239). Full erasure only if sure of no links; "I usually just link it" (5244).
- Splitting a subject (5484-5489): note where each went, "perhaps the initials of the destination subject, and the speed #". Move REF/PJ/RS/POI too; place out cards.
- [INFER] model: source entry stays, status=moved, moved_to {subject, number}; destination gets new number (optional moved_from); source page reprints. [GAP] printed marker.
- Retirement: "mark a RED check into the speed's box when it is retired" (3061-3062), though Kimbro does not do it. Do NOT un-check speeds removed from a map: "Let it go." (3072-3080).
- Routing rules (human only; 5294-5304): knowledge used in B goes to B; many-many goes to most general; one object in many contexts goes to its native domain; one object in one context goes to the context; a particularly relevant domain wins; action in a domain goes to that domain. "Put specific things in specific places, and general things in general places." (5398-5399)

## 3. Subjects, abbreviations, creation, special subjects

- Subject = tab section; title "a good, 1-3 word title" (5424).
- GSR (7014-7017): "Grand Subject Registry. A two page (or more) list of all of your subjects"; "attach flags and other metadata to subjects identified by NAME". Hashed by first letter (4562-4565). "every subject has an appearance on the GSR" (4603-4604). Includes unmapped subjects (4578-4581), the Unplaced section (4587-4592), archived subjects with red "old" mark: "You don't put stuff in here, it's old." (4594-4599). Half subjects are not on it (4825-4826).
- [INFER] subject record: full name, abbreviation, status (half, active, old), flags, parent, created, per-list next_number.
- Abbreviations (A/S) (2038-2046; 4048-4055; 6974-6979): key/value hash, per-subject plus global. Examples: MTK Mental Techniques (7042), MP Metaphysics (1545), Eth, Spt, Imgn (6176-6184), GKI (6988), PFT (7053), ECollab Electronic Collaboration (5232), SYS (5065), SOC (5086), PPL People, EDU Education (4797-4799), PHist (2849), ACTS (6953). [GAP] case-insensitive match, uniqueness.
- Ordering: by EXPANDED name, not abbreviation (1544-1548; 2693; 4509-4510); REFs numeric: REF3, REF9, REF21 (2731-2732).
- New subject checklist (5670-5691): tab; move pre-existing pages (e.g. half-subject speed lists, unplaced notes); GSR and GSMOC entry; P&P (optional: "For about 1/2 of my subjects, I don't have P&P pages", 5689-5690); a Subject Speed page. Program: "new subjects" (6593) = record + empty speed list + registry entry.
- Half subjects (4543; 4814-4831; 6715-6717): just speed lists or accumulating in Unplaced. Promote at 5-10 pages (2589-2591), 7-10 (4409-4410), or speed list of 10-20 (6747).
- Subject feeder (4996-5021): a subject used as a staging bin whose speeds are later forwarded; justifies the move command.
- Growth: "If in doubt, go general in the beginning. Then, extract subjects within" (5479-5480); source subject stays "loose" (5513-5514).
- Special subjects (5695-5740): Chrono, Strategy (TOC plus chronological pages "numbered simply 1,2,3...", 5709-5710), Zeitgeist (monthly sheet, 5720-5722). Optional.
- CHAOS (4324-4333; 1107-1110; 1131; 4354-4355): "papers that have hoards of thoughts on them with no obvious subject placement, or ... hopelessly beyond recovery"; filed under C; dumped to archive often. [INFER] reserved subject, archivable freely.
- UNPLACED (1110-1115; 4391-4412): proto-subjects. "DO NOT NUMBER THE PAGES. There IS NO SUBJECT yet, and thus no pagination." (4400-4401). The would-be subject is written on the page (4403-4407); sorted A-Z by would-be subject (1114). [GAP] numbering for unplaced speeds: unnumbered, or numbered per would-be subject.
- "unplacable list" (5159-5164): speed list "before all the speeds" for "out there" thoughts that fit nowhere; "make a place ... for unplaced or unplacable" (4584-4585). Fallback target for blank/unknown subject [GAP].
- Only the LATEST speed page per subject is carried (1092-1098; 4523-4527), so "only changed pages" fits.

## 4. TOCs

Attitude: "I frequently wish that these things would just DIE" (7124-7126); "just an index of titles connecting them to page numbers" (7128-7130); "comforting to have TOC's over your POI's and Ref's and RS's" (7133-7134); main utility "providing you the next number to number something" (211-213). The TOC doubles as the number allocator.

- Required by the program (6212-6218): speed lists, POI titles, REFs; "If there's a TOC for it, put it in there!"
- Candidates from the segment list [INFER]: POI (number + title; title is the ID, 1387-1388 "POI #26 - the Kitty Model"; build at ~10 POI, 2593), REF (the references list, "like your Speeds, but it captures references", 7106; "special form of TOC", 2689; entry = ref# + citation, 1502-1504; expanded or not, 2675-2679), RS (7117-7118), PJ, Cht (4024), TD ("TD1.. on a single page", 4172-4175), DD (hash TOC, 4220-4224), CEP (4141-4146), I index (A-Z hash, 3944-4007), A/S (A-Z hash), GSR (A-Z hash), P&P collection (alphabetical by subject, 4382-4383).
- TOC order per subject (2691-2708): by expanded segment name (Point of Interest, Project, Reference, Research); numeric within segment.
- Gaps OK; list "P7" only when reached (5263-5265). Paper adds a 1-3 word mnemonic (2670-2672; 2667-2668 "POI5 is about Naming").
- Hash lists: letters left, keys then values; index puts numbers after Z, symbols after numbers (4001-4002); three lines per letter when large (3998); name grouping AB / C / DEF / GHI / J / KL / MNO / PQR / ST / UVWXYZ (4063-4072).
- "Subject Speeds and Refs TOC" (4498-4521): all subjects alphabetical (expanded), in pairs: latest speed page first, then ALL references. Concrete print job [INFER].
- Generic entry [INFER, unspecified]: list, number, title/hint, page_id, status, mnemonic; one line per entry.

## 5. Output

- Config in DB (6620-6621): txt/doc/rtf, font, size, color. Dirt simple (6627-6628): "2 columns, point 8 text, tabbed, fit 70 to the page." So columns=2, size=8pt, tab-separated fields, 70 entries per page. ("100 speeds" at 6168-6169 is aspirational.) Targets: .txt, .doc (6627; Word automation 6204).
- [GAP] txt columns: ignore columns or interleave 35+35.
- Fixed per-page capacity means no reflow (6629-6634). Recommend fixed number ranges per page so unchanged pages stay identical. Entries never renumber or delete, so only touched pages and the last page change.
- Changed pages = since last request/receipt (6573, 6597); duplicates = reprint named pages without altering change tracking (6596). Need per-page revision and last_printed.
- Receipt (6600): format unspecified [GAP].
- Page furniture (1360-1367; 2897-2908): subject full name + range top-left; date top-right; footer ID; archive box; sequence mark.
- Blank forms: "Blank Speed Pages", "Blank Pan-Subject Speed Pages" (4293-4295); pan page 40+ lines (936-937). Optional blank pan form printout [INFER].

## 6. Colors and annotations

Code (1987-2020): RED = Error, Warning, Correction; BLUE = Structure, Diagram, Picture, Links, Keys; GREEN = Meta, Definition, Naming, Brief Annotation, Glyphs; BLACK = Main Content. "Page numbers are blue" (2005); "Blue is for Keys, Black is for values." (2001).
- Entry-level uses: moved speed red cross-out + red pointer (5236-5238); archived page red Old glyph + red SEE ALSO (1473-1476); retired = red check (3061-3062); mapped = check in V (2912); modifications in red (4206); trash red X (1161); archived subjects red "old" in GSR (4597); unmapped subjects green in GSR (4579-4580); index and A/S keys blue (3970-3971).
- Output config lists "color?" (6621) but the dirt-simple spec is monochrome. Text mapping [INFER, optional]: [x] transcribed/mapped, [R] retired, struck text plus "-> Eth S57" for moved, OLD plus SEE ALSO id for archived. For .doc, red runs optional.
- "change/update/annotate entries" (6590): keep original text, add annotation lines (cf. red corrections 1995; green brief annotations 1997) [INFER].

## 7. Acronyms section (6942-7140)

Header "This Document's Abbreviations and Shorthand (A/S)" (6942). Entries: ACEPOS (6944, philosophy); ACTS (6953, GTD hybrid; "absorbed the someday/maybes into the subjects' speed lists", 6963-6964; points to ch. VI for software that opens "both doors", 6966-6968); A/S (6974: per-section plus global; "Global A/S should have a page for names, and some hash pages", 6978-6979); FF (6981, joke); GKI (6988, example subject); GMOC (7007 = GSMOC); GSMOC (7009; every non-archived subject appears); GSR (7014; flags/metadata by NAME); GTD (7019); I (7028; "alphabet based index. (Can contain numbers and glyphs as well.)"); M (7031; "NEVER same as GSMOC"); MOC (7033); MTK (7042); PFT (7053); PJ (7076); POI (7078-7080: "Bound by a TOC, generally. Placed on the SMOC."); P&P (7082-7094: one page per subject, INCL/EXCL, global tab); REF (7096-7110: numbering by reference # and book structure, no normal TOC); RS (7112-7118: note REF#s, "linear flow in your RS#", "conventional TOC's"); SMOC (7120); TOC (7124-7134: keep a record of next numbers); V/VER (7136-7139: version, lowercase v).
Relevant for the program: A/S = abbreviation store; GSR = subject table with flags; TOC = next-number record.

## 8. Ambiguities and decisions for the builder [GAP]

1. Change-file grammar: only the move example exists (6174-6184). Define add/edit/annotate/move/archive/new-subject, multi-block files, comments, idempotency (batch id or hash to avoid double numbering on re-ingest).
2. Pan-subject ASCII format: undefined. Suggest rows "[x] | subject | hint | psi | content" with a "date:" header, and accept "#n:hint:content" style (3176).
3. Number assignment on ingest: next available per destination list, in file order; never reuse or renumber (5095, 5235).
4. Blank/unknown subject: pending queue, UNPLACED or unplacable list (5159-5164), or error. Missing hint allowed (1020-1026). Unknown abbreviation in a change file: error (new subjects is explicit, 6593).
5. Move semantics: source stays marked moved with pointer; chains collapse to final target (1826-1830); POI/REF moves need out cards and renumbering (5250-5271), so include only if those lists are in scope.
6. Archive semantics: entry (retired), page (archive box), subject (GSR old). "mark to archive" (6592) unspecified; suggest entry-level plus derived page-level.
7. V column is tied to maps (out of scope); toggle only by explicit command.
8. Page size: 70 vs 100; fixed number ranges vs length-based; "fit 70 to the page" counts entries. Multi-row speeds overflow.
9. "since last request": per-page revision; duplicates must not clear the dirty set.
10. Receipt content/format; diagnostics (stats, dump, validate).
11. Entry types in MVP: speeds (S), POI titles, REF list; optionally RS, PJ, hash lists (I, A/S, DD, GSR). Generic DB: lists(type, subject, toc flag), entries(number, title/hint, content, status, annotations, psi, date).
12. Date format; psi codes in ASCII; ASCII sequence/archive marks.
13. Subject names vs abbreviations: store both, print full names, sort by full name (1544).
14. YAML store behind an interface (6610-6616); single file vs directory; atomic writes; backups; library choice.
15. Language: C# or Python (6204, 6559); .docx via library vs Word COM (6204); rtf only in config list (6621).
16. Versioned lists (A/S2, M2-1) need a version instead of TOC # (1580-1626); optional.

## Quick reference
- Pan speeds: 927-1036, 1168-1222, 2986-2995, 3176, 6153-6166.
- Page layout, IDs, marks: 1353-1698, 1786-1830, 2895-2980.
- Segments, TOCs, physical order: 2539-2770, 3765-4265.
- Registry, Unplaced, Chaos, new subject, special: 4283-4620, 5053-5745.
- Computers chapter and Geek Block: 6116-6645.
- Acronyms: 6942-7140. Color code: 1987-2036.
