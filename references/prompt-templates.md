# 早期美式电视卡通风格提示词模板 v5

## 强制提示词顺序

所有路径必须按此顺序，不得把复杂度锁放到末尾：

1. `content_facts`：主体、关系、场景和剧情动作。
2. `reference_manifest`：真实图片、职责、必需性和附件状态。
3. **紧接场景主体**写：复杂度上限、层级数、主／次物件预算、大面积空白、弱透视、背景线密度。
4. 有角色时写角色几何锁。
5. 写可动层／固定背景层差异、综合色与光影。
6. 写有限自由创作。
7. 加载近期日本动漫场景负面模块及其他相关模块。
8. 写 `continuity_lock`、质量检查与用户明确输出规格。

正向只使用 `early American hand-drawn television animation`、`traditional cel animation with simplified painted background` 等明确描述；不得使用 `anime style`。禁用或改写：`丰富、精致、细腻、沉浸式、电影感、氛围感、充满生活细节`。用户要求复杂世界时写成：`主题丰富但表现概括，用少量代表物、分组剪影和大综合色块说明世界，不增加无剧情功能的小物或纹理。`

## 内容事实表

```yaml
content_facts:
  story_world: "{用户世界；未指定时原创，不得取自内置图}"
  time_period: "{时代}"
  location: "{地域与地点}"
  culture: "{文化语境}"
  genre: "{类型与基调}"
  subject_and_action: "{人物、关系、一个主要动作及结果}"
  scene_content: "{动作所需建筑、环境和道具}"
  plot: "{剧情事实、因果与结束状态}"
  composition: "{用户指定构图；未指定则使用平视、侧向或轻微三分之四视角}"
  preserve: "{全部冻结项}"
```

## 真实参考选择与绑定

- 原创人物：真实附加 `assets/reference-01-character-geometry-v5.jpg`。
- 纯场景：真实附加 02–05 至少一张。
- 人物场景：真实附加 01 和 02–05 至少一张。
- 植物优先 03；暖综合色优先 04；红橙粉固有色／晚霞优先 05；冷综合色／分层优先 02。
- 每张场景图的 `role` 必须逐字包含或等义明确表达：**复杂度上限 + 线密度上限 + 色块面积关系**；其冷暖、植物或晚霞职责只能作为附加职责。
- 按美学职责选图，不按厨房、公园、街角等内容相似性选图。

```yaml
reference_manifest:
  - path: "assets/reference-01-character-geometry-v5.jpg"
    role: "仅参考角色共同基础几何、线条与可动层线条；不复制具体身份、服装、人物关系、场景或道具"
    required: true
    attached: false
  - path: "{实际选择的02–05路径}"
    role: "场景复杂度上限 + 线密度上限 + 色块面积关系；{追加冷暖／植物／晚霞职责}；不复制具体内容"
    required: true
    attached: false
selected_assets:
  - "assets/reference-01-character-geometry-v5.jpg"
  - "{实际选择的02–05路径}"
reference_input_mode: "real_image_reference"
attachment_instruction: "把 selected_assets 中每个 JPEG 作为真实 image reference/image input 附加；不得只把路径写进文本。调用后核验实际载荷，再更新 attached。"
```

删除不适用项和占位行。仅产出提示词时 `attached: false` 表示待执行。能力不支持图片参考时保持 `false`，改为 `reference_input_mode: text_only_fallback` 并明确“仅文字近似：当前生成能力不支持真实图片参考”。

## 场景复杂度与空间硬锁

