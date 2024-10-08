# 基于NB-IoT的环境监测系统设计
# 1 绪论
## 1.1 背景和意义
21世纪，伴随医疗技术的突飞猛进，疾病治疗的空间尺度也在不断地缩小。当治疗技术深入到人体最基本的单位——细胞的层面时，便可以从根源上为患者解除病痛，达成更为彻底的治疗效果，因此细胞治疗成为了异于传统药物治疗和手术治疗的新热点。细胞治疗，指将（深）低温保存的人体健康细胞通过移植或是静脉注射的方式实现更新，进而促进人体重建组织的这一种治疗方法。这一技术的出现，为恶性肿瘤、原发性高血压、阿尔兹海默症等多种难以治愈的疾病提供了新的技术可能。

细胞存储作为细胞治疗技术产业链的上游基础，其供给的细胞质量高低，关系细胞治疗技术的发展。目前，广泛使用的生物细胞保存技术是超低温细胞冷冻存储技术。它是在低温（77K）条件下，通过抑制细胞生命活动，从而达到长期存储的目的，并且在解冻后仍能使得细胞保持生理活性的一种技术。

低温具有两面性，它既可以让生物样品处于低代谢状态，长期存储并保持活性，又能够在保存的过程中损伤细胞。在低温保存的过程中，样品的损伤主要来源自溶质损伤和机械损伤：
1) 溶质损伤。溶质损伤是由于温度曲线下降过慢导致的，此时水发生凝固，胞间游离的金属离子浓度突变增大；对于蛋白质等生物大分子而言，空间结构可能会被破坏，造成变性，引发功能不可逆损伤。
2) 机械损伤。机械损伤通常由冰晶生长的机械力引起。冰晶在细胞内和溶质中均存在，并且冰晶的体积与细胞存活率呈负相关。
上述两种冷冻损伤与温度梯度变化率关系密切，冰晶的形成和溶质浓度的改变亦受降温速度影响，是优化（深）低温保存的重要方向。
## 1.2 国内外发展现状
### 1.2.1 自动化样本库系统
国际上在近十几年内开展了一系列生物样本低温环境自动化存储的的研究和实践。以Biolix STC(Li CONi C,列支敦士登)193K自动化智能存储系统为例,其展示了自动化样本库系统的基本架构,组成的硬件包括热交换区、输送机械臂、程控降温仪(冰箱或液氮罐),均接受样本管理软件传达的指令。
### 1.2.2 玻璃化保存技术
对于RNA、组织等珍贵样品，玻璃化保存则更为普遍，这要求环境温度低于137K；玻璃化保存时生化反应都趋近于停止。上海理工大学已经在生物样本的深低温存储方面积累了雄厚的理论基础，主要包括：置核-程序降温热控制方法，低温保护剂的热物性，生物体低温保存过程热分析。
## 1.3 目前存在的问题
1) 现有玻璃化保存技术逐渐普及，但大多数程控降温设备的温度监测范围仍局限在223K至193K之间，无法覆盖玻璃化保存所需的137K以下的温度范围。
2) 市场上出现了以机械臂为核心的自动化样本处理设备，极大地降低了操作人员发生冻伤的风险。然而，目前多数此类设备依赖进口，采购成本高昂。
3) 虽然现有的样本库系统具备与计算机的通信功能，但数据主要在本地进行处理和存储，不利于样本信息的共享，也缺乏便捷的远程报警机制。
## 1.4 本课题主要工作
本课题针对当前生物样本库在温度监测领域的现状，提出了一种基于窄带物联网（NB-IoT）的温度监测系统，旨在提升深低温保存技术的应用效果及系统的整体智能化水平。具体内容如下：
1) 应用温度检测范围下限低于137K的温度传感器，适配玻璃化保存技术。
2) 提供多种常见通信接口，以提高系统的兼容性和扩展性，避免样本中心重新采购昂贵设备的必要。
3) 引入窄带物联网技术和云数据托管平台，实现对温度异常等事件的主动上报，并支持通过短信推送提醒功能。
# 2 系统方案设计
## 2.1 温度采集原理
在计算机发明之前，人类主要依赖度量衡工具和自身感官来获取被测物体的各类信息，随后使用纸笔记录数据。然而，随着科学研究的深入以及工业生产需求的增加，数据处理的复杂性和精确度要求逐渐提高，传统的手动数据采集和记录方式显然已无法满足这些需求。

为了克服传统数据采集和记录方法的局限性，计算机和传感器应运而生。计算机作为一种用于高速运算的电子设备，能够自动化地处理大量数据，从而显著提升了人类的计算能力。然而，计算机只能处理二进制形式的电信号，而现实中大多数信号为非电量信号。因此，如果仍采用人工方式将这些信号输入计算机，数据处理的效率将大大降低。这就需要引入传感器技术。传感器是一种检测装置，能够感知被测物体的物理量信息，并将这些信息按照特定规律转换成电信号或其他所需形式（如频率或脉冲信号）。通过传感器，非电量信号被转换为电信号，再经过模数转换器将这些电信号（模拟信号）转换为数字信号（如二进制数），从而实现与计算机的有效连接与数据处理。

温度传感器是传感器技术中的重要组成部分，主要分为接触式和非接触式两大类。接触式温度传感器包括热电偶和热敏电阻，这些传感器需与被测物体直接接触；而非接触式温度传感器则通过检测物体所产生的热辐射来测量温度。
1) 热电偶是一种基于塞贝克效应的温度传感器，利用两种不同金属在接合点处的温差产生电压来测量温度。1821年，德国科学家塞贝克发现，当两种不同金属连接成回路，并在两端接点处施加不同温度时，回路中会产生电流，称为热电流，而产生的电势则称为热电势。热电偶由两个不同的导体或半导体材料构成闭合回路，当两接点（测量端与冷端）存在温差时，回路中产生的热电势与温差成正比。通过测量热电势，可以确定测量端的温度。
2) 热敏电阻
热敏电阻由半导体材料构成，其电阻率对温度变化极为敏感。根据电阻率随温度变化的不同特性，热敏电阻可分为正温度系数（PTC）热敏电阻和负温度系数（NTC）热敏电阻。PTC热敏电阻的电阻值会随温度升高而增加；相反，NTC热敏电阻的电阻值会随温度升高而降低。热敏电阻的特性常用Steinhart-Hart方程来表征，该方程将热敏电阻的电阻值转换为开尔文温度值。
3) 电阻式温度检测器（RTD）
电阻式温度检测器（RTD）是一种基于金属电阻随温度变化而改变的特性来测量温度的传感器。RTD的工作原理是利用导体或半导体的热阻效应，即电阻值随温度变化而变化，通过测量其电阻值来推算温度。常用的RTD材料为铂（如Pt100），因其具有优良的稳定性和易于获取的优点。
4) 红外线温度传感器
红外线温度传感器（或称辐射温度计）的原理是通过透镜将物体发射的0.7 µm至20 µm波长的红外线聚焦到热电堆检测元件上。热电堆是一种能够吸收红外线并产生与温度相对应电信号的检测元件。该电信号经过放大和发射率补偿后，显示物体的温度。
## 2.2 热电偶分析
塞贝克效应表明，由两种导体A和B再两端焊接构成的闭合回路，当其两接点保持某一温度差时，回路中将有电流流过。此回路称为热电回路，其中出现的电流称为热电流。在热电回路中存在一种电动势，称为热电动势$E_{AB}$，通常称为温差电动势。回路中的热电源$I$的大小决定于热电动势和回路中的电阻。

