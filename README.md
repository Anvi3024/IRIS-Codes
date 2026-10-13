# Submission materials

This folder is the clean, submission-facing selection. It contains the editable LaTeX research paper draft, its compiled PDF, the Markdown source, a research synopsis, four result figures, all five consolidated result tables, the source registry, an asset manifest and a self-contained [computational code folder](code/README.md). Development diaries, handoff notes, abandoned future proposals and duplicate overviews are not copied into it.

The paper builds on the synopsis but is still a draft for independent student review and authorship. It does not certify eligibility or authorship and must not be submitted unchanged as independently authored student text. One accurate assistance acknowledgement is retained. Final contributor roles, independently written abstract/references and applicable anonymity requirements remain for student/guide review.

The manuscript focuses on helping Delhi address water availability through better outage response. The current experiment demonstrates conditional distributional comparisons, not improved actual water availability, total shortage reduction or tanker transport efficiency. The future-work section proposes measured inputs, shadow-mode comparison and separate operational extensions; none has been implemented or validated.

## Editing and reproduction

For runnable research code, setup and commands, start with [code/README.md](code/README.md). `code/run.py` supports integrity checks, 62 selected computational tests, regeneration of the paper figures/tables from saved results, and optional full simulation replay. Required frozen inputs/protocols are included unchanged; unrelated ingestion and document-generation code are excluded. This snapshot can be copied away from the parent repository.

The LaTeX manuscript can be edited using `research_paper_draft.tex`. The python document builder script generates this .tex file from `manuscript.md`.

```sh
# From the repository root, using its locked research environment:
.venv/bin/python src/build_submission_package.py
.venv/bin/python scripts/build_paper_latex.py
.venv/bin/python src/build_submission_code.py
.venv/bin/python -m pytest -q
```

The packager verifies both frozen experiments and their complete saved outputs. It rejects edited submission assets rather than overwriting them. Figure/table hashes match the upstream consolidation. `asset_manifest.json` covers the exported Markdown/assets, not subsequent human edits or the generated PDF.

No scripts upload, email or submit anything. Review the compiled PDF before upload. The directory is prepared for review, not a declaration that all submission requirements have been satisfied.

## Research workspace retained separately

The full source evidence, dictionaries, protocols, notebooks and complete experiment outputs remain at the repository root. Frozen files must not be deleted, moved, rewritten or relocked. They are dependencies of reproducibility, not unrelated material to purge. The root duplicate overview and the synopsis being edited by the user are left untouched.

The source registry retains contextual sources that are not numerical inputs. Acquisition status and raw paths distinguish downloads from manual excerpts and referenced sources. The paper cites only sources used in its text; the registry is not a claim of a fully acquired corpus.
