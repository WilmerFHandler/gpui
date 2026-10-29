# Local patches

Handler's fork of `gpui` 0.2.2 (crates.io) with the maintained change below,
licensed under Apache-2.0; see `LICENSE-APACHE`. The first commit is the unmodified
published package; the second applies this patch.

## Truncation runs reused across measurements

`src/elements/text.rs` truncated the text runs captured by the measure closure in
place. Taffy measures a text element at several widths, so a narrow pass shrank the
runs and a later, wider pass laid out longer text with the shrunken runs. When the
boundary fell inside a multibyte character, `MacTextSystem::layout_line` panicked
with "byte index is not a char boundary", for example on `I’d pick the Dyson…`.
The patch truncates a per-measurement copy of the runs. Upstream Zed fixed the same
bug by making `truncate_line` borrow the runs and return new ones.