现在将回路断开，如图所示，图中导体A、B分别称为热电极A和热电极B。我们假设用$E_A$和电阻$R_A$来代表热电极A，用$E_B$和$R_B$来代表热电极B。如图就是用电位差计测量$V_{ab}$的等效电路。当达到平衡时$I=0$,此时有
$$\Delta V=E_{ab}=E_B-E_A \tag{1}$$
$(1)$式表明，用电位差计测得的电压降$\Delta V_{ab}$就是热电回路$(AB)$中的热电动势$E_{AB}$。由$(1)$式可知，此时热电回路中的热电动势$E_{AB}$与回路中电阻无关，仅与热电回路两接点间的温度差$\Delta T=T_2-T_1$有关。定义$\Delta V_{ab}$对$\Delta T$的微分热电动势为热电回路的热电势率，用$S_{AB}$表示，有
$$S_{AB}=\lim_{\Delta T \to 0}\frac{\Delta V_{ab}}{\Delta T}=\frac{dV_{ab}}{dT} \tag{2}$$
$S_{AB}$也称为塞贝克系数，从$(2)$式可以看出它代表该热电回路的灵敏度。考虑到$(1)$式，$(2)$式也可以写为
$$S_{AB}=\lim_{\Delta T \to 0}\frac{E_B}{\Delta T}-\lim_{\Delta T \to 0}\frac{E_A}{\Delta T}=S_B-S_A \tag{3}$$
式中$S_A$和$S_B$分别为热电极A、B的绝对热电势率。可见，某热电回路$(AB)$的塞贝克系数$S_{AB}$的正负和大小仅决定于构成此热电回路的两个热电极的绝对热电势差之差。
如果某热电回路的$S_{AB}$是已知的，则该回路的热电动势$E_{AB}$可由$(2)$式积分求得：
$$E_{AB}=\int_{T_0}^T{S_{AB}(T)}{\rm d}T \tag{4}$$
考虑到$(3)$式，上式又可表为
$$E_{AB}=\int_{T_0}^T[S_B(T)-S_A(T)]{\rm d}T \tag{5}$$
如果在有限温度范围内$(S_B-S_A)$基本恒定不变，则$(5)$式可表示为
$$E_{AB}=(S_B-S_A)\Delta T \tag{6}$$
式中$\Delta T=T_2-T_1$，表示热电回路两接点间的温度差。在实践中要应用$(6)$式来求某热电回路的热电动势值，必须假设两种热电极才是A、B的热电性是均匀的，两种热电极材料的绝对热电势差之差$(S_B-S_A)$与温度无关。这样，如果将热电回路中的一个接点保持在某一个恒定不变的温度（例如冰点）$T_0$时，则$E_{AB}$便仅随另一个接点的温度$T$而变化。也就是说，此时$E_{AB}$仅仅是温度$T$的单值函数。这样，我们可以利用这种$E_{AB}$与$T$的一一对应的关系来测量温度。

不同金属配对就构成不同的热电偶。每种热电偶的$E-T$特性常用表格、曲线的关系表示。在实验室中精确测定热电偶的$E-T$特性的程序称为分度，由此而得到的$E-T$对用表称为该种热电偶的分度表。

热电偶的两个热电极在测量端相互连接，在参考端分别与铜导线连接，铜导线的另一端分别与电测仪表的正、负接线柱连接。如图所示的就是热电偶测温的基本热电回路。
## 2.3 环境监测系统设计框图
![](D:/Project_Travis/git_temperatureMonitor/IMG/SYS.svg)
1) 核心处理器：负责接收来自温度采集模块的数据。处理器将对这些数据进行计算处理，接着把经过处理的数据进行打包，通过NB-IoT模块发送至云平台。
2) NB-IoT/WiFi模块：NB-IoT和WiFi模块作为终端设备与云服务器之间的桥梁，主要负责无线数据传输。NB-IoT模块以其低功耗和广覆盖的特点，适合用于长期的温度监测和数据上传；WiFi模块则提供了局域网内的数据传输选项，适用于需要高数据传输速率和实时性的应用场景。
3) 云平台：云平台作为系统的管理中心，承担着数据存储、分析和报警等多项功能。所有监测数据通过NB-IoT或WiFi模块上传至云端，实现对多个冻存架的统一温度管理和监控。云平台不仅支持实时数据的展示和分析，还允许用户设定多种参数阈值以监控温度曲线的变化，确保各个冻存架的温度始终处于设定范围内。一旦检测到温度梯度超出预设的安全阈值，系统将立即触发报警机制，通过多种方式（如短信或邮件）通知管理人员，确保及时响应和处理异常情况。同时，云平台提供数据历史记录和分析工具，以便用户对过去的温度变化进行回顾和研究，为进一步的优化和改进提供依据。
4) 温度采集模块：负责环境温度的实时采集。该模块使用热电偶传感器，将环境温度转化为电信号。然后，这些电信号经过滤波器和放大器的调理处理，再通过模数转换器（ADC）转换为数字信号，核心处理器接收这些数字信号进行进一步计算处理。
5) 电源模块：电源模块为系统提供稳定的电力支持，兼容插电和电池供电两种模式。插电模式下，系统可以持续运行，同时插电接口兼作电池充电口，确保电池在断电情况下提供备份电源。通过BUCK电路，系统能够将输入电压转换为各个模块所需的不同工作电压，确保各组件的正常运行。
6) 通讯模块：负责将核心处理器的数据按照指定的通讯协议发送至通信端口。该模块支持多种通信协议，以确保与其他设备的兼容性。任何支持相应通讯协议的设备均可通过有线连接即时查看和分析数据。
7) 用户交互模块：用户交互模块是系统与用户之间的直接接口，旨在提供直观、便捷的操作体验。该模块包含多种硬件交互设备，有彩色液晶显示屏、按键、蜂鸣器和RGB状态指示灯。彩色液晶显示屏用于显示系统的实时数据和状态信息，按键提供用户输入的功能，蜂鸣器用于报警提示，RGB状态指示灯则显示系统的当前工作状态，如此实现终端声光报警。
8) 文件模块：文件模块负责存储和管理核心处理器生成的数据，提供本地数据备份功能。该模块能够存储多达50,000组数据，确保在通信中断或其他特殊情况下，数据仍能被完整保存。
# 3 系统的硬件层设计
## 3.1 温度传感器选择
在本研究中，针对低温环境中液体温度的监测，经过详细的比较分析和与导师的讨论，最终选定了K型热电偶作为温度测量的核心传感器。

本研究要求系统具备低功耗特性。对于常见的电阻式温度传感器，例如热敏电阻和RTD（电阻温度检测器），其工作原理决定了其必须依赖外部电源供电以进行温度检测。这种依赖导致了热耗和漏电流的产生，不符合本课题对低功耗的要求。而热电偶则利用热电效应直接产生电动势，无需额外电源，从而降低了系统的功耗。

