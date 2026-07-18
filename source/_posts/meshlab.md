---
title: meshlab
date: 2025-08-20 11:11:21
tags:
---
### 网址链接

- Meshlab 修补建议（英文）：
	- <https://www.utilities-online.info/blogs/how-to-cleanup-and-refine-meshes-in-meshlab>
- Meshlab 入门指南（中文/文档整理）：
	- <https://wenku.csdn.net/column/5fikxfpejm>
- meshlab 官方网站：
    - <https://www.meshlab.net/>
### 功能概览

![](/image_meshlab/menu.png)

下面是对应的菜单项及过滤器的中文说明

### - `Show current filter script`
	- 显示当前过滤器脚本
![](/image_meshlab/script.png)

#### 对话框功能详解
- 列表区（中间大方框）
显示当前脚本中的过滤器（按顺序排列）。每一行是一条“过滤器调用（带当前参数）”。可以看到同一个过滤器多次出现在脚本中（表示会多次运行，参数可不同）。
- Move Up / Move Down
将选中的过滤器项在脚本中上移或下移，改变执行顺序。顺序至关重要：前面的过滤器会影响后面过滤器的输入。
- Remove
删除选中的脚本项（从流水线中移除该步骤）。
- Edit Parameters
编辑选中过滤器的参数，会弹出该过滤器的参数对话框，让你调整阈值、选项等，编辑后脚本项会保存这些参数。
- Save Script
把当前过滤器序列导出为脚本文件（便于复用或在其他项目/机器上重放）。（通常以 MeshLab 的脚本格式保存为 XML/脚本文件，可以在下次用 Open Script 载入。）
- Open Script
加载已保存的过滤器脚本并显示在列表中。
- Clear Script
清空当前列表，重置脚本编辑区。
- Apply Script
按列表顺序执行脚本中的所有过滤器（对当前活动图层/选择的网格应用）。执行时会使用列表中保存的参数；部分过滤器在运行时若未完整参数可能仍弹出对话。
- Close
关闭脚本编辑对话框（不自动保存脚本，除非你点了 Save Script）。

#### 一般流程

1. 在主界面用 Duplicate Layer 复制当前网格，保留备份。
2. 打开脚本编辑器，按你想要的处理流程把一系列过滤器加入列表（或先用 GUI 单次操作，然后在脚本窗口用 Save Script 导出）。
3. 选中每个过滤器，点 Edit Parameters 细调参数（特别是会删除或改变拓扑的过滤器）。
4. 使用 Move Up/Down 调整顺序，确保依赖关系正确（例如先修复非流形，再删除孤立小片段，再做简化）。
5. 先在副本上点 Apply Script 运行一次，观察结果；若满意就 Save Script 备用。
6. 若需对多个模型批量执行，可把脚本保存，再用 MeshlabServer / PyMeshLab 或 MeshLab 的脚本载入批处理。


### - `Selection`
	- 选择（Selection）

![](/image_meshlab/selection.png)

Conditional Face Selection
条件性面选择（按条件选择面）
Conditional Vertex Selection
条件性顶点选择（按条件选择顶点）
Delete ALL Faces
删除所有面
Delete Selected Faces Del
删除所选面 Del
Delete Selected Faces and Vertices Shift+Del
删除所选面与顶点 Shift+Del
Delete Selected Vertices Ctrl+Del
删除所选顶点 Ctrl+Del
Dilate Selection Ctrl+Shift++
膨胀选择（扩张选区） Ctrl+Shift++
Erode Selection Ctrl+Shift+-
腐蚀选择（收缩选区） Ctrl+Shift+-
Invert Selection Ctrl+Shift+I
反选 Ctrl+Shift+I
Select 'problematic' faces
选择“有问题”的面（自动标记的异常面）
Select All Ctrl+Shift+A
全选 Ctrl+Shift+A
Select Border
选择边界（边界环）
Select Connected Faces
选择连通面（与所选面连通的一组面）
Select Convex Hull Visible Points
选择凸包上可见点（可见的凸包点）
Select Faces by Color
按颜色选择面
Select Faces by view angle
按视角选择面（根据面法线与视线夹角）
Select Faces from Vertices
从顶点选择面（由选中的顶点反选相关面）
Select Faces with edges longer than...
选择边长超过…的面
Select None Ctrl+Shift+D
清除选择（不选任何） Ctrl+Shift+D
Select Outliers
选择异常点/离群面（噪声/离散片段）
Select Self Intersecting Faces
选择自相交面
Select Vertex Texture Seams
选择顶点处的纹理缝（UV 接缝处顶点）
Select Vertices from Faces
从面选择顶点（由选中的面反选其顶点）
Select by Face Quality
按面质量选择（根据质量指标筛选面）
Select by Vertex Quality
按顶点质量选择（根据质量指标筛选顶点）
Select non Manifold Edges
选择非流形边
Select non Manifold Vertices
选择非流形顶点
Select small disconnected component
选择小的断开连通分量（小独立片段）
    


