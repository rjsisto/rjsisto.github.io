---
layout: distill
title: Case Profitability
date: 2024-4-16
tags: R, plotly, ggplot
featured: true
toc:
- name: Overview
- name: Analysis
  subsections:
  - name: Apple Phone Case Sales
  - name: Samsung Phone Case Sales
  - name: Google Phone Case Sales
  - name: Motorola Phone Case Sales

- name: Conclusion
---


## Overview

This post is an analysis of the profits made on different cases sold by a particular phone store.

## Analysis

The charts and analysis below are on the profit of case sales by different case brands for different phones. Each chart shows the average profit for each case brand sold for different phone brands (Apple, Samsung, etc.). The bar lengths indicate how many of each case brand we sold for each phone. The colors indicate the average profitability of each case brand (with a corresponding legend to the right of each chart). The case brands are sorted in order of most to least profitable. Each chart is also interactive, so you can hover over each bar to see specific details (exact profitability, exact numbers of cases sold) about each one and you can zoom in for more detail. Case brands are only included if they sold 5 or more cases for each phone brand, to give a clearer picture of which case brands are selling for each phone.

Before delving into each phone brand separately, let’s outline a few overarching points. OtterBox is by far the best-selling case brand for both Apple and Samsung phones, making it the best-selling case brand overall. This is likely due to the brand recognition that OtterBox has within this segment. After OtterBox, there is no standout brand that sells exceptionally well with multiple phones. In terms of case profits, both Samsung and Motorola have relatively low variability in the profits made from different case brands.

#### Apple Phone Case Sales

<div class="l-page">
<iframe src="{{ '/assets/cases_post/apple.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>

Apple cases have the most variability between profits earned per case brand, having both the most and least profitable cases. There is a whopping \\$17 difference in profit made between selling a Prodigee and PopSocket case for Apple phones.

The Prodigee cases are the most profitable cases that we sell for any phone, let alone Apple, with an average profit of \\$31.16 on each case sold. The Zagg cases come in a close second with an average profitability of \\$30.03. Zagg is also the second-best selling case after OtterBox, selling 123 cases to OtterBox’s 249 cases in 2023. CaseMate, which sits just below OtterBox in terms of profitability, sold the third most cases for iPhones, with 88 sold.

#### Samsung Phone Case Sales

<div class="l-page">
<iframe src="{{ '/assets/cases_post/samsung.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>

Samsung case profits have much less variability in profit gained between case brands than in Apple cases. The difference between the most and least profitable case brand is less than \\$10, with the most profitable case brand being UAG, making about \\$24.2 of profit per case sold. This is much less than Apple case profits and begs the question of whether the Samsung Galaxy A series phones are dragging down the profitability of the other, high-end, Galaxy S Series phones.

The least profitable case brand, Incipio, makes \\$3.5 less than the next least profitable brand. Similar to Apple cases, OtterBox runs away with the most cases sold than any other case brand, selling 2.7x more than the next best-selling case brand. However, a big difference between Apple and Samsung is the number of ItSkins cases sold. ItSkins did not even make the minimum threshold (5) of cases sold for Apple phones, while it was the second-best selling case for Samsung phones. This is probably because Samsung sells more budget phones than Apple does, and ItSkins being a more budget case brand than the other case brands that we stock.

#### Google Phone Case Sales

<div class="l-page">
<iframe src="{{ '/assets/cases_post/google.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>

Google case sales are interesting, to say the least. OtterBox is still the best-selling brand, but not by as much as they are for Apple and Samsung phones. OtterBox is also the most profitable case sold for Google Phones, with a higher profitability than both Apple and Samsung OtterBox cases. The other two best-selling cases for Google phones, Speck and ItSkins, are about \\$8 and \\$10 less profitable than OtterBox. This makes OtterBox the clear choice to recommend to customers looking to buy a case for Google phones.

#### Motorola Phone Case Sales

<div class="l-page">
<iframe src="{{ '/assets/cases_post/moto.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>

Motorola only has two cases brands that sold a significant number of cases. Motorola is the only one where OtterBox isn’t the best-selling case brand, with ItSkins selling just 5 more. This makes sense as Motorola is very much a budget phone brand, and ItSkins is a budget case brand. In terms of profitability, both case brands bring in almost the same amount of money per case sold.

## Conclusion

The easiest way to increase profits from case sales is to recommend cases that sell for more profit per unit sold. Taking the top two best-selling case brands for iPhones, OtterBox and Zagg, we can see the real benefits for recommending certain case brands over others. We make an extra \\$7.46 per Zagg case than an OtterBox case. If we were able to convince 100 iPhone customers to buy Zagg over OtterBox cases, that would be an extra \\$743 of total profit made. That’s only just more money for the company, that’s more money for the associates as well, as each associate would make an extra \\$1.49 per case sale.

Now some customers will only buy a particular case brand, but many customers do ask associates for recommendations or advice on a case. This is where we can sway customers to go for more high profit case brands. Most customers do not need the protection that OtterBox cases provide, and this is where associates can steer customers towards case brands that will still provide excellent protection for their phones, but also make more money for the company and the associate.
