---
name: main-image-workflow
description: 运行0925 v2.6.0.2跨境电商方图流程；以更稳定的2.6.0为基线，并整合此前训练过程与本次对话新增规则：固定五框架启动门禁、未上传当前Excel不得解析、生成前冻结六份事实清单、产品/Logo/代言人/截图/底部合同强锁、代言人默认缩小并固定左下角或右下角、A/B/C/D四次独立单图与Business对齐回归。不增加用户操作。
---

# 0925 v2.6.0.2 主工作流

## 最高优先级加载顺序
开始任何任务前必须完整读取：
1. `references/v2.6.0.2_master_training_snapshot.md`（最高优先级）；
2. `references/v2.6.0.2_corrections_R144_R154.md`；
3. `references/v2.6.0.2_regression_T192_T206.md`；
4. `references/v2.6.0.1_endorser-corner-default.md`；
5. `references/v2.6.0_execution-asset-handoff-and-single-canvas.md`；
6. `references/v2.6.0_corrections_R135_R142.md`；
7. `references/v2.6.0_regression_T181_T190.md`；
4. `references/v2.5.9_asset-fidelity-screenshot-quality.md`；
5. `references/v2.5.9_corrections_R127_R134.md`；
6. `references/v2.5.9_regression_T171_T180.md`；
7. `references/v2.5.7_four-image-default-and-no-collage.md`；
8. v2.5.5/v2.5.4/v2.5.3/v2.5.2/v2.5.1全部既有有效契约、核心指令、纠错清单、回归清单、类目提示词、Excel标注规范和填写清单。

冲突优先级：用户当前明确要求 ＞ 当前Excel方图正式内容及其明确框外标注 ＞ v2.6.0.2 完整训练继承总文件与Business对齐快照 ＞ v2.6.0.1代言人默认角落小图修复 ＞ v2.6.0执行硬门禁 ＞ v2.5.9事实/质量快照 ＞ v2.5.7默认四图快照 ＞ 其他既有不可回退规则 ＞ 类目建议 ＞ 自由美化。

## 用户操作流程不增加
仍保持原五步：选类目 → 上传当前Excel/附件 → 自动解析并等待“确认出方案” → 确认后输出A/B/C/D方案并等待“开始生成” → 用户只说一次“开始生成”，系统自动完成A→B→C→D四次独立单图事务。不得要求用户额外确认素材、逐图继续、重复上传或逐图授权。

## v2.6.0 生成调用级源素材交接门禁

### 1. 真实源素材必须绑定到每一次单图事务
- `SOURCE_ASSET_HANDOFF_GATE=ACTIVE`。
- 每张A/B/C/D在真正调用图像生成/编辑能力前，必须建立当前JOB的 `GENERATION_SOURCE_BINDING`，至少包含：
  - `PRODUCT_SOURCE_REF`：当前Excel明确指定的产品/外包装源图；
  - `LOGO_SOURCE_REF`：当前Excel明确指定的Logo原图；
  - `ENDORSER_SOURCE_REF`：当前Excel/附件明确指定的代言人/人物原图（如该工单存在）；
  - `ENDORSER_POSITION_CONTRACT`：代言人位置、侧别、锚点、相对画布坐标/区域、相对尺寸、朝向与允许变化；
  - 当前JOB的 `COPY_LEDGER`、`SCREENSHOT_STRUCTURE_CONTRACT`、`BOTTOM_CONTRACT`。
- 以上源图必须作为真实视觉源对象参与当前事务；**禁止只把产品、Logo、代言人转写成文字描述后交给图像模型重新生成**。
- `ACTUAL_SOURCE_IMAGE_REF_REQUIRED=ACTIVE`、`NO_TEXTUAL_PROTECTED_ASSET_SUBSTITUTION=ACTIVE`。
- 任何必需源素材引用缺失、失效、未绑定或无法直接参与当前图像事务时，必须 `BLOCKED_ASSET_HANDOFF` 或 `BLOCKED_COMPOSITOR`，不得继续生成完整广告图。

### 2. 产品/包装/Logo：只允许源像素身份，不允许生成式重建
- `PROTECTED_ASSET_RENDERING_MODE=COMPOSITE_OR_EDIT_SOURCE_ONLY`。
- 产品/包装/标签/规格/颜色/图案/比例/数量/内部像素以及Logo图形、文字、颜色、比例、间距均不得由模型重新绘制。
- 当前执行路径如果不能保证源素材直接参与并最大限度保持其身份和外观，则禁止把“相似生成产品/Logo”作为回退方案。
- 允许的仅是既有规则允许的抠图、去背景、等比例缩放、整体移动、明确授权的轻旋、曝光/对比度匹配、接触阴影/反射和自然融合。
- 每张完成后必须执行 `PRODUCT_PIXEL_IDENTITY_CHECK` + `LOGO_IDENTITY_CHECK`；任一不一致立即 `REJECTED_QA`。

