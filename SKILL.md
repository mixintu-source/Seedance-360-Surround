---
name: Seedance 2.0 Mini/Fast
description: 把单角色三视图（正/侧/背全身立绘）做成「固定姿态 + 镜头匀速 360° 环绕」的展示视频提示词，并按 Seedance 2.0（Seedance 2.0 Mini/Fast）真实参数给出可直接粘贴的执行包：幕布颜色按角色配色推荐并等用户确认、肢体零动作、首尾帧闭环、时间戳分镜、@素材用途映射、防溢、自检与抽帧 QA。当用户提到 Seedance 2.0 / 即梦 / doubao-seedance、图生视频或文生视频提示词、三视图转视频、角色 360° 环绕展示、转盘展示、人物展示片时使用。
version: 1.2.0
---

# 角色三视图 → 固定姿态 360° 环绕展示视频（Seedance 2.0）

## 0. 用法（先读这段，再往下取需要的块）

- **输入**：单角色三视图（正面/侧面/背面全身立绘）。**输出**：可直接粘贴到 Seedance 2.0 的执行包（参数 + 正向提示词 + 禁止项 + 分镜表 + 自检）。单主体、单镜头、一镜到底、无音频、无文字层。
- **执行顺序（不得跳步）**：① 读角色配色 → 输出幕布推荐并**等用户确认** → ② 定参数与入口（第 1 节）→ ③ 拆图并分配素材角色（第 2 节）→ ④ 按第 8 节顺序组装正向词 → ⑤ 填第 9 节交付模板 → ⑥ 过第 10 节自检 → ⑦ 出片后过第 11 节抽帧 QA。
- **三条硬红线**：确认幕布前不产出任何提示词；角色全片零肢体动作（运动只来自镜头）；0 秒与尾帧必须是同一正面机位。
- 本文所有英文块都是可直接粘贴的**操作载荷**，不是说明文字；`<>` 为角色专属占位。

---

## 1. 平台事实与参数（不要凭记忆改）

### 1.1 参数表（左右两列是同一模型的不同通道，取值以当前通道报错为准）

| 项 | Ark API：`Seedance 2.0 Mini/Fast` | 即梦网页端 |
|---|---|---|
| 模型档 | 标准档 `Seedance 2.0 Mini/Fast`（画质最好）；快档 `doubao-seedance-2-0-fast-260128`（省时、画质略降） | Seedance 2.0，档位以界面为准 |
| 时长 | 整数 **4–15 秒**，默认 5 | 4–15 秒 |
| 比例 | 21:9 / 16:9 / **4:3** / 1:1 / **3:4** / 9:16 / adaptive | 同左 |
| 分辨率 | **480p / 720p** | 最高可到 2K |
| 图片 | ≤9 张，可带角色：`reference_image` / `first_frame` / `last_frame` | ≤9 张，用 `@图片1`…`@图片9` 引用 |
| 视频 | ≤3 段，单段 480p–720p，总 2–15 秒 | ≤3 段 |
| 音频 | ≤3 段、总 ≤15 秒，且**必须与图片或视频同时使用** | ≤3 段 |
| 混传上限 | 图+视频+音频合计 ≤12 个文件 | 同左 |
| 声音 | `generate_audio` **默认 true** | 界面开关 |
| 水印 | `watermark` **默认 true**（部分账号关不掉） | 以界面为准 |
| 帧率 | 24 fps | 24 fps |

**本 skill 的默认组合（改任何一项都要写进交付说明）**：`duration=5`、`ratio=3:4`、`resolution=720p`、`generate_audio=false`、`watermark=false`。通道不支持 3:4 时退 `9:16` 或 `adaptive`，不支持 720p 时退 480p（抠像后期更吃分辨率，优先 720p）。**720p + 24fps + 5 秒够用**；要更高清走 2K 通道，不要在本档硬提。

### 1.2 平台硬限制（踩了就是白跑一趟）

