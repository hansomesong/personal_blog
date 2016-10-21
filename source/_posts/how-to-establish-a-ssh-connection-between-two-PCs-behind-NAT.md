title: how to establish a ssh connection between two PCs behind NAT
date: 2015-08-02 14:22:43
tags:
---

我想在家里通过SSH访问位于办公室里的电脑。不幸的是，从我家里的电脑到办公室里的电脑至少要通过两个NAT。还在学校专门提供了一台机器提供SSH中转。

电脑的名字叫做：ssh.rennes.telecom-bretagne.eu, 只是我忘记了，给ssh开发的端口号是哪个了。所以第一步需要通过nmap看看目标主机上哪些端口是开放的。

Smart Grid Traffic Behaviour Discussion， 2010，April
这篇文章属于讨论性质的文章，最大的亮点在于为学术界定义了 Smart grid 的 Traffic Characteristics。当然文章里也提到了一句：
>Overload control has been prioritized for work in Rel-10 time frame together with several other functinalities.

当然文章还指出了，我们可以做改进的apsects:
- Radio resource overload control
- MME overload control

# Reliable reporting for Massive M2M communications with periodic resource pooling, 2014, August
关注的case: periodic reporting
问题： massive M2M devices, 保证report被在给定的dealine之前以比如99%的可靠性被发送
方法：a M2M-dedicated resource pool, 作者论证了这个资源池的规模至少要多大。
分析简述：首先一个 M2M device 在上述的资源池里要发送的transmission number是随机变量，因为：1）自last reporting至current reporting之间的arrived reports的数目是随机的。2）retransmission mechanism 这么一个事实 使得 问题变得较为复杂了。(不然，我们可以提前周期性的给这些设备分配资源了)
a massive number of devices 使得 中心极限定理 可以使用了。 

对于确定的 资源池，在保证一定的 reliability level的前提下，最多可以服务多少 M2M device 呢？

System model:
Reporting Interval (RI), 可以被 视为 Poisson process 亦可以被视为 Uniform distribution。作者只考虑 Poisson process 的情况，但同时也是，他们推导的结论，亦适用于 Uniform distribution 的情况。
Time-frequency 资源被分作两个资源池：一个给M2M，另一个给non-M2M。并且指出了这种 seperation 是常用手段。接下来，M2M-dedicated resource pool 也被分为两部分：preallocated and common pool。

我比较好奇，上文中提到的LTE simulator究竟是何方神圣？


#Radio Resource Allocation in LTE-Advanced Cellular Networks with M2M Communications
作者认为，M2M的特性有：group-based communications, low or no mobility, time-controlled, time-tolerant, small data transmission, secure connection, MTC monitoring, priority alarm messages, extra low power consumption, etc.
提出 group-based communications 的特性，蛮有意思的。
作者也指出了(不知是否为首创)MTCD-related transmission有三种情景：Direct transmission, Multiple-hop transmisison, peer-to-peer transmission(实际上就是D2D)
。
作者在本文中对network applications进行了分类：Class1 Elastic applications, Class2 Hard Real-time appliation, Class3 Delay adaptivea applications, Class4 Rata-adaptive applications。
作者提出了user utility的指标来优化，作者认为：simple QoS indicator是不足以
“the user utility of a service is a measurement of its QoS performance based on the provided network services such as the bandwidth, transmission delay, and loss ratio.” “With the control of the eNB, radio resources can be efficiently shared between the eNB-to-UE and eNB-to-MTCD links by using the utility-based scheduling scheme.” 貌似Utility这个函数值在0，1之间，可是具体究竟是什么就无从得知了。

总得来说，这篇文章虽然不是致力于 Energy-saving/efficiency 的，它主要是讨论在M2M参与的情况下，如何在Cellular network中进行RRA，以减少 Co-channel inteference和提高network efficiency。(不就是提高服务用户数么)它给我们定义了一个很好的分析模型框架使得日后的许多研究都可以这个框架进行。

这篇文章中关于 Architectual enhancement 的部分(比如各类名词的引入，比如MTCG功能的介绍)可以吸收整合进我的SOTA。另外 “On the assumption of half-duplex MTCGs” 的描述，是本文作者之首创还是真的有文献如此论述过呢？作者并未回答，鉴于MTCG 这种新型网络节点并非此文作者引入，我倾向于某个3GPP文档里，有关于 MTCG 双工模式的探讨。“To avoid self-inteference and reduce implementation complexity, half-duplex MTCGs are preferred for deployement in the networks.”