### 3. 代言人身份与位置双锁
- `ENDORSER_IDENTITY_AND_POSITION_ABSOLUTE_LOCK=ACTIVE`。
- 身份层继续锁定脸、五官、发型、肤色、妆容、服装、配饰、姿势、手势、朝向、身体比例、手持关系、签名。
- 位置层新增 `ENDORSER_POSITION_CONTRACT`：
  1) Excel对象本身有明确位置/尺寸时，以其标准化坐标和尺寸为最高优先级；
  2) 方图截图/结构锚点明确人物位置时，以截图结构位置为准；
  3) 方图框外有明确位置修改说明时，仅按该说明修正；
  4) 若没有任何明确位置，首次解析时按既有规则选定左下或右下并冻结该选择，A/B/C/D不得每张重新选择。
- 一旦合同冻结，A/B/C/D不得擅自换边、换角、镜像、上下漂移、显著改变相对尺寸或遮挡关系。
- `ENDORSER_POSITION_CHECK` 在每张交付前必须PASS；失败只重做当前图。
- 若 Excel / 截图 / 框外标注没有给出更高优先级的人物尺寸与位置要求，则启用默认规则：代言人缩小为辅助元素，只允许左下角或右下角，不得放大居中，不得遮挡主体产品、标题、Logo、价格区和底部主句。
- `ENDORSER_DEFAULT_CORNER_ANCHOR_ACTIVE=ACTIVE`、`ENDORSER_SMALL_SCALE_DEFAULT_ACTIVE=ACTIVE`。

### 4. 一次调用只能一个方案、一个画布、一个结果
- `SINGLE_CANVAS_CALL_CONTRACT=ACTIVE`。
- 每次A/B/C/D调用前必须设置唯一 `ACTIVE_VARIANT`，其值只能是A或B或C或D之一。
- 当前单图调用的设计指令只允许包含当前 `ACTIVE_VARIANT` 的艺术方向；其他三个方案的完整设计描述必须从当前调用上下文排除。
- 当前调用中禁止出现会诱发多方案画布的集合指令，如“生成四张/四个方案/四宫格/四种设计并列/多方案预览/联系表/拼接展示”。
- `CALL_PAYLOAD_AUDIT` 必须满足：`ACTIVE_VARIANT_COUNT=1`、`FULL_AD_CONCEPT_COUNT=1`、`CANVAS_COUNT=1`、`OUTPUT_TARGET_COUNT=1`、`OTHER_VARIANTS_EXCLUDED=PASS`。
- 任一不满足，不得调用图像工具。

### 5. 拼接零容忍前移到展示之前
- `PRE_DISPLAY_CANVAS_MULTIPLICITY_GATE=ACTIVE`。
- 图像返回后、展示给用户之前先检查：四宫格、二宫格、分屏、拼接、联系表、多面板、多画布、多个完整广告方案并列、一个画面中存在多个完整主图版式，全部 `REJECTED_QA`。
- 拼接结果不得展示、不得裁切、不得拆分冒充独立图、不得计入 `FOUR_OUTPUT_COUNTER`；只重做当前ACTIVE_VARIANT。

## v2.5.9及以前全部事实/质量规则继续生效
- 产品与Logo绝对源素材锁、文案绝对正确锁、代言人身份姿势锁、截图结构与框外标注锁、底部合同固定皮肤可变、四图最大安全差异、高端商业质量门槛全部继续生效。
- 四张仍须事实正确100%、文案正确100%、风格差异显著、全部高端大气精致和谐自然、低AI感。
- 差异化只能发生在非事实视觉层，不能改变产品/Logo/代言人/文案/截图事实/底部合同。

## 默认四图继续生效
- 默认 `DEFAULT_OUTPUT_MODE=FOUR_VARIANTS_REQUIRED`，用户未明确要求单图时严格A→B→C→D四次独立 `SINGLE_IMAGE_TRANSACTION`。
- `FOUR_OUTPUT_COUNTER != 4` 不得交付；1/2/3张不得声称完成。
- 四张必须是四个独立1:1结果，严禁拼接。

## 失败处理：宁可阻塞，不得错误交付
- 若真实源素材无法绑定到当前图像调用，返回 `BLOCKED_ASSET_HANDOFF`/`BLOCKED_COMPOSITOR`；不得生成相似产品、相似Logo、相似代言人兜底。
- 若A/B/C/D某一张失败，仅重做当前图，已通过图保持锁定。
- 任何受保护素材错误、人物位置错误或拼接结果均不得交付。

## 不可回退
v2.5.9及以前有效规则全部继续生效。本版本在 2.6.0 基线上新增 R143、T191（代言人默认角落小图修复），并保留 R135–R142、T181–T190。后续版本不得删除或弱化“真实源素材绑定、代言人位置合同、单方案单画布调用、展示前拼接阻断”四项执行硬门禁。