### - `Cleaning and Repairing`
	- 清理与修复

![](/image_meshlab/Cleaning.png)
Merge Close Vertices
合并接近的顶点：把空间上非常接近的顶点按容差合并为一个，清理重叠/冗余顶点。
Merge Wedge Texture Coord
合并楔形纹理坐标：将同一顶点在不同面的分离 UV 合并，减少纹理接缝。
Remove Duplicate Faces
删除重复面：移除位置/顶点集合完全相同的重复面。
Remove Duplicate Vertices
删除重复顶点：去除位置相同或几乎相同的顶点并修正面索引。
Remove Isolated Folded Faces by Edge Flip
通过翻转边移除孤立折叠面：用边翻转尝试修复或去除小的折叠/扭曲面片。
#### Remove Isolated pieces (wrt Diameter)
![](/image_meshlab/RIPD.png)
按直径移除孤立片段：根据组件几何尺寸（直径）阈值删除很小的独立连通块。
- Enter max diameter of isolated pieces (abs and %)
- 左侧（world unit / 绝对距离）：以模型坐标单位直接输入“最大允许直径”。凡直径小于等于此值的连通组件会被删除。
- 右侧（perc）：以模型尺寸的相对百分比输入（相对于模型的参考长度，通常是包围盒对角线）。示例：如果包围盒对角线 = 1.04277，则 perc = 10 → 对应绝对阈值 ≈ 0.104277（截图即为此关系）。公式：阈值_abs = bbox_diag * (perc / 100)。
- Remove unreferenced vertices（复选框）
含义：删除被移除组件后，是否同时清除不再被任何面引用的顶点（通常勾选以清理残留悬空顶点）。
- PyMeshLab Filter / meshing_remove_connected_component_by_diameter
对应脚本名；可点击 “Copy PyMeshLab call to clipboard” 导出可复现命令，便于批处理。
- Default / Help / Close / Apply
Default：恢复默认参数；Help：查看帮助；Apply：执行；Close：关闭对话框。

##### Remove Isolated pieces (wrt Face Num.)

![](/image_meshlab/RIPF.png)
按面数移除孤立片段：根据面数量阈值删除小的独立连通块。
- Enter minimum conn. comp size（最小连通组件面数）
这是“最小连通组件面数”的阈值（以三角面/多边形面计）。所有面数小于该值的独立连通组件（即与主体不连通的小岛）将被删除。换句话说：把这个数值当作“保留组件的最小面数”，低于它的连通块会被清掉。  
示例：若设为 `25`，则任何只有 `24` 面或更少的独立小块都会被删除；`25` 面及以上的连通块会被保留。

- Remove unreferenced vertices（复选框）
含义：在删除那些小连通块之后，是否同时删除不再被任何面引用的顶点（悬空顶点）。  


- PyMeshLab Filter
名称：meshing_remove_connected_component_by_face_number 
说明：这是对应的脚本调用名称。点击 “Copy PyMeshLab call to clipboard” 可以复制可复现的脚本命令，便于批处理或记录参数。

- 一般取值
非常保守（只删单个三角 / 极小片段）：1–5
常规清理（删噪声小岛）：20–200（视模型复杂度和总面数而定）  
 激进清理（仅保留主体 / 删除大多数小组件）：将阈值设为比第二大连通块面数更大的值，或直接使用“保留最大连通块”策略

- 快速估算
先运行一次统计（Compute Connected Components）或查看图层信息，得到各连通块的面数分布；然后根据分布选择阈值（例如：去掉小于中位数或低频的小块）。


