一种基于网络增强的基因功能相似性计算方法（拟南芥数据）

项目描述
本代码库包含论文《一种基于网络增强的基因功能相似性计算方法》在拟南芥（Arabidopsis thaliana）数据上的应用实现。方法基于拟南芥基因相互作用网络上的重启随机游走算法，整合 GO 注释与生化通路信息，计算基因功能相似性。

数据集信息
原始数据文件
- `aracyc_pathways.20230103` – AraCyc 数据库的 EC 分组数据（来源：TAIR - Arabidopsis）。
- `tair.gaf` – TAIR 的 GO 注释文件（来源：下载注释 |基因本体联盟）。
- `AraNet.txt` – 原始基因相互作用网络（来源：AraNet - 批量下载）。
- `AraNet_GS.txt` – 金标准基因对网络（来源：AraNet - 批量下载）。
- `go-basic.obo` – 基因本体术语总库（来源：下载本体）。

中间数据文件
- `lfcPair.txt` – LFC 评分基因对，源自 `aracyc_pathways.20230103` 预处理。
- `geneList.txt` – 筛选后的基因列表。
- `netMatrix.txt` – 网络邻接矩阵。
- `pMatrix.txt` – 归一化概率转移矩阵。
- `child.txt`– GO 后代术语集合。
- `son.txt`–GO中直接后代术语集合。

结果数据文件
- `result0.9.mat` – 重启参数为 0.9 时的随机游走结果矩阵。
- `similarityResult0.9.txt` – 重启参数为 0.9 时基因对的相似度得分。
- `lfc0.9.txt` – 各 EC 分组下的算法得分计算结果。

代码信息
| 类名 | 功能描述 |
|---------|---------------|
| `Annotation` | GO 注释信息数据模型 |
| `FunctionNet` | 本体术语网络数据模型 |
| `LfcPair` | LFC 基因对获取与预处理工具 |
| `Term` | GO 术语数据模型 |
| `Reader` | 文件读取辅助类 |
| `Matrix` | 矩阵运算辅助类 |
| `RandWalk` | 重启随机游走算法实现 |
| `Calculator` | 核心计算类，提供三个主要接口 |
| `Main` | 程序入口，串联完整流程 |

使用说明
1. 导入项目
打开 IntelliJ IDEA，选择`打开项目`。
选择包含源代码的根目录，按默认选项完成导入。
确保项目 SDK 设置为JDK 8 或更高版本。

2. 配置数据路径
将所有原始数据文件放置于项目根目录下的 `buf/` 文件夹中（或按代码中指定的路径）。其中，go-basic.obo直接置于该目录下，其他数据文件放置于`buf/Arabidopsis_thaliana/` 目录下。若路径不一致，请在 Main.java 或相关类中修改文件读取路径。若路径不一致，请在 `Main.java` 或相关类中修改文件读取路径。

3. 选择术语领域
  注意Main.main()中的Calculator.data_initializer(gene_type, alpha, "molecular_function");
一行的"molecular_function"参数，这说明此时该程序将选择"molecular_function"作为术语领域， 可将其改为"biological_process"以更换术语领域。

4. 运行程序
在项目视图中找到 `Main` 类（通常位于 src/ 目录下）。
右键点击 `Main` 类，选择 Run 'Main.main()'。
程序将按顺序执行数据加载、网络构建、随机游走、结果输出。

5. 结果输出
结果文件默认生成于`results/` 文件夹下。

运行环境要求
IntelliJ IDEA（Community 或 Ultimate 版均可）
JDK 8 或更高版本
内存建议：堆内存 ≥ 4 GB

方法步骤概述
算法流程在 Calculator 类中被划分为四个主要阶段：

数据加载与解析：读取 GO 层次结构（go-basic.obo）、注释文件（sgd.gaf）和相互作用网络（YeastNet.v3.txt）。

网络预处理：构建邻接矩阵并将其归一化为概率转移矩阵（pMatrix.txt）。

重启随机游走：利用 RandWalk 类对每一对基因执行随机游走计算。

结果评估：将相似度得分与基于通路的 EC 分组（LFC 评分）进行对比评估。

关于详细的数据预处理步骤，请参见论文正文“材料与方法”部分。

引用文献
 [作者列表]. 一种基于网络增强的基因功能相似性计算方法. PeerJ Computer Science, [年份].

许可证 & 贡献说明
本项目基于 [MIT 许可证](LICENSE) 开源。
