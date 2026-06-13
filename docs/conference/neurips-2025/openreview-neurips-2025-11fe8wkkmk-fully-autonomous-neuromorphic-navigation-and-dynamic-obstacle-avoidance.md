---
title: Fully Autonomous Neuromorphic Navigation and Dynamic Obstacle Avoidance
title_zh: 全自主神经形态导航与动态避障
authors: "Xiaochen Shang, Luo Pengwei, Xinning Wang, Jiayue Zhao, Huilin Ge, Bo Dong, Xin Yang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=11fe8wKkmk"
tags: ["query:uav"]
score: 4.0
evidence: 针对无人机机载计算的能量约束
tldr: 针对无人机机载计算和感知在实时导航与避障中面临的高延迟和严苛能量约束，本文提出了一种全神经形态框架，实现端到端避障，总延迟仅2.3毫秒。该方法受生物系统效率启发，无需目标识别或轨迹计算即可精确检测和躲避动态障碍，并引入首个单目...。实验表明，该框架显著降低了机载功耗，为小型无人机在复杂环境中的自主飞行提供了高效且低能耗的解决方案。
source: NeurIPS-2025-Accepted
selection_source: conference_retrieval
figures_json: "[{\"url\": \"assets/figures/openreview/openreview-neurips-2025-11fe8wkkmk/fig-001.webp\", \"caption\": \"\", \"page\": 4, \"index\": 1, \"width\": 1965, \"height\": 1097}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-11fe8wkkmk/fig-002.webp\", \"caption\": \"\", \"page\": 4, \"index\": 2, \"width\": 2037, \"height\": 664}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-11fe8wkkmk/fig-003.webp\", \"caption\": \"\", \"page\": 5, \"index\": 3, \"width\": 3383, \"height\": 2020}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-11fe8wkkmk/fig-004.webp\", \"caption\": \"\", \"page\": 6, \"index\": 4, \"width\": 500, \"height\": 793}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-11fe8wkkmk/fig-005.webp\", \"caption\": \"\", \"page\": 6, \"index\": 5, \"width\": 501, \"height\": 793}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-11fe8wkkmk/fig-006.webp\", \"caption\": \"\", \"page\": 6, \"index\": 6, \"width\": 500, \"height\": 793}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-11fe8wkkmk/fig-007.webp\", \"caption\": \"\", \"page\": 6, \"index\": 7, \"width\": 500, \"height\": 793}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-11fe8wkkmk/fig-008.webp\", \"caption\": \"\", \"page\": 6, \"index\": 8, \"width\": 522, \"height\": 859}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-11fe8wkkmk/fig-009.webp\", \"caption\": \"\", \"page\": 6, \"index\": 9, \"width\": 473, \"height\": 860}, {\"url\": \"assets/figures/openreview/openreview-neurips-2025-11fe8wkkmk/fig-010.webp\", \"caption\": \"\", \"page\": 8, \"index\": 10, \"width\": 1280, \"height\": 720}]"
motivation: 无人机在机载计算和感知下实现实时导航与动态避障受限于延迟和能量约束。
method: 提出全神经形态框架，利用生物启发方法实现端到端避障，延迟仅2.3毫秒。
result: 框架实现了无需目标识别或轨迹计算的移动物体检测与避障，降低了机载功耗。
conclusion: 该工作为无人机低功耗自主避障提供了高效解决方案，推动神经形态计算在机器人感知与控制中的应用。
---

## Abstract
Unmanned aerial vehicles could accurately accomplish complex navigation and obstacle avoidance tasks under external control. However, enabling unmanned aerial vehicles (UAVs) to rely solely on onboard computation and sensing for real-time navigation and dynamic obstacle avoidance remains a significant challenge due to stringent latency and energy constraints. Inspired by the efficiency of biological systems, we propose a fully neuromorphic framework achieving end-to-end obstacle avoidance during navigation with an overall latency of just 2.3 milliseconds. Specifically, our bio-inspired approach enables accurate moving object detection and avoidance without requiring target recognition or trajectory computation. Additionally, we introduce the first monocular event-based pose correction dataset with over 50,000 paired and labeled event streams. We validate our system on an autonomous quadrotor using only onboard resources, demonstrating reliable navigation and avoidance of diverse obstacles moving at speeds up to 10 m/s.