Remove T-Vertices
删除 T 型顶点：消除边与边中点相连形成的 T 交点，改善拓扑一致性。
Remove Unreferenced Vertices
删除未被引用的顶点：移除不被任何面引用的孤立顶点。
Remove Vertices wrt Quality
按质量移除顶点：依据面/三角形质量（如退化、细长）移除导致低质量的顶点或元素。
Remove Zero Area Faces
删除零面积面：移除面积为零或近似零的退化面。
Repair non Manifold Edges
修复非流形边：处理被 3 个或以上面共享的边等拓扑问题。
Repair non Manifold Vertices by splitting
通过拆分修复非流形顶点：将冲突的顶点拆分为多个以消除拓扑异常。
Select Self Intersecting Faces
选择自相交面：定位与选中与自身或其他面发生相交的面片。
Snap Mismatched Borders Alt+   对齐不匹配的边界（快捷键 Alt+）：把相邻补丁/网格间不对齐的边界顶点吸附对齐以修复接缝。


### - `Create New Mesh Layer`
	- 新建网格图层
![](/image_meshlab/new_mesh.png)

Annulus
圆环 — 生成环状平面/圆环网格。
Box/Cube
盒子 / 立方体 — 生成长方体或立方体基元。
Cone
圆锥 — 生成圆锥体。
Dodecahedron
十二面体 — 正十二面体基元。
Dodecahedron (symmetric)
对称十二面体 — 对称化处理的十二面体。
Fit a plane to selection
对选区拟合平面 — 用平面拟合当前选中的点/面。
Fractal Terrain
分形地形 — 生成分形噪声驱动的地形网格。
Grid Generator
网格/格子生成器 — 生成规则网格（格子）平面。
Icosahedron
二十面体 — 正二十面体基元。
Implicit Surface
隐式曲面 — 基于隐函数生成的曲面（如 SDF）。
Noisy Isosurface
带噪声的等值面 — 在噪声场上提取的等值面，用于测试/效果演示。
Octahedron
八面体 — 正八面体基元。
Points on a Sphere
球面点集 — 在球面上均匀/指定分布生成点集。
Sphere
球体 — 生成球面或实心球网格。
Sphere Cap
球冠（球帽）— 球体的一部分（切去后的帽状部分）。
Structure Synth Mesh Creation
StructureSynth 网格创建 — 使用 StructureSynth 风格规则生成结构化网格（基于规则语法的结构合成）。
Tetrahedron
四面体 — 正四面体基元。
Torus
环面 / 圆环面 — 生成圆环形托罗斯网格（甜甜圈形状）。

### - `Remeshing, Simplification and Reconstruction`
	- 重网格化、简化与重建

![](/image_meshlab/rebuild.png)

Alpha Complex/Shape
Alpha 复形/Alpha 形状 — 基于点集构建 alpha 复杂/形状（拓扑/几何近似）。
Alpha Wrap
Alpha 包装 — 用 alpha 算法生成封装表面。
Build a Polyline from Selected Edges
从所选边构建折线 — 将选中边转为折线/多段线。
##### Close Holes
填补孔洞 — 自动封闭网格中的孔洞。
![](/image_meshlab/closehole.png)
- Max size to be closed
能被自动关闭的最大孔洞“边界长度”阈值（以边的数量/边环长度计）。例如 30 表示边界由 ≤30 条边组成的孔洞会被填补，超过的孔洞不会自动关闭。
- Close holes with selected faces（复选）
仅对当前选中的面所在的连通区域中的孔洞执行关闭操作（用于局部修补）。
- Select the newly created faces（复选）
填补后自动选中新生成的面，便于立刻检查或对新面进行后处理（如平滑、重新计算法线）。建议勾选用于验证结果。
- Prevent creation of selfIntersecting faces（复选）
尝试避免产生自相交三角面（若可能，会采取更保守的生成策略）。一般保持勾选以减少拓扑错误。
- Refine Filled Hole（复选）
对填补得到的面进行优化/细化，使新生成面更均匀或边长更接近周围网格。若不勾选，生成的面可能比较粗糙。
- Hole Refinement Edge Len (abs and %)（绝对/百分比）
细化时目标边长，左为绝对世界单位，右为相对于模型尺度（通常是包围盒对角线）的百分比。用来控制新网格的密度/细分等级。
- PyMeshLab Filter 名称：meshing_close_holes（可以复制脚本以复现