1. **不支持上传含写实真人脸的素材**（图片、视频都会被系统拦截）。→ 三视图必须是 2.5D / 3D / 插画风格的脸；「写实化」只写在**提示词**里，不靠上传真人照片。输入本身就是写实真人照时：先重绘为非写实三视图，或改用外观文字描述 + 非真人参考图。
2. **IP 规避**：不要直接写知名角色/品牌名，改写为外观描述（发色、服饰结构、道具形制）。
3. **参考视频会显著加价**；2K 比 720p 贵得多。5 秒静音 720p 是最省的组合。
4. **单次最多 15 秒**。要更长的环绕：先生成 ≤15 秒一段，`return_last_frame=true` 取回末帧，再用「视频延长」接下一段，衔接点必须写明「上一段结尾画面 = 下一段开始画面」。
5. 静音必须**双写**：参数 `generate_audio=false` **且**提示词里写「无音频、无配乐、无音效」。只关参数，模型仍可能补音效。
6. **通道能力差异（v1.2.0 新增，落地前必查）**：
   - 部分网页端/API 通道**没有独立的 `negative_prompt` 字段**。此时禁止项必须并入正向提示词末尾的「正向承诺声明」，或改用「绝对承诺」句式表达。
   - 部分通道**没有独立的 `watermark` 开关**。默认出片可能带水印，需在交付说明中提醒用户「若带水印，后期裁切或换通道重出」。
   - 部分通道**不支持分离的 `first_frame` / `last_frame` 参数**。此时降级为：正面全身图居首作为参考图锁定姿态，侧面/背面图补充外观，并在提示词中强约束「首尾同机位、同构图、360° 精确回正」。

### 1.3 入口选择（二选一，同一任务不要混用）

| 入口 | 何时用 | 做法 |
|---|---|---|
| **A. 首尾帧（优先）** | 只要整圈闭环、姿态最稳 | 首帧 = 尾帧 = **同一张正面全身图**；提示词写环绕。0° 与 360° 由首尾帧钳死，不会漂 |
| **B. 全能参考** | 侧面/背面的结构（露背、后颈结、裙后印花）必须被还原 | 正/侧/背三张一起传，每张后面跟一句用途（第 2 节），姿态基准仍只认正面图 |

选 A 保姿态、选 B 保细节。**默认 A**；用户明确要求"背面细节必须准"时用 B，并在自检里额外核对姿态漂移。**若通道不支持 A（无独立首尾帧参数），强制降级为 B**，并在交付说明中注明「尾帧为构图约束，非像素级复用」。

---

## 2. 素材准备与素材角色映射

三视图是一张拼图，**先切图再用**：正面、侧面、背面各存一张单图（拼图整张传会让模型把档案文字、配色板、表情小图当成画面内容）。

| 素材 | 素材角色 | 必须写进提示词的用途句 |
|---|---|---|
| 正面全身 | `first_frame` + `last_frame`（入口 A）/ `reference_image` 中的「姿态基准」（入口 B） | `首帧与尾帧均为该图机位与外观；姿态、持物方式仅以正面图为准` |
| 侧面全身 | `reference_image` | `仅作侧向外观参考（发型侧面轮廓、衣侧结构、道具侧面造型），不作动作指令` |
| 背面全身 | `reference_image` | `仅作背面外观参考（后颈系结、露背结构、裙后印花、发型背面），不作动作指令` |

**引用语法按通道走**：即梦网页端用 `@图片1`、`@图片2`…（官方命名，不要写 `@img1`）；Ark API 按提交顺序引用（`[Image1]` 或 `@图片1`，以通道说明为准），真正的绑定靠 `role` 字段而不是文字编号。**每个素材后面必须跟一句用途**——多素材时最常见的废片原因就是把图、角色、用途搞混。

> **v1.2.0 新增**：若用户只给了合并设定图（含中文标签、脸部特写、配饰特写、表情四宫格），**必须先本地裁出正面/侧面/背面三张纯全身参考图**，剔除标签与细节卡后再上传，否则文字与特写会被带入画面污染背景。

---

## 3. 背景决策（先推荐 → 用户确认 → 再锁定）