---

## 论文详细总结（自动生成）

### 1. 研究动机与核心问题
*   **核心挑战**: 小型无人机仅依靠机载传感器与计算资源实现实时导航和动态避障，面临极严苛的**延迟**与**能耗**约束。传统视觉方法（SLAM、目标识别+轨迹估计）内存和算力消耗巨大（数百MB～GB级内存、数百GFLOPS），难以在微型平台上部署。
*   **研究动机**: 受生物系统（如青蛙视觉对运动物体的高效检测与静态背景抑制）启发，神经形态传感器的异步稀疏事件流天然具备高速、低功耗的潜力。然而，现有方法多将事件流处理成“事件帧”，未能充分利用其稀疏特性，性能接近传统方法。
*   **整体含义**: 本文提出一个**全神经形态框架**，旨在仅凭一颗单目事件相机和IMU，在机载神经形态芯片上同步完成长距离导航与端到端动态避障，将避障延迟压缩至毫秒级，能耗降至传统架构的21%。

### 2. 方法论
整体系统部署在Speck神经形态SoC（32.7万神经元）上，由**导航模块**和**避障模块**协同工作，共享同一事件相机。

*   **导航：基于Event Visual‑Homimg的IMU漂移校正**
    *   **核心思想**: 利用昆虫视觉归巢（visual‑homing）原理：向外飞行时在固定**快照点**记录事件流；返回时依靠IMU航位推算飞行，并定期飞回快照点附近，用事件脉冲神经网络（SCNN）估计相对位姿，校正IMU累积漂移。
    *   **关键技术**:
        *   **误差模型**: 定量推导IMU位置误差随时间呈三次方级累积，据此设定快照间隔（7.5米），确保漂移控制在0.6米半径的捕捉区内。
        *   **CalibNet (Siamese SCNN)**: 输入两段50ms事件流（向外快照、返回实时），特征提取后拼接，输出相对位姿（Δx, Δy, 偏航角）。采用循环校正直至误差低于阈值。
    *   **数据集**: 构建首个**单目事件位姿校正数据集**（50,234对标注事件流），覆盖4种室内场景，通过运动捕捉系统获取4自由度真值。

*   **动态避障：仿蛙眼Event RF模型与势场法**
    *   **核心思想**: 模仿蛙眼视网膜神经节细胞的**ERF（兴奋性感受野）**和**IRF（抑制性感受野）**空间不对称性，直接处理事件流，抑制静态背景事件，增强动态障碍物事件，无需目标识别或轨迹预测。
    *   **Event RF模型细节**:
        *   **能量场**：由ERF能量（带时间衰减核、高斯空间核）与IRF抑制能量（含延时Δt、独立衰减τi）做差得到。静态物体事件被IRF快速抵消；动态物体因位移持续引入新ERF，能量维持高位。
        *   **参数选择**：通过理论推导与实验，确定最优参数集：A₁/A₂ = 1.22，τₑ=5ms，τᵢ=25ms，Ith/Eth=1.2，σₓ=σᵧ=2，Δt=5ms。
    *   **运动指令生成**：将能量图转化为二维势场，利用连通域检测评估障碍物危险程度，对危险区域做形态学膨胀，再以梯度下降法生成避障机动（方向与强度）。

*   **处理流程**
    1. 事件流输入 → 2. 计算ERF与IRF能量衰减 → 3. 生成动态能量图 → 4. 势场转换+连通域检测 → 5. 梯度下降输出控制指令。总延迟仅**2.3毫秒**。

### 3. 实验设计
*   **数据集与场景**
    *   **仿真**：Gazebo + ESIM事件相机模拟器，三张定制地图（40m～130m），障碍物按尺寸（硬币/网球/篮球）和距离（0.2m至>2m）细分。
    *   **真实环境**：室内飞行大厅（10m×10m）、室外广场（含静态箱体）、办公室走廊；测试抛掷物、稀疏/密集人群；极端光照（亮/闪烁/暗/全黑）。