允许状态：`READY_FOR_PLAN`、`READY_FOR_COMPOSITE`、`WAITING_IMAGE_CAPACITY`、`DELIVERABLE`、`BLOCKED_PARSE`、`BLOCKED_ASSET`、`BLOCKED_ASSET_HANDOFF`、`BLOCKED_COMPOSITOR`、`REJECTED_QA`。禁止静默降级。


## v2.6.0.2 训练继承补充
- 启动阶段必须先展示固定五框架，不得自动猜类目。
- 未上传“当前这次”的Excel前，不得使用历史Excel或历史附件进入解析。
- 解析完成后必须冻结六份事实清单：PRODUCT_SOURCE_MANIFEST、LOGO_SOURCE_MANIFEST、ENDORSER_SOURCE_MANIFEST、SCREENSHOT_STRUCTURE_MAP、COPY_LEDGER、BOTTOM_CONTRACT。
- 仅允许调用当前Excel冻结的正式文案；禁止新增 filler copy。
- 方图截图必须进入 SCREENSHOT_STRUCTURE_MAP 并映射到成图。
- 底部一体化连续底栏与主次层级必须保持。
- 四图必须是四个独立文件；禁止四宫格、拼贴、联系表。
- 第2-4张在无明确小展示要求时，产品主体应更大；若无授权展示台，默认不新增展示台。
- Business 与 Plus 使用相同回归测试 T206；重大偏差一律 REJECTED_QA。

## v2.6.0.4 四图差异化与代言人原貌增量锁

### A/B/C/D 强差异化矩阵
- `FOUR_VARIANT_DIFFERENTIATION_GATE=ACTIVE`。A、B、C、D 仍是四次独立单图事务，每张必须是完整可交付画面。
- 生成前冻结 `VARIANT_DIFFERENTIATION_MATRIX`：
  - A：参考忠实/信息清晰，采用稳定主视觉与规整留白；
  - B：高端商业/非对称构图，采用明显不同的主体比例、动线、景别与材质；
  - C：专业营养/模块化信息结构，采用不同的网格、光线、景深与图形层级；
  - D：柔和生活方式/情绪场景，采用不同的场景、视角、色彩气质与留白关系。
- 四张图至少在构图、主体占比、视角/景别、背景场景、光线、材质、留白位置、视觉动线、信息模块形态、主色配色十个维度中有八个维度形成可见差异；不得只换背景色或轻微挪动元素。
- 差异只允许发生在非事实视觉层。Excel 产品、Logo、截图结构、代言人、签名、COPY_LEDGER 文案和 BOTTOM_CONTRACT 的内容/顺序/分组/层级不可漂移。
- 每张调用上下文只允许出现当前 `ACTIVE_VARIANT` 的单一艺术方向；生成后执行 `VARIANT_DIFFERENCE_CHECK`，若两张排版骨架近似则只重做当前图并标记 `REJECTED_QA_LAYOUT_COLLISION`。

### 底部文案内容与排版合同
- `BOTTOM_COPY_AND_LAYOUT_LOCK=ACTIVE`。底部文案必须逐字来自当前 Excel 的 `COPY_LEDGER`，文字内容、行序、左右分组、上下层级、对齐关系、底边连续贴合和 0px 边界保持不变。
- 允许随每张画面调整文字框的外形、圆角/切角、描边、透明度、阴影和配色，使其与整体画面协调；不得改变文字、行数、分组、相对位置、主次关系或造成底栏断裂。
- `BOTTOM_BOX_STYLE_ADAPTIVE=ACTIVE` 只改变容器视觉皮肤，不改变合同几何；主句必须始终强于辅助句且保持可读。

### 代言人原貌、原姿势与角落锚定
- `ENDORSER_SOURCE_PIXEL_LOCK=ACTIVE`、`ENDORSER_POSE_EXACT_LOCK=ACTIVE`。代言人必须直接使用当前 Excel 原图，脸、五官、发型、肤色、妆容、服装、配饰、姿势、手势、朝向、身体比例、手持关系、表情和原始透明边界均保持不变；禁止重绘、换脸、换衣、改姿势、镜像、明显裁切、局部补绘或生成相似人物。
- 在 Excel/截图未规定更高优先级几何时，代言人作为辅助元素缩小放置在左下角或右下角，默认占方图高度约 8%–18%，不遮挡产品、Logo、标题、价格区或底部主句；首次选定一侧后 A/B/C/D 全部冻结同一侧、同一朝向和相对锚点。
- 只允许等比例缩放、整体移动和自然融合；不允许因风格变化放大居中、换边、漂移或改变人物与手持物的相对关系。
- 每张交付前执行 `ENDORSER_PIXEL_IDENTITY_CHECK`、`ENDORSER_POSE_CHECK`、`ENDORSER_CORNER_ANCHOR_CHECK`；任一失败即 `REJECTED_QA_ENDORSER`，只重做当前图。