Convex Hull
凸包 — 生成点云或网格的凸包。
Create Solid Wireframe
生成实体线框 — 由边生成可视化的实体线框结构。
Cubic stylization
立方风格化 — 将网格风格化为更“方/立方”外观。
Curvature flipping optimization
曲率翻转优化 — 与曲率/法线方向相关的拓扑/几何优化。
Cut mesh along crease edges
沿折痕边切割网格 — 在锐边/折痕处切割以分离片段或创建缝。
Generate Scalar Harmonic Field
生成标量谐波场 — 计算网格上的标量谐波场（用于参数化/分割等）。
Global Align Meshes
全局对齐网格 — 对多模型进行全局配准对齐。
ICP Between Meshes
网格间 ICP（迭代最近点）对齐 — 用 ICP 算法配准两个网格/点云。
Iso Parametrization Build Atlased Mesh
等参化：构建带图集的网格 — 生成带纹理图集的参数化网格。
Iso Parametrization Remeshing
等参化重网格化 — 基于参数化实施重网格化。
Iso Parametrization transfer between meshes
等参化：在网格间传递参数化 — 将参数化/纹理从一个网格转移到另一个。
Iso Parametrization: Main
等参化：主控面板 — 等参化主功能入口。
Make mesh developable
使网格可展（可展开为平面） — 将网格处理为可展曲面（便于制造/折纸等）。
Marching Cubes (APSS)
马尔奇立方体（APSS）— 基于 APSS 的体素等值面重建。
Marching Cubes (RIMLS)
马尔奇立方体（RIMLS）— 基于 RIMLS 的等值面重建。
Mesh Boolean: Difference
网格布尔：差集 — 网格差集运算。
Mesh Boolean: Intersection
网格布尔：交集 — 网格交集运算。
Mesh Boolean: Symmetric Difference (XOR)
网格布尔：对称差（XOR） — 网格异或运算。
Mesh Boolean: Union
网格布尔：并集 — 网格并集运算。
Planar flipping optimization
平面翻转优化 — 与面朝向/平面翻转相关的优化处理。
Points Cloud Movement
点云移动 — 对点云进行平移/变换或运动模拟。
Refine User-Defined
精化（用户定义） — 对用户定义区域或参数进行细化处理。
Remeshing: Isotropic Explicit Remeshing
重网格化：各向同性显式重网格化 — 生成更均匀三角网格。
Select Crease Edges
选择折痕边 — 自动选择网格中的尖锐/折痕边。
Simplification: Clustering Decimation
简化：聚类降面 — 基于聚类的网格简化方法。
Simplification: Edge Collapse for Marching Cube meshes
简化：用于 Marching Cube 网格的边塌缩 — 为体素网格设计的塌缩简化。
Simplification: Quadric Edge Collapse Decimation
简化：二次型边塌缩降面 — 常用的四舍简化算法（保形损失小）。
Simplification: Quadric Edge Collapse Decimation (with texture)
简化（含纹理）：二次型边塌缩 — 同时考虑纹理坐标的简化。
Subdivision Surfaces: Butterfly Subdivision
曲面细分：Butterfly 算法 — 一种插值细分算法（保留原顶点位置）。
Subdivision Surfaces: Catmull-Clark
曲面细分：Catmull–Clark — 常用细分曲面算法（用于四边形网格）。
Subdivision Surfaces: Doo Sabin
曲面细分：Doo–Sabin — 四边形细分算法的一种。
Subdivision Surfaces: LS3 Loop
曲面细分：LS3 Loop — Loop 类细分的变体。
Subdivision Surfaces: Loop
曲面细分：Loop — 常用三角网格细分算法。
Subdivision Surfaces: Midpoint
曲面细分：中点细分 — 基于中点的简单细分方法。
Surface Reconstruction: Ball Pivoting
表面重建：球体铰接（Ball Pivoting） — 从点云重建网格的一种方法。
Surface Reconstruction: Screened Poisson
表面重建：Screened Poisson — Poisson 重建的改进方法，常用于扫描点云重建。
Surface Reconstruction: VCG
表面重建：VCG（库）方法 — 使用 VCGlib 提供的重建算法。
Tri to Quad by 4-8 Subdivision
三角转四边：4→8 细分法 — 通过细分将三角网格转为四边形为主。
Tri to Quad by smart triangle pairing
三角转四边：智能三角配对 — 通过配对三角形生成四边面。
Turn into Quad-Dominant mesh
转换为四边主导网格 — 将网格转换为以四边为主的拓扑。
Turn into a Pure-Triangular mesh
转换为纯三角网格 — 转成全部三角形的网格结构。
Uniform Mesh Resampling
均匀网格重采样 — 重新采样以得到更均匀分布的顶点/三角。
Vertex Attribute Seam
顶点属性接缝 — 处理或标记顶点属性（如 UV/颜色）产生的接缝。
Voronoi Filtering
Voronoi 过滤 — 基于 Voronoi /权重场的滤波或重构操作。



