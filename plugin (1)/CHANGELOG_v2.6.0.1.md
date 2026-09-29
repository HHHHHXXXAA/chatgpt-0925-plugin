# CHANGELOG v2.6.0.1

## 类型
Rollback fix

## 基线
- 回退到 v2.6.0 作为主执行基线；
- 保留 v2.6.0 的源素材调用级锁、代言人位置合同、单画布调用与四图独立事务；
- 新增一项默认规则：代言人素材默认缩小并放置于左下角或右下角。

## 新增
- R143：ENDORSER_DEFAULT_CORNER_ANCHOR
- T191：ENDORSER_SMALL_CORNER_REGRESSION

## 说明
本包用于替代当前 2.6.3 的 Business 部署方向，优先恢复你在 2.6.0 上更满意的整体行为。
