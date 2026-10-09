# v05: keep the final full-width table with its discussion

Purpose: v04 already fit all body text on eight pages, but declared the post-call double-column float at the start of page eight, forcing that table onto page nine. Move the complete post-call subsection before policy costs so its full-width table can occupy page eight. Every sentence, table cell, equation, and binding block is unchanged; no font, margin, class, or float-package changes. v04 source/build retained.

The first two LaTeX passes now use native `-draftmode`: they still compute layout/references but skip redundant PDF output. The third pass emits the actual PDF and all existing page/log/provenance gates still run. This is an export efficiency change, not removal of a check.

Status: merged, final build `submission_20261008_114253` passed and all nine PDF pages were independently inspected. Promoted to the unique master; previous master/provenance archived. Broader scientific gate remains HOLD.