**背景色是全流程唯一真源。确认后，正向词、禁止项、参数确认、分镜、交付说明五处必须字面一致。** 确认前不得写正向词、禁止项、分镜。

### 3.0 流程（硬性）

1. 从三视图提取主色、辅色、发色、肤色、道具色，标出**与幕布同色系或高饱和相近**的项。
2. 按 3.1 矩阵给**首选 + 备选 + 不推荐**，逐条说明理由（为什么适合这个角色、为什么排除其他颜色）。
3. 明确询问「是否采用 <首选>，备选 <备选>？」→ **等确认**。
4. 确认后锁定 A 绿 / C 蓝 / B 白 / D 灰，全文按该方案替换。
5. 禁止不经推荐直接锁色，也禁止只给"绿幕/白幕"二选一而漏掉蓝幕、灰幕。

```
【幕布颜色推荐】
角色配色摘要：<主色/辅色/发色/肤色/道具色；标出同色系或高饱和相近项>
- 首选：<色> — 理由：<与主体哪些颜色对比充足；避开哪些同色冲突；后期或直出适配>
- 备选：<色> — 理由：<首选不成立时的替代逻辑；为何排除其余颜色>
- 不推荐：<色> — 原因：<与主体哪一色系冲突/融合/后期不可用>
请确认采用哪一种（首选/备选/其他），确认后我再产出完整脚本。
```

### 3.1 选择矩阵

| 幕布 | 适用（角色配色） | 排除 | 理由 |
|---|---|---|---|
| **A 绿幕 #00B140（默认）** | 主体含白/米白/月白/黑/红/紫/粉/蓝/金；发色黑/棕/金；肤色正常 | 主体大面积绿/黄绿/青绿，或发色鲜绿、青绿 | 与肤色、黑发、白/红/紫/蓝衣对比高；despill 工艺成熟 |
| **C 蓝幕 #0047BB** | 主体含绿/黄绿/青绿、鲜黄、橙；发色黑/棕 | 主体大面积蓝/青/深蓝，或发色鲜蓝 | 与绿/黄/橙对比高，专治绿衣绿发 |
| **B 白幕（纯白无缝）** | 主体深色/高饱和（黑、深红、藏蓝、墨绿），且用户要**直接成片不后期** | 主体含白/米白/奶白/浅灰/银白/白蕾丝/白网纱；或要抠像 | 浅色主体贴白幕会边缘融合，抠像无对比度 |
| **D 灰幕 #808080** | 纯白与纯黑同时大面积出现；要中性直出 | 要抠像（灰幕抠像远弱于绿/蓝）；主体大面积灰/银灰 | 对白与黑都有中等对比；抠像兼容性最差 |

### 3.2 锁定后取对应一组（正 / 负 / 高风险材质）

**A 绿幕**
- 正向：`plain seamless pure chroma-key green screen background (#00B140), flat even studio lighting, strictly no cast shadow, subject stands at least 1.5m in front of the screen, neutral white rim light on subject edges, no green spill on skin hair or clothing`
- 负向：`green rim light, green color bleeding, green tint on white fabric, green spill on mesh, uneven green, shadow on green screen`
- 高风险：浅色/米白缎面与蕾丝、黑白半透明网纱、白丝绸 → 交付说明标「建议后期 despill」。

**C 蓝幕**
- 正向：同上，把 green 换成 `chroma-key blue screen background (#0047BB)`，`no blue spill`。
- 负向：`blue rim light, blue color bleeding, blue tint on white fabric, blue spill on mesh, uneven blue, shadow on blue screen`
- 高风险：白/米白/浅蓝/银白、半透明网纱。

**B 白幕**
- 正向：`plain seamless pure white background, flat even studio lighting, strictly no cast shadow, no background gradient`
- 负向：`grey background, colored background, green screen, green rim light, shadow on backdrop, uneven lighting, backdrop wrinkle, vignette`（**删掉所有 blue 项**）

**D 灰幕**
- 正向：`plain seamless neutral grey background (#808080), flat even studio lighting, strictly no cast shadow, no background gradient`
- 负向：`white background, green screen, blue screen, colored background, shadow on backdrop, uneven lighting, backdrop wrinkle, vignette`
- 交付说明注明「如后期需抠像，建议改绿/蓝幕重做」。