```yaml
scene_complexity_lock:
  main_activity_zone: "单镜头只有一个主活动区，人物与关键动作居中；不设竞争焦点"
  spatial_layers: "大型空间通常不超过3层：前景遮挡层／主要活动层／简化背景层；不要求凑满"
  primary_visual_objects: "约3–7个，只留剧情和地点识别必需的大物件"
  secondary_small_objects: "尽量0–5个；无剧情功能则删除"
  repeated_objects: "同类只用2–4个代表，其余合并为色块或剪影，禁止铺满"
  quiet_area: "保留大面积空墙、地面、天空或单综合色背景；安静大色块占主导"
  object_internal_lines: "主要物件通常只保留2–3条识别线；背景更少且不逐物闭合"
  complex_world_rule: "主题丰富但表现概括；按少量建筑群、设备组、摊位组或植物团分组，不逐件描绘，不解除预算"
scene_space_lock:
  default_view: "平视、侧向或轻微三分之四视角"
  depth: "浅空间、舞台式横向展开、弱透视、少消失线"
  background_function: "固定绘景只服务人物表演，不作沉浸式世界展示"
  user_view_override: "用户明确指定其他视角时服从，但仍减少透视线、空间层和细节"
scene_line_lock:
  movable_layer: "角色和可动物件使用较深、清楚、基本封闭的赛璐珞轮廓"
  fixed_background: "固定背景少线、浅线、不完全闭合，许多边缘只靠大综合色块"
  density_hierarchy: "背景线密度、对比和饱和度低于人物与主要道具"
  material_limit: "不画大量砖缝、木纹、瓦片、叶片、草叶、餐具、书脊、按钮或窗格"
```

## 角色、综合色与自然硬锁

```yaml
character_geometry_lock:
  body_ratio: "约2.5头身；头约占全身40%；肩宽明显小于头宽；短躯干短肢"
  face: "近圆或略纵长头；点眼；醒目圆鼻或C形鼻；单根宽大弧嘴；脸内留白多"
  per_character: "逐角色锁定 head_shape / nose_shape / eye_spacing / mouth_width_ratio"
  continuity: "跨视角、跨图、多镜头不切换几何"
rendering_lock:
  medium: "early American hand-drawn television animation; traditional cel animation with simplified painted background"
  color: "中等饱和、中等对比、大块固有色；背景综合色更安静"
  shadow: "通常0–1层扁平综合色，只在必要时保留"
  light: "无复杂局部光源、动态光斑、写实反射高光、景深、体积光或黄昏辉光"
  sunset: "参考05仅允许画面下部或地平线附近一条克制综合色带，不做戏剧化天空"
plant_lock:
  trees: "每棵树仅用少量不对称综合色团；远树为综合色剪影"
  grass: "草地为大色块，不铺密集草线"
  clouds: "少量、平面、简单边缘；保留大面积天空"
```

## 文生图

```text
内容事实与场景主体：{content_facts；先写人物、一个主要动作、动作所需场景}。
真实参考与职责：{reference_manifest；按强制组合真实附加}。
立即锁定场景：{scene_complexity_lock + scene_space_lock + scene_line_lock}。
角色几何：{有角色时填写 character_geometry_lock}。
综合色与光影：{rendering_lock；有植物时填写 plant_lock}。
自由创作：{仅补足身份与剧情必需空缺；复杂世界必须“主题丰富但表现概括”}。
必须保留：{身份、人数、世界、时代、文化、关键道具、服装、文字、剧情和用户构图}。
禁止复制：{内置图具体身份、地点、物件、时代、事件和构图}。
负面模块：{recent_japanese_animation_scene + 相关人物／植物模块}。
连续性：{continuity_lock}。质量检查：{thumbnail_test + deletion_test + drift_test}。
输出规格：{仅用户或上游明确值}。
```

## 图生图

```text
内容事实与场景主体：{从用户文字和图片提取 content_facts}。
用户图片职责：{逐张说明 identity/content/composition/text/preserve}。
真实内置参考职责：{reference_manifest；与用户图一起作为真实图像输入}。
立即锁定场景：{scene_complexity_lock + scene_space_lock + scene_line_lock}。
角色几何与渲染：{character_geometry_lock + rendering_lock + 适用 plant_lock}。
必须保留：{身份、人数、关系、姿势、构图、文字、关键道具、建筑功能、服装、剧情、冻结区}。
允许改变：{获准风格化的几何抽象、线条、平涂、综合色、背景边缘、细节层级或光线}。
重绘规则：不是滤镜；把可动对象重绘为较深、清楚、基本封闭的赛璐珞轮廓；把固定背景重绘为少线浅线、开放边缘和大综合色块；删去无剧情功能的小物和纹理。
优先级：用户明确事实与 preserve > 用户内容图 > 用户构图图 > 内置美学图。
负面模块：{recent_japanese_animation_scene + 相关模块}。连续性与检查：{continuity_lock + 三项测试}。
```

## 单人／群像／角色设定

