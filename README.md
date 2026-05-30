from pathlib import Path

content = """# Pesticidata

A pesticide compounds dataset containing basic properties, hazard/risks and detection methods for knowledge base direct use and SFT。

说明：本项目中的 pesticide compounds 采用广义口径，涵盖农药、相关化学危害因子、重点污染物和检测对象。数据集面向知识库直接调用、AI Agent/RAG 检索问答和 SFT 使用。

## 中文版

## 1. 项目简介

Pesticidata 是一个面向农药、危害因子和重点化学污染物的结构化数据集。项目来源于周教授团队关于系统整理危害因子、化学污染物基本性质、风险危害、法规信息和检测方法的科研需求。

本项目将公开数据库、法规文件和开放获取文献中的信息整理为稳定 JSON 数据，便于知识库构建、AI Agent 调用、RAG 检索问答和监督微调数据构建。

## 2. 仓库结构

Pesticidata/
├── agent-ready-structural/
│   ├── 0-compounds/
│   │   └── key_pollutants_lite.json
│   ├── 1-regulations/
│   │   └── key_pollutants_regulations_global.json
│   └── 2-detections/
│       ├── example.json
│       └── key_pollutants_detections.json
├── sft-ready/
│   └── key_pollutants_sft.json
├── LICENSE
└── README.md

## 3. 文件说明

"key_pollutants_lite.json"：重点危害因子/污染物基础属性数据，包括化合物名称、CAS 号、分子式、分子量、SMILES、IUPAC 名称、化学物理性质、毒理与危害信息等。该文件为轻量版，SVG 结构式内容已删除或置空，以减小文件体积。

"key_pollutants_regulations_global.json"：法规信息数据，包括监管状态、限值、ADI/ARfD、检测方法和相关法规条款等。数据包含 GB 2763-2026，并整合 PubChem、EU Pesticides Database、US EPA CompTox Dashboard、US eCFR 等公开来源。

"example.json"：检测数据结构示例，建议首次使用者先查看该文件。

"key_pollutants_detections.json"：从 DOI 命名 Markdown 文献中提取的检测数据，包括文献信息、化合物名称、检测方法、检测性能和真实样本信息。

"key_pollutants_sft.json"：面向 SFT 或 RAG 问答的指令数据，由结构化基础属性、法规和检测数据转换而来。

## 4. 数据统计

"compound_record_count": 166
表示基础属性数据中收录了 166 个重点危害因子/污染物/检测对象。

"regulation_record_count": 166
表示法规数据中包含 166 个与基础属性数据对应的化合物记录。

"sft_sample_count": 11459
表示当前构建了 11,459 条不重复的 SFT 样本。后续如有更新，应以实际 JSON 文件为准。

## 5. 检测数据集验证结果

* JSON 格式校验通过
* 扫描 DOI 命名 Markdown：1053 篇
* 无可抽取检测数据并标记 **pass**：433 篇
* 有检测数据：620 篇
* 化合物检测记录：4025 条
* 含 LOD：143 篇
* 含 linear range：6 篇
* 含 IC 指标：14 篇
* 含真实样本信息：540 篇

## 6. 主要字段

"compound_name"：标准化化合物名称。
"cas_number"：CAS 登记号。
"category"：污染物或危害因子类别。
"function"：用途或功能类别。
"group"：化学分组或功能分组。
"properties"：基础识别信息和化学物理性质。
"safety_and_hazards"：危害、毒理和风险信息。
"regulation"：法规和限值信息。
"article_name"：文献标题。
"doi"：文献 DOI。
"compound_detection"：文献中提取的化合物检测信息。
"assay_name"：检测方法名称或方法描述。
"limit_of_detection_lod"：检出限 LOD。
"linear_range"：线性范围。
"sensitivity_ic50"：IC 类指标。
"real_sample_detection"：真实样本检测信息。
"sample_type"：样本或基质类型。
"concentration_confirmed_via_instrumental_method"：与仪器确认或实测浓度相关的信息。

## 7. 特殊值说明

"pass"：表示该字段或该文献中没有可靠提取到对应信息。不代表原文中一定不存在该信息。

"0"：表示检测数据中保留的原始数值为 0。其具体业务含义仍需结合原始文献和字段语境进一步确认。

## 8. 数据来源与处理流程

数据来源包括 PubChem、CAS Common Chemistry、GB 2763-2026、EU Pesticides Database、US EPA CompTox Dashboard、US eCFR，以及 DOI 命名的开放获取文献 Markdown。

处理流程为：

重点危害因子/污染物名单
→ 公开数据库检索
→ 基础属性、危害风险和法规信息整理
→ 开放获取文献 PDF 收集
→ MinerU 解析为 Markdown
→ 脚本 + LLM 辅助字段提取
→ 数据清洗与 JSON 校验
→ Agent-ready 结构化数据
→ SFT-ready 指令数据

## 9. 使用方式

可以使用 Python 直接读取 JSON 文件：

import json
from pathlib import Path

file_path = Path("agent-ready-structural/0-compounds/key_pollutants_lite.json")

with open(file_path, "r", encoding="utf-8") as f:
    data = json.load(f)

print(data.keys())
print(data["metadata"])

## 10. 适用场景

Pesticidata 可用于危害因子和重点污染物知识库构建、法规信息检索、检测方法整理、AI Agent 工具调用、RAG 问答、SFT 指令微调数据构建，以及风险—法规—检测方法的交叉分析。

## 11. 数据质量与局限性

本数据集经过 JSON 格式校验，并对检测数据提取结果进行了统计验证。但检测方法相关数据由 AI 辅助流程从文献 Markdown 中提取，仍可能存在遗漏、误提取或语义偏差。

"pass" 不应被解释为原文中一定不存在相关信息。法规、限值和检测方法信息仅供科研和原型系统开发使用，在用于监管、食品安全判定或其他高风险场景前，应回查官方来源和原始文献。

## 12. 团队与贡献

熊邦锭：
邹茹冰：
范岩亭：
丁惠鹏：
黄鑫雨：

---