> **铁律**：不得「正向绿幕、交付白幕」，不得「白幕方案还留着 green 负向项」。锁定后只保留一套正/负向，另一种幕布的项必须删净。

---

## 4. 姿态冻结与肢体零动作（本 skill 的核心价值）

### 4.1 肢体零动作（正向必写、禁止项必写，不可省略）

角色在全片**不得有任何肢体动作**，包括隐性微动作。特别注意：**非持物手必须绝对下垂，不得触碰身体或道具。**

- 禁止：手臂/手指/肩部动作，头转、点头、歪头，重心偏移、换腿、踮脚、屈膝。
- 禁止：非持物手抬起、摸包、摸腰、手指蜷缩。
- 禁止：呼吸起伏、胸腔扩张、眨眼、眼球转动、嘴部开合、发丝飘动、衣料随呼吸产生褶皱变化。
- 唯一允许的"运动"：**镜头环绕产生的视差**。角色在世界坐标中完全静止，如同一尊冻结的雕像，但保留照片级质感。

```
The character is completely motionless in world space: no limb movement of any kind —
no arm, hand, finger, shoulder, head, neck, torso, leg, ankle or foot motion; no weight
shift, no breathing, no blinking, no eye movement, no micro-movement. The body is frozen
like a statue while retaining photorealistic skin, fabric and hair appearance.
Crucial: the non-holding hand hangs perfectly straight and motionless at the side, never
touching the body or the prop. Both hands keep their exact initial position and finger curvature.
The subject does NOT turn or rotate itself. Only the camera moves around the completely stationary subject.
```

**分镜表述要求**：B01–B03「核心画面与动作」栏不得出现"转身/走动/抬手/调整"等动词，主语必须是镜头；只写"镜头到达某角度时可见的外观细节"。

### 4.2 只允许一个冻结姿态

三视图里的多角度是**外观参考**，不是镜头内动作指令。正向必写：

```
One frozen instant. The character holds exactly ONE fixed pose for the entire shot, using
the FRONT view's pose and prop-holding method as the sole reference. The side and back view
descriptions of the reference sheet are for APPEARANCE ONLY (print layout, hair texture,
garment cut) and must NOT be interpreted as pose changes or prop-position changes during the orbit.
```

### 4.3 道具持法锁定（以手提包为例）

- 从三视图**选定一种**持法（建议以正面为准，如「右手拎提手于体侧」），同时写进正向词与交付模板「核心信息」。
- 正向：`the handbag stays in the same hand, same height, same orientation for the entire 360° orbit; no transfer between hand and elbow, no swinging.`
- 禁止项：`handbag switching hands, handbag moving to elbow, prop position change, prop swing, prop disappearing`.
- 通用冻结：头发/发髻纹理/衣褶/网纱/包/戒指/鞋带 —— `perfectly motionless while retaining photorealistic appearance`。

> **v1.2.0 新增**：若参考图本身是斜挎包，但用户要求改为手提，必须在正向词中明确写死 `the shoulder strap is removed / the bag is held by the top handle at the side, no strap across the body`，并在抽帧 QA 中重点检查肩带是否残留。

---

## 5. 运镜：匀速整圈 + 首尾闭环

### 5.1 正向四量（缺一即不合格）

```
Camera: one continuous, smooth, CONSTANT-SPEED 360-degree horizontal orbit at a FIXED
radius and FIXED height, no zoom, no dolly, no cut, no handheld shake. Starts at the FRONT
view, ends at EXACTLY the same FRONT view — identical framing, identical subject size.
Total yaw = 360° exactly: no over-rotation, no under-rotation. Constant angular velocity
throughout: no ease-in, no ease-out, no pause, no speed ramp. Subject stays centered with
constant size for the whole shot. The subject never rotates; only the camera moves.
```

### 5.2 时间—角度对照（5 秒 / 72°/秒，必须与时间戳分镜吻合）

