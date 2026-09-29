---
name: history-inheritance
description: Load and enforce the complete 0925 training history migration, including the v2.6.0 stable baseline, error cases, change process, and regression gates.
---

# 0925 complete history inheritance

Before any 0925 job, load `skills/main-image-workflow/references/history-inheritance-migration.md` and all seven files in `docs/training-inheritance/history-migration/`. Treat them as executable inheritance assets, not background notes.

## Non-regression gates

- Start with the fixed five-framework choice; do not guess a category.
- Wait for the current job's Excel before parsing. The current Excel is the only fact source; historical workbooks and prior attachments are excluded.
- Read square-region images, screenshots, text boxes, shapes, DISPIMG/cell images, and rich-text emphasis. A square screenshot is a structural component and must be mapped into the result.
- Freeze `PRODUCT_SOURCE_MANIFEST`, `LOGO_SOURCE_MANIFEST`, `ENDORSER_SOURCE_MANIFEST`, `SCREENSHOT_STRUCTURE_MAP`, `COPY_LEDGER`, and `BOTTOM_CONTRACT` before planning or generation.
- Bind the actual current Excel product, Logo, and endorser source images for each A/B/C/D call. Never substitute a generated or similar product, Logo, or person.
- Keep the v2.6.0 bottom contract: continuous bottom bar, unchanged grouping/order, and primary sentence visibly stronger than supporting text.
- Use only COPY_LEDGER text. No filler, invented claims, prices, certifications, or identity statements.
- Generate A/B/C/D as four independent single-image transactions. Reject collage, grid, contact sheet, split, or multi-panel output before display.
- Keep the endorser small in the left-bottom or right-bottom corner unless a higher-priority Excel geometry or annotation contract says otherwise; freeze the chosen anchor across variants.

Any failed gate is `BLOCKED_*` or `REJECTED_QA`; never silently degrade or deliver a plausible substitute.

## v2.6.0.4 回归增量

- 四张图必须通过 `VARIANT_DIFFERENTIATION_GATE`：构图骨架、主体占比、场景/背景、光线、材质、留白、动线和信息模块不得模板化重复。
- 底部文案的字面内容、行序、分组、层级和几何排版冻结；仅允许文字框外观和配色随画面自适应。
- 代言人必须绑定当前 Excel 原图，原貌、原姿势、原朝向和手持关系不变，缩小固定左下或右下并跨 A/B/C/D 保持同一锚点。
- 任一差异化、底部合同或代言人门禁失败均为 `REJECTED_QA`，不得交付相似模板或替代人物。

