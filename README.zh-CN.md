# ai4ee-pcb

[English](README.md)

[ai4ee-pcb](.) 是 AI 自动设计与生成 PCB 的项目集合，全部项目基于 **KiCad** 设计，收录由 AI Agent（LLM 工具链）自主完成「需求规格 → 器件选型 → 原理图 → 版图布线 → 设计审查 → 制造文件导出」全流程后产出的 PCB 工程。

## 子项目列表

| 子项目 | 简介 | 状态 | 生成模型 / Token |
| --- | --- | --- | --- |
| [ECG12_Portable](ECG12_Portable/README.md) | 便携式 12 导联 ECG 采集板（工程样品）：KiCad 10，45 × 45 mm 四层板，110 个采购器件、14 个测试点、6 个双面定位点，Rev B | 已布线（原生 ERC / DRC 0 违规、0 未连接），待样机验证 | GPT-6 High，约 58 万 tokens |

## 许可证

[Apache License 2.0](LICENSE)

> ⚠️ 各子项目均为工程样品，不代表医疗器械认证。涉及人体接口（如 ECG）的项目，其安全性与合规性以子项目内文档为准，需单独评估与持证验证。
