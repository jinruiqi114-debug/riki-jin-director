# riki-midjourney

**Midjourney 提示词分析 Skill。** 把一条现成的 MJ 静态图像提示词（或一张参考图）拆成可读的视觉结构，说清它到底会生成什么、哪些是原文写明的、哪些是推断出来的、哪些根本没写，最后给出一条可直接复制测试的中文试验提示词。

它不做创作，也不做评判。默认只回答"这条提示词在描述什么"，不回答"这条提示词好不好"。

---

## 它解决什么问题

大多数提示词问题不是词不够，而是词之间的关系不清楚：

- 写了 `soft diffused lighting` 又写了 `deep set eyes`，光会把眼窝打平，这两个要求互相抵消
- 写了 `low saturation` 却没有任何冷色来源，出图只会是脏灰而不是蓝调
- 写了 `full body` 又要 `head to toes` 加马匹全身，竖幅根本装不下
- 服装写了九件，但没说裙长、肩甲在哪一侧，剪影每次都不一样

这个 Skill 的工作是把这些隐性冲突、缺失信息和视觉后果摆到台面上，让你在烧掉几十次生成额度之前就知道问题在哪。

## 什么时候用

- 手上有一条英文提示词，想知道它实际会生成什么画面
- 有一张喜欢的参考图，想反推出可用于 Midjourney 的提示词
- 想弄清某个影调（低-key、蓝调、港片胶片感）需要哪些具体条件才能成立
- 出图反复不对，需要定位是描述缺失、互相冲突，还是模型能力边界
- 想拆解服装、材质、光线、构图这些具体维度，而不是笼统地说"感觉不对"

## 什么时候不该用

| 需求 | 应该用 |
| --- | --- |
| 把剧本拆成分场表、写视频时间轴 | `riki-storyboard` |
| 直接生成图片或视频 | 图像 / 视频生成工具本身 |
| 评价提示词好坏、给打分 | 本 Skill 不做，只做客观拆解 |
| 修改提示词 | 需要你明确说"改"，否则不会主动改写 |

## 输出结构

每次分析固定五段，顺序不变：

**1. 原始提示词** —— 完整引用，不改写、不删减。

**2. 一句话视觉总览** —— 概括这条提示词的目标画面。

**3. 多维拆解表** —— 逐维度列出「原文依据 / 视觉作用 / 信息状态」。

| 维度 | 看什么 |
| --- | --- |
| 主体与特征 | 身份、外观、服装、姿态、表情、识别锚点 |
| 影调 | 亮度、明暗比例、反差、黑位、白位、雾感、压迫感 |
| 质感与媒介 | 皮肤、织物、金属、胶片颗粒、绘画或 3D 质感 |
| 真实感 | 纪实、电影写实、超写实、商业摄影、插画、CG |
| 光影 | 光源方向、色温、软硬、阴影、轮廓光、反射 |
| 色彩 | 主色、辅色、点缀色、饱和度、明度、冷暖关系 |
| 构图与视角 | 景别、机位高度、主体位置、留白、前中后景、画幅 |
| 镜头与焦距 | 广角 / 标准 / 长焦倾向、透视、景深、背景虚化 |
| 情绪氛围 | 情绪词对应的姿态、光线、空间和色彩表达 |
| 环境与参数 | 时间、地点、天气、道具，以及 `--ar`、`--v`、`--stylize` 等 |

**4. 可复用关键词组** —— 按视觉功能分组，可直接搬到别的提示词里。

**5. 中文试验提示词** —— 一条完整、忠实原意、可直接复制的版本。

回复 `ok` 后，输出对应的英文完整版，不重复分析、不追加解释。

## 信息状态三分法

这是整套方法的骨架。每条结论都必须能归到三类之一：

- **明确**：原文直接写出，例如 `soft diffused studio lighting`
- **推断**：由关键词组合推导出的视觉倾向，例如"中灰背景会压低整体反差"
- **缺失**：原文没提供，例如"光源方向未指定，模型可能默认正面平光"