红外线温度传感器虽然具有非接触测温的便利性，但其测量范围通常集中在正温度区域，且更适用于远距离测温。在本研究中，测量环境为低温液体，要求传感器必须直接接触测量介质。红外传感器在此情境下的应用受限，因而被排除在外。

在确定采用热电偶作为温度传感器的基础上，本研究进一步探讨了不同类型热电偶的适用性。通过对比不同厂家生产的热电偶的适用测温范围，最终筛选出适用于本研究的两种热电偶类型：T型热电偶和K型热电偶。

T型热电偶以其优越的低温测量性能著称，然而其较高的成本使其在经济性上相对劣势；相较之下，K型热电偶不仅在测温范围上能够满足本研究的需求，其成本亦较为低廉，约为T型热电偶的三分之一。因此，综合考虑测温性能与经济性，本研究最终选择了K型热电偶作为温度传感器。
## 3.2 信号调理模块设计
将热电偶产生的电压变换成精确的温度读数并非轻松的事情，原因是多方面的：电压信号太弱，温度电压关系呈非线性，需要参考接合点补偿，且热电偶可能引起接地问题。下文将逐一分析这些问题。

电压信号太弱:最常见的热电偶类型有J、K和T型。在室温下，其电压变化幅度分别为52 μV/°C、41 μV/°C和41 μV/°C。其它较少见的类型温度电压变化幅度甚至更小。

因为电压信号微弱，信号调理电路一般需要约40dB左右的增益，这是相当简单的信号调理。更棘手的事情是如何识别实际信号和热电偶引线上的拾取噪声。热电偶引线较长，经常穿过电气噪声密集环境。引线上的噪声可轻松淹没微小的热电偶信号。

一般结合两种方案来从噪声中提取信号。第一种方案使用差分输入放大器（如仪表放大器）来放大信号。因为大多数噪声同时出现在两根线上(共模)，差分测量可将其消除。

第二种方案是低通滤波，消除带外噪声。低通滤波器应同时消除可能引起放大器整流的射频干扰（1 MHz以上）和50 Hz/60 Hz(电源) 工频干扰。在放大器前面放置一个射频干扰滤波器（或使用带滤波输入的放大器）十分重要。50Hz/60Hz滤波器的位置无关紧要—它可以与RFI滤波器组合放在放大器和ADC之间，作为∑-Δ ADC滤波器的一部分，或可作为均值滤波器在软件内编程。

参考接合点补偿：要获得精确的绝对温度读数，必须知道热电偶参考接合点的温度。当第一次使用热电偶时，这一步骤通过将参考接合点放在冰池内来完成。但对于大多数测量系统而言，将热电偶的参考接合点保持在冰池内不切实际。大多数系统改用一种称为参考接合点补偿（又称为冷接合点补偿）的技术。

参考接合点温度使用另一种温度敏感器件来测量—一般为IC、热敏电阻、二极管或RTD（电阻温度测量器）。然后对热电偶电压读数进行补偿以反映参考接合点温度。必须尽可能精确地读取参考接合点—将精确温度传感器保持在与参考接合点相同的温度。任何读取参考接合点温度的误差都会直接反映在最终热电偶读数中。

电压信号非线性:热电偶响应曲线的斜率随温度而变化。例如，在0°C时，T型热电偶输出按39 μV/°C变化，但在100°C时斜率增加至47 μV/°C。有三种常见的方法来对热电偶的非线性进行补偿。

选择曲线相对较平缓的一部分并在此区域内将斜率近似为线性，这是一种特别适合于有限温度范围内测量的方案，这种方案不需要复杂的计算。K和J型热电偶比较受欢迎的诸多原因之一是它们同时在较大的温度范围内灵敏度的递增斜率（塞贝克系数）保持相当恒定。

另一个方案是将查找表存储在内存中，查找表中每一组热电偶电压与其对应的温度相匹配。然后，使用表中两个最近点间的线性插值来获得其它温度值。

第三种方案使用高阶等式来对热电偶的特性进行建模。这种方法虽然最精确，但计算量也最大。每种热电偶有两组等式。一组将温度转换为热电偶电压（适用于参考接合点补偿）。另一组将热电偶电压转换成温度。在参考集合点处于任何其它温度时，必须使用参考接合点补偿。

接地要求：
设计热电偶信号调理时应在测量接地热电偶时避免接地回路，还要在测量绝缘热电偶时具有一条放大器输入偏压电流路径。此外，如果热电偶尖端接地，放大器输入范围的设计应能够应对热电偶尖端和测量系统地之间的任何接地差异。

对于非隔离系统，双电源信号调理系统一般有助于接地尖端和裸露尖端类型获得更稳定的表现。因为其宽共模输入范围，双电源放大器可以处理PCB（印刷电路板）地和热电偶尖端地之间的较大压差。如果放大器的共模范围具有在单电源配置下测量地电压以下的某些能力，那么单电源系统可以在所有三种尖端情况下获得满意的性能。要处理某些单电源系统中的共模限制，将热电偶偏压至中间量程电压非常有用。这完全适合于绝缘热电偶简单或整体测量系统隔离的情况。但是，不建议设计非隔离系统来测量接地或裸露热电偶。
![](D:/Project_Travis/git_temperatureMonitor/IMG/amp.jpg)
实用热电偶解决方案：热电偶信号调理比其它温度测量系统的信号调理更复杂。信号调理部分产生的误差可能会降低精度，尤其在参考接合点补偿段。如下图是一种简单的模拟集成硬件解决方案，它使用一个IC将直接热电偶测量和参考接合点补偿结合在一起。

增益和输出比例系数: 微弱的热电偶信号被AD8495放大122倍，形成5-mV/°C的输出信号灵敏度（200°C/V）。\
降噪:高频共模和差分噪声由外部RFI滤波器消除。低频率共模噪声由AD8495的仪表放大器来抑制。再由外部后置滤波器解决任何残余噪声。\
参考接合点补偿:由于包括一个温度传感器来补偿环境温度变化，AD8495必须放在参考接合点附近以保持相同的温度，从而获得精确的参考接合点补偿。\
非线性校正:通过校准，AD8495在K型热电偶曲线的线性部分获得5 mV/°C输出。\
绝缘、接地和裸露热电偶的处理:图所示为一个接地1MΩ电阻，它适用于所有热电偶尖端类型。
## 3.3A/D 采样模块设计
STM32微控制器只能对数字信号进行接收与处理，因此传感器检测输出的模拟信号必须进行模数转换，使震动信号数字化。ADC在模数转换中起着桥梁的作用，占据了数字采样电路的核心地位。随着电子科学的快速发展，基于过采样技术的24-bitΣ-ΔADC逐渐成熟起来，并在地震信号采集中广泛使用。Σ-Δ调制型ADC是目前分辨率最高的ADC类型，其采用了噪声整形、过采样、数字低通滤波和抽取等高端技术，且在模拟输入端不需加快速滚降的抗混叠滤波器。基于以上优点，本文选用24位高分辨率的Σ-Δ模数转换芯片ADS1254设计模数转换电路。