| 时间 | 累计角度 | 应呈现 | 分镜标注 |
|---|---|---|---|
| 0.0s | 0° | 正面（首帧，= 尾帧） | 正面全身居中 |
| 0.5s | 36° | 正面偏侧 | 首秒内必须有可见角度变化 |
| 1.0s | 72° | 明显 3/4 侧 | B01 末 |
| 1.5s | 108° | 正侧面（≈90°） | — |
| 2.5s | **180°** | **正背面** | B02 的背面节点对齐此处 |
| 4.0s | 288° | 另一侧 3/4 | B03 起 |
| 5.0s | **360°** | **精确回正面** | B03 收束，闭环 |

### 5.3 禁止项

```
over-rotation, under-rotation, more than one turn, less than one turn, speed ramp,
acceleration, deceleration, ease in, ease out, pause, hold, spin, zoom, dolly, cut,
scene change, handheld, camera shake, wobble, orbit direction reversal
```

---

## 6. 极致写实风格强制锚定（v1.1.0 新增：彻底粉碎动漫感）

**如果用户明确要求“真人实拍质感”、“我不要动漫/3D”，必须把以下正向词放在所有提示词的最顶端（最高权重），并在负向词中进行极其严苛的重罚。**

**正向摄影参数词（必写）**：
```
Raw cinematic live-action footage, shot on ARRI Alexa 65 with 85mm lens. Hyper-realistic human skin texture with visible microscopic pores, subtle skin imperfections, fine peach fuzz, natural subsurface scattering. Detailed individual hair strands. Real physical fabric weave, natural wrinkles and tension. Real-world practical lighting with soft cinematic shadows. Absolutely NO digital painting or CGI feel.
```

**负向防御词（必写，全量填入负向字段；若通道无负向字段，改写为正向承诺声明）**：
```
anime, manga, illustration, digital painting, 3D render, Unreal Engine, Octane Render, CGI, Pixar, Disney, cartoon, game character, 2.5D, semi-realistic, stylized, plastic skin, smooth skin, airbrushed, doll-like, wax figure, oversized eyes, sharp chin, unnatural proportions, perfect symmetrical face, oversaturated colors.
```

---

## 7. 外观一致性（负形 + 白名单）

外观清单必须与参考图逐项对应，不得增删；**必须写"关键负形（没有什么）"防脑补，并给"配件白名单"**。

示例（换角色时整段替换）：
- 发型：单侧麻花辫垂右肩、碎刘海
- 服装：薄荷绿挂脖短背心（**小 V 领；腹部绝对无横带**）；灰绿白格高腰超短裙（**右侧高开衩，高度与位置锁死**）
- 鞋：白色一字带厚底凉拖
- 道具：草编托特包（**右手拎提手于体侧，同手同高同朝向**）
- 白名单：细银链小贝壳坠、贝壳脚链、薄荷绿指甲。**绝不允许：手腕珍珠手链、其他首饰。**
- 裸露：光腿不穿袜、双臂无袖套

正向写法：`The character strictly matches the reference image: [...逐项]. CLEAN UNBROKEN midriff (no horizontal straps across the belly). ONLY these accessories are allowed: shell anklet, shell necklace, mint nails. NO bracelet on either wrist.`
一致性禁止项：`outfit change, clothes transforming, missing accessory, extra accessory, extra straps, belt across stomach, bikini top style, neckline changing, skirt slit moving, skirt length changing, print mismatch, hairstyle change, missing anklet, extra bracelet, wrong shoe`.

---

## 8. 提示词组装

### 8.1 正向词顺序（照抄顺序，不要重排）

1. **极致写实摄影参数词（第 6 节，最高权重置顶）**
2. **技术规格头**：`3:4 竖屏，24fps，5 秒，单镜头一镜到底，无音频。`
3. 背景与防溢（3.2 已确认那一组）
4. 素材用途句 + 外观清单（含负形与白名单）（2、7）
5. 肢体零动作（4.1）→ 冻结姿态（4.2）→ 道具锁定（4.3）
6. 运镜四量（5.1）→ 时间戳分镜（8.2）
7. 结尾**正向承诺声明**（8.3）

