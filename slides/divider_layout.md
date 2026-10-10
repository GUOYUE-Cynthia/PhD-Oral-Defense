# 切换页 (Section Divider) 想要的 Layout

> 在下面的方框里画/描述你想要的版式。方框就是一页幻灯片的边界
> (比例 16:9,实际渲染尺寸 1280 × 720)。
> 你可以直接在框里用文字、箭头、缩进来摆放元素,或在下方"说明"里补充。

---

## 1. 页面边界示意 (16:9) — 在这里面画

```
┌──────────────────────────────────────────────────────────────────────┐
│ ↑ 上边距                                                                │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                              (页面中线)                                  │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│ ↓ 下边距                                                                │
└──────────────────────────────────────────────────────────────────────┘
  左边距 →                                                      ← 右边距
```

---

## 2. 要放的元素 (供参考,可增删)

- **Part 标号**:例如 `Part I · Learning representations`
- **大标题**:例如 `Parametric Koopman Decompositions` + 标签 `PK-NN`
- **副标题 / 一句话简介**:例如 `Unknown dynamics — learn a representation in which evolution is linear.`
- **发表信息**:
  - 标签:`Published as` / `Manuscript`
  - 论文标题:`"Learning Parametric Koopman Decompositions for Prediction and Control"`
  - 期刊出处:`Y. Guo et al. · SIAM J. Appl. Dyn. Syst. 24(1), 744–781 · 2025`

---

## 3. 我想要的样子 (在这里写/画)

<!-- 在下面这个空框里摆你想要的布局;不够用就多复制几个框 -->

```
┌──────────────────────────────────────────────────────────────────────┐
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
│                                                                        │
└──────────────────────────────────────────────────────────────────────┘
```

### 文字说明 (随便写)

- 标题区位置:
- 发表信息位置:
- 字号 / 对齐 (左对齐? 居中?):
- 颜色 / 是否要卡片 / 分隔线:
- 其他:

---

## 4. 参考:目前的实现长这样

```
┌──────────────────────────────────────────────────────────────────────┐
│                                                                        │
│                                                                        │
|
│                                                                        │
│                                                                        │
│                   PART I · LEARNING REPRESENTATIONS                     │
│              Parametric Koopman Decompositions  [PK-NN]                 │
│            Unknown dynamics — learn a representation …                  │
│                                                                        │

│      Published as Koopman Decompositions for Prediction and Control
          on SIAM Journal on Applied Dynamical Systems
│           Yue Guo, Milan Korda, Ioannis G Kevrekidis, Qianxiao Li

│                                                                        │
│                                                                        │
└──────────────────────────────────────────────────────────────────────┘
```
