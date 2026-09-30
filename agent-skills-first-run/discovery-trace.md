# 13/24 技能发现与渐进式披露追踪

1. 发现：扫描 project 和 user 范围的直接子目录；候选为 project/evidence-report、user/evidence-report、user/meeting-brief。此时只读身份元数据，不加载正文。
2. 目录：按本次实验声明的 project > user 策略，project/evidence-report 胜出；目录还发布 meeting-brief。模型可见目录占 461 字符。
3. 激活：选中 project/evidence-report 后，才加载它的 SKILL.md 正文，占 70 字符；其余技能正文未加载。
4. 按分支加载资源：处理报告格式时，读取所选技能的 references/format.md，占 46 字符；没有读取未选技能的参考文件，也没有在目录阶段运行脚本。
