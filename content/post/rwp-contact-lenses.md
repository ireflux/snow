---
title: "「译文」| 隐形眼镜" 
date: 2021-08-19
lastmod: 2021-08-22
draft: false
categories: ["Real-world Problems"]
author: "sherry"
---
本文翻译自[Real-world Problems for Phys25, Summer 2006](https://users.physics.unc.edu/~deardorf/phys25/rwp/)系列文章第五篇[A Solution to the Real-World Problem:  Contact Lenses](https://users.physics.unc.edu/~deardorf/phys25/rwp/rwp3sol.html)

---

为何隐形眼镜比普通眼镜薄得多，但在矫正视力方面却同样有效？

提示：将球面透镜的焦距与透镜的折射率和前后表面的曲率半径联系起来的方程称为透镜制造方程：`1/f = (n - no)(1/r1 - 1/r2)`, 其中 no = 1.00 代表空气。

<!--more-->

## 收集信息 

隐形眼镜直接戴在眼睛上，角膜半径约为 0.8 厘米（参考文献 3），折射率为 1.42 至 1.52（参考文献 3），厚度约为 0.2 毫米。

普通眼镜通常戴在距离眼睛 2 厘米处，折射率为 1.5 到 1.7（参考文献 2），曲率半径约 10 厘米（通过检查几种眼镜并与类似曲率的球进行比较而发现），厚度为 2 - 5 毫米（比隐形眼镜厚约 10 倍）。

## 组织信息

从镜片制造方程可以看出，对于给定的屈光力 (1/f)，镜片的厚度取决于镜片材料的折射率（n 越大，屈光力越大）以及两者之间的相对差异镜片前后表面的曲率半径（半径差异越大，镜片越厚）。镜片的厚度还取决于所用材料的类型以及它必须具有多大的刚性才能避免破裂或撕裂。

## 分析

由于以下原因，隐形眼镜可以做得比普通眼镜更薄：

1. 由于隐形眼镜直接戴在眼球上，因此隐形眼镜矫正视力所需的屈光力略低于普通眼镜。例如，假设一个人有远视，近点为 57 厘米（如示例 27-3 所示）。距离眼睛 2.0 厘米的普通眼镜所需的屈光力为 2.53 D (f = 39.5 厘米)。但是，如活动示例 27-4 中所述，同一个人将需要屈光力仅为 2.25 D (f = 44.5 厘米) 的隐形眼镜。虽然这个约 10% 的差异比较显著，但对于视力较好的人来说，相对差异要小很多，但这种影响只是隐形眼镜和普通眼镜之间厚度差异的一部分。
2. 隐形眼镜可以由具有更高折射率的材料制成。虽然这可能是真的，但是大多数现代眼镜都是由轻质塑料材料制成的，折射率 n = 1.5 至 1.7，而隐形眼镜的指数通常为 1.4 至 1.5（参考文献 2&3），因此这个似乎无法作为解释。
3. 隐形眼镜的平均曲率半径约为普通眼镜的 1/10。为了检查这种平均半径差异的影响，假设我们比较了具有相同折射率 (n = 1.5) 和屈光力 (2.0 D) 的普通眼镜和隐形眼镜。根据透镜制造方程，并假设空气作为周围介质，我们可以求解一个曲率半径，给定另一个：`1/r2 = 1/r1 - 1/[f(n-1)]`。因此，对于隐形眼镜 (r1 = 1.00 厘米)：r2 = 1.04 厘米，而对于普通眼镜 (r1 = 10.0 厘米)：r2 = 16.7 厘米。这意味着使用类似折射率的材料，隐形眼镜的半径仅需要 4% 的差异，即可产生与半径差异 50% 的普通眼镜相同的屈光力！显然，这是一个重要因素，可以解释大多数隐形眼镜和普通眼镜之间的厚度差异。但是请注意，这种效果对于隐形眼镜来说并不那么明显，因为隐形眼镜的后表面接触角膜，而角膜的折射率几乎与隐形眼镜相同，因此基本上所有的折射都必须发生在隐形眼镜的前表面。
4. 隐形眼镜不需要像普通眼镜那样坚硬，因此可以做得更薄。虽然这一点很显然，但它却是隐形眼镜和普通眼镜之间厚度差异最重要的原因。

## 学习

有趣的是，镜片的厚度更多地取决于平均曲率半径，而不是它相对于眼睛的位置。隐形眼镜所需的前后半径差异相对较小，这意味着这些镜片的制造精度必须比眼镜高得多。这是精密工程的又一个奇迹。

## 参考资料

1. http://hyperphysics.phy-astr.gsu.edu/hbase/geoopt/lenmak.html
2. http://www.healthatoz.com/healthatoz/Atoz/ency/eye_glasses_and_contact_lenses.jsp
3. http://www.ophthalmic.hyperguides.com/default.asp?section=/tutorials/clinical/contact_lenses/tutorial.asp
4. http://research.opt.indiana.edu/library/waveGuidedLens/waveGuidedLens.html
5. http://www.findarticles.com/p/articles/mi_g2601/is_0005/ai_2601000518
6. http://www.eyeglasshouse.com/contactcosmetic.htm
7. http://cvu.strath.ac.uk/courseware/msc/jbaggot/lenses/lenses.html
