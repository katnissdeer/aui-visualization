# 学术论文有用性多维测度框架（AUI）可视化展示系统

## 产品需求文档（PRD）

---

## 1. 问题陈述（Problem Statement）

传统的学术论文评价体系过度依赖"唯数量论"的引文指标（如影响因子、被引频次），这种方法无法区分"实质性知识采纳"与"表面提及"，导致评价结果与论文真实学术价值存在偏差。学术研究者、政策制定者需要一个能够多维度、可交互地展示论文"有用性"的可视化工具，以支持更科学的学术评价决策。

本项目旨在构建一个基于 **AUI（Academic Usefulness Index）框架** 的交互式数据可视化看板，核心价值在于：

- **直观对比**：可视化展示诺贝尔奖获奖论文与对照组论文在"形式"、"结构"、"语义"三个维度的得分差异
- **深度洞察**：支持知识网络拓扑分析和语义渗透度挖掘
- **教育普及**：帮助非技术背景的用户理解"学术有用性"的多元评价方法

---

## 2. 解决方案（Solution）

构建一个基于 Web 的交互式数据可视化看板，采用 **Vue 3 + ECharts** 技术栈，以静态网站形式部署。

### 核心特性

| 模块 | 可视化类型 | 核心功能 |
|------|-----------|---------|
| **AUI Dashboard** | 雷达图、南丁格尔玫瑰图、堆叠柱状图 | 多维指标对比，权重分解 |
| **Structure Explorer** | 力导向图 | 知识网络拓扑，二阶引用关系 |
| **Semantic Text Viewer** | 文本高亮组件 | 知识渗透度对比，实质复用识别 |

### 数据流架构

```
┌─────────────────────────────────────────────────────────┐
│                      后端服务                            │
│  (计算完成：网络嵌入向量、SciBERT得分、AUI合成指数)       │
└─────────────────────────┬───────────────────────────────┘
                          │ 预计算 JSON 数据
                          ▼
┌─────────────────────────────────────────────────────────┐
│                    前端静态站点                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐       │
│  │ AUI Dashboard│ │Structure    │ │ Semantic    │       │
│  │ (雷达/玫瑰图) │ │ Explorer    │ │ Text Viewer │       │
│  └─────────────┘  │ (力导向图)  │  └─────────────┘       │
│                   └─────────────┘                        │
└─────────────────────────────────────────────────────────┘
```

---

## 3. 用户故事（User Stories）

### 3.1 学术研究者

1. 作为学术研究者，我希望查看诺奖论文与对照组论文的 AUI 综合得分对比，以便快速评估论文的"有用性"差异
2. 作为学术研究者，我希望通过雷达图直观看到论文在"形式"、"结构"、"语义"三个维度的得分分布，识别其优势领域
3. 作为学术研究者，我希望使用南丁格尔玫瑰图查看不同维度下的细分指标（如被引频次、时序分布、跨学科性），发现隐藏模式
4. 作为学术研究者，我希望通过堆叠柱状图查看 AUI 得分的权重构成，理解为何诺奖论文获得高分
5. 作为学术研究者，我希望点击图表中的特定数据点，查看该指标的定义和计算方法

### 3.2 知识网络探索者

6. 作为知识网络探索者，我希望通过力导向图查看核心论文在引用网络中的拓扑位置
7. 作为知识网络探索者，我希望通过节点大小识别高中心性论文，通过颜色区分中介中心性
8. 作为知识网络探索者，我希望缩放和平移知识网络图，探索不同层级的引用关系
9. 作为知识网络探索者，我希望悬停在某个节点上，查看该论文的详细信息（AUI得分、标题、作者、年份）
10. 作为知识网络探索者，我希望点击节点，展开其二级引用网络，了解知识的传播路径

### 3.3 语义分析爱好者

11. 作为语义分析爱好者，我希望查看施引文献与被引论文之间的"知识渗透度"量化指标
12. 作为语义分析爱好者，我希望悬停在引文上，高亮显示被保留的核心上下文（如共被引关系、作者自述）
13. 作为语义分析爱好者，我希望系统能够清晰区分"实质复用类"引用与"一般提及"，并用不同颜色标记
14. 作为语义分析爱好者，我希望对比诺奖论文与对照组论文的语义复用深度，直观理解高有用性论文的特征
15. 作为语义分析爱好者，我希望点击任意引文，查看其在原文中的完整上下文

### 3.4 系统交互

