一种基于网络增强的基因功能相似性计算方法（Python 实现）
======================================================================

项目描述
--------
本代码库为论文《一种基于网络增强的基因功能相似性计算方法》的 Python 语言实现版本。
代码实际使用部分包括：第一部分（随机游走相关性得分矩阵计算）与第四部分（结果数据绘图）。
其中人类基因数据实验仅完成了随机游走相关性得分矩阵的计算部分。

本实验覆盖三种模式生物数据：
    - 酿酒酵母 (Saccharomyces cerevisiae)
    - 拟南芥 (Arabidopsis thaliana)
    - 人类 (Homo sapiens)

========================================================================
数据集信息
========================================================================

一、酵母 (Yeast) 数据
--------------------
原始数据文件：
    `biochemical_pathways.tab` – 用于通路分析的酶学委员会（EC）分组数据（来源：http://sgd-archive.yeastgenome.org/）。
`sgd.gaf` – 酵母基因的基因本体注释文件（来源：https://current.geneontology.org/products/pages/downloads.html）。
`YeastNet.v3.txt` – 原始酵母基因相互作用网络（用于构建 `netHashMap`，来源：https://www.functionalnet.org/yeastnet/）。
`YeastNet.v3.benchmark.txt` – 黄金标准基准基因对（用于生成 `geneList.txt`，来源：https://www.functionalnet.org/yeastnet/）。
`go-basic.obo` – 基因本体术语总库文件（来源：https://www.geneontology.org/docs/download-ontology/）。

中间数据文件：
    lfcPair.txt                      - LFC 评分基因对，源自 biochemical_pathways.tab 预处理
    geneList.txt                     - 筛选后基因列表，源自 YeastNet.v3.benchmark.txt
    netMatrix.txt                    - 原始基因网络邻接矩阵
    pMatrix.txt                      - 归一化概率转移矩阵
    child.txt                        - GO 后代术语集合
    son.txt                          - GO 直接后代术语集合

结果数据文件：
    result0.9.mat                    - 重启参数 0.9 的随机游走结果矩阵
    similarityResult0.9.txt          - 重启参数 0.9 的基因对相似度得分
    lfc0.9.txt                       - 各 EC 分组下的 LFC 评估得分
    yeast_biological_process.zip     - biological_process 领域结果打包
    yeast_molecular_function.zip     - molecular_function 领域结果打包

绘图所需其他算法结果文件：
    0.9.txt
    18.txt
    bo.txt
    lfcresult18.txt


二、拟南芥 (Arabidopsis thaliana) 数据
--------------------------------------
原始数据文件：
aracyc_pathways.20230103        - AraCyc 数据库 EC 分组数据（来源：https://www.arabidopsis.org/download/list?dir=Pathways%2FArchived_Data_dumps%2FPMN9_September2014）
注：数据获取困难：官方需申请 + 专用软件，TAIR 仅存 2014 年旧版，国内多数网站无法访问，建议使用配套预处理文件。
    tair.gaf                         - TAIR 基因本体注释文件（来源：https://current.geneontology.org/products/pages/downloads.html）。
    AraNet.txt                       - 原始基因相互作用网络（来源：https://www.functionalnet.org/aranet/download.html）。
    AraNet_GS.txt                    - 金标准基因对网络（来源：https://www.functionalnet.org/aranet/download.html）。
    go-basic.obo                     - 基因本体术语总库（来源：https://www.geneontology.org/docs/download-ontology/）。

中间数据文件：
    lfcPair.txt                      - LFC 评分基因对，源自 aracyc_pathways.20230103 预处理
    geneList.txt                     - 筛选后基因列表，源自 AraNet_GS.txt
    netMatrix.txt                    - 原始基因网络邻接矩阵
    pMatrix.txt                      - 归一化概率转移矩阵
    child.txt                        - GO 后代术语集合
    son.txt                          - GO 直接后代术语集合