ADS1254是24位高精密、高动态范围的Σ-Δ型ADC，在最高采样率20.8kHz时有19-bit无噪声精度（极低的噪声：1.8ppm）。ADS1254的模拟输入信号的采样速率由外部系统时钟（CLK）的频率确定，能够以极高的分辨率进行高速采样。如图3-16所示，ADS1254的主要功能模块是四阶Σ-Δ调制器、数字滤波器、控制逻辑和串行接口。模拟输入信号首先经Δ-Σ调制器进行调制，然后经Sinc5数字低通滤波器进行滤波处理，最后将结果数据经过串行接口输出。

ADS1254外部集成4个全差分模拟信号端口，差分输入信号电压必须介于AGND和AVDD之间，通过控制CHSEL0和CHSEL1引脚实现采样通道选择。ADS1254的标准系统时钟频率为8MHz，调制器频率相对于系统时钟频率是固定不变的，可以通过系统时钟频率除以6得到。因此，系统时钟频率为8MHz，调制器频率为1.333MHz。此外，调制器的过采样率相对于调制器频率也是固定的，当调制器频率为1.333MHz时，调制器的过采样率为64，数据速率为20.8kHz。使用较慢的系统时钟频率将导致较低的数据输出速率。

本系统通过STM32微处理器为ADS1254模块提供1M、2M和4M系统时钟频率，因此最大采样频率可达10.4K、5.2K和2.6K。DOUT/DRDY输出信号有两种模式：第一种操作模式是数据就绪模式（DRDY），用于表示信号已完成模数转换，数据已经放入输出寄存器中，可以读取；第二种操作模式是数据输出（DOUT）模式，用于将数据串行移出数据输出寄存器（DOR）。DOUT/DRDY的基本时序见图3-17，在t2、t3和t4时间内，DOUT/DRDY引脚在DRDY模式下工作，在将新数据内部传输到DOR之前，DOUT/DRDY引脚持续为高电平。在t1时间内，A/D转换的结果将被写入DOR。在t2时间内，DOUT/̅DRDY引脚将持续为低电平。在t3时间内，DOUT/DRDY引脚为高电平，用来表示可以读取新数据。此时，DOUT/DRDY引脚的功能将变为DOUT模式。在t7之后，数据将在引脚上移出。

STM32控制器在t6后向ADS1254提供SCLK高电平信号，需要读取数据在SCLK的上升沿被锁存，在SCLK的下降沿被输出。为了获取有效数据，必须在DOUT/DRDY引脚恢复为DRDY模式之前读取整个DOR中的数据。如果在DOUT模式期间未向ADS1254提供SCLK信号，则DOR的MSB将出现在DOUT/DRDY线上，直到t4时间结束。如果DOR中数据未完全读取（即提供的SCLK少于24个），最后一位数据状态将出现在DOUT/DRDY线上，直到t4时间结束。
![](D:/Project_Travis/git_temperatureMonitor/IMG/adc.jpg)
通道CH1输入的是震动传感器输出的经过放大、滤波和调理后的采样信号，通道CH2输入的是数字检波系统供电电压。ADS1254采样芯片每次只能启动一个模数转换通道。采样芯片正常工作需要外接2个供电电压：AVDD模拟供电输入电压与数字电源供电电压均为5V；VREF是ADS1254参考电压输入端口，本文为采样芯片提供2.5V参考电压。为了充分发挥ADS1254工作性能，保证采样精度，采用10uF和100nF陶瓷贴片电容给芯片电源进行滤波；除此之外，为了防止模拟信号遭受数字信号的干扰，ADS1254需要隔离处理模拟信号地和数字信号地。本文采用准电压源REF3125为ADS1254提供+2.5V的参考电压，REF3125的特性如下： 
1) 低压降：5mV； 
2) 高输出电流：±10mA； 
3) 高精度：最大0.2％； 
4) 低IQ：最大115uA； 
5) 低温漂：从0℃到75℃为15ppm/℃最大值)。
## 3.4 核心处理器设计
电源滤波 复位 时钟
## 3.5 电源模块设计
本系统的电源架构如下图所示。透过个人电脑或是其他5V直流电源供给，经一个Micro-USB的插头接入设备；充电管理芯片LTC4054将对单芯18650电池充电，充满为4.2V。同时本系统支持离电工作，此时电池负责供给电能。经由XC6220线性电源芯片将4.2V的电池电压稳压至3.3V恒定负责MCU、通讯模块和用户交互等的供电；或是经过MIC29302降压至3.7V左右，为WiFi模块提供低噪电源;电池亦可boost到5V，作为SD卡对上位机通讯环节的供给，由ME2108芯片完成。
![](D:/Project_Travis/git_temperatureMonitor/IMG/pwr.SVG)
### 3.5.1 充电管理
本课题使用的锂离子电池型号为3.7V/1800mAh，充电限制电压为4.2V。锂离子充电电压不能高于4.2V，大于4.5V时比较危险，当电压上升到5V左右时产生爆炸，根据锂离子电池的特性，其充电过程多是先恒流充电，再恒压充电。

本设计采用LTC4054充电管理芯片。LTC4054是凌特公司的一款锂离子电池充电芯片，他是专为单节锂离子电池充电需要设计的采用CC/CV算法的线性充电器。他能够提供达800mA的充电电流，也可以从USB端口取电工作。用他设计的充电电路所需外围元器件少，且超小型的封装，特别适合便携设备应用。锂离子电池充电管理芯片共有5个引脚，其具体功能如下：
1) CHRG：充电状态输出，在锂离子电池充电工程中。当充电循环结束时，一个月20uA的弱下拉电流源被连接至CHRG引脚，充电结束的状态。当LTC4054检测到一个欠压闭锁条件时，CHRG引脚被强制为高阻抗状态，此时进入充电准备状态。
2) GND：地。
3) BAT：充电电流输出。该引脚向锂离子电池提供充电电流。当充电电压最终达到锂离子电池的充电电压4.2V 时，LTC4054进入停机模式，电池已完成充电。
4) VCC：该引脚向充电器提供正输入电源。VCC的变化范围为4.25V到6.5V之间，并应通过至少一个1μF电容器进行旁路，当VCC降至BAT引脚电压的30mV以内，此时充电电流降至2uA以下，LTC4054进入停机模式，电池已完成充电。
5) PROG：充电电流设定、充电电流监控和停机引脚。在该引脚与地之间连接一个精度为1%的电阻器$R_{PROG}$，可以设定充电电流。
![](D:/Project_Travis/git_temperatureMonitor/IMG/4054.jpg)
正常充电循环为：当VCC引脚电压升值UVLO门限电平以上且在PROG引脚与地之间连接了一个精度1%的设定电阻器或当一个电池与充电器输出端相连时，一个充电循环开始。如果BAT引脚电平低于2.9V，则充电器进入涓流充电模式。在该模式中，芯片以提供月1/10的设定电流，以便将电池电压提升至一个安全的电平，从而实现满电流充电。当BAT端电压上升超过2.9V时充电器进入恒流充电模式，以编程设定的电流对电池充电。当BAT端电压接近最后的充电电压4.2V时LTC4054进入恒压充电模式，充电电流开始减小。当充电电流下降到充电电流设定值的1/10时充电周期就结束。充电周期终止后，LTC4054回继续检测BAT端的电压，当电池电压降至4.05V时充电循环重新开始，这确保电池被维持在一个充满电状态，并免除了进行周期性循环启动的需要。