推断和缺失不会被写成事实。这一点很重要——大部分"出图不对"其实来自把推断当成了已知条件。

## 三种输入方式

**纯文字提示词**：直接用当前文字分析，不引入其他上下文。

**参考图片**：按可见证据反向拆解，依次看画幅与景别、主体层级、动作视线、主光与阴影、影调色彩、材质颗粒，最后是环境与空气透视。只写看得见的东西，镜头型号、真实焦段、拍摄地点这类无法确认的内容会明确标注为无法确定。

**诊断与修改**：只有你明确要求"排查、检查、优化、修改"时才进入这个模式。此时先说明问题及其对画面的影响，然后必须输出一条整合全部修改的完整提示词，不会只给替换片段。

## 使用示例

```text
分析这条 Midjourney 提示词，重点说影调和人物服装，并生成中文试验提示词。

Full body frontal portrait, head to toes, centered vertical composition,
seamless medium gray studio background. Mature battle hardened Chinese general...
```

```text
根据这张参考图拆解提示词，分析构图、光影、影调、色彩和材质，然后给中文试验版。
```

```text
这条提示词出的图总是没有蓝调，帮我排查原因，并给出完整修改版。
```

## 拆解示例（前后对比）

以下三段来自真实使用记录，只保留与问题相关的片段。

### 示例一：为什么这条提示词出不了蓝调

**输入（节选）**

```text
seamless medium gray studio background ... Aged bronze, antique gold, faded vermilion,
charcoal, ivory palette against deep black. 1990s Hong Kong wuxia film aesthetic,
soft diffused studio lighting, warm highlights, low saturation, fine film grain.
```

**拆解结论**

| 维度 | 发现 | 状态 |
| --- | --- | --- |
| 影调 | 背景为中灰，高光指定为暖，五个颜色全为暖或中性，整条提示词没有一个冷色来源 | 明确 |
| 光影 | 全面漫射且未指定光位，人物与马会平贴在背景上，骨相与金属厚度读不出来 | 推断 |
| 色彩 | 暗部只写了"中性、略冷"，这个程度的冷在 MJ 里会被渲染成中性灰 | 推断 |
| 缺失 | 光源方向、黑位倾向、冷暖分离的具体方式 | 缺失 |

**改后**

```text
medium gray studio backdrop cooling into blue-gray toward the edges ...
a cold blue-grey moonlight from high behind-right rimming the rider's shoulder and the horse's neck ...
shadows sinking into low-saturation slate blue instead of black,
lifted film blacks with a faint blue-green cast,
low saturation with a clear warm-cool split
```

**改动对照**

| 原词 | 改为 | 原因 |
| --- | --- | --- |
| `medium gray backdrop` | `cooling into blue-gray toward the edges` | 让背景本身承担一部分冷色 |
| `soft diffused lighting, warm highlights` | 追加 `cold blue-grey moonlight from high behind-right` | 冷色必须落到一个具体光源上，靠形容词暗示不出来 |
| `neutral and slightly cool` shadows | `sinking into low-saturation slate blue` | "略冷"在 MJ 里等于中性 |
| 无 | 明确写出 `clear warm-cool split` | 低饱和下没有冷暖分离，画面只会整体发灰 |

### 示例二：全身 + 骑马，竖幅装不下

**输入**

```text
Full body portrait, head to toes, frontal to camera, centered vertical composition ...
```

**拆解结论**

| 维度 | 发现 | 状态 |
| --- | --- | --- |
| 构图 | 竖幅内人物头顶到脚趾已占画面高度约 85–90%，没有余量放下一匹马 | 推断 |
| 空间关系 | 若人马同时正面朝向镜头，马头会遮住躯干与腹甲 | 推断 |
| 缺失 | 马头的朝向、马身入画范围、手持缰绳的动作 | 缺失 |