*   **对比基准**
    *   方法1 (RGB‑D目标检测+轨迹优化，19.1ms)
    *   方法2 (RGB‑D跟踪，39.5ms)
    *   方法3 (事件相机直接避障但无导航，3.56ms)
    *   方法4 (LiDAR‑based，机载27ms)
    *   *注：方法1‑3均依赖非神经形态硬件，方法4使用非视觉传感器。*
*   **评估指标**
    *   避障延迟、质心位置误差、避障成功率（SR）、能耗。

### 4. 资源与算力
*   **机载平台**：Speck神经形态SoC（32.7万神经元），功耗极低（文中称比传统器件低两个数量级，执行相同算法能耗降至5%，系统总能耗降至21%）。
*   **训练计算**：**文中未明确说明**训练CalibNet或调整Event RF模型所用的GPU型号、数量或训练时长。仅提到SCNN网络结构，但未提供训练所需的算力细节。

### 5. 实验数量与充分性
*   **实验数量丰富**：
    *   导航：3地图×10次 = 30次独立飞行测试。
    *   动态避障：300次仿真（3尺寸×4距离×25次），并测试了低速/高速/超高速。
    *   组合任务：3地图×50次 = 150次。
    *   真实环境：室内10次，室外/走廊多种障碍物各若干次，光照鲁棒性多条件。
    *   消融/参数分析：详细分析Event RF模型关键参数（A₁/A₂, τ, Δt等）对性能的影响。
*   **充分性与公平性**：实验覆盖仿真到真实、简单到复杂环境、不同障碍物速度与光照，对比方法均用原代码在相同仿真环境中复现，评估公平。但缺乏与其他神经形态导航方法（如[33]）的直接对比。

### 6. 主要结论与发现
*   提出的全神经形态架构可在**仅2.3ms**延迟下实现端到端动态避障，远低于现有方法（平均减少88.6%），同时保持**94.5%**的避障成功率。
*   系统能在导航过程中可靠躲避速度高达**10m/s**的障碍物，能耗仅为传统架构的**21%**。
*   事件相机天然对光照变化鲁棒，系统在亮/暗/闪烁环境下性能稳定（全黑除外）。
*   仿生Event RF模型无需目标识别或轨迹估计，即能准确提取运动物体并生成避障指令，质心定位误差仅0.02m。

### 7. 优点
*   **极低延迟**：全神经形态处理链将避障延迟压缩至2.3ms，为高速障碍物留出更长的反应窗口。
*   **生物启发性**：巧妙借箭青蛙视觉通路的ERF/IRF机制，实现简单高效的静态‑动态事件分离，极具创新性。
*   **系统完整性**：首次将视觉归瘾导航与神经形态避障统一在单个低功耗SoC上，完整演示了从感知到控制的机载闭环。
*   **开源贡献**：发布了首个大规模单目事件位姿校正数据集（5万对），填补了领域空白。
*   **真实环境验证**：在真实四旋翼上进行了复杂环境、多速度和极端光照实验，可信度高。

### 8. 不足与局限
*   **深度信息缺失**：单目事件相机无法直接获取距离，避障依赖势场区域占画幅比例判断危险程度，二维控制可能无法完美处理三维空间中的规避。
*   **导航依赖性**：依赖IMU航位推算，在极端长航时或高频振动下漂移可能超出校正能力；整个导航假设返航时环境静态、无动态遮挡。
*   **黑暗失效**：事件相机需要光强变化才能工作，全黑环境下系统无法运行。
*   **人群密集度限制**：在密集人群（成功率仅54.8%‑62.1%）等极度杂乱场景中，势场法可能因障碍物占满视野而失效。
*   **训练算力不透明**：未说明训练SCNN网络所需的计算资源，难以评估复现成本。
*   **应用局限**：目前仅演示了返回已走过路线的导航，未涉及完全未知环境中的建图与自主探索。

（完）