充电电流设定：充电电流值是采用一个连接在PROG引脚与地之间的高精度电阻$R_{PROG}$来设定的。电池充电电流是PROG引脚输出电流的1000倍。设定电阻和充电电流采用式$(7)$来计算。
$$R_{PROG}=\frac{1000V}{I_{CHG}} \tag{7}$$
本项目设定充电电流为$I_{CHG}=500mA$，则$R_{PROG}=2KΩ$。
从BAT引脚输出的充电电流课通过监视PROG引脚电压随时确定，如式$(8)$所示。
$$I_{BAT}=\frac{V_{PROG}}{R_{PROG}}\times1000 \tag{8}$$
充电终止：当充电电流在达到最终浮充充电电压滞后降至设定值的1/10时，充电循环被终止。在这种状态下，BAT引脚上的所有负载都由电池来供电。在待机模式中，LTC4054对BTA引脚电压进行连续监控。如果该引脚电压降至4.05V的再充电门限电压以下，则开始另一个充电循环。当电池电量为空时，此时电池充电需要最长的充电时间由式$(9)$计算。
$$T=\frac{1800mAh}{500mA}\doteq3.6h \tag{9}$$
### 3.5.2 降压电路
#### 3.5.2.1 XC6220
选用XC6220将4.2V的电池电压转换成3.3V。该芯片具有高精度、低噪声、高速和低损耗等特点,符合电池供电电路的要求。其具有Torex创新的GreenOperation(GO)功能，搭搭演唱了电池寿命。通过GO功能，当负载电流较小时(≤2mA)，XC6220可自动从HS模式（典型功耗为50μA），转换到PS模式（功耗8μA）。当负载电流要求≥5mA时，该器件将迅速切换回HS模式。
![](D:/Project_Travis/git_temperatureMonitor/IMG/6220.JPG)
对于前级电源噪声的退耦，此处采用经验值，用10μF和100nF两颗电容并联。前级电源中的噪声经由滤波电容导入地层，剩下的高质量电源则送进XC6220芯片；其中10μF电容负责滤除低频噪声,100nF电容过滤高频噪声。
![](D:/Project_Travis/git_temperatureMonitor/IMG/6220_CL.JPG)
数据手册中指出，负载侧的电容对ESR的要求更高，容值也应尽量按照参考值进行设计
由于输出电压依照设计为3.3V，前级滤波电容选用10μF，故负载侧电容确定为4.7μF的陶瓷电容。

同时XC6220芯片支持片选，在用户需要时，可以使芯片失压，降低功耗。当CE脚为逻辑高电平时，芯片工作；否则失压进入节电模式。CE脚在集成电路内部系弱上拉强下拉的MOSFET阵列，但相关文档并未查明输入阻抗，此处选用经验值1K电阻上拉到电源脚，又并联一路信号与主控连接，便于控制芯片的开关，降低功耗。
#### 3.5.2.2 MIC29302
MIC29302芯片是美国Micrel公司研制的一种新型                                                                                   低压差、大电流、低功耗线性稳压器，它的静态工作电流极小，可以很稳定地工作在小电流状态。输出晶体管采用单片工艺制造地超级PNP管，这种晶体管允许饱和，所以稳压器可以有一个非常低的压降电压，最低至200mV。满负载工作时，压差仅有350mV.因为使该芯片的效率很高，功耗降低。最大输入电压26V，最大输出电流可达3A，输出电压连续可调，输出稳定精度为1%，输出电压温度系数典型值为20ppm/℃，工作温度范围从-40℃到125℃。

该芯片不仅有过流、过热保护功能，同时还具有电源电压极性、尖峰脉冲保护功能。它主要应用于高效线性稳压器、自动电子设备、高效开关电源输出稳压器、电池供电设备和高效绿色计算机系统。
![](D:/Project_Travis/git_temperatureMonitor/IMG/29302.png)
其中，1脚为可以与TTL/CMOS信号兼容的使能(ENABLE)管脚，高电平有效——该脚加入TTL高电平时，内部偏压电路工作，该稳压器接通电源；该脚加入TTL低电平时，内部控制器关断，流入该器件的静态电流只有5μA。2脚为电源输入端；3脚为接地端；4脚为稳压器的输出端；5脚为调节端，即反馈输入端(ADJ)，通过外界电阻和电位器构成分压器以调节输出电压的大小。

输出电压由$V_{OUT}=1.242\times(\frac{R1}{R2}+1)$决定。变换为$\frac{R1}{R2}=\frac{V_{OUT}}{1.242}-1$，对于本项目要求的3.8V，可以得到两电阻的约束关系为$\frac{R1}{R2}\doteq2.059$。假设R1为100KΩ，则R2取值约为48.567KΩ，与供应商货物清单比对，最终选择47KΩ1%电阻。
### 3.5.3 升压电路
系统在移除USB供电时由一节18650锂电池供电，电池满蓄电压为4.2V；而SD卡模块需要的供电电压为5V，对此需要设计升压转换电路。ME2108是一种采用CMOS工艺制造的地静态电流的PFM开关型DC/D升压转换器件。该芯片采用现金的电路设计和制造工艺，极大地改善了开关电路固有地噪声问题，减小对周围电路的干扰。
![](D:/Project_Travis/git_temperatureMonitor/IMG/2108.jpg)
外围电路仅需接一只二极管、一只电感和一只电容便可使其工作，利用电感对能量的存储，并通过其与输入端电源共同的泄放作用，从而获得高于输入电压的输出电压。

由于内部开关频率高达188KHz，本设计选用了具有快速切换功能的肖特基二极管1N5817。对于输出端电容，厂家技术手册建议47uF，供应商则建议100uF；为了减少元件种类，降低采购压力，同时获得更平滑的电流，此处最终确定为100uF电容。输入侧解耦电容同前文，不再赘述。

值得一提的是CE使能引脚（高电平有效），为了系统的低功耗设计，特没有将CE脚完全固定在高电平上，而是采用了一个可以有单片机控制的开关电路：当I/O为高电平时，三极管导通，MOS管栅极被拉低，电池电源导通；当I/O为低电平时，三极管不导通，MOS管不导通，电池电源不导通。

三极管的基极驱动电压只要高于Ube的死区电压即可控制三极管导通，硅材料三极管的死区电压一般为0.6V，锗材料三极管的死区电压一般为0.3V，所以控制三极管的电压对于硅材料的三极管来说只要高于0.6V左右即可，而对于锗材料的三极管来说只要高于0.3V左右即可。所以MCU先连接控制三极管。

而MOS管就不一样了，MOS管是电压型驱动，其驱动电压必须高于其死区电压Ugs的最小值才能导通，不同型号的MOS管其导通的Ugs最小值是不同的，一般为4V左右，最小的也要2.5V，但这也只是刚刚导通，其电流很小，还处于放大区的起始阶段，一般MOS管达到饱和时的驱动电压需6V~10V左右。