结果数据文件：
    result0.9.mat                    - 重启参数 0.9 的随机游走结果矩阵
    similarityResult0.9.txt          - 重启参数 0.9 的基因对相似度得分
    lfc0.9.txt                       - 各 EC 分组下的 LFC 评估得分
    Arabidopsis_thaliana_biological_process.zip   - biological_process 领域结果
    Arabidopsis_thaliana_molecular_function.zip   - molecular_function 领域结果

重要提醒：拟南芥 EC 分组数据（aracyc_pathways）获取极困难官方完整库：https://plantcyc.org/downloads/，需填写许可协议、下载专用软件，国内访问与下载极不稳定。TAIR 存档：
https://www.arabidopsis.org/download/list?dir=Pathways%2FArchived_Data_dumps%2FPMN9_September2014，仅能获取 2014 年旧版数据，无新版完整文件。复现本实验时，建议直接使用项目已提供的预处理后 lfcPair.txt，避免重新下载原始 EC 数据。


三、人类 (Human) 数据
--------------------
原始数据文件：
    PubChem_pathway_text_human.csv  - PubChem 来源的 EC 分组数据
    goa_human.gaf                  - 人类基因本体注释文件 (来源: https://current.geneontology.org/products/pages/downloads.html)
    HumanNet-XC.tsv             - 原始基因相互作用网络（来源：https://www.functionalnet.org/humannet/download.html）
    HumanNet-GSP.tsv               - 金标准基因对网络（来源：https://www.functionalnet.org/humannet/download.html）
    go-basic.obo                    - 基因本体术语总库（来源：https://www.geneontology.org/docs/download-ontology/）

中间数据文件：
    geneList.txt                     - 筛选后基因列表，源自 HumanNet-GSP.tsv
    netMatrix.txt                    - 原始基因网络邻接矩阵
    pMatrix.txt                      - 归一化概率转移矩阵

结果数据文件：
    result0.9.mat                    - 重启参数 0.9 的随机游走结果矩阵


========================================================================
代码文件说明
========================================================================

Annotation          注释信息数据模型
FunctionNet         本体术语网络数据模型
Term               本体术语数据模型
Reader             特殊类型文件读取辅助工具类
PrintResult          画图辅助工具类
PrintMap           数据绘图类
RandWalk           随机游走算法类
Calculator           核心计算类，按算法流程划分为四个部分，提供三个主要接口
Main               算法启动类，组合三个接口串联整个流程


========================================================================
使用说明 (Python 环境)
========================================================================

本程序在 PyCharm 集成开发环境中编写与运行，以下为导入与运行步骤。

1. 环境准备
   - 安装 PyCharm 社区版或专业版。
   - 确保已安装 Python 3.12 或更高版本（推荐使用 Anaconda 发行版）。
   - 在 PyCharm 中配置好 Python 解释器（File -> Settings -> Project -> Python Interpreter）。
   - 确保已安装CUDA_Toolkit。

2. 安装依赖库
   本程序依赖以下 Python 库：
       - numpy
       - scipy
       - pandas
       - matplotlib
       - seaborn
       - network
       - cupy
       - scikit-learn

   在 PyCharm 中安装依赖的推荐方式：
       - 打开 PyCharm 的终端窗口（View -> Tool Windows -> Terminal）。
       - 执行命令：pip install -r requirements.txt
   若项目根目录无 requirements.txt，可手动执行：
       pip install numpy scipy pandas matplotlib seaborn networkx

3. 导入项目
   - 打开 PyCharm，点击 "Open" 或 "File -> Open..."。
   - 选择包含本代码的根目录文件夹，点击 "OK" 完成导入。
   - 等待 PyCharm 完成索引与虚拟环境识别。

4.数据路径配置
提醒：拟南芥原始 EC 数据获取困难，建议直接使用项目提供的 lfcPair.txt，无需重新下载原始文件。
   - 请将对应物种的原始数据文件按照以下结构放置于项目 buf/ 目录下：
         buf/yeast/
         buf/arabidopsis/
         buf/human/
   - 若代码中数据读取路径为绝对路径或特定相对路径，请在 Main.py 或相关配置文件中修改为实际存放路径。

5. 选择术语领域
  注意Main.main()中的Calculator.data_initializer(gene_type, alpha, "molecular_function");
一行的"molecular_function"参数，这说明此时该程序将选择"molecular_function"作为术语领域， 可将其改为"biological_process"以更换术语领域。

6. 运行程序
   - 在 PyCharm 项目工具窗口中，展开目录找到主入口文件 Main.py（或 Main 类所在脚本）。
   - 右键点击 Main.py，选择 "Run 'Main'"。
   - 程序将依次执行：
         a. 读取 GO 本体文件并构建术语层次关系
         b. 读取对应物种的基因注释文件
         c. 读取基因相互作用网络，构建邻接矩阵与概率转移矩阵
         d. 执行重启随机游走算法，计算基因相似度得分矩阵
         e. 输出结果文件及绘图所需数据文件
   - 运行过程中的日志与错误信息将显示在 PyCharm 下方的 "Run" 窗口中。

7. 绘图
   - Python 版功能限制：本版本仅验证并使用「随机游走相关性得分矩阵计算」与「结果绘图」模块；基因对相似度计算、LFC 评分代码未实际运行测试，无法保证可正常执行，请谨慎使用。
   - 相似度得分计算完成后，可运行 PrintResult.py 或 PrintMap.py（根据实际文件名调整）生成图表。
   - 操作方式同样为右键点击对应脚本文件，选择 "Run"。

8. 注意事项
   - 人类数据实验仅完成了随机游走得分矩阵的计算，未进行后续评估与绘图。
   - 处理大型网络（如人类 HumanNet）时，请确保 PyCharm 的内存设置足够（Help -> Edit Custom VM Options 中可调整 -Xmx 参数）。

========================================================================
方法步骤概述
========================================================================

1. 数据加载与解析：
   - 解析 GO 结构文件 go-basic.obo，生成 child.txt 与 son.txt。
   - 解析对应物种的 .gaf 注释文件，建立基因与 GO 术语的映射关系。
   - 解析基因相互作用网络文件，构建无向/有向网络图。

2. 网络预处理：
   - 将相互作用网络转换为邻接矩阵 (netMatrix.txt)。
   - 对邻接矩阵进行行归一化，生成概率转移矩阵 (pMatrix.txt)。

3. 重启随机游走：
   - 调用 RandWalk 类，针对基因对计算重启随机游走下的稳态分布相似度。
   - 输出结果矩阵文件 (result0.9.mat) 及成对相似度文件 (similarityResult0.9.txt)。

4. 评估与绘图（酵母和拟南芥）：
   - 基于 EC 分组文件计算 LFC 得分 (lfc0.9.txt)。
   - 绘制不同本体领域下的性能对比图。


========================================================================
数据可用性与引用
========================================================================

本研究所用的原始数据文件（除 GO 本体库外）均来源于各公共数据库：
   - YeastNet:  https://www.functionalnet.org/yeastnet/
   - AraNet:    https://www.functionalnet.org/aranet/download.html
   - HumanNet:  https://www.functionalnet.org/humannet/download.html
   - GO 本体:   https://www.geneontology.org/docs/download-ontology/
   - TAIR:      https://www.arabidopsis.org/
   - SGD:       https://www.yeastgenome.org/
   - GOA:       https://www.ebi.ac.uk/GOA

支撑本研究复现的输入数据文件已作为补充材料随文提交（或上传至公共数据仓库）。

若使用本代码或数据，请引用以下文献：
   [作者列表]. 一种基于网络增强的基因功能相似性计算方法. PeerJ Computer Science, [年份].


========================================================================
许可证
========================================================================

本项目基于 MIT 许可证开源。详细信息请参见 LICENSE 文件。
