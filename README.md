# ☁️ Sky: CoL .vol 3D Texture Editor

> 一个用于解码、查看、编辑和重新编码《Sky: Children of the Light》（光·遇）`.vol` 体积纹理文件的工具集。

![GitHub](https://img.shields.io/badge/license-MIT-green)
![HTML](https://img.shields.io/badge/tool-HTML-blue)


---

## 📖 简介

《Sky: Children of the Light》（光·遇）使用 `.vol` 格式的文件存储 **3D 体积纹理（Volume Texture）**，用于游戏中的云朵渲染、地形材质和噪声纹理生成。这些文件通常位于 `Data/Tex3D/` 路径下。

本项目包含两个工具：

| 工具 | 说明 |
|------|------|
| **`vol-editor.html`** | 浏览器可视化编辑器，支持拖拽、切片查看、画笔编辑、滤镜、导出 |

---

## 🧩 .vol 文件格式

| 字段 | 说明 |
|------|------|
| **魔数** | `VOLU`（Volume Texture） |
| **版本** | 4 |
| **头部** | 256 字节（含 64 字节文件名） |
| **纹理数据** | `dim³` 字节的 8-bit 灰度体素数据（0-255） |
| **尾部** | 3840 字节（用途未知，保留不变） |
| **维度** | 通常为 32³、64³ 或 128³ |

已知的 `.vol` 文件（共 20 个）：

```
BallNoise, Blocks, Brick, Caustic, CloudFluffy, CloudNoise,
Cobblestone, Coral, Corroded, Cracks, FlatLayers, HillRock,
PerlinNoise, Pyro, RockNoise, ShelfRock, SoftRock, SquareNoise,
Tubes, Waves
```

---

## 🖥️ vol-editor.html — 浏览器可视化编辑器

### 使用方法

直接用浏览器打开 `vol-editor.html`，无需安装任何依赖。

### 功能

- **拖拽加载** — 拖入 `.vol` 文件自动解析
- **文件信息** — 显示魔数、版本、维度、体素总数等
- **3D 切片查看** — 滑动 Z 轴滑块逐层查看，支持 XY / XZ / YZ 三个平面
- **画笔编辑** — 直接在切片上绘画，可调颜色、笔刷大小、强度
- **填充工具** — 区域洪水填充
- **滤镜调整** — 亮度、对比度、Gamma、颜色反转、直方图均衡、3D 平滑模糊
- **重置纹理** — 一键恢复到原始状态
- **导出 .vol** — 修改后导出为游戏可用的 `.vol` 文件（字节级精确还原）
- **导出 PNG** — 导出当前切片为 PNG 图片
- **批量导出** — 将所有切片打包为 ZIP 下载
- **键盘快捷键** — 方向键切换切片

### 截图预览

| 功能 | 说明 |
|------|------|
| 🎨 加载文件 | 拖拽或点击选择 `.vol` 文件 |
| 👁 切片查看 | 拖动滑块遍历 Z 轴，切换三个观察平面 |
| ✏️ 编辑绘制 | 画笔工具直接在纹理上修改 |
| 📦 导出 | 导出修改后的 `.vol` 回游戏使用 |

---


### 解码-编辑-编码 工作流

```
1. decode → 得到 .raw（体素数据）+ .template.json（模板）
2. 编辑 .raw 文件（可用 Python / C / 任何二进制编辑器）
3. encode → 生成新的 .vol 文件，可直接放入游戏使用
```

---

## 🔍 技术细节

### 文件结构

```
[0x000 - 0x0FF] 256 字节头部（魔数 VOLU + 版本 + 文件名 + 填充）
[0x100 - 0x10F] 16 字节内嵌元数据（偏移 0x0C 处为维度值）
[0x110 - ...]   纹理数据（dim³ 字节，8-bit 灰度）
[... - 结尾]    3840 字节尾部（保留，原样复制）
```

### 体素数据

- 每个体素为 1 字节（0-255 灰度值）
- 数据排列顺序：`data[z * dim * dim + y * dim + x]`
- 不可编辑的部分（头部、尾部、元数据前 16 字节）在解码时被保留，编码时原样写回

---

## 📂 项目结构

```
.
├── README.md           # 本文件
├── vol-editor.html     # 浏览器可视化编辑器
└── Tex3D.zip/            # 示例文件（可选）
```

---

## 📝 License

MIT

## 🙏 致谢

- [thatgamecompany (TGC)](https://www.thatgamecompany.com/) — 制作了《Sky: Children of the Light》
- 所有逆向工程和游戏模组社区的朋友们