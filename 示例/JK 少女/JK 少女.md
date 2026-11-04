# 一、生成前决策与参数确认

- **背景决策**：**蓝幕 #0047BB（已锁定）**。主体核心色薄荷绿/灰绿/青绿与蓝幕为对比色，边缘分离度高；深黑褐发色、白色格纹、草编色与蓝幕对比充足；避开绿衣选绿幕的同色溢色问题，后期 despill 成熟。
- **高风险材质预警**：薄荷绿网眼针织背心、轻薄棉麻格纹裙、白色一字带凉拖、草编托特包口薄荷绿丝巾——已点名防蓝溢，交付说明标注"建议后期对网眼与丝巾边缘做 despill"。
- **肢体零动作（硬约束）**：全片角色无任何肢体/手指/头肩/腿脚/重心/呼吸/眨眼微动作，仅镜头视差产生运动感。
- **冻结姿态锁定（核心）**：全片一个冻结姿态，以正面视角站姿与持法为唯一参照；三视图侧/背描述仅作外观参考，不得作动作或道具位移指令。**持法锁定：右手拎草编托特包提手于体侧，同手、同高度、同朝向，不换手、不移肘、不摆动；左手自然垂于体侧。**
- **模型**：Seedance 2.0 Mini/Fast
- **时长**：5 秒
- **比例**：3:4 竖屏（全身展示）
- **音频**：关闭

---

# 二、正向提示词

```
plain seamless pure chroma-key blue screen background (#0047BB), flat even studio lighting, strictly no cast shadow. The character stands at least 1.5m in front of the blue screen; neutral white rim/back light on subject edges, no blue spill on skin, hair or clothing; post-production despill applied to semi-transparent mesh and light fabric edges.

The character strictly matches the reference image: a young JK girl, age 18, 168cm, slim and well-proportioned; deep black-brown long hair in a single side braid falling over the right shoulder, wispy bangs; mint-green and white fine-check bucket hat with brim slightly pressed down casting a faint shadow under the eyes; mint-green halter-neck cropped top with thin straps tied in a bow at the nape of the neck, small V-neckline at the front, entire back open exposing shoulder blades and butterfly bones; grey-green and white plaid high-waisted ultra-mini skirt with a high slit on the right side reaching the upper thigh; bare legs, no socks; white single-strap thick-sole slide sandals; woven straw tote bag with a mint-green silk scarf tied at the opening; thin silver chain with a small shell pendant; shell anklet; mint-green nail polish. Summer mesh knit and light cotton-linen, high skin exposure while keeping the JK uniform silhouette.

The character is completely motionless in world space: no limb movement of any kind — no arm, hand, finger, shoulder, head, neck, torso, leg, ankle or foot motion; no weight shift, no breathing, no blinking, no eye movement, no micro-movement. The body is frozen like a statue while retaining photorealistic skin, fabric and hair appearance. Camera parallax is the ONLY source of apparent movement.

One frozen instant. The character holds exactly ONE fixed pose for the entire shot, using the FRONT view's pose and prop-holding method as the sole reference. The side and back view descriptions from the reference sheet are for APPEARANCE ONLY (print layout, hair texture, garment cut) and must NOT be interpreted as pose or prop-position changes during the orbit. The straw tote bag stays in the same right hand, same height, same orientation for the entire 360° orbit; no transfer between hand and elbow, no swinging. Hair, braid texture, fabric folds, mesh, tote bag, shell pendant, shell anklet and sandal straps remain perfectly motionless while retaining photorealistic appearance.

Camera: one continuous, smooth, CONSTANT-SPEED 360-degree horizontal orbit at a FIXED radius and FIXED height, no zoom, no dolly, no cut, no handheld shake. Starts at FRONT view, ends at EXACTLY the same FRONT view. Total yaw = 360° exactly — no over-rotation, no under-rotation. Constant angular velocity throughout: no ease-in, no ease-out, no pause, no speed ramp. Subject stays centered with constant size for the whole shot. Camera parallax is the only source of apparent movement. No text.
```