### - `Polygonal and Quad Mesh`
	- 多边形与四边形网格



### - `Color Creation and Processing`
	- 颜色生成与处理
    ![](\image_meshlab\smooth.png)

### - `Smoothing, Fairing and Deformation`
	- 平滑、整形与变形
Craters Generation
隕石坑生成 — 生成类似坑洞/凹陷的表面细节（用于地形效果）。
Depth Smooth
深度平滑 — 对深度场/距离场进行平滑处理，去除噪声。
Directional Geometry Preservation
方向性几何保留 — 平滑时保留主要方向/锐边特征的约束方法。
Export to Sketchfab
导出到 Sketchfab — 将模型打包并上传/导出为 Sketchfab 可发布格式。
Fractal Displacement
分形位移 — 用分形噪声对顶点施加位移，生成自然粗糙表面。
HC Laplacian Smooth
HC 拉普拉斯平滑 — Humphrey–Chew（HC）改进的拉普拉斯平滑，能更好保留细节。


Laplacian Smooth
拉普拉斯平滑 — 基本的邻域平均平滑（会产生收缩/模糊）。


Laplacian Smooth (surface preserving)
拉普拉斯平滑（表面保留） — 带表面/体积保留约束的拉普拉斯平滑，减少收缩。


MLS projection (APSS)
MLS 投影（APSS）— 基于 APSS 的移动最小二乘投影，用于点云/网格重构和平滑。
MLS projection (RIMLS)
MLS 投影（RIMLS）— 基于 RIMLS 的投影方法，另一种 MLS 重构变体。
Per Vertex Geometric Function
每顶点几何函数 — 在顶点层面计算/应用几何函数（用于变形或分析）。
Random Vertex Displacement
随机顶点位移 — 对顶点施加随机扰动（模拟噪声或制造粗糙感）。
ScaleDependent Laplacian Smooth
缩放相关拉普拉斯平滑 — 根据局部尺度自适应的拉普拉斯平滑。
Smooth Face Normals
平滑面法线 — 平滑面法线向量以改善光照/渲染或用于后续滤波。
Smooth Vertex Quality
平滑顶点质量 — 平滑/滤波顶点质量属性（quality attribute）。

##### Taubin Smooth
- Taubin 平滑 — 无收缩的拉普拉斯平滑方法，通过交替滤波减小体积损失。
- Lambda（λ）
含义：第一步低通滤波的缩放系数（正值）。增大 λ 会增强第一次滤波的平滑强度。
常见范围/建议：0.2 ～ 1.0；常用默认约 0.5。截图中 1 表示比较强的第一次滤波。
- mu（μ）
含义：第二步低通滤波的缩放系数（通常为负值），用于抵消第一次滤波造成的收缩并形成近似无收缩效果。
常见范围/建议：常用约 −0.52 ～ −0.6，截图中 −0.53 是经典的经验值。
- Smoothing steps（迭代次数）
含义：执行多少次 λ/μ 的一轮组合（每一步包含一次 λ 滤波和一次 μ 滤波）。
建议：小规模噪声 5–10；中等 10–30；过多会过度平滑并丢失细节。
- Affect only selected faces（仅作用于选中面）
含义：只对当前选中的面应用平滑，未选区域保持不变。用于局部修复。
- Preview（预览复选框）
含义：勾选后在应用之前显示实时预览（减慢响应），便于调参。


TwoStep Smooth
两步平滑 — 分两步/两阶段执行的平滑流程（通常先粗后细）。
UnSharp Mask Color
颜色反锐化掩膜（Unsharp Mask）— 在颜色通道上增强局部对比以锐化视觉细节。
UnSharp Mask Geometry
几何反锐化掩膜 — 在几何层面增强局部细节（类似锐化几何特征）。
UnSharp Mask Normals
法线反锐化掩膜 — 通过处理法线来增强视觉上的细节/对比。
UnSharp Mask Quality
质量反锐化掩膜 — 基于质量属性的锐化/增强过滤器。
Vertex Linear Morphing
顶点线性形变 — 按线性方式对顶点位置进行形态混合或插值变形