作者最后也做了 system-level simulation，但是用的究竟是 什么工具呢？

# Energy-efficent Uplink Resource Allocation in LTE network with M2M/H2H co-existence under statistical QoS guarantees
作者试图从 Resource allocation 的角度研究 LTE 上行信道 能量效率问题。突然有一个疑问：为什么 Radio Resource Allocation 会对能量消耗有影响？
我的理解是：Radio Resource Allocation 实际上决定了跟 UE 分配多少个 RBs 的问题，实际上就是分给了 UE 多少 Bandwidth, 而根据香浓公式(不过要注意，这是极限情况)。速度一定的情况下，带宽B是可以可以决定信噪比SNR的，而信噪比SNR是可以影响误码率的，通信的正常进行是以保证误码率BER在一定水平之内为前提的。为了达到一定的信噪比，则对 UE 的发射功率P 提出了要求，在 time slot 长度一定的时候，单位 slot 消耗的能量就是确定的。由此可见，RRM(RRA)是可以影响到能量效率 Energy efficiency 的。
所以这篇文章中，作者将 Energy efficiency 问题转换成了一个 optimization 的问题，并且用数学方法求出了极值。虽然其中涉及的数学知识，至今都不是很明白，但个人感觉，这篇文章，相当完美。

# On radio resource allocation in LTE networks with machine-to-machine communications
依然是 Aijaz 的作品，采用了 heuristic 算法 来解决 energy-efficient resource allocation 问题。

# A Tractable Model of the LTE Access Reservation Procedure for Machine-Type Communications
作者认为：The number of served devices in a single LTE cell is not determined by the available aggregate rate, but rather by the limitations of the LTE access reservation protoco. 故而提出了一个 数学模型 研究现行 LTE接入机制 上行信道可以容纳多少 M2M 设备。我比较喜欢此文中的数学推导，马尔科夫链。

# Machine Type Communications in 3GPP Networks: Potential, Challenges, and Solutions
作者认为：
>This explains the focus of 3GPP on finding efficient mechanisms to handle congestion at mainly the control plane of EPS that may happen due to nearly simultaneous signaling messages issued by a significant number of mobile devices. Thanks to the wide bandwidth of LTE and other emerging radio technologies (e.g., LTE-advanced), congestion due to user data packets is, for the time being, of less impor- tance.

因为作者提出：
>One aspect which is missing so far in MTC signaling congestion control consists in the lack of a mechanism that handles a bulk of similar signaling messages from MTCs in a single shot (i.e. bulk MTC signaling handling). 


我们是不是可虑构造一个表格，这个表格里面，我们详细对比 跟Energy(甚至是M2M)有关的proposal。

经常看到 Utility function 这样的表述，其为何意？


# Modeling and Characterization of Transmission Energy Consumption in Machine-to-Machine Networks
作者认为：A key step in building energy-efficient protocols for large-scale M2M communications is to assess, model or characterize a network energy consumption profile. 可见作者之最终目的为：build energy-efficient protocol。所用知识为随机几何。
>We borrowed tools from stochastic geometry and Poisson point process to model and study energy consumption in large-scale M2M communications by deriving the cumula- tive distribution function (CDF) of energy consumption. 

# Energy-saving massive access control and resource allocation schemes for M2M communications in OFDMA cellular networks
作者之侧重点在于：Energy-based access control and resource allocation，缺点在乎于 a single cell。

Green Communications: Principles, Concepts and Practice 这本书是2015年新出的书，可以考虑。

D2D communication is a feature that has been introduced in the 3GPP Release 12.

5G考虑使用 毫米波波段

64QAM貌似适合在离基站比较近的地方用，QPSK适合在比较远的地方用。我猜测64QAM速度高但抗干扰能力差，反之，QPSK速率低，但是可以恢复出信号。

LTE-M 的 活动 我没有考虑过啊。。。这个可以加到 SOTA 之中
在 Design of an improved energy efficient clustering in M2M communication 中，作者的描述：long term evolution for machine type communication (LTE-M) network, which comes under LTE network. 感觉LTE-M是 EXALTED 项目的一部分。


    