一般MCU IO输出电压为3.3V，很可能无法打开MOS管，所以MCU不直接连接控制MOS管。而且三极管带负载的能力没有MOS管强，所以MCU控制三极管，然后再控制MOS管来控制负载设备。
## 3.6 通信模块设计
### 3.6.1 有线通信
#### 3.6.1.1 RS485
本项目设计的环境检测系统可实现与PC互联，在内部集成RS485通讯功能。RS485是一种半双工串行通信总线，用缆线两端的电压差值来表示传递信号，具有良好的抑制共模干扰能力，广泛应用在各种物联网场景中。本研究采用WS3085作为RS485收发器，最高可实现500kbps的无误码数据传输。
![](D:/Project_Travis/git_temperatureMonitor/IMG/3085.jpg)
此芯片通过5V直流供电，主微控单元系3.3电源系统，故在电路中设计了NPN型三极管实现电流放大。具体电路的工作原理为：
1) 接收。当连接设备没有数据时，TX为高电平，三极管导通，此时芯片RE低电平使能，RO有效，芯片为接收状态。
2) 发送。当主MCU发送数据1时，TX为高电平，三极管导通，DE低电平，驱动器输出为高阻态，发送端与AB断开，此时A为1，B为0，即输出逻辑1；当发送数据时，TX为低电平，三极管截止，DE高电平，驱动器使能，因此驱动器输出A为0，B为1，即逻辑0。
#### 3.6.1.2 SD存储卡
![](D:/Project_Travis/git_temperatureMonitor/IMG/CH376.jpg)
本项目对于采样数据支持本地备份，以SD卡的形式,此将CH376T作为文件管理控制芯片。CH376T能够支持文件管理功能，打开、新建或删除，支持长文件名，而且提供文件读写功能。此外，还支持8位并口、SPI接口及异步串口通讯。SD卡优先与主MCU通信，透过并行接口；主微控制器再走SPI协议与CH376T通信，实现文件（内容）增删改查。
### 3.6.2 无线通信
#### 3.6.2.1 窄带物联网
窄带物联网方案采用上海移远通信的EC600N-CN全网通无线通信模块。模块支持最大下行速率10Mb/s和最大上行速率5Mb/s。采用MAIN_TXD和MAIN_RXD串口引脚与核心处理器串口进行数据交换。
![](D:/Project_Travis/git_temperatureMonitor/IMG/pwr.png)
模块的供电范围为3.4V-4.5V，需要确保输入电压不低于3.4V。为了减少电压跌落，需要使用低ESR(ESR≤0.7Ω)的100μF滤波电容。同时建议分别给VBAT_BB和VBAT_RF预留3个具有最佳ESR性能的片式多层陶瓷电容(MLCC)(100nF、33pF和10pF)。另外，为了保证电源稳定，本设计在电源前端加Vrwm=4.7V、低钳位电压和高峰值脉冲电流Ipp的TVS管。
![](D:/Project_Travis/git_temperatureMonitor/IMG/key.png)
当模块处于关机模式，可以通过拉低PWRKEY使模块开机；模块在开机状态下，拉低PWRKEY引脚一段时间后释放，模块将执行关机流程。控制PWRKEY引脚的方式可以是直接通过一个按键开关，按钮附近需放置一个TVS管及一个1kΩ电阻用于ESD防护。亦可使用开集驱动电路实现：当输入为高电平时，NPN三极管导通，Output被拉到GND，输出为高电平；当输入为低电平时，NPN三极管闭合，Output相当于开路——即逻辑电平翻转。
![](D:/Project_Travis/git_temperatureMonitor/IMG/sim.png)
模块提供USIM接口，USIM接口符合ETSI和IMT-2000规范，支持1.8V和3.0V的USIM卡。在USIM接口的电路设计中，为了确保USIM卡的良好性能和可靠性，本项目特别注意了以下几个问题：
1) 在USIM卡引脚增加了TVS阵列，选择的TVS阵列的寄生电容被控制在15pF以下，获得了良好的ESD性能；
2) 在模块和USIM卡之间串联0Ω的电阻便于调试；
3) 在USIM_DATA、USIM_CLK和SUIM_RST线上并联33pF电容用于滤除射频干扰。
4) 当USIM卡走线过长或者有比较近的干扰源的情况下，USIM_DATA上的上拉电阻有利于增加USIM卡的抗干扰能力。

模块提供三个UART：主UART、调试UART和辅助UART。由于辅助UART不使用，下面仅描述前两种UART的主要特性：
1) 主UART支持4800bps、9600bps、19200bps、38400bps、57600bps、115200bps、230400bps、460800bps和921600bps波特率，默认波特率为115200bps。支持RTS和CTS硬件流控，可用于AT命令通信和数据传输。
2) 调试UART支持115200bps波特率，用于部分日志输出。
![](D:/Project_Travis/git_temperatureMonitor/IMG/vol.png)
模块的UART电平为1.8V，而核心处理器的系统电平为3.3V，因此需在模块与核心处理器的UART连接中增加如TXS0108E的电平转换器。但考虑到成本控制，同时实际计划使用的波特率远低于460kbps（芯片手册认为高于该频率时三极管将处于常开状态），故改用三极管电平转换电路。
![](D:/Project_Travis/git_temperatureMonitor/IMG/ant.png)
模块有一个主天线接口，天线端口阻抗为50Ω。为获得更佳的射频性能，本设计预留了一个Π型匹配电路，其中电容默认不装配在测试阶段。
#### 3.6.2.2 无线局域网
![](D:/Project_Travis/git_temperatureMonitor/IMG/pwr8266.png)
ESP8266只有Pin11和Pin17两个数字电源管脚。数字电源无需在电路中增加滤波电容。由于本课题中ESP8266模块仅用作串口无线透传，故不考虑模拟电源部分。
![](D:/Project_Travis/git_temperatureMonitor/IMG/osc.png)
目前厂家提供的固件可支持40MHz，26MHz及24MHz的晶振。晶振外部输入输出所加的对地调节电容C1，C2可不设为固定值，该值范围在6pF到22pF，具体值需要通过对系统测试后进行调节确定。选用的晶振自身精度需在±10PPM。暂定晶振频率为26MHz，对地调节电容为10pF。
![](D:/Project_Travis/git_temperatureMonitor/IMG/uart8266.png)
U0TXD线上串联一只499R电阻用于抑制80MHz谐波。
![](D:/Project_Travis/git_temperatureMonitor/IMG/ant8266.png)
ESP8266输出端阻抗为39+j6Ω，所以最佳后端天线匹配阻抗为39-j6Ω（从天线方向看进来）。

