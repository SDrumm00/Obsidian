---
Created:
  - 20240312 9:15 AM
aliases: 
tags:
  - woorkstation
  - linux
  - hardware
  - ZFS
  - Pantheon
Subject: Workstation Roadmap
---
-----------------
## Abstract
### How should I build my workstation out for the next 3-5 yrs?

I want my workstation to migrate to my server rack in the future so that means no more "prosumer" parts or gaming parts. More parts intended for workstation builds and professional work requirements.

I still game, just, not as much and mostly Steam. So... [[NixOS]] for sure. Best [[Linux 1]] OS I have seen to date. Plus it has all the capabilities I require in my host OS to get the work done.

_Also, no more RGB builds.....EVER_

### Build Philosophy
See the [[Workstation Philosophy]] for the core values of this machine.
### Possible paths to take as of today
1) Upgrade/convert my threadripper build into a workstation from and RGB/gaming machine.
2) Build an older generation dual Xeon (LGA2011) workstation.
3) Build a single socket workstation with many threads and many PCIe lanes while retiring my old workstation for a server in my rack...

I hesitate to go route 2 because with CPU;s having so much power now, 64 cores and 128 threads is within reach (AMD EPYC). Higher frequency fewer cores is possible in Xeons. Maybe both will have both in the near future?

Route 1 is desirable because it paves the way for route 3. I can build my next workstation in a few years, save a few bucks now AND move my current workstation to the server rack at that time.

### So my path is: 
1 > 3

Here is the list of items required to complete the build so far

| Item       | Unit Price | Qty | Total Cost | Link                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ---------- | ---------- | --- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CPU        | $380       | 1   | $380       | https://www.ebay.com/sch/i.html?_nkw=threadripper+2950x&_sacat=175673&_svsrch=1&LH_PrefLoc=2&LH_All=1&_sop=15                                                                                                                                                                                                                                                                                                                                                                                  |
| CPU Cooler | $100       | 1   | $100       | [https://www.amazon.com/gp/product/B074DX2SX7/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1](https://www.amazon.com/gp/product/B074DX2SX7/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)                                                                                                                                                                                                                                                                                                   |
| RAM        | $48        | 2   | $96        | [https://www.crucial.com/memory/server-ddr4/mta18adf2g72az-3g2r](https://www.crucial.com/memory/server-ddr4/mta18adf2g72az-3g2r)                                                                                                                                                                                                                                                                                                                                                               |
| Fans       | $35        | 1   | $35        | [https://www.amazon.com/dp/B09C6DQDNT/ref=emc_bcc_2_i](https://www.amazon.com/dp/B09C6DQDNT/ref=emc_bcc_2_i)                                                                                                                                                                                                                                                                                                                                                                                   |
| HBA        | $318       | 1   | $318       | [https://www.ebay.com/itm/165522543628?epid=0&hash=item2689e9940c:g:U2IAAOSwXUlioA5F](https://www.ebay.com/itm/165522543628?epid=0&hash=item2689e9940c:g:U2IAAOSwXUlioA5F)                                                                                                                                                                                                                                                                                                                     |
| SSD's      | $42        | 16  | $672       | [https://www.amazon.com/Crucial-BX500-NAND-2-5-Inch-Internal/dp/B07YD579WM/ref=sr_1_4?qid=1685989552&refinements=p_n_feature_keywords_six_browse-bin%3A6158683011%2Cp_n_feature_three_browse-bin%3A6797521011&rnid=6797515011&s=pc&sr=1-4&th=1](https://www.amazon.com/Crucial-BX500-NAND-2-5-Inch-Internal/dp/B07YD579WM/ref=sr_1_4?qid=1685989552&refinements=p_n_feature_keywords_six_browse-bin%3A6158683011%2Cp_n_feature_three_browse-bin%3A6797521011&rnid=6797515011&s=pc&sr=1-4&th=1) |

All in, it's over $1600 dollars. Ouch.  So we do it in chunks as money permits.

After reviewing benchmarks online... I don't think it's worth it to upgrade the CPU.... $380 for only a %20 increase in performance is not that great...

DATE: 20230627
Turns out, I made a few revisions...

| Item       | Unit Price | Qty | Total Cost | Link                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ---------- | ---------- | --- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CPU Cooler | $100       | 1   | $100       | [https://www.amazon.com/gp/product/B074DX2SX7/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1](https://www.amazon.com/gp/product/B074DX2SX7/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1)                                                                                                                                                                                                                                                                                                   |
| RAM        | $48        | 2   | $96        | [https://www.crucial.com/memory/server-ddr4/mta18adf2g72az-3g2r](https://www.crucial.com/memory/server-ddr4/mta18adf2g72az-3g2r)                                                                                                                                                                                                                                                                                                                                                               |
| Fans       | $35        | 1   | $35        | [https://www.amazon.com/dp/B09C6DQDNT/ref=emc_bcc_2_i](https://www.amazon.com/dp/B09C6DQDNT/ref=emc_bcc_2_i)                                                                                                                                                                                                                                                                                                                                                                                   |
| HBA        |            | 1   |            | Gonna use the existing one I have in the basement, it just needs flashed into IT Mode                                                                                                                                                                                                                                                                                                                                                                                                          |
| SSD's      | $42        | 16  | $672       | [https://www.amazon.com/Crucial-BX500-NAND-2-5-Inch-Internal/dp/B07YD579WM/ref=sr_1_4?qid=1685989552&refinements=p_n_feature_keywords_six_browse-bin%3A6158683011%2Cp_n_feature_three_browse-bin%3A6797521011&rnid=6797515011&s=pc&sr=1-4&th=1](https://www.amazon.com/Crucial-BX500-NAND-2-5-Inch-Internal/dp/B07YD579WM/ref=sr_1_4?qid=1685989552&refinements=p_n_feature_keywords_six_browse-bin%3A6158683011%2Cp_n_feature_three_browse-bin%3A6797521011&rnid=6797515011&s=pc&sr=1-4&th=1) |

Lowers the bill to $231 plus disks. Which I need to review pricing on spinning rust now rather than all flash. Should be able to bring that $673 down to under $300.

Operating Systems of choice are:
1) Ubuntu Pro
2) Debian Stable
3) NixOS
4) Redhat or CentOS-Stream

### Progress
To see how far we are in relation to our goal state, please see the [[Workstation Setup]] file for the current state.