### 8.2 时间戳分镜（写进提示词正文，不只写在表格里）

```
0.0–1.0秒：镜头以 72°/秒匀速向右环绕，画面由正面过渡到明显的右侧 3/4 视角；角色保持绝对冻结姿态，纹丝不动。
1.0–2.5秒：镜头继续匀速，经过正侧面（≈90°），在 2.5 秒时正好到达正背面（180°），可见<后颈系结/露背结构/裙后印花>。
2.5–4.0秒：镜头继续匀速，经过左侧 3/4（≈288°），<侧面结构>逐一经过；角色姿态如雕塑般定格，无任何动作。
4.0–5.0秒：镜头匀速收束，在 5.0 秒回到与首帧完全相同的正面机位（360°），保持同一冻结姿态，构图与人物尺寸与首帧一致。
```

> Seedance 对时间戳分段的理解显著好于笼统描述；**5 秒的四段是标配**，不要写"1–3s 侧面→背面"这种没有角度的含糊句子。

### 8.3 正向承诺声明（内联在提示词末尾，最有效的约束形式）

> **注意：在正向提示词中，千万不要写“禁止XX”，AI 的注意力机制会被“XX”吸引。必须用正向的“绝对承诺”句式。** 若通道无独立负向字段，需在此处额外内联「画面绝对纯净、无任何文字、无任何水印、完全静音、无旁白音效、无 3D/CG 感、皮肤为真人毛孔质感」。

```
全片画面绝对纯净，无任何文字或水印；全片完全静音，无任何旁白与音效；镜头保持平稳匀速环绕，角色全程如雕像般定格静止（特别是非持物手绝对下垂且不接触任何物体），服装与配件保持完美一致。真人实拍电影质感。
```

### 8.4 负向字段（**仅当通道支持 `negative_prompt` 时**才填）

通道没有负向字段就**不要单列一节负向词**（模型读不到）。有该字段时按序拼接：**风格重罚词（6）** → 动作/冻结类（4.1 + 4.3）→ 运镜类（5.3）→ 形变类 → 背景类（**只留已确认幕布那一套**）→ 文字/音频类 → 一致性类（7）。

---

## 9. 交付模板（每个角色按此结构产出）

```
## 视频生成参数确认
- 模型：Seedance 2.0 Mini/Fast（快档：-fast-260128）
- 入口：A 首尾帧（首=尾=正面全身图） / B 全能参考（正+侧+背）
- 时长：5 秒        - 比例：3:4 竖屏        - 分辨率：720p（24fps）
- 声音：generate_audio=false + 提示词双写静音
- 水印：watermark=false；若账号关不掉 → 后期裁切 / 交付说明注明
- 素材映射：正面→first_frame+last_frame（姿态与持法唯一基准）；侧面→reference_image（仅外观）；背面→reference_image（仅外观）
- 背景：<A 绿 / C 蓝 / B 白 / D 灰 + 防溢措施>（全文唯一真源）
- 风格锚定：**真人实拍电影质感、极度写实皮肤纹理与织物（坚决抵制任何 3D/CG/半写实画风）**
- 肢体零动作：全片无肢体/手指/头肩/腿脚/重心/呼吸/眨眼微动作；非持物手绝对下垂；角色不自行旋转，只有镜头在动
- 冻结姿态：<唯一姿态 + 持物方式>；三视图其余角度仅作外观参考
- 主线：角色静止站立于<已确认幕布>，镜头定半径定高匀速环绕，依次揭示正面→侧面→背面→侧面→正面
- 开场推进：首帧正面全身居中，0.5 秒内可见角度变化，1.0 秒末为明显 3/4 侧（≈72°）
- 结尾收束：5.0 秒回到与首帧完全相同的正面机位，累计恰 360°
- 画面文字/图形层：无      - 字幕：无      - 音轨：关闭

**分镜表（三行，角度对齐 5.2；主语必须是镜头）**

| 时间 | 核心画面与动作（镜头主语） | 台词/声音 | 运镜衔接 | 钩子与亮点 |
|---|---|---|---|---|
| B01 00:00–00:01（0°→72°） | 正面全身居中；<外观要点>；<持法锁定>；角色完全静止 | 无 | 定半径匀速右移启动 | 正面建立，首秒可见角度变化 |
| B02 00:01–00:03（72°→216°） | 经侧面到 **2.5s 正背面（180°）** 再到另一侧；可见<露背结构/后颈系结/裙后印花>；开衩与持法保持锁定 | 无 | 匀速持续，主体居中、尺寸恒定 | 背面细节揭示，三视角一致 |
| B03 00:03–00:05（216°→360°） | 经另一侧回到**精确正面**，与首帧构图一致 | 无 | 匀速收束，精确回正 | 360° 闭环定格 |
```