其他引脚均悬空，特说明：为降低成本，节省主MCU的IO资源，没有设计复位电路。直接通过POR上电复位。
## 3.7 PCB抗干扰设计
层安排 隔地 包地 散热 封装 差分线 射频
# 4 系统的OS层设计
## 4.1 STM32
标准库（Standard Peripheral Library）是STMicroelectronics提供的最基本的库。它提供了对STM32微控制器的底层寄存器和外设的直接访问。标准库的设计目标是提供高度灵活性和低层次的硬件控制，以满足对性能和资源的严格要求。使用标准库，开发人员可以直接操作寄存器来配置和控制微控制器的功能，但需要手动编写大量的底层代码。标准库适用于对性能要求较高的应用和对代码大小和效率有严格要求的项目。
#### 4.1.1 通用GPIO操作
GPIO操作是MCU开发过程中最基本需要实现的功能，所有外设资源的操作本质上都是由GPIO和一系列定时器等共同实现。GPIO的操作分为输入和输出，下文分别介绍实现路线。
![](D:/Project_Travis/git_temperatureMonitor/IMG/gpio.svg)
本文以LED为例描述GPIO输出的实现路线。在使用GPIO前，需要先对其初始化。首先，实例化GPIO_InitTypeDef类为GPIO_InitStructure对象，以便配置GPIO。接着，使能对应的AHB上的时钟，使GPIO口获得分频震荡源。然后配置GPIO；输出，推挽，无上下拉，100Mhz的最高速度。再给GPIO_Pin成员赋值为对应需要操作的引脚在单片机内部的地址。最后调用一次GPIO_Init函数即可完成GPIO的初始化。

需要控制某个引脚输出高电平或是低电平的时候对BSRR寄存器赋值即可。BSRR寄存器有16位的位宽，对应一组GPIO的0号到15号管脚，此处用管脚地址的宏定义表示。

要想知道某个IO口的高低电平状态，则配置GPIO_MODE=GPIO_Mode_IN；然后按16位的格式读IDR寄存器，然后再查需要的位的状态即可。IDR(Input Data Register)是端口输入数据寄存器，是只读的，高16位保留，只用了低16位，且只能以16位的格式读出值。在固件库中操作IDR寄存器来获得IO端口电平的函数是
> uint8_t GPIO_ReadInputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin)

这样读取GPIOA的第5个端口的电平状态的代码是
> GPIO_ReadInputDataBit(GPIOA, GPIO_Pin_5);

返回值为 Bit_SET(高电平) 或 Bit_RESET(低电平)
#### 4.1.2 USART外设
本课题设计的系统在内部几个MCU之间采用串口通信。其中STM32单片机作为主机，ESP8266和NB模块作为从机。USART这种全双工通信方式在组成上分为物理层和协议层，物理层已经由芯片厂家和PCB上的Trace实现链通，在软件开发阶段主要是协议层面的操作。
![](D:/Project_Travis/git_temperatureMonitor/IMG/uart_init.svg)
在配置串口外设之前，优先需要完成GPIO的配置。先使能时钟，再将引脚映射为串口，然后配置服用功能。为了降低功耗，节省时钟资源，保证系统流畅，可以将GPIO的速度适当降低；实际通讯时每次包含的数据量并不大，对带宽的要求相对宽松，此处取50MHz。另外为了增加发送线的驱动能力，防止受到干扰意外拉低，发送错误数据，配置时勾选内部上拉。

UART硬件参数方面，有波特率、字长、停止位、校验位、流控等；依次赋值对应串口初始化对象中的成员变量即可。设定后用USART_Init()函数生效，并通过USART_Cmd()使能串口。

在开发过程中，CPU的一个小缺陷会导致串口配置好直接Send第1个字节发送不出去。经过验证，在Send之前清楚一次完成标志可以解决这一问题。
> USART_ClearFlag(USART1, USART_FLAG_TC);

工程应用时，基本不采用阻塞式通信，本课题在设计软件时考虑到分时调度问题，特为串口启用了一个专门的中断源以响应接收到的数据。发送数据也占有一个中断源，但仅在发送时申请中断，一旦完成缓冲区数据的发送，立刻取消中断源。
> USART_ITConfig(USART1, USART_IT_RXNE, ENABLE);

串口需要发送的内容并不直接送至寄存器并执行，而是先压入FIFO的缓冲队列——按照时间顺序发送信息。
![](D:/Project_Travis/git_temperatureMonitor/IMG/uart_send.svg)
缓冲区处理会先获取串口对象，如果串口类没有被成功实例化，则会直接跳出，不进行任何处理方法；否则调用UartSend()函数。具体为：
1) 判定缓冲区是否满，如果是则等待释放空间
2) 将新数据依次填入发送缓冲区
3) 遍历数据直到没有更多新数据需要缓冲
4) 启用串口发送中断源交由NVIC跳出

接收数据的方法是发送的逆过程。当串口接收到数据时，触发中断回调，将数据填入缓冲队列。
#### 4.1.3 同步串行接口
本项目使用到SPI协议的器件有CH376和HT1621。在开发过程中，为了降低coding工作量，均调用了第三方代码：CH376为硬件SPI，HT1621为软件SPI，即通过手动控制引脚输出高低电平模拟SPI时序。硬件SPI初始化与UART类似，软件SPI同GPIO配置，不再赘述。
## 4.2 WiFi模块
### 4.2.1 串口流控
透传功能使⽤的是 TCP 协议，每包数据是 1460（取决于协议栈），只要⽹络良好，buffer 空间没有被消耗完，就可以不停地传输数据。对于透传，串⼝接收数据间隔超过约 20 ms，就会认为数据接收结束，将已经接受的数据传输到⽹络。如果⽹络不好，就可能会丢弃⼀些数据，因此，为避免这种情况，软件设计时将串⼝设置为流控模式。
> #define UART_RX_FLOW_EN (BIT(23))
#define UART_RX_FLOW_THRHD 0x0000007F
#define UART_RX_FLOW_THRHD_S 16
#define UART_TX_FLOW_EN (BIT(15)

接收方向的硬件流控可以配置阈值，当rx fifo中的长度大于所设的阈值，U0RTS脚就会拉高，阻止对方发送。配置接收流控阈值的相关配置都在UART_CONF1定义的寄存器中。发送方向的流控只需配置使能，该寄存器在UART_CONF0中。

接口函数为*void UART_SetFlowCtrl(uint8_t uart_no,UART_HwFlowCtrl flow_ctrl,uint rx_thresh)*在RXFIFO中字节数大于阈值后，RTS会被拉高。
### 4.2.2 TCP透传
TCP和UDP的主要差别在于传送资料的可靠性：TCP提供可靠的传送，而UDP不保证传送的可靠性。因此，鉴于本设计对资料的完整性有更高的要求，选用TCP。
![](D:/Project_Travis/git_temperatureMonitor/IMG/tcp8266.svg)
测试AT功能后，开始AT指令的发送。首先设置模组进入STA模式并连接WiFi，然后连接到指定的TCP服务器，最后设置设备进入透传模式。向ESP8266发送任意数据到串口都将被直接转发到服务器中。
## 4.3 NB-IoT模块
### 4.3.1 HTTP访问
#### 4.3.1.1 GET方法
在基于AT指令集的HTTP通信应用中，整个流程可以分为网络配置、HTTP请求发送、以及HTTP响应的读取与处理三个主要部分。
![](D:/Project_Travis/git_temperatureMonitor/IMG/get1.png)
![](D:/Project_Travis/git_temperatureMonitor/IMG/get2.png)
首先，系统需要完成PDP（Packet Data Protocol）上下文的配置。通过指令AT+QHTTPCFG="contextid",1，将PDP上下文ID设置为1，确保后续操作能够基于正确的网络会话进行。随后，使用AT+QHTTPCFG="responseheader",1启用HTTP响应头输出功能，使得HTTP响应头信息可以被捕获并解析。接着，通过AT+QICSGP=1,1,"UNINET","","",1指令，将PDP上下文1的APN（Access Point Name）设置为“UNINET”，确保设备能够通过中国联通网络进行数据通信。为使配置生效，可以执行AT+CFUN=1,1重启网络功能模块。为了确认PDP上下文的激活状态，使用AT+QIACT?指令检查当前的网络连接状态，并在必要时通过AT+QIACT=1手动激活网络连接。

