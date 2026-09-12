# ind-safety-level-checker

> **状态：RESERVED（占位 · 开放认领）** — 本仓已按平台协议规范建好行业接入四件套骨架，
> 等待具备本域资质的运营方认领并填充真实规则。

把「这条产线风险几级、哪些证照和作业票齐没齐」拆成可核验的属性，让 AI 只做提示，不下安全结论。

## 这个域管什么

工业企业安全生产风险分级与特种设备合规核验

安全生产的风险分级管控、特种设备检验、特种作业持证，任何一环缺失都可能造成人身事故。AI 能做的是把要件与有效期摆清楚，安全条件认定权在企业与监管部门。

## 域标识

| 项 | 值 |
|---|---|
| 域 ID | `ind`（全局唯一，一经分配不复用） |
| 域名称 | 工业 · 安全生产风险分级 |
| Profile 版本 | `domain/1.0` |
| 当前状态 | `RESERVED` |
| 占位时间 | 2026-09-12 |

## 属性清单

| 属性键 | 类型 | 说明 |
|---|---|---|
| `ind.safety_risk_level` | enum | 风险分级（红橙黄蓝四级），须由企业按标准评定并留痕 · 取值 red/orange/yellow/blue/undetermined |
| `ind.safety_production_license` | enum | 安全生产许可当前状态 · 取值 licensed/unlicensed/exempt/pending |
| `ind.special_equipment_status` | enum | 特种设备检验有效期状态 · 取值 inspected_valid/due_inspection/overdue/not_applicable |
| `ind.work_permit` | enum | 动火、受限空间等危险作业票审批状态 · 取值 valid/missing/expired/not_applicable |
| `ind.ai_control_authority` | enum | AI 在生产控制中的权限边界，须显式声明 · 取值 advisory_only/auto_shutdown_enabled/process_control/none |

## 本域红线（不可逾越，机器可读）

1. 风险等级须由企业按标准自行评定并留痕，AI 不得代为定级
2. 不得输出可投产、可开机或符合安全生产条件的放行类表述，安全条件认定由监管部门与专业机构作出
3. AI 不得直接下发停机、工艺参数调整等控制指令，除非具备独立安全确认

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。

## 行业接入四件套

| 文件 | 作用 |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 |
| `checker` | 本域核验器（MCP 工具，**只出示核验，不下判定**） |

## 核心原则

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**隐私是准入条件，不是整改事项。** 缺失 `privacy.json` 或任一必填字段不符，
符合性校验直接失败——不是警告，是拒绝接入。

## 参考依据

- 中华人民共和国安全生产法
- GB/T 33000 企业安全生产标准化基本规范
- 特种设备安全监察条例

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。

## 认领方式

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.
