# 0925 v2.6.0

针对实测再次出现的四图拼接、代言人替换/位置漂移、产品/包装替换、Logo替换进行执行层修复。

新增：
- SOURCE_ASSET_HANDOFF_GATE：每个A/B/C/D调用前绑定真实产品/Logo/代言人源图；
- NO_TEXTUAL_PROTECTED_ASSET_SUBSTITUTION：文字描述不能替代真实源素材；
- ENDORSER_POSITION_CONTRACT：冻结代言人侧别、区域、相对坐标和尺寸；
- SINGLE_CANVAS_CALL_CONTRACT + CALL_PAYLOAD_AUDIT：一次调用一个方案、一个画布、一个输出；
- PRE_DISPLAY_CANVAS_MULTIPLICITY_GATE：拼接结果展示前拒绝；
- R135–R142、T181–T190。

不增加用户操作步骤；v2.5.9及以前全部有效规则继续生效。