**中文释义**：纯蓝幕 #0047BB、平光无投影；主体离幕≥1.5m、中性白轮廓光、无蓝溢、后期对网眼/浅色布边做 despill。角色严格按参考图：18 岁 JK 少女，168cm 纤细匀称；深黑褐单侧麻花辫垂右肩、碎刘海；薄荷绿白细格渔夫帽微压檐、眼下淡影；薄荷绿挂脖短背心、颈后细带蝴蝶结、正面小 V 领、整片露肩胛与蝴蝶骨；灰绿白格高腰超短裙、右侧高开衩至大腿上段；光腿无袜；白色一字带厚底凉拖；草编托特包口系薄荷绿丝巾；细银链小贝壳坠；贝壳脚链；薄荷绿指甲。网眼针织+轻薄棉麻、露肤高但保留 JK 轮廓。**肢体零动作**（无任何肢体/手指/头肩/腿脚/重心/呼吸/眨眼微动，如冻结雕像，仅镜头视差）。**冻结姿态**：仅以正面站姿与持法为准，侧/背仅作外观参考；托特包固定右手拎于体侧同高同朝向、不换手不摆荡；头发/辫纹/衣褶/网眼/包/贝壳坠/脚链/鞋带纹丝不动。**运镜四量**：起始正面、结束精确正面、总 yaw=360°、恒定角速度；固定半径高度、无变焦/推拉/剪辑/手持抖动，主体居中尺寸恒定。无文字。

---

# 三、负向提示词

```
limb movement, arm movement, hand movement, finger movement, shoulder movement, head turn, head tilt, nodding, weight shift, shifting stance, leg movement, ankle movement, foot movement, toe movement, knee bend, breathing, chest rise, blinking, eye movement, mouth movement, micro-movement, idle motion, swaying body, walking, gesturing, hair sway, fabric flutter, mesh tearing, handbag swinging, handbag switching hands, handbag moving to elbow, prop position change, prop swing, prop disappearing, wobble, shake, jitter, drift, fall, morph, outfit change, clothes transforming, warp, stretch, elongate, deformation, distorted limbs, extra fingers, melting, flicker, over-rotation, under-rotation, more than one turn, less than one turn, speed ramp, acceleration, deceleration, ease in, ease out, pause, hold, spin, zoom, dolly, cut, scene change, handheld, camera shake, blue rim light, blue color bleeding, blue tint on white fabric, blue spill on mesh, uneven blue, shadow on blue screen, text, watermark, subtitles, audio, missing accessory, extra accessory, print mismatch, hairstyle change, missing ring, wrong shoe
```

**中文释义**：动作/冻结类（肢体零动作全项 + 走动/手势/发丝飘/衣料飘/网眼撕裂/包摆荡/换手/移肘/道具位移/消失/抖动/形变/换装/闪烁）→ 运镜类（过转/欠转/多圈/少圈/变速/加减速/缓入缓出/停顿/旋转/变焦/推拉/剪辑/换场/手持/抖动）→ 背景类（仅蓝幕方案项：蓝溢/蓝染白布/网眼蓝溢/蓝幕不均/蓝幕投影）→ 文字/音频类 → 一致性类。**全文不含与已确认蓝幕方案冲突项，且已含肢体零动作负向项。**

---

# 四、输出交付模板

## 视频生成参数确认
- 模型：Seedance 2.0 Mini/Fast
- 总时长：5 秒
- 比例：3:4（竖屏，全身人物展示）
- 内容形态：通用创意视频（人物固定姿态 360° 环绕展示）
- 数量：1 条完整视频
- 素材：上传角色三视图设定图作为人物外观/服装/发型/配饰的唯一视觉参考（仅参考正/侧/背三个全身立绘，忽略图中文字档案、配色板和表情小图）
- 背景：**蓝幕 #0047BB（已锁定）+ 主体离幕≥1.5m + 中性白轮廓光 + 网眼/丝巾边缘 despill**（全文唯一真源）
- 肢体零动作：角色全片无任何肢体/手指/头肩/腿脚/重心/呼吸/眨眼微动作，仅镜头视差产生运动感
- 冻结姿态：正面站姿、右手拎草编托特包于体侧为唯一参照；三视图侧/背仅作外观参考，不作动作或道具位移指令
- 主线：角色在蓝幕上保持完全固定站立姿态，镜头以固定半径、固定高度做连续匀速 360° 水平轨道环绕，仅通过视差依次揭示正面→侧面→背面→侧面→正面
- 创意亮点：角色如冻结瞬间般完全静止——麻花辫、格纹裙褶、网眼背心、草编包、贝壳坠、脚链纹丝不动，唯一运动来自镜头环绕的纯视差；三视角薄荷绿格纹、露背结构、开衩位置、道具位置严格一致
- 开场推进：首帧正面全身居中，0.5 秒内镜头启动向右环绕，第 1 秒末呈现明显侧面（≈72°）
- 镜头与节奏计划：单镜头一镜到底，5 秒匀速完成精确 360° 环绕，起始与结束均为正面，主体始终居中、尺寸恒定
- 转场、运镜与特效计划：无转场；唯一运镜为固定半径固定高度水平轨道环绕；无粒子、无光影特效、无文字层；纯蓝幕平光、主体中性白轮廓光、无投影
- 核心信息：JK 少女 —— 薄荷绿白细格渔夫帽 / 薄荷绿挂脖露背短背心（颈后蝴蝶结、露肩胛） / 灰绿白格高腰超短裙（右侧高开衩） / 光腿 / 白色一字带厚底凉拖 / 草编托特包（薄荷绿丝巾） / 细银链贝壳坠 / 贝壳脚链 / 薄荷绿指甲
- 结尾收束：精确回到起始正面机位，累计旋转恰为 360°，全身造型完整呈现
- 画面文字/图形信息层：无
- 字幕：无
- 音轨：关闭（无旁白、无台词、无 BGM、无声效）

