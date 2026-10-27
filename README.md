## 0. Claude Project instructions (set once)

Paste this into your Claude Project's custom instructions so every chat in it behaves consistently.

```
I'm writing an academic paper on [topic] for [course / conference / journal]. 
Act as a critical research collaborator, not a cheerleader.

Rules:
- Never invent papers, authors, results, or citations. If you're unsure a paper 
  exists or says something, say so.
- When you reference a paper from my uploaded files, name the file.
- Distinguish clearly between (a) what a source says, (b) your inference, 
  (c) your suggestion.
- Push back when my argument is weak or a claim is unsupported.
- Writing style: clear, precise academic prose. No filler, no hype words 
  like "groundbreaking" or "revolutionary."
```

## 1. Scoping (Claude)

```
I want to write a paper on [rough topic]. My background: [e.g., MS CS student, 
backend + ML experience]. Target: [course paper / workshop / survey / 
empirical study], about [page count].

Help me narrow this down:
1. Propose 5 specific research questions within this topic, ranging from 
   survey-style to empirical. For each, say what kind of contribution it 
   would make and how feasible it is for one person in [timeframe].
2. For the strongest 2, list the subtopics I'd need to cover.
3. Give me search keywords and synonyms for arXiv and Semantic Scholar, 
   including terms the field used 1-2 years ago that may have been renamed.
```

## 2. Discovery

**Claude (Research mode on):**

```
Research question: [your narrowed question]

Build a landscape of the work on this, focusing on [date range, e.g., 
2024 to now]. I need:
- Foundational papers (the ones everything else cites)
- Most important recent papers, especially from the last 6 months
- Major benchmarks and datasets used
- Key research groups or labs active in this area
- Open debates or competing approaches

For every paper give title, authors, year, venue or arXiv ID, and a link. 
Flag anything you couldn't verify directly.
```

**NotebookLM (source discovery):**

```
Find recent academic papers and technical reports on [research question], 
prioritizing arXiv, conference proceedings (NeurIPS, ICML, ICLR, ACL, etc.), 
and official lab publications. Skip blog posts and news articles.
```

## 3. Curation (Claude)

Paste in titles and abstracts from your candidate list.

```
Here are [N] candidate papers with abstracts for my research question: 
[question].

Sort them into:
- CORE: directly addresses my question, must read fully
- SUPPORTING: useful context, background, or a single relevant result
- SKIP: off-topic or redundant with something better

For each, one line on why. Then tell me what's missing: any obvious 
subtopic, counterargument, or well-known paper not represented here.

[paste titles + abstracts]
```

## 4. Extraction (NotebookLM)

**Per-paper extraction** (run on every core paper, same format each time):

```
For the paper "[title]", extract the following using only what the paper 
says. Cite passages for each point.

- Problem: what gap or question it addresses
- Method: the core approach in 3-4 sentences
- Data / benchmarks: what it was evaluated on
- Key results: specific numbers, with the baselines they're compared against
- Claimed novelty: what it says is new versus prior work
- Limitations: what the authors themselves admit
- Unstated weaknesses: anything in the setup that limits generalization 
  (small eval set, single domain, missing baselines, etc.), clearly marked 
  as your observation rather than the authors' claim
```

**Cross-paper comparison table:**

```
Build a comparison table across all sources with these columns: paper, 
year, approach category, benchmark(s), main metric and score, compute or 
scale if reported, key limitation. Only include values stated in the 
papers; write "not reported" otherwise.
```

**Disagreements:**

```
Where do these sources disagree or report conflicting results on 
[specific question / benchmark / claim]? For each conflict, cite both 
sides and note any differences in setup that might explain it.
```

**Trends:**

```
Ordering the sources by publication date, how has the approach to 
[subtopic] changed? What did earlier papers assume that later ones 
dropped or challenged?
```

Save each answer as a note, then export or copy them all into one text file for Claude.

## 5. Synthesis (Claude)

Upload the NotebookLM notes file plus the key PDFs to your Project.

**Taxonomy:**

```
Using the extraction notes and papers I've uploaded, propose a taxonomy 
that organizes these approaches. Give me 2 alternative ways to slice it 
(e.g., by method vs. by problem setting) and tell me which better 
supports an argument about [your question].
```

**Gap analysis:**

```
Based only on the uploaded material:
1. What questions remain open or under-studied?
2. Which widely repeated claims rest on weak evidence (one paper, small 
   eval, no replication)?
3. Where are the evaluation practices inconsistent enough that results 
   aren't comparable?
Rank the gaps by how well a [course paper / workshop paper] could address 
them.
```

**Thesis + outline:**

```
My thesis is: [one or two sentences]. 

Stress-test it first: give me the strongest objection a reviewer would 
raise and what evidence in my sources supports or weakens it.

Then propose a section-by-section outline. For each section, list which 
uploaded papers it draws on and the one point that section must 
establish.
```

**Devil's advocate (run once your outline is set):**

```
Act as a skeptical reviewer for [venue]. Read my outline and thesis and 
give me the 5 most likely reasons this paper gets rejected. Be specific.
```

## 6. Writing (Claude)

**Section draft:**

```
Draft the [section name] section following the outline. Key point it must 
establish: [point]. Sources to draw on: [file names].

Requirements:
- ~[word count] words
- Mark every factual claim with [CITE: filename] so I can verify it
- Where you're making an inference rather than reporting a source, phrase 
  it as such
- Don't pad. If a paragraph doesn't advance the point, cut it.
```

**Related work specifically:**

```
Write the related work section organized by [taxonomy from step 5], not 
paper by paper. For each group, explain what the group has in common, 
where it falls short relative to my question, and how my work differs. 
Mark citations as [CITE: filename].
```

**Revision pass:**

```
Revise this section for clarity and concision. Keep my argument and 
structure. Flag (don't silently fix) any claim that seems overstated 
relative to the evidence. Show changes as a short list after the 
revised text.

[paste section]
```

## 7. Verification

**NotebookLM, for every [CITE] tag:**

```
Does [paper title] support this exact statement? Quote the relevant 
passage. If it only partially supports it or says something weaker, tell 
me how the statement should be adjusted.

Statement: "[your sentence]"
```

**Claude, final audit:**

```
Final check on the full draft:
1. List every claim that has no citation but needs one
2. List any place where two sections contradict each other
3. Check that the abstract and conclusion match what the body actually 
   shows
4. Flag any sentence that sounds like hype or overclaiming
```

Two tips on using these: keep the per-paper extraction prompt identical across every paper (that's what makes step 5 work), and start a fresh chat inside the Project for each major stage so context stays clean.

Want me to turn this into a doc you can keep open while you work?