### - `Quality Measure and Computations`
	- 质量测量与计算
    
![](\image_meshlab\quanlity.png)

Clamp Vertex Quality
夹紧顶点质量 — 将顶点质量值限制在给定范围内（裁剪异常值）。
Colorize by approximated geodesic distance from the selected points
按从选定点近似测地距离着色 — 根据顶点到所选点的近似测地距离生成颜色映射。
Colorize by border distance
按边界距离着色 — 根据顶点到网格边界的距离着色（用于可视化边界影响）。
Colorize by geodesic distance from a given point
按指定点的测地距离着色 — 以某一给定点为源，按测地距离着色。
Colorize by geodesic distance from the selected points
按所选点的测地距离着色 — 以所选点集为源按测地距离着色。
Compute Ambient occlusion
计算环境遮蔽 — 计算 AO 值以评估遮挡/光照暗部（用于可视化）。
Compute Area/Perimeter of selection
计算选区面积/周长 — 统计当前选中区域的面积与周长。
Compute Geometric Measures
计算几何度量 — 计算多种几何指标（面积、边长、曲率等）。
Compute Obscurance
计算遮蔽程度（Obscurance） — 计算表面可见性/遮挡度量（与 AO 类似）。
Compute Planar Section
计算平面截面 — 在指定平面上提取截线/截面。
Compute Shape-Diameter Function
计算形状直径函数（SDF） — 用于分割与粗细度分析的形状度量。
Compute Topological Measures
计算拓扑度量 — 统计拓扑信息（连通分量、孔洞、Euler 特征等）。
Compute Topological Measures for Quad Meshes
为四边网格计算拓扑度量 — 针对四边形网格的拓扑统计。
Create Selection Perimeter Polyline
从选区创建周界折线 — 将选区边界导出为多段线/折线。
Geometric Cylindrical Unwrapping
几何圆柱展开 — 将圆柱状表面展开为平面参数化（用于纹理展开）。
Overlapping Meshes
重叠网格检测 — 检测并标记网格间或网格自重叠区域。
Per Face Quality Function
面质量函数 — 计算并保存每个面的质量度量函数。
Per Face Quality Histogram
面质量直方图 — 面质量分布可视化统计。
Per Face Quality Stat
面质量统计 — 输出面质量的统计量（均值、方差等）。
Per Face Quality according to Triangle shape and aspect ratio
按三角形形状与纵横比的面质量 — 根据三角形形状/长宽比评估面质量。
Per Vertex Quality Function
顶点质量函数 — 计算并保存每个顶点的质量度量。
Per Vertex Quality Histogram
顶点质量直方图 — 顶点质量分布可视化统计。
Per Vertex Quality Stat
顶点质量统计 — 输出顶点质量的统计量（均值、方差等）。
Quality Mapper applier
应用质量映射器 — 使用质量映射规则将质量值映射并写入属性。
Quality from raster coverage (Face)
从栅格覆盖得到面质量 — 根据栅格/影像覆盖计算面级质量。
Quality from raster coverage (Vertex)
从栅格覆盖得到顶点质量 — 根据栅格/影像覆盖计算顶点级质量。
Reorient face normals by geometry
按几何重定向面法线 — 根据几何一致性修正面法线朝向（统一外法线/内法线）。
Saturate Vertex Quality
饱和值顶点质量 — 将顶点质量按阈值饱和（截断极值）。
Select by Face Quality
按面质量选择 — 根据面质量阈值选择面。
Select by Vertex Quality
按顶点质量选择 — 根据顶点质量阈值选择顶点。
Transfer Quality: Face to Vertex
传递质量：从面到顶点 — 将面级质量插值/传递到顶点属性。
Transfer Quality: Vertex to Face
传递质量：从顶点到面 — 将顶点级质量聚合/传递到面属性。
Vertex Quality from Camera
来自相机的顶点质量 — 根据相机可见性/视角从相机数据计算顶点质量（例如可视性评分）。




### - `Normals, Curvatures and Orientation`
	- 法线、曲率与朝向




### - `Mesh Layer`
	- 网格图层




### - `Raster Layer`
	- 光栅/栅格图层




### - `Range Map`
	- 距离图 / 深度图（根据上下文可选译法）




### - `Point Set`
	- 点集




### - `Sampling`
	- 采样




### - `Texture`
	- 纹理




### - `Camera`
	- 相机




### - `Other`
	- 其他