**改后**

```text
Full body portrait of a mounted rider, head to toes, frontal to camera,
centered vertical composition ...
Black horse in three-quarter turn, head and neck angled slightly to one side of frame ...
hands resting loosely on leather reins ...
```

**改动对照**

| 原词 | 改为 | 原因 |
| --- | --- | --- |
| 单人全身 | 人物全身，马只入画头、颈、前胸 | 竖幅容不下马全身，马身必须出画 |
| 人马均正面 | 人物正面，马 3/4 侧转 | 避免马头遮挡躯干 |
| 无手部描述 | `hands resting loosely on leather reins` | 骑马必须有缰绳才成立 |

### 示例三：写了 Asian，出图仍是西方面孔

**输入（节选）**

```text
A single East Asian man in his thirties ... aviator sunglasses ...
worn military-surplus clothing ...
```

**拆解结论**

| 维度 | 发现 | 状态 |
| --- | --- | --- |
| 主体与特征 | `Asian` 只出现一次，后面八十多个词全是西式军装与装备 | 明确 |
| 道具 | 墨镜遮住眼睛，而眼睛是族裔最直接的识别点 | 推断 |
| 缺失 | 具体骨相特征（眼型、面中、鼻根、发质） | 缺失 |

**改后**

```text
A single Chinese man in his mid to late twenties, Han Chinese, unmistakably from mainland China ...
Han Chinese face: broad cheekbones, a squarish jaw, monolid eyes with an epicanthic fold,
thick straight black eyebrows, a low straight nose bridge ... straight black hair ...
aviator sunglasses pushed up onto the brim ... authentic Chinese casting
```

**改动对照**

| 原词 | 改为 | 原因 |
| --- | --- | --- |
| `East Asian` | `Chinese` / `Han Chinese`，首尾各一次 | `East Asian` 权重分散，容易漂向日韩或泛亚混血 |
| 无骨相描述 | 单眼皮、内眦赘皮、方颌、山根低、黑直发 | 族裔要靠骨相锚定，只写一个标签不够 |
| 墨镜戴着 | `pushed up onto the brim` | 遮住眼睛会立刻丢掉族裔特征 |

## 目录结构

```text
riki-midjourney/
├── SKILL.md                                  # 任务边界、执行流程、固定输出格式
├── references/
│   ├── midjourney-prompt-decomposition.md    # 十维拆解定义与中文试验版规则
│   └── image-prompt-decomposition.md         # 参考图反向拆解规则
└── LICENSE                                   # MIT
```

`SKILL.md` 是入口。`references/` 里的内容按需加载，不参与触发判断。

## 安装与调用

把仓库放进技能目录，保证 `SKILL.md` 位于仓库根目录，且 frontmatter 的 `name` 与目录名一致：

```text
<skills-dir>/riki-midjourney/SKILL.md
```

触发只读 `SKILL.md` frontmatter 里的 `name` 和 `description`，本 README 只服务于仓库说明，不影响触发。

调用方式：`/riki-midjourney`。

## 与同系列技能的区分

| 技能 | 负责 |
| --- | --- |
| `riki-midjourney`（本仓库） | 单条 MJ 提示词 / 单张参考图的分析与改写 |
| `riki-storyboard` | 剧本拆场、导演制作表、MJ 静帧与 Seedance 视频提示词 |

两者共用 Riki Jin 的视觉方法，但一个处理"一张图怎么描述"，一个处理"一段戏怎么拆"。

## 设计原则

1. **先保留原文**，再解释它，不替用户重写
2. **区分明确、推断、缺失**，不把推断说成事实
3. **描述视觉后果**，不说"这个不好"
4. **参数以官方文档为准**，不凭旧版本经验硬写
5. **改写时保持未被要求修改的部分不动**，优先修主体身份、空间关系、构图，再润色风格词

## 许可证

MIT，见 [LICENSE](LICENSE)。