16. 作为看板使用者，我希望在页面顶部切换不同的论文对比组，快速切换分析对象
17. 作为看板使用者，我希望使用筛选器按年份范围、学科领域过滤数据
18. 作为看板使用者，我希望将当前视图导出为 PNG 图片，用于报告或演示
19. 作为看板使用者，我希望全屏查看某个图表，获得更佳的沉浸式体验
20. 作为看板使用者，我希望在移动设备上也能正常浏览和使用看板（响应式设计）

---

## 4. 实现决策（Implementation Decisions）

### 4.1 技术栈选型

| 层级 | 技术选型 | 理由 |
|------|---------|------|
| **框架** | Vue 3 (Composition API) | 组合式 API 便于逻辑复用，ECharts 官方 Vue 封装成熟 |
| **图表库** | ECharts 5.x | 雷达图、力导向图、堆叠柱状图支持完善，性能优异 |
| **状态管理** | Pinia | 轻量级，适合中等复杂度看板项目 |
| **路由** | Vue Router 4 | 多页面 SPA 架构 |
| **构建工具** | Vite | 开发体验好，构建速度快 |
| **样式** | Tailwind CSS (可选) | 快速搭建布局，也可使用 SCSS |
| **部署** | 静态网站 (GitHub Pages / Vercel) | 无服务端，按需加载 |

### 4.2 模块划分

```
src/
├── components/
│   ├── charts/                    # 图表组件
│   │   ├── RadarChart.vue         # 雷达图组件
│   │   ├── RoseChart.vue          # 南丁格尔玫瑰图
│   │   ├── StackedBarChart.vue    # 堆叠柱状图
│   │   └── ForceGraph.vue         # 力导向图
│   ├── dashboard/                 # 看板模块
│   │   ├── AUIDashboard.vue       # 主仪表板
│   │   ├── DimensionCard.vue      # 维度卡片
│   │   └── ScoreComparison.vue    # 得分对比
│   ├── network/                   # 网络图模块
│   │   ├── NetworkExplorer.vue    # 网络探索器
│   │   ├── NodeInfoPanel.vue      # 节点信息面板
│   │   └── NetworkControls.vue    # 缩放/筛选控件
│   ├── semantic/                  # 语义模块
│   │   ├── SemanticViewer.vue     # 语义查看器
│   │   ├── CitationHighlighter.vue # 引文高亮
│   │   └── KnowledgePenetration.vue # 知识渗透度
│   └── common/                    # 通用组件
│       ├── Header.vue             # 顶部导航
│       ├── Sidebar.vue            # 侧边栏
│       ├── ExportButton.vue       # 导出按钮
│       └── TooltipInfo.vue        # 悬浮提示
├── composables/                   # 组合式函数
│   ├── useChartData.ts           # 图表数据处理
│   ├── useNetworkData.ts          # 网络图数据处理
│   └── useSemanticData.ts         # 语义数据处理
├── stores/                        # Pinia 状态管理
│   ├── paperStore.ts              # 论文数据状态
│   ├── comparisonStore.ts        # 对比组状态
│   └── uiStore.ts                 # UI状态（筛选、主题）
├── types/                         # TypeScript 类型定义
│   ├── index.ts                   # 核心类型
│   ├── paper.ts                   # 论文相关类型
│   └── chart.ts                   # 图表数据类型
├── utils/                         # 工具函数
│   ├── dataParser.ts              # JSON 数据解析
│   └── formatters.ts              # 数值格式化
├── views/                         # 页面视图
│   ├── HomeView.vue               # 首页/仪表板
│   ├── NetworkView.vue            # 网络图页
│   └── SemanticView.vue           # 语义分析页
├── App.vue
└── main.ts
```

### 4.3 核心 JSON 数据结构接口

