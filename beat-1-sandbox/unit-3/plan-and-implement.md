# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

manasbhati24

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56#issuecomment-5965029016

I drafted a fix plan for #56 based on the local reproduction on commit `2f4e82f` (reproduction report [here](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/56#issuecomment-5865589522)).

### Environment & Observed Baseline
- macOS 15.4.1 (x86_64), Python 3.11.7, pytest 9.1.1, commit `2f4e82f`
- Baseline: `StructuralChunker.chunk()` returns `chunks produced: 0` for headingless text, and `test_document_with_no_headings` yields `XFAIL [100%]`.

### Proposed Approach
- **Diagnosis:** In `ingestion/chunking/structural_chunker.py`, `_extract_sections` drops regular content lines when `heading_stack` is empty (line 120) and flushes sections only when a heading exists (line 124), returning an empty section list for documents without headings.
- **Scope:** Confined to `chunk()` in `ingestion/chunking/structural_chunker.py` and enabling `test_document_with_no_headings` in `tests/unit/test_structural_chunker.py`. `_extract_sections` retains its role of extracting markdown headings; preamble handling in headed documents, metadata schemas, and `SemanticChunker` remain untouched.
- **Implementation:** In `chunk()`, immediately following `sections = self._extract_sections(text)`, check `if not sections:`. If the document contains non-empty text, assign a fallback section `[{"content": text.strip(), "path": [], "level": 0}]`. The existing loop handles downstream routing: single-chunk creation for texts within `SECTION_TOKEN_LIMIT` (800 tokens) with `heading_path = ""` and `heading_level = 0`, and delegation to `self.semantic_chunker.chunk` for texts exceeding 800 tokens. I checked `ingestion/` and `rag/` to confirm that empty `heading_path` strings and `heading_level: 0` are safely tolerated downstream.
- **Verification:** Remove `@pytest.mark.xfail(strict=True, reason="issue #56: structural chunker drops documents with no headings")`, verify `test_document_with_no_headings` transitions from `XFAIL` to `PASSED`, verify the structural chunker unit test suite passes, test large (>800 token) headingless documents, and run `make check && make test-unit`.

---

## Your branch

**Branch**

fix/56-headingless-docs

**Evidence**

Before:

```bash
$ .venv/bin/pytest tests/unit/test_structural_chunker.py -k test_document_with_no_headings -v
======================================= test session starts =======================================
platform darwin -- Python 3.11.7, pytest-9.1.1, pluggy-1.6.0 -- /Users/manas/CodePath/301/pathreview-ai301-fa26-s3/.venv/bin/python
cachedir: .pytest_cache
rootdir: /Users/manas/CodePath/301/pathreview-ai301-fa26-s3
configfile: pyproject.toml
plugins: anyio-4.15.1
collected 15 items / 14 deselected / 1 selected

tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings XFAIL [100%]

================================ 14 deselected, 1 xfailed in 0.17s ================================

$ .venv/bin/python -c "from ingestion.chunking.structural_chunker import StructuralChunker; c = StructuralChunker(); print('chunks produced:', len(c.chunk('This is a plain document with no headings at all. ' * 20, {})))"
chunks produced: 0
```

After:

```bash
$ .venv/bin/pytest tests/unit/test_structural_chunker.py -k test_document_with_no_headings -v
======================================= test session starts =======================================
platform darwin -- Python 3.11.7, pytest-9.1.1, pluggy-1.6.0 -- /Users/manas/CodePath/301/pathreview-ai301-fa26-s3/.venv/bin/python
cachedir: .pytest_cache
rootdir: /Users/manas/CodePath/301/pathreview-ai301-fa26-s3
configfile: pyproject.toml
plugins: anyio-4.15.1
collected 15 items / 14 deselected / 1 selected

tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings PASSED [100%]

================================ 1 passed, 14 deselected in 0.16s =================================

$ .venv/bin/pytest tests/unit/test_structural_chunker.py -v
======================================= test session starts =======================================
platform darwin -- Python 3.11.7, pytest-9.1.1, pluggy-1.6.0 -- /Users/manas/CodePath/301/pathreview-ai301-fa26-s3/.venv/bin/python
cachedir: .pytest_cache
rootdir: /Users/manas/CodePath/301/pathreview-ai301-fa26-s3
configfile: pyproject.toml
plugins: anyio-4.15.1
collected 15 items

tests/unit/test_structural_chunker.py::TestStructuralChunker::test_empty_input_returns_empty_list PASSED [  6%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_whitespace_only_input PASSED [ 13%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_no_headings PASSED [ 20%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_document_with_nested_headings PASSED [ 26%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_heading_path_format PASSED [ 33%]
tests/unit/test_structural_chunker.py::TestLargeSectionSubChunked::test_large_section_sub_chunked PASSED [ 40%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_chunk_metadata_includes_heading_level PASSED [ 46%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_chunk_metadata_structure PASSED [ 53%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_preserve_source_metadata PASSED [ 60%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_multiple_h1_headings PASSED [ 66%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_heading_path_breadcrumb PASSED [ 73%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_chunks_have_text_content PASSED [ 80%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_section_extraction_with_multiple_levels PASSED [ 86%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_heading_not_in_middle_of_content PASSED [ 93%]
tests/unit/test_structural_chunker.py::TestStructuralChunker::test_empty_sections_handled PASSED [100%]

======================================= 15 passed in 0.27s ========================================

$ .venv/bin/python -c "from ingestion.chunking.structural_chunker import StructuralChunker; c = StructuralChunker(); chunks = c.chunk('This is a plain document with no headings at all. ' * 20, {}); print('chunks produced:', len(chunks)); assert len(chunks) >= 1"
chunks produced: 1

$ .venv/bin/python -c "from ingestion.chunking.structural_chunker import StructuralChunker; c = StructuralChunker(); chunks = c.chunk('This is a long sentence with lots of words repeated for token volume. ' * 120, {}); print('large doc chunks:', len(chunks)); assert len(chunks) > 1; assert all(c.metadata.get('heading_level') == 0 for c in chunks)"
large doc chunks: 4
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1: 20/20.

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

Package: pkg-01
Rubric decision: reject
Gold label: reject

Explanation:
The candidate plan claims:
> "The REQUEST_ITEM tokenizer in httpie/cli/requestitems.py is the problem. Its separator regex fails to recognize header1:xyz and x=1 as valid items when an optional flag appears earlier in the argument list... The Python-version difference is a red herring"

However, step 4 of the reproduction evidence explicitly proves:
> "--debug on the failing run shows the error is raised by argparse's parse_args while consuming positionals; the request items are never handed to HTTPie's item parser."

The plan invents an internal tokenizer defect that contradicts the author's own debug evidence and dismisses the Python version divergence as a "red herring". Under our rubric, check `diagnosis-grounded` requires that the plan:
> "Identifies a concrete defect or mechanism that directly accounts for the target bug described in the issue and demonstrated by the repro artifacts; fails if it contradicts repro measurements, solves a different bug, or blames an unrelated component"

Because the plan blames an unrelated component (`requestitems.py`) and contradicts the repro artifact showing the crash occurs upstream in standard library `argparse`, `diagnosis-grounded` received `fail`. Under our verdict rule, a single failed required check yields a `reject` verdict, matching the gold label.

**Check rationale**

Quoted check from `rubric.md`:
```markdown
| diagnosis-grounded | Plan's stated cause read against issue description and repro evidence | Identifies a concrete defect or mechanism that directly accounts for the target bug described in the issue and demonstrated by the repro artifacts; fails if it contradicts repro measurements, solves a different bug, or blames an unrelated component | required |
```

This check explicitly reads the stated cause against both the issue description and the reproduction evidence. We formulated the pass condition to include the explicit negative clause: "fails if it contradicts repro measurements, solves a different bug, or blames an unrelated component". Without that boundary, LLM evaluators frequently give a passing grade to plans that sound plausible and name existing repository files (such as `requestitems.py` in `pkg-01`), even when the candidate's diagnosis directly contradicts the debugger traces or runtime evidence captured during local reproduction.

**Trade-offs**

This strict requirement gives up tolerance for plans that propose valid heuristic workarounds when the underlying third-party or upstream defect cannot be patched directly. For example, if a contributor accurately observes an upstream Python `argparse` bug and proposes a pre-parsing normalization wrapper inside HTTPie, phrasing the proposal as "the tokenizer is broken" fails `diagnosis-grounded` because it misnames the defect mechanism.

Nothing changed elsewhere across the non-negative packages: in `eval-run.txt`, all 7 clear-accept packages (`pkg-05`, `pkg-08`, etc) passed this check cleanly because genuine plans accurately attribute the defect to the specific conditional branch or mechanism identified in their reproduction steps.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