发送HTTP请求是第二个步骤。通过AT+QHTTPURL=23,80，设置访问的目标URL，例如“http://www.sina.com.cn/”，并将超时时间设为80秒。此操作确保HTTP请求能够准确定位目标服务器。接着，通过`AT+QHTTPGET=80`指令发送HTTP GET请求，并设置最大等待时间为80秒。系统返回的响应包括HTTP状态码和内容长度。例如，+QHTTPGET: 0,200,601710表示HTTP请求成功（状态码200），且响应体的长度为601710字节。

读取HTTP响应信息是最后一个关键步骤。通过UART接口直接读取响应数据。使用AT+QHTTPREAD=80指令，系统通过UART接口输出完整的HTTP响应头和响应体。
#### 4.3.1.2 POST方法
在基于AT指令集的HTTP通信中，发送POST请求的流程可以通过以下步骤实现。该流程包括配置PDP上下文、发送HTTP POST请求、以及读取和解析HTTP响应信息。下面是该过程的详细描述，适用于学术研究中的应用示例。
![](D:/Project_Travis/git_temperatureMonitor/IMG/post1.png)
![](D:/Project_Travis/git_temperatureMonitor/IMG/post2.png)
首先，需要配置PDP上下文以建立网络连接。通过执行AT+QHTTPCFG="contextid",1，将PDP上下文ID设置为1。随后，使用AT+QIACT?指令检查PDP上下文的状态，确保设备能够成功接入网络。若未激活网络连接，则可以通过AT+QIACT=1激活指定的PDP上下文。

其次，通过AT+QICSGP=1,1,"UNINET","","",1指令将PDP上下文1的APN（接入点名称）配置为中国联通的“UNINET”。为了使该配置生效，执行AT+CFUN=1,1重启网络模块，并再次通过AT+QIACT?确认PDP上下文已经成功激活，确保设备获得有效的IP地址，例如+QIACT: 1,1,1,"172.22.86.226"。

接下来，通过AT+QHTTPURL=59,80指令设置HTTP请求的目标URL，超时时间为80秒。在此示例中，访问的URL为http://api.efxnow.com/DEMOWebServices2.8/Service.asmx/Echo?。此URL用于调用一个Web服务，该服务的POST请求体会回显发送的消息。该步骤确认了设备能够向远程服务器发送请求。

为了发送HTTP POST请求，使用AT+QHTTPPOST=20,80,80，其中20表示POST请求体的长度，80秒为POST数据的输入和响应的超时时间。POST请求体通过UART接口输入。在此示例中，发送的POST请求体为Message=HelloQuectel，长度为20字节。请求发送成功后，设备返回+QHTTPPOST: 0,200,177，表示HTTP POST请求已成功处理（状态码200），并且服务器响应体的内容长度为177字节。

为了读取HTTP响应信息，使用AT+QHTTPREAD=80，其中80秒是读取HTTP会话的最大等待时间。通过UART接口，设备会输出完整的HTTP响应头和响应体。此响应表示服务器成功接收并处理了发送的POST数据，并回显了消息HelloQuectel，其中ASCII值也被一同返回。整个过程最后以+QHTTPREAD: 0表示成功输出HTTP响应信息，表明HTTP会话顺利完成。
### 4.3.2 低功耗
模块内部有一个较低优先级的Sleep任务，负责检测模块是否能够进入睡眠模式。睡眠控制变量（AT+QSCLK）和模块内部其他的任务（RF、USB、UART 等）一起，向Sleep 任务投票决定模块能否进入睡眠模式。只有当睡眠控制变量与其他任务均同意模块进入睡眠时，Sleep任务才会被执行，模块进入睡眠模式。
![](D:/Project_Travis/git_temperatureMonitor/IMG/low.png)
模块进入睡眠模式后，RF任务并不会被关闭，模块处于DRX模式下。DRX是一种节省终端功耗的工作模式，其基本原理是让模块周期性地进入睡眠状态。
# 5 系统的软件层设计
## 5.1 下位机部分
### 5.1.1 显示任务
### 5.1.2 数据采集任务
通过读取 PT1000 温度传感器的ADC值，计算并返回对应的温度。PT1000 是一种电阻式温度传感器，其电阻值会随着温度变化。因此，通过对ADC采样的值进行线性插值，就能得到温度。
![](D:/Project_Travis/git_temperatureMonitor/IMG/dso.svg)
首先，该函数通过从指定的ADC通道获取多次采样数据，并对其进行累加和求平均值。这一过程有助于减小由于噪声或瞬态波动导致的测量误差，从而确保得到一个更加稳定和精确的ADC值。函数会采集50次数据，通过累加这些采样结果并取平均值，得到最终的ADC均值ad。

接下来，函数通过与预先建立的温度-ADC值映射表MapTempToADC进行对比，确定当前ADC值所对应的温度区间。MapTempToADC是一个二维数组，其中每一项包含一个特定的温度值及其对应的ADC读数。如果采集的ADC值小于该表中的最小值，则函数直接返回温度为0°C；若大于表中的最大值，则返回温度为240°C。这样可以避免ADC值超出已知范围时产生不准确的温度估算。

当ADC值在表中某两个已知的温度点之间时，函数将执行线性插值算法。它根据查找表中的温度和ADC值，找到与当前ADC值最接近的两个点，并使用以下公式进行线性插值计算：
$$temp=tempL+(tempH-tempL)\times\frac{ad-adcL}{adcH-adcL}$$
其中，tempL和tempH分别代表查找表中对应于adcL和adcH的温度下界和上界，adcL 和adcH则是相邻的两个ADC值。通过这一公式，函数能够准确推算出在ADC值ad所对应的温度。

最后，函数对插值结果进行微调，将结果减去200，以满足特定的温度测量需求。最终的温度值以浮点数形式返回。
## 5.2 上位机部分
### 5.2.1 PC端应用程序
### 5.2.2 Web端交互页面
### 5.2.3 移动端应用程序
# 6 测试与分析
## 6.1 硬件测试
### 6.1.1 电源纹波
### 6.1.2 充电曲线
### 6.1.3 调理电路SNR
### 6.1.4 天线阻抗
## 6.2 软件测试
### 6.2.1 测温精度
### 6.2.2 网络延迟
# 7 总结与展望
## 7.1 总结
## 7.2 展望
### 7.2.1 成本控制
### 7.2.2 功耗控制
### 7.2.3 结合点补偿