```typescript
// ======================
// 1. 论文基础数据类型
// ======================
interface Paper {
  id: string;
  title: string;
  authors: string[];
  year: number;
  journal: string;
  doi?: string;
  isNobel: boolean;              // 是否为诺奖论文
  auiScore: number;              // AUI 综合得分 (0-100)
  
  // 形式维度 (Form)
  form: {
    citationCount: number;       // 被引频次
    temporalDistribution: {      // 时序分布
      recentCitationRatio: number; // 近5年引用占比
      peakYear: number;           // 引用峰值年份
      citationHalfLife: number;  // 引用半衰期
    };
    interdisciplinarity: {        // 跨学科性
      signal: number;            // 信号强度 (0-1)
      breadth: number;            // 广度 (跨学科数量)
    };
  };
  
  // 结构维度 (Structure)
  structure: {
    centrality: {
      degree: number;             // 度中心性
      betweenness: number;        // 中介中心性
      eigenvector: number;        // 特征向量中心性
      pagerank: number;           // PageRank 值
    };
    topology: {
      potential: number;          // 势能值
      hubValue: number;           // 枢纽价值
      clusterCoefficient: number; // 聚类系数
    };
  };
  
  // 语义维度 (Semantics)
  semantics: {
    depth: {
      adoptionScore: number;      // 知识采纳得分 (0-1)
      reuseType: 'substantive' | 'peripheral' | 'mention'; // 复用类型
      contextualSimilarity: number; // 上下文相似度
    };
   复用: {
      citedContexts: CitingContext[]; // 引用上下文列表
    };
  };
}

// ======================
// 2. 引用上下文 (Citing Context)
// ======================
interface CitingContext {
  contextId: string;
  citingPaperId: string;
  citingPaperTitle: string;
  citationSentence: string;       // 引文所在句子
  preservedKeyTerms: string[];    // 保留的核心术语
  coCitationRelations: string[]; // 共被引关系
  authorSelfMention?: string;     // 作者自述内容
  knowledgePenetrationScore: number; // 知识渗透度 (0-1)
  reuseClassification: 'substantive' | 'peripheral' | 'mention';
}

// ======================
// 3. AUI Dashboard API 响应
// ======================
interface DashboardAPIResponse {
  meta: {
    generatedAt: string;
    comparisonPeriod: string;
    totalPapers: number;
  };
  overview: {
    nobelPapers: Paper[];
    controlPapers: Paper[];
  };
  dimensionScores: {
    form: {
      nobel: { score: number; breakdown: FormBreakdown };
      control: { score: number; breakdown: FormBreakdown };
    };
    structure: {
      nobel: { score: number; breakdown: StructureBreakdown };
      control: { score: number; breakdown: StructureBreakdown };
    };
    semantics: {
      nobel: { score: number; breakdown: SemanticsBreakdown };
      control: { score: number; breakdown: SemanticsBreakdown };
    };
  };
  weights: {
    w1: number;  // Form 权重
    w2: number;  // Structure 权重
    w3: number;  // Semantics 权重
  };
}

interface FormBreakdown {
  citationCount: number;
  temporalDistribution: number;
  interdisciplinarity: number;
}

interface StructureBreakdown {
  centrality: number;
  topology: number;
}

interface SemanticsBreakdown {
  depth: number;
 复用: number;
}

// ======================
// 4. 网络图 API 响应
// ======================
interface NetworkAPIResponse {
  meta: {
    focusPaperId: string;
    focusPaperTitle: string;
    depth: 1 | 2;  // 一阶/二阶网络
  };
  nodes: NetworkNode[];
  links: NetworkLink[];
}

interface NetworkNode {
  id: string;
  label: string;
  year: number;
  isFocus: boolean;        // 是否为焦点论文
  isNobel: boolean;
  // 可视化映射
  visual: {
    size: number;          // 基于度中心性
    color: string;         # 基于中介中心性 (颜色梯度)
    opacity: number;
  };
  metrics: {
    degree: number;
    betweenness: number;
    eigenvector: number;
  };
}

interface NetworkLink {
  source: string;
  target: string;
  type: 'citation' | 'co-citation';
  weight: number;
}

// ======================
// 5. 语义分析 API 响应
// ======================
interface SemanticAPIResponse {
  meta: {
    focusPaperId: string;
    focusPaperTitle: string;
    abstract: string;          // 焦点论文摘要
  };
  citationAnalysis: {
    totalCitations: number;
    substantiveCount: number;
    peripheralCount: number;
    mentionCount: number;
    avgKnowledgePenetration: number;
  };
  comparisons: CitationComparison[];
  // 按复用类型分组的引文
  groupedCitations: {
    substantive: CitingContext[];
    peripheral: CitingContext[];
    mention: CitingContext[];
  };
}

interface CitationComparison {
  nobelContext: CitingContext;
  controlContext: CitingContext;
  metric: 'penetration' | 'similarity' | 'contextLength';
  nobelValue: number;
  controlValue: number;
}
```

### 4.4 图表配置规范

#### 雷达图配置

```typescript
interface RadarChartConfig {
  indicator: Array<{
    name: string;           // 如 '被引频次', '时序分布', '跨学科性'
    max: number;
    min: number;
  }>;
  series: Array<{
    name: '诺奖论文' | '对照组';
    value: number[];
    areaStyle: { opacity: number };
    lineStyle: { width: number };
  }>;
  legend: { data: string[] };
  tooltip: { 
    trigger: 'item';
    formatter: (params) => string;
  };
}
```

#### 力导向图配置

