# mythoshell 品牌资产交付

- `mythoshell-logo.svg`：可缩放路径版横向主标（标志渐变 + 深色字标）。
- `mythoshell-logo-light.svg` / `mythoshell-logo-dark.svg`：浅底/深底字色 SVG。
- `mythoshell-mark.svg`：独立图形标志 SVG。
- `mythoshell-logo-*-transparent.png`：透明底 PNG。
- `mythoshell-logo-on-light.png` / `mythoshell-logo-on-dark.png`：带背景预览 PNG。
- `mythoshell-mark-transparent.png`：独立图形标志透明 PNG。
- `favicon.svg`、`favicon.ico`、`favicon-{size}x{size}.png`：网站图标尺寸组。

## 展板标注的颜色
- Ocean Blue: `#3B82F6`
- Deep Purple: `#8B5CF6`
- Slate Dark: `#0F172A`
- Cool Gray: `#94A3B8`
- Soft Light: `#F8FAFC`

## 关于还原精度
原始输入是一张 1536×1024 的 PNG 品牌展板，并非原始设计源文件。透明 PNG 从展板顶部主标区域按像素抠取；SVG 是基于抠取轮廓转成路径，并以品牌色渐变填充，不是原始 AI/Figma 字体轮廓文件。因此字形边缘、渐变和极细节会受展板分辨率影响。PNG 是最接近展板像素外观的版本；SVG 适合常规缩放和网页使用。