**分镜表（三行，角度对齐第 4 节；主语必须是镜头，不得写角色动作）：**

| 时间 | 核心画面与动作 | 台词/声音 | 运镜衔接 | 钩子与亮点 |
|---|---|---|---|---|
| B01 00:00–00:01（0°→72°） | 正面全身居中；薄荷绿格渔夫帽微压檐、单侧麻花辫垂右肩、挂脖背心小 V 领与颈后蝴蝶结、灰绿格裙高开衩、右手拎草编托特包于体侧；角色完全静止 | 无 | 定半径环绕启动，向右匀速 | 正面建立，首秒可见角度变化，人物完全静止 |
| B02 00:01–00:03（72°→216°） | 镜头到达侧面→**背面（≈2.5s/180°）**→另一侧；可见整片露背与肩胛骨、颈后系带蝴蝶结、裙后格纹与开衩边缘、麻花辫背面纹理、草编包侧面与丝巾；角色无任何动作 | 无 | 匀速持续，主体居中、尺寸恒定 | 露背与格纹背面细节揭示，三视角一致性 |
| B03 00:03–00:05（216°→360°） | 镜头经另一侧回到**精确正面**；帽檐淡影、贝壳坠、脚链、凉拖一字带全程纹丝不动 | 无 | 匀速收尾，精确回正 | 360° 闭环，一致性定格收束 |

---

# 五、生成前自检清单

- [x] **幕布推荐与确认**：已按 1.0 流程输出"首选蓝幕+备选灰幕+不推荐绿幕/白幕"及逐条理由；用户已确认采用蓝幕 #0047BB
- [x] **幕布与主体配色**：主体含大面积薄荷绿/灰绿/青绿 → 未选绿幕（改蓝幕）；含白色格纹/白凉拖 → 未选白幕
- [x] **背景五处一致**：正向、负向、参数确认、分镜、交付说明五处均为蓝幕 #0047BB，无绿幕/白幕/灰幕残留
- [x] **负向无冲突**：仅保留蓝幕溢色项，未同时出现绿幕/白幕正负向项
- [x] **防溢措施**：蓝幕方案已含主体离幕≥1.5m、中性白轮廓光、网眼/丝巾 despill 说明，并标注高风险材质
- [x] **肢体零动作**：正向写死无任何肢体/手指/头肩/腿脚/重心/呼吸/眨眼微动；负向含 limb/arm/hand/head/weight shift/breathing/blinking 等；分镜无动作动词、主语为镜头
- [x] **冻结姿态**：正向写死"一个冻结瞬间 + 仅以正面姿态/持法为准 + 其余角度仅作外观参考"
- [x] **道具锁定**：右手拎托特包于体侧、同高同朝向写死；负向含 handbag switching hands / moving to elbow / swing
- [x] **运镜四量**：起始正面、结束精确正面、总 yaw=360°、恒定角速度齐全
- [x] **运镜负向**：over/under-rotation、speed ramp、ease in/out、pause、spin、zoom、dolly、cut 齐全
- [x] **分镜角度对齐**：1s≈72°、2.5s≈180°（背面）、5s=360°（精确正面）；无"接近正面"模糊措辞
- [x] **外观逐项**：帽/背心/裙/开衩/光腿/凉拖/托特包/丝巾/贝壳坠/脚链/指甲逐项对应参考图，无增删
- [x] **参数对齐**：Seedance 2.0 Mini/Fast、5 秒、3:4、音频关闭、三视图参考
- [x] **交付说明**：已注明模型通道、蓝幕方案（含 despill 建议）、音频关闭

---