```typescript
interface ForceGraphConfig {
  nodes: Array<{
    id: string;
    symbolSize: number;    // 映射度中心性 [10, 50]
    itemStyle: {
      color: string;       // 映射中介中心性 (色阶)
      borderColor: string;
      borderWidth: number;
    };
  }>;
  links: Array<{
    source: string;
    target: string;
    lineStyle: {
      width: number;       // 映射权重
      opacity: number;
      curveness: number;
    };
  }>;
  layout: 'force';
  force: {
    repulsion: number;     // 节点斥力 [100, 500]
    edgeLength: number;
    friction: number;
    gravity: number;
  };
  roam: boolean;            // 开启缩放平移
}
```

### 4.5 交互设计决策

| 交互场景 | 实现方案 |
|---------|---------|
| 图表数据切换 | 使用 Vue Router query 参数保存状态，支持浏览器历史导航 |
| 节点悬停 | 使用 ECharts tooltip 或自定义 HTML overlay |
| 网络图缩放 | ECharts 内置 zoom 插件 + 鼠标滚轮 |
| 引文高亮 | 自定义 Vue 组件 + CSS transition 动画 |
| 导出功能 | 使用 html2canvas + ECharts 内置 exportPNG |
| 响应式布局 | Tailwind CSS 响应式类 + 图表 resize 监听 |

---

## 5. 测试决策（Testing Decisions）

### 5.1 测试策略

- **单元测试**：使用 Vitest + Vue Test Utils
- **E2E 测试**：使用 Playwright（可选，用于关键用户流程）
- **视觉回归测试**：使用 Chromatic（可选）

### 5.2 测试覆盖重点

| 模块 | 测试重点 |
|------|---------|
| `dataParser.ts` | JSON 解析异常处理、边界值测试 |
| `RadarChart.vue` | 数据映射正确性、legend 切换 |
| `ForceGraph.vue` | 节点渲染数量、交互响应 |
| `paperStore.ts` | 状态变更、筛选逻辑 |

### 5.3 良好测试的定义

- 仅测试外部行为，不测试内部实现细节
- 模拟数据而非依赖真实 API
- 图表组件测试使用 shallow mount
- 网络图组件测试可使用 mock 数据验证渲染

---

## 6. 超出范围（Out of Scope）

以下功能明确不在本次 PRD 范围内：

1. **后端计算逻辑**：所有网络嵌入向量、SciBERT 相似度得分、AUI 合成指数计算均由后端完成，前端仅负责解析和渲染预计算的 JSON 数据
2. **用户认证系统**：无需登录，所有数据公开访问
3. **数据持久化**：不支持用户自定义数据保存
4. **实时数据更新**：不支持 WebSocket 实时推送，数据刷新需刷新页面
5. **论文检索功能**：不支持通过关键词搜索论文
6. **导出原始数据**：仅支持图表导出图片，不支持导出原始 JSON
7. **多语言支持**：初期仅支持中文界面

---

## 7. 进一步说明（Further Notes）

### 7.1 性能优化考量

- 力导向图节点数量控制在 200 以内，超过则采用采样策略
- 大数据集使用 Web Worker 进行数据处理
- 图表使用 `setOption` 增量更新避免全量重绘

### 7.2 浏览器兼容性

- Chrome >= 90
- Firefox >= 88
- Safari >= 14
- Edge >= 90

### 7.3 后续迭代方向（v2.0）

- 支持用户上传自定义 JSON 数据进行对比分析
- 增加时间轴动画，展示知识网络演化
- 支持论文详情页，展示完整的论文元数据

---

## 附录：Mock Data 示例

### Dashboard 数据示例

```json
{
  "meta": {
    "generatedAt": "2026-04-15T10:00:00Z",
    "comparisonPeriod": "2000-2025",
    "totalPapers": 50
  },
  "dimensionScores": {
    "form": {
      "nobel": { "score": 85.2, "breakdown": { "citationCount": 92, "temporalDistribution": 78, "interdisciplinarity": 86 } },
      "control": { "score": 45.6, "breakdown": { "citationCount": 48, "temporalDistribution": 52, "interdisciplinarity": 37 } }
    },
    "structure": {
      "nobel": { "score": 91.3, "breakdown": { "centrality": 94, "topology": 88 } },
      "control": { "score": 38.2, "breakdown": { "centrality": 35, "topology": 42 } }
    },
    "semantics": {
      "nobel": { "score": 88.7, "breakdown": { "depth": 90, "复用": 87 } },
      "control": { "score": 28.4, "breakdown": { "depth": 25, "复用": 32 } }
    }
  },
  "weights": { "w1": 0.3, "w2": 0.35, "w3": 0.35 }
}
```

---

**文档版本**: v1.0  
**创建日期**: 2026-04-15  
**状态**: 草稿