---

## 10. 生成前自检（逐项打勾，缺一不可）

- [ ] **幕布**：已输出「首选+备选+不推荐+理由」并等到用户确认；确认前未产出提示词
- [ ] **幕布配色**：主体含绿/黄绿/青绿未选绿幕；含白/米白/网纱未选白幕（除非用户明确要求直出）；黑白并存直出才考虑灰幕
- [ ] **五处一致**：正向、禁止项、参数确认、分镜、交付说明的背景描述字面一致；未残留另一套幕布的正/负向项
- [ ] **防溢**：绿/蓝幕已含离幕距离、中性轮廓光、despill；已点名高风险材质（浅缎面/蕾丝/网纱/白丝绸）
- [ ] **平台限制**：三视图**不含写实真人脸**；未直接写知名 IP/角色名；时长在 4–15 整数
- [ ] **参数**：`duration=5`、`ratio=3:4`、`resolution=720p`、`generate_audio=false`、`watermark=false` 已逐项核对并按通道实际能力调整
- [ ] **入口与素材**：入口 A/B 二选一未混用；三视图已切单图；每个素材都跟了用途句；若通道无首尾帧参数，已降级为 B 并注明
- [ ] **风格锚定**：正向顶端已置顶 `Raw cinematic live-action footage` 等实拍参数；负向已填满所有反 CG/反动漫重罚词（或已在正向承诺中内联）
- [ ] **肢体零动作**：正向含 4.1 全段（含"非持物手绝对下垂"与"角色不自行旋转"）；禁止项含 limb/arm/hand/head/weight shift/breathing/blinking/hand touching bag；分镜无动作动词
- [ ] **冻结姿态与道具**：4.2 全段已写；持包手/高度/朝向写死；若改手提，已写死"无肩带"；禁止项含 switching hands / moving to elbow / swing
- [ ] **运镜**：起始正面、结束精确正面、总 yaw=360°、恒定角速度四项齐全；**时间戳分镜与 5.2 角度表逐行对得上**
- [ ] **外观**：逐项对应参考图无增删；已写关键负形（如"腹部无横带"）与白名单（禁多手链/禁少脚链）
- [ ] **静音与文字**：参数 + 提示词双写静音；**结尾已用正向承诺声明（绝不用“禁止XX”句式）**
- [ ] **交付说明**：模型档位、入口、幕布方案（绿/蓝含 despill 提示）、水印处理方式、通道能力降级说明均已注明

---

## 11. 成片 QA（抽帧 0s / 1s / 2.5s / 4s / 5s，任一不合格即重做）

1. **写实度（最高优先级）**：有没有变成 3D 游戏建模或二次元？皮肤是否像真人（有毛孔/微瑕疵），而不是塑料假人？
2. **肢体**：非持物手是否乱动/抬起/摸包/摸腰；持物手高度、朝向是否位移。
3. **服装变异**：腹部是否多出横带或抽褶；领口形状是否改变（小 V 变深 V/比基尼）；开衩位置是否上下滑动。
4. **配饰增减**：是否凭空多出手链/戒指/耳环，或丢失脚链/项链坠；若改手提，是否残留斜挎肩带。
5. **闭环**：0s 与 5s 帧是否完全一致（构图、站位、主体尺寸）；累计是否**严丝合缝**转满 360°（<340° 或 >380° 视为不合格）。
6. **防溢**：放大网纱、颈后系带、丝巾边缘，看是否有残留绿边/蓝边（有则按交付说明 despill）。
7. **静音与水印**：音轨是否真的无声；画面是否带平台水印（带则裁切或换参数重出）。