使用文生图或图生图模板。角色后方即使只是背景，也必须套用场景复杂度锁。每个原创角色至少三个身份锚点；01 只需真实附加一次，但逐角色分别锁定几何。人物必须压过环境，背景不得抢脸部轮廓和动作姿态。

## 场景设定

```text
场景主体：{世界、时代、文化、地点功能、一个主活动区及剧情用途}。
真实场景参考：{至少一张02–05；role 明确为复杂度上限 + 线密度上限 + 色块面积关系，并真实附加}。
立即锁定场景：{scene_complexity_lock + scene_space_lock + scene_line_lock}。
综合色与自然：{rendering_lock + 适用 plant_lock}。
设计方法：只用少量大形、代表物、分组剪影和识别线说明内容；背景信息不足是主动简化。
复杂世界转译：主题丰富但表现概括，不逐建筑、逐设备、逐摊位或逐装饰描绘。
负面与检查：{recent_japanese_animation_scene + thumbnail_test + deletion_test + drift_test}。
连续性与规格：{continuity_lock；仅明确输出值}。
```

## 剧本／分镜

先写不依赖内置图的剧情节拍。每个未来生成单元都写：一个主要动作、一个主活动区、通常至多三层、物件预算、安静色块、弱透视和背景线密度；再写镜头。人物单元附 01，场景单元附至少一张 02–05，人物场景两者都附。复杂地点用一两个代表性建立镜头交代，不通过增加背景细节说明世界。

## 文生视频

```text
全局内容与主体：{content_facts}。
真实参考：{reference_manifest；按强制组合真实附加并核验}。
全局场景锁：{scene_complexity_lock + scene_space_lock + scene_line_lock}。
全局角色与渲染：{character_geometry_lock + rendering_lock + 适用 plant_lock}。
镜头 {编号}：
- 起始状态：{人物、道具、简化环境和光线}。
- 唯一主要动作：{准备—主要动作—结果—短暂停顿／衔接；不得加入第二主要动作}。
- 少量环境响应：{仅与动作直接相关的一至少数变化；不使用粒子群或复杂光变}。
- 镜头：{固定或克制移动，服务动作可读；禁止漫游、航拍、长距离推拉或通过运动展示大量背景}。
- 结束状态：{下一镜头继承状态}。
- 风格保持：{一个主活动区、至多三层、物件预算、安静大色块、背景少线浅线}。
负面与检查：{recent_japanese_animation_scene + video + 三项测试}。
连续性与规格：{continuity_lock；仅明确输出值}。
```

## 图生视频

```text
内容与首帧：{用户文字及首帧中的身份、内容、构图、动作起点和 preserve}。
真实参考：{reference_manifest；与首帧共同传入并核验}。
立即锁定场景：{scene_complexity_lock + scene_space_lock + scene_line_lock}。
角色与渲染：{character_geometry_lock + rendering_lock + 适用 plant_lock}。
起始状态：严格继承首帧。
唯一主要动作：{准备—主动作—结果—短暂停顿／衔接}。
环境响应：{少量必要反馈}。
镜头：固定或克制，不能借运动补充或展示大量背景信息。
结束状态：{完整可继承状态}。
负面、连续性与检查：{recent_japanese_animation_scene + video + continuity_lock + 三项测试}。
```

## 多镜头连续性

所有镜头共用同一复杂度、空间、线密度和附件职责锚点。每镜头保持一个主活动区、一个主要动作、少量环境响应；通常至多三层。不能随镜头推进累积小物、纹理、粒子或光效。镜头结束状态成为下一镜头起始状态；如需新地点，重新建立同等简化预算，而不是提高细节。

## 局部风格迁移

```text
内容事实：{未授权维度全部冻结}。
真实参考：{角色区域含01；场景区域含至少一张02–05并标注三项上限职责}。
必须保留：{身份、世界、时代、地点、文化、建筑功能、物品、服装、背景、构图、镜头、剧情、文字及未指定原画风}。
允许变化：仅{目标区域或维度}。
场景锁：{只要目标涉及环境就应用完整 scene_complexity_lock / scene_space_lock / scene_line_lock}。
重绘：不是滤镜；只重绘获准维度，其余冻结。
```

## 用户指定的既有角色设定边界

