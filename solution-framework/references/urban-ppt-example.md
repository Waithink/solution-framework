# URBAN 女装 PPT 参考模板（2026-05-13 验证）

> 此文件记录了为女装客户URBAN生成解决方案PPT的完整方法，作为后续同类方案的参考。

## 场景回顾

- **客户**: URBAN（女装品牌，50家门店，直营为主+少量加盟）
- **痛点**: 会员管理 + 多门店管理 + 私域运营 + 渠道协同 + 导购赋能
- **上线目标**: 2026年10月
- **初始尝试**: SmartCanvas Slide AI PPT（被用户拒绝2次——"太粗糙/内容少/空置率高"）
- **最终方案**: PptxGenJS 手写JS布局

## 技术方案

### 环境
- Node.js + PptxGenJS（`npm install pptxgenjs`）
- LAYOUT_WIDE（13.3" × 7.5"）

### 配色（女装时尚调性）
| 角色 | Hex | 用途 |
|------|-----|------|
| 炭灰 | 2D2D2D | 主背景色/深色卡片 |
| 深炭灰 | 1A1A1A | 封底/深色幻灯片 |
| 暖杏白 | F5F0EB | 内容页背景 |
| 玫瑰金 | C17B6F | 点缀色/重要元素 |
| 软白 | FAF8F5 | 浅色背景变体 |
| 深色文字 | 333333 | 标题/强调 |
| 正文色 | 555555 | 正文 |

### 字体搭配
- 标题: Georgia（衬线，优雅气质）
- 正文: Calibri（无线衬，清晰）

## PPT结构（15页）

| 页码 | 类型 | 主要元素 |
|------|------|---------|
| 1 | 封面 | 品牌大字+痛点概览+微盟品牌 |
| 2 | 目录 | 左右两列编号+标题+子标题 |
| 3 | 行业机遇 | 3大数卡+4趋势卡片 |
| 4 | 痛点诊断 | 左侧客户概况+右侧5个痛点卡片 |
| 5 | 客群画像 | 3列特征+L1-L4分层金字塔 |
| 6 | 全链路蓝图 | 深色背景5步横向流程+箭头连接 |
| 7 | 会员体系 | 左侧金字塔+右侧规则卡片 |
| 8 | 多门店管理 | 总部→大区→门店三层+底部核心能力 |
| 9 | 导购赋能 | 2×2四模块卡片布局 |
| 10 | 私域SOP | 沉淀路径条+四季运营日历 |
| 11 | 产品配置 | C端/B端/中台产品+版本建议 |
| 12 | 实施路径 | P0-P3时间轴+里程碑标记 |
| 13 | 同业标杆 | 4品牌案例卡片 |
| 14 | 为什么微盟 | 4大优势+竞品对比表格 |
| 15 | 总结 | 感谢页+品牌落款 |

## 密度设计原则（验证有效）

每页内容布局遵循以下原则：
1. **3-5个内容块**：卡片/数据/文字段落交替
2. **无大面积空白**：每个内容块间距0.3-0.5"
3. **布局多样性**：相邻页面不使用完全相同的布局
4. **每页正文80-200字**：非3-5行简单bullet

## 关键代码模式

### 工具函数模式
```javascript
const C = { charcoal: "2D2D2D", cream: "F5F0EB", rose: "C17B6F", ... };

function rcard(slide, x, y, w, h, color) {
  slide.addShape(pres.shapes.RECTANGLE, {
    x, y, w, h, fill: { color }, shadow: makeShadow()
  });
}

function makeShadow() {
  return { type: "outer", color: "000000", blur: 4, offset: 1, angle: 135, opacity: 0.08 };
}

function addMulti(slide, x, y, w, h, lines, opts) {
  const items = lines.map((text, i) => ({
    text,
    options: { breakLine: i < lines.length - 1 }
  }));
  slide.addText(items, { x, y, w, h, fontSize: opts?.fontSize || 14, ... });
}

function secLabel(slide, num, title) {
  slide.addShape(pres.shapes.ROUNDED_RECTANGLE, {
    x: 0.7, y: 0.35, w: 1.2, h: 0.35, fill: { color: C.rose }
  });
  slide.addText("PART " + String(num).padStart(2, "0"), {
    x: 0.7, y: 0.35, w: 1.2, h: 0.35, fontSize: 10, color: C.white, bold: true, align: "center"
  });
  slide.addText(title, {
    x: 0.7, y: 0.75, w: 11.5, h: 0.6, fontSize: 30, fontFace: "Georgia", color: C.charcoal, bold: true
  });
  slide.addShape(pres.shapes.RECTANGLE, { x: 0.7, y: 1.35, w: 1.5, h: 0.04, fill: { color: C.rose } });
}
```

## ⚠️ 注意事项
- 所有颜色 hex 不带 `#`
- Option 对象不可复用（PptxGenJS mutation）
- `breakLine: true` 用于多行文本数组
- 中文用实际 UTF-8 字符，不可用 unicode escape