---

## 12. 迭代与重试（单变量原则）

- **每次只改 1–2 个关键词、只修一个问题**，不要整段重写——重写会让你失去归因能力。
- 常见废片 → 改法：
  - **滑向动漫/3D** → **把第 6 节的实拍参数词移到提示词的最最最开头（赋予最高权重）；负向词疯狂叠加 `3D render, CGI, plastic skin`；若还不行，换一张本身面部阴影和纹理更接近真人的三视图。**
  - 动作过多 → 4.1 整段前置，禁止项补 `idle motion, swaying body`。
  - 整圈不闭合 → 换入口 A（首=尾=正面图）；仍不闭合则把时间戳分镜的四段写细、加 `identical framing`。
  - 服装变异 → 把负形句改成全大写短句，并把它移到外观清单最前。
  - 防溢不干净 → 增大离幕距离描述、加 `no colored bounce light on subject`，或直接改灰幕/白幕直出。
  - 斜挎残留 → 正向追加 `no shoulder strap visible, bag held only by the top handle`。
- 连改 2 次仍不过：降级方案是「换入口 + 简化外观描述（只保留 6–8 个关键项）」，而不是继续加词。

---

## 13. 高频缺陷对照表

| 缺陷 | 表现 | 修复位置 |
|---|---|---|
| **动漫化/半写实感** | **脸圆、大眼、皮肤像塑料、3D建模感强** | **6 极致写实风格强制锚定 + 负向重罚 + 迭代重试** |
| 未推荐即锁幕布 | 跳过确认直接绿幕 | 3.0 流程 |
| 幕布与主体同色 | 绿衣配绿幕、白衣配白幕 | 3.1 矩阵 |
| 背景二义 | 正向绿幕、交付白幕、负向还留 green | 3 铁律 + 10 自检 |
| 隐性肢体动作 | 呼吸起伏、重心偏移、手指微动 | 4.1 + 禁止项 |
| 非持物手乱动 | 左手抬手摸包/摸腰 | 4.1「绝对下垂」 |
| 角色自行转圈 | 变成角色转身，姿态崩 | 4.1「角色不自行旋转」+ 5.1 |
| 服装变异 | 小 V 变比基尼、多出横带、开衩滑动 | 7 负形句 |
| 配饰增减 | 丢脚链、长出珍珠手链 | 7 白名单 |
| 姿势漂移 | 侧/背视角被当成动作指令 | 4.2 |
| 持包不锁 | 手中/体侧/肘弯混用 | 4.3 |
| 斜挎残留 | 改手提但肩带仍在 | 4.3 新增 + 12 迭代 |
| 整圈不严 | "回到接近正面" | 5.1 四量 |
| 首尾不接 | 尾帧与首帧差几度 | 入口 A（首=尾）或 1.3 降级说明 |
| 分镜角度粗 | "1–3s 侧面→背面"无角度 | 5.2 + 8.2 时间戳 |
| 静音失败 | 参数关了仍出音效 | 1.2 条 5「双写」 |
| 带水印 | 忘了 `watermark` 默认 true | 1.1 参数表 + 11 条 7 |
| 上传被拦 | 三视图是写实真人脸 | 1.2 条 1 |
| 素材混淆 | 多图时用途没写清 | 2 用途句 |
| 合并图污染 | 整张拼图喂入，文字/特写入画 | 2 新增「先切图」提醒 |

---

**使用方式**：① 读角色配色 → 出幕布推荐 → 等确认；② 定入口与参数（1.1、1.3）；③ 切图并写素材用途（2）；④ 按 8.1 顺序组装正向词、8.2 写时间戳、8.3 写禁止项；⑤ 按 9 出交付包；⑥ 过 10 自检；⑦ 出片后过 11 抽帧 QA；⑧ 不过走 12 单变量迭代。