用户提供或明确指定既有角色设定时，只继承用户要求保留的身份与外形锚点；不从参考资产补入其他角色、关系、物件、地点、时代或桥段。环境仍执行完整场景复杂度锁与真实附件规则。

## 模块化负面提示

```yaml
negative_modules:
  recent_japanese_animation_scene: "高密度建筑或街景，复杂多点透视，广角畸变，低机位仰角，精细室内陈设，无数电线、招牌、窗格，写实云层，樱花花瓣，逆光轮廓光，镜头光晕，空气透视，景深虚化，体积光，玻璃或金属写实高光，精致食物特写，复杂天气粒子，电影级色彩脚本，日漫式黄昏辉光，背景大量描边和材质纹理，背景与角色同等粗黑闭合精细"
  scene_density: "多个竞争活动区，超过必要空间层，重复物铺满，密集砖缝、木纹、瓦片、叶片、草叶、餐具、书脊、按钮或窗格，无剧情功能的小物，沉浸式世界展示"
  character: "头横向压扁，写实人体比例，肩宽过大，大眼或眼距漂移，鼻部弱化或下坠，小写实嘴、双唇、嘴宽漂移，群像同脸，未由用户指定的既有角色"
  plant: "逐片叶子，密集草线，圆球树，整齐波浪树冠，写实云层，花瓣，繁密枝叶，精细自然背景"
  rendering: "纯机械矢量，完全等粗线，强烈手抖，频繁断线，多重描边，粗糙水彩，厚纸纹，光滑3D，厚涂，数字渐变，写实反射高光"
  video: "多个主要动作，镜头漫游展示背景，复杂环境响应，粒子群，角色几何闪变，服装道具漂移，动作融化，肢体增殖，背景呼吸或形变，轴线跳变，结束状态断裂"
```

## continuity_lock

```yaml
continuity_lock:
  content_world_anchor: "{用户世界、时代、地域、文化、类型、场景和剧情}"
  character_geometry_anchor: "{逐角色头型、鼻型、眼距、嘴宽比例}"
  identity_anchor: "{身份、发型、服装、固有色、关系和身份锚点}"
  prop_anchor: "{关键道具身份、功能、固有色、尺寸、归属和位置}"
  scene_complexity_anchor: "一个主活动区；通常至多3层；主物件约3–7；次要小物尽量0–5；重复物2–4个代表；大面积安静背景"
  scene_space_anchor: "{平视／侧向／轻微三分之四；浅空间；弱透视；少消失线，或用户视角的简化版本}"
  scene_line_anchor: "可动层深而较闭合；固定背景少线、浅线、开放边缘；背景低对比低线密度"
  motion_anchor: "每镜头一个主要动作、少量环境响应；镜头不展示大量背景"
  reference_manifest_anchor: "{实际路径、职责、required、attached、reference_input_mode；场景图含三项上限职责}"
  forbidden_reference_content: "不复制内置图具体身份、地点、建筑、陈设、设施、时代、剧情或构图"
```

## 调用前后与质量检查

```yaml
preflight:
  selected_assets_nonempty: "{true/false}"
  all_paths_exist: "{true/false}"
  character_has_01: "{true/false/not_applicable}"
  scene_has_02_to_05: "{true/false/not_applicable}"
  scene_asset_role_has_three_limits: "{复杂度上限、线密度上限、色块面积关系是否齐全}"
  one_activity_zone_and_budget_locked: "{true/false}"
  content_replacement_test_passed: "{true/false}"
postflight:
  tool_supports_image_reference: "{true/false}"
  actual_call_carried_all_selected_images: "{true/false/not_executed}"
  manifest_attached_states_verified: "{true/false/not_executed}"
quality_tests:
  thumbnail_test: "缩小后是否首先读到角色、动作和2–4个大色块；若先读到纹理、密集物件或光效则失败"
  deletion_test: "删除30%–50%背景小物是否不影响剧情；若不影响，删除这些小物后重做"
  japanese_animation_drift_test: "是否命中 recent_japanese_animation_scene；命中任一项则简化重做"
  fallback_notice: "{空，或：仅文字近似：当前生成能力不支持真实图片参考}"
```

任一适用附件组合、三项场景职责、场景预算或质量测试失败时，不得标记完成。无论文字描述是否充分，只要工具支持，仍必须真实附加参考图。
