---
title: "「译文」| 偷电问题" 
date: 2021-07-28
lastmod: 2021-07-28
draft: false
categories: ["Real-world Problems"]
author: "sherry"
---
本文翻译自[Real-world Problems for Phys25, Summer 2006](https://users.physics.unc.edu/~deardorf/phys25/rwp/)系列文章第四篇[A Solution to the RWP for Exam 1 - Stealing Power](https://users.physics.unc.edu/~deardorf/phys25/rwp/pizzasol.html)。

---

据报道，一名农民通过策略性地将一大圈电线放在跨越其田地的高压输电线路下方，从而偷走了电力。几年以来，这个农民将免费获取的电力用于他的农场操作设备，直到最终被电力公司发现。最终，尽管输电线路没有建立物理连接，但农民仍被判窃电罪名成立。使用此信息和您的物理学知识回答以下问题：

<!--more-->

1. 在没有检查农民财产的情况下，电力公司是怎么发现电力被偷了的?

原则上，电力公司可以计算输送的电力和购买的电力之间的差额。其中存在的无法解释的差异（除了通过沿传输线的电阻加热造成的功率损失之外）可能是有人偷取电力的结果。实际上，在这种情况下单个用户窃取的电量很可能太小而无法检测到。

2. 农民利用什么原理从输电线路偷电？

农民使用法拉第感应定律在靠近高压输电线路的线圈中感应出电压和电流。这种做法是有可能的，因为通过传输线的交流电改变了它周围的磁场，如果感应线圈的位置使磁场线穿过它的区域，那么就会感应出电动势，交流电就会流过线圈。

3. 农民必须如何放置线圈才能最有效地实现他的目标？

线圈应尽可能靠近传输线放置，因为磁场与到传输线的距离成反比，并且线圈应垂直放置在与传输线对齐的垂直平面上，以最大限度地提高来自传输线的磁通量环绕传输线的磁场线。

4. 假设最低传输线距离地面 10 米，并承载 60 Hz 的交流电流，最大 150 A，电压为 230 kV。如果线圈是边长为 5 米的正方形形状并接触地面，则要产生 120 V 的标准电压大约需要多少匝线圈？

线圈中感应的最大电动势取决于磁通量的变化：`emf（电动势） = Nd(phi)/dt = NAdB/dt`。

对于这个问题，磁通量会发生变化，因为磁场强度因交流电而变化，交流电在每个方向上达到峰值两次，或每 1/120 秒一次。这意味着磁场每个周期会从最大值变为零四次，因此 `dt = 1/240 秒 = 4.2 毫秒`。传输线周围的最大磁场可以根据通过导线的最大电流估算，使用长直导线的公式：`B(wire) = uo*I/(2pi*r)`。由于方形感应线圈从传输线延伸出 5 到 10 米 的距离，因此线圈远端的磁场会较弱，最近端的磁场会较强，但使用 7.5 米的平均距离可得出特定电流的平均磁通量。因此，距传输线 7.5 米处的最大磁场为 `B = (4pi*10^-7 T*m/A)(150 A)/(2pi)/(7.5 m) = 4.0 uT`，其中与参考文献 2 中指示的 230 kV 线路附近 6 uT 的平均磁场一致。

产生 120 V 电压（或 170 V 峰值电压）所需的匝数为：`N = (170 V)(4.2 毫秒)/(25 米 ^ 2)/(4.0 uT) = 7140 圈`

5. 为什么线圈中的电压会随着一天中的时间而变化？你预计它什么时候最大和最小？

线圈中感应出的电压取决于传输线产生的磁场强度，磁场强度取决于通过线路的电流量，而这取决于用户对来自这条线路的电力（负载）的需求。我们应该预计这种电力需求在下午和晚上时间最大，而在每天的清晨时间最小。峰值电力需求的这种每日变化可以在参考文献 2 的图表中看到。

6. 线圈中电流的频率是多少？传输线中的电压与线圈之间的相位角是多少？

由于线圈中感应的电流是由传输线中电流的变化引起的，因此它们应该具有相同的 60 Hz 频率。然而，这些电流（或相应的电压）之间的相位角将是 90 度，因为当传输线中的电流变化最大时线圈中的感应电流达到最大值，这发生在一个周期的 1/4 或 `360/4 = 90 度`在传输线中的电流峰值之前和之后。

7. 如果线圈和与其相连的所有设备的总阻抗为 200 欧姆，那么消耗能量的最大速率是多少？

能量消耗的最大速率（功率）为：`Pmax = Irms*Vrms = (Vrms)^2/Z = (120 V)^2/(200 ohms) = 72 W`

可以通过包含适当的`power factor（功率因数）= R/Z` 来更准确地估计平均功率，这可以通过计算线圈的电感和确定感抗来确定。根据参考文献 3 得知，该线圈的电感约为 1600 H（这是一个非常大的电感）。感抗为 `X(L) = 2(pi)(60 Hz)(1600 H) = 603000 ohms`！这远大于规定的 200 欧姆阻抗，因此功率因数似乎接近于零！

8. 假设成本为 0.10 美元/kW-h，农民在一年内窃取的能源价值大约是多少？

如果我们假设农民有一半的时间使用设备（一天的平均功耗为 36 W），那么一年的总成本将是：`cost（成本）= (36 W)(24 小时/天)(365 天/年)($0.10/kW-h) = $31.54/年`。即使在最大功率为 72 W 的情况下，被盗能源的价值也仅为 63 美元/年。这似乎不值得付出努力、风险和成本（见下文）。

9. 估计制作线圈的成本，假设农民使用 12 号铜线，成本为 0.15 美元/英尺。评估农民设计的成本效益并计算其投资的大致回收时间。

需要 7140 圈电线，每圈长度为 20 米，农民将需要 143 公里的电线！以 0.15 美元/英尺 = 0.50 美元/米计算，这条线将花费 71400 美元！这意味着即使在最大能源消耗率下，投资回收时间也至少为 1000 年！

10. 你会给农民什么建议来最大限度地提高他的设计效率？

为了最大限度地提高线圈中感应的电动势，同时使用最少的电线以节省成本，农民应将线圈尽可能靠近传输线，并使用尽可能大面积的线圈（因为将线圈的面积增加一倍，方形线圈使感应电动势增加四倍，而成本仅增加一倍）。在线圈内放置铁会提高这个变压器的效率，但效果会相当小，可能不值得付出努力。

11. 从这个问题中还可以学到什么其他东西？

从上面的计算来看，这种偷电方式似乎是非常低效和不切实际的。我严重怀疑是否有农民真的这样做过（也许这是一个适合[《流言终结者》](https://www.imdb.com/title/tt0383126/)这样的节目去调查的神话！）。这种法拉第定律的应用，本质上是一种效率极低的电力变压器。法拉第定律还有许多其他应用遍布我们的现代生活。这只是一个不建议的。

# 参考文献

1) http://sci-phys-plasma.caeds.eng.uml.edu/1999/01-99-011.htm
2) http://infoventures.com/private/federal/q&a/qaenvn2a.html
3) http://www.technick.net/public/code/cp_dpage.php?aiocp_dp=util_inductance_rectangle