# Fix Plan: Issue #56 - Structural chunker silently drops headingless documents

## Branch
`fix/56-headingless-docs`

## Diagnosis
In `ingestion/chunking/structural_chunker.py`, `_extract_sections` iterates through lines and appends regular content lines only when `heading_stack or current_section_lines` evaluates to true (line 120). For a document without markdown headings, `heading_stack` remains empty throughout the loop, so `current_section_lines` never receives lines. Furthermore, the trailing flush requires `if current_section_lines and heading_stack:` (line 124). As a result, `_extract_sections` returns an empty list `[]`, causing `StructuralChunker.chunk()` to return `[]` and silently drop the document from indexing. This directly accounts for the reproduction output of `chunks produced: 0` and the `XFAIL [100%]` result on `test_document_with_no_headings`.

## Scope
- In scope:
  - `ingestion/chunking/structural_chunker.py`: In `chunk()`, if `_extract_sections(text)` returns an empty list for non-empty text, assign a fallback section containing the stripped text with `path: []` and `level: 0` so it flows into the standard chunk generation pipeline.
  - `tests/unit/test_structural_chunker.py`: Remove the `@pytest.mark.xfail(strict=True, reason="issue #56: structural chunker drops documents with no headings")` decorator on `test_document_with_no_headings`.
- Out of scope:
  - Modifying `_extract_sections` internals (preserving its single responsibility to return only parsed structural sections).
  - Changing how documents with existing markdown headings are parsed or handling preambles before the first heading.
  - Modifying `SemanticChunker`, token budgeting thresholds, or metadata schema fields.

## Approach
1. In `ingestion/chunking/structural_chunker.py`:
   - Keep `_extract_sections` intact to preserve its role as a pure structural extractor.
   - In `chunk(text: str, metadata: dict)`, immediately following `sections = self._extract_sections(text)`, add a fallback check for headingless text:
     ```python
     if not sections:
         sections = [{"content": text.strip(), "path": [], "level": 0}]
     ```
   - In the downstream loop, `heading_path = " > ".join(section["path"])` evaluates to `""` and `heading_level` evaluates to `0`. If `section_tokens <= self.SECTION_TOKEN_LIMIT` (800 tokens), it emits a single `Chunk`. If `section_tokens > self.SECTION_TOKEN_LIMIT`, it delegates to `self.semantic_chunker.chunk(section_text, section_metadata)`.
2. In `tests/unit/test_structural_chunker.py`:
   - Remove the `@pytest.mark.xfail(strict=True, reason="issue #56: structural chunker drops documents with no headings")` decorator from `test_document_with_no_headings`.
3. Before finalizing, inspect downstream consumers of `heading_path` and `heading_level` in `ingestion/` and `rag/` to confirm that an empty path (`""`) and level `0` are handled safely.

## Test Plan
1. Run the target test:
   ```bash
   .venv/bin/pytest tests/unit/test_structural_chunker.py -k test_document_with_no_headings -v
   ```
   Expected output: transitions from XFAIL to PASSED.

2. Run the structural chunker unit test suite:
  ```bash
  .venv/bin/pytest tests/unit/test_structural_chunker.py -v
  ```
  Expected output: the structural chunker unit test suite passes with zero regressions.

3. Run the standalone reproduction snippet:
  ```bash
  .venv/bin/python -c "from ingestion.chunking.structural_chunker import StructuralChunker; c = StructuralChunker(); chunks = c.chunk('This is a plain document with no headings at all. ' * 20, {}); print('chunks produced:', len(chunks)); assert len(chunks) >= 1"
  ```
  Expected output: chunks produced: 1 and assertion succeeds.

4. Test large headingless documents exceeding SECTION_TOKEN_LIMIT (800 tokens):
  ```bash
  .venv/bin/python -c "from ingestion.chunking.structural_chunker import StructuralChunker; c = StructuralChunker(); chunks = c.chunk('This is a long sentence with lots of words repeated for token volume. ' * 120, {}); print('large doc chunks:', len(chunks)); assert len(chunks) > 1; assert all(c.metadata.get('heading_level') == 0 for c in chunks)"
  ```
  Expected output: large doc chunks: >1 via semantic sub-chunking with metadata intact.

5. Run repository pre-PR checks per docs/CONTRIBUTING.md:
  ```bash
  make check && make test-unit
  ```
  Expected output: all linting, formatting, and unit tests pass cleanly.

## Risks and Unknowns
- Low Risk: Headingless documents exceeding SECTION_TOKEN_LIMIT (800 tokens). SemanticChunker.chunk copies incoming metadata (semantic_chunker.py:55, 82), so heading_path: "" and heading_level: 0 carry through to sub-chunks without KeyError. Step 4 of the test plan explicitly verifies this boundary.
- Downstream safety: Inspected ingestion/ and rag/; outside the chunker, heading_path is only documented in base.py:21 and heading_level is not read elsewhere, confirming empty string paths and level 0 carry zero regression risk.

## Deviations
None. The implementation matched the plan exactly: added the fallback section assignment in `chunk()` immediately following `_extract_sections(text)` when no sections are extracted from non-empty text, and removed the strict `xfail` decorator on `test_document_with_no_headings`. All 15 structural chunker unit tests pass.
