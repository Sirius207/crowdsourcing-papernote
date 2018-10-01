# [Crowd] Latency-oriented Task Completion via Spatial Crowdsourcing  ICDE 2018
Paper Link
https://www.tik.ee.ethz.ch/file/7e3fe399256a08bb03ccfb878425da2d/icde18-zeng.pdf



# Outline
----------

為什麼要看這一篇？ 目標為何？

- 了解在分配空間性質 Question（POI相關） 上，如何進行準確度與回答延遲時間的 Trade off。
  - 因為自身 work 也有這trade off 問題。

看前想了解什麼？

- 問題從哪來？
  - 平台已知
- 問誰問多少人？
  - Acc 最大的，問到問題錯誤率低於定值則停止。
- 如何整合回答？
  - 每當有人回答就重新計算問題的錯誤率直到完成。
- 如何切換準確度？
  - 先定義怎樣的準確度(錯誤率)叫問題完成，往後則根據每位新user回答的情況更新當下問題錯誤率(當錯誤率低於所定值表示問題完成)

看完得到什麼？

- 方法
  - 以 worker 準確度計算問題當前錯誤率來判斷問題是否完成的做法。
  - 算法亦可根據當下環境切換不同策略


- 評估方式
  - 證明 NP-Hard 的過程
  - Approximation Ratio 的算法
  - CR 的算法 (這次是)
  - 時空間複雜度計算方式


## Basic (From paper)
1. 對方想解什麼問題
2. 問題的限制與挑戰
3. 解決的概略流程
    - 碰到，有解了什麼問題 (怎麼解的可以之後再說)
    - 為什麼會想要這樣解？
4. 如何評估
5. 綜合特點 (為什麼會上)
6. ⭐️ 整體價值 (**對題目的可能幫助, 與自身研究問題相關的地方)**
7. 引用 paper 與 延伸方向 (本篇位於 Graph 的哪個位置, 漏洞？)
8. English


## Advanced (From yourself)
- 對於論文有提到的重要問題或阻礙
  - 看之前：你會怎麼解？
  - 看之後：你覺得可能(還)可以怎麼做？
- 這個分支有沒有其他值得解的問題？


## Others
- 作者是誰，哪個Lab，寫過什麼？


# 1. 總結
----------
## 1. 對方想解什麼問題

**概括**


- 如何用回答準確度換回答時間
- 最小化回答時間(人數)，但保有一定準確度


**情境:**

- 概念：
  - 指派worker 回答某地點的yes/no問題，如這家店有停車場嗎？ 
  - 準確度不能低於某值。


**目標：**

- 最小化所有問題 complete 的時間 (Latency)
  - 當估計錯誤率低於一定值時視為問題已 complete
  - latency 可根據 worker 出現的時間得到。


## 2. 問題的限制與挑戰

**限制**


- offline:
  - 任務被指派後便無法更改
  - 問題回答容量限制
  - 錯誤率限制


- Online:
  - Worker 出現資訊未知
  - Worker 一出現必須立刻指派任務

**挑戰**

- *how to trade-off latency and quality of task completion*.
- *how to deal with the online scenario and make arrangement among workers and tasks with partial spatiotemporal information*
- --
- offline LTC 問題是 NP hard


**已知**

- Task
  - Task 內容（問題都已知只等著丟出去問）
    - yes, no, skip
  - Task 錯誤容忍率
    - 每個都一樣
- Worker
  - Worker 歷史準確度
    - 不低於 66%
  - Worker 回答上限
    - 每個人都一樣
- 預測準確度
  - 根據 worker 歷史準確度跟 與 task 的距離來訂


- 任務完成
  - 錯誤率低於多少算完成
  - 所有任務都可以達到這個低錯誤率


- 完成時間
  - 根據最後一個回答的人的回答時間來看


## 3. 解決的概略流程


**offline:**

- Approximation Ratio: 
  - worst case 所需 worker 數 / 最佳狀況所需 worker 數



- MCF-LTC:
  - 方法： 先 Iteratively 選擇一個 batch 的 worker ，再從中找出 local-optimal 的 arrangement。
  - Approximation Ratio: 7.112
  - 時間複雜度：O(|W|K / δ|T * (|T|^3 + |T|^2) = |W||T|^2).
    


**online:**

- Competitive Ratio: ≤
  - 算法 max latency / optimal latency




- LAF: 
  - 方法：基於 Greedy 的算法，持續 assign largest ACC 的 Task 給 worker。
  - Competitive Ratio: 5.5
  - 時間複雜度： $$O(|W|(|T|logK + KlogK)) = O(|W||T|logK) = O(|W||T|)$$


- AAM: 
  - 方法: 對於容易的任務採用 LGF 策略，困難的任務則採用 LRF 策略。
    - 先知道完成所有任務平均需要多少 worker，以及完成剩下的任務最多需要多少 worker，再根據當下這兩個數字的比較來判斷現在是任務太多還是有困難的任物卡著，進而決定採用什麼策略。
  - Competitive Ratio:
![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538403422194_image.png)

  - 時間複雜度：  $$O(|W|(|T|+ |T|logK + KlogK)) = O(|W||T|logK) = O(|W||T|)$$



## 4. 如何評估

**資料集來源**

- Real
  - Foursquare
      - 包含 user 在紐約與東京的 checkin places 資料
      - Task 的資料則根據 POI 的位置產生
      - 由於資料沒有 worker 的歷史準確度，他使用常態分佈來產生該數據。
![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538377925617_image.png)



- Synthetic
  - 產生一 1000*1000 之 grid, 每個grid代表 10m * 10m
  - worker 與 task 的回答距離是 300m
![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538377919353_image.png)



**Baselines**

- Base-Off: offline 算法，task 採用 greedy 的方式 assign 給新 worker
  - Base-off is an offline baseline where tasks with fewer workers nearby (from the remaining workers) are greedily assigned to the new worker when s/he arrives on the platform.
- Random


**評估數值**

- 數值
  - 任務完成延遲時間：依據被第幾位 Worker 解掉來算
  - 運作時間
  - 記憶體用量
- 拓展性

**其他設置**

- C++
- 512G memory


**最終效果**

- 改變 T
  - 原則上 MCF-LTC 最好，但在 T 超過 5000 時，AAM 的效果會更好
    - 因為 T大 那 batch 也大，有些 worker 的效果不錯但 index 很大而MCF-LTC傾向選擇 ACC 大的 worker，導致總延遲率會提高。
  - MCF-LTC 的時空間用量最大，而LTC 跟 AAM的時間跑得比 random 久，因為要維護一個 heap，但空間用量 與 random 差不多。


- 改變 K
  - K變大，因為worker可以做更多，所有算法的延遲率自然都會下降
    - MCF-LTC 的延遲率是最佳的(不過當K是4時，AAM 的效果比 MCF-LTC 還好)
  - 與改變T類似，MCF-LTC最大，另外兩個 online 算法也比 random 久。


- 改變 準確度分佈, 錯誤容忍率
  - MCF-LTC 跟 AAM 效果最好
  - 時空間用量與改變K接近



- 拓展性 (改變T到 剛才最大值的兩倍)
  - MCF-LTC 在 T 很高時效率很低，但AAM 跟 LAF 還在可接受範圍內


- 在真實資料的效果
  - AAM 效果最好，
  - MCF-LTC 效果也不錯，永遠超過 Base-Off。 但效率是最差的
  - LAF 效率是最好的


## 5. 綜合特點 (為什麼會上)


**貢獻 (自述)**

- 第一篇探討 quality 跟 latency 之間 trade off 的 work
- 提出 LTC 問題並證明他是 NP-Hard
- 在 offline 環境中 提出 MCF-LTC算法，在online 環境中提出兩個 greedy based 的算法
- 用真實與合成資料來評估效果與效率


**其他 (判斷)**

- 用 worker quality 來計算當前問題的總錯誤率的方式
- --
- 提出的 AAM 的效果不錯(即便只是根據環境切換策略)，甚至部分狀況會比 MCF-LTC 還要好
- NP-Hard 證明過程，以及其為此評估效果得 Approximation ratio 跟 CR 值計算可以參考。



## 6. 整體價值 (可延伸應用)

**方法**

- 整合回答方面透過過濾 worker 歷史準確度來確保 錯誤率低於一定值（整合不同回答）。


**應用**

- Task 已知(回答 yes/no 問題)，Worker 未知的環境
  - 本篇論文不考慮準確度不高的 worker 的情況，也假設所有問題都可以達到該低錯誤率。
  - 不考慮 worker 回答 skip 的情況。


**與自身題目之差異**

- 本篇 work 的問題(Task)已知，但自身的 Task 還需要將 Task 分割為子 Task 
  - 但也可以先假設子task已知，測試在線下線上環境的效果。
- 本篇 work 的問題選項也已知(yes/no)，但自身 work 可能需要根據回答來產生不同問題選項
- 本篇 work 的 worker 是自願，但自身的 work 應該會有預算，所以要考慮更嚴謹的 quality control。

**可擷取**

- 其對於錯誤率的定義
- Approximation ratio & CR 計算方式
- 算法可變化後當作類似往後 baseline



## 7.  引用 與 延伸方向 (本篇位於 Graph 的哪個位置)


- 考量 Worker 準確度與回答錯誤率之地理空間問題指派。


- 2018 年的 work 目前被引用 3 次
  - [**Multi-Worker-Aware Task Planning in Real-Time Spatial Crowdsourcing**](https://link.springer.com/chapter/10.1007/978-3-319-91458-9_18)
    - 幫忙 worker 做 task 計畫，確保其利潤最高
  - [**Specialty-Aware Task Assignment in Spatial Crowdsourcing**](https://arxiv.org/pdf/1804.07550)
    - 探討 worker 本身有不同 Skill 彼此要合作的情況。
    - 極大化完成任務個數
  - [**Online Variant of Parcel Allocation in Last-mile Delivery**](https://arxiv.org/abs/1806.05983)
  



## 8. English


The idea of our proposed algorithm is to iteratively select a batch of workers and make an local optimal arrangement for these workers.

In terms of running time and memory, MCF-LTC consumes the most. Both AAM and LAF consume more time than Random but with similar memory cost.

In this paper, we identify the Latency-oriented Task Com-
pletion (LTC) problem in spatial crowdsourcing.


# 2. 內容
----------



## 0. Abstract

對於社群媒體與LBS而言， Spatial-crowdsourcing 透過手機用戶帶來一種新的搜集地理資料的方式。 舉例，當用戶到一家店 在臉書 checkin時，臉書可以即時發送一些像是”這家店幾點開”這類的小問題。 但既然 worker 的數目有限，平台需要透過一些策略確保問題的品質與完成時間。 一般來說在crowdosurcing 的 quality control 跟 latency control 不管用，因為他們假設 worker 數是夠的，也忽略了一些空間參數。

這篇論文中，他們定義了 LTC 問題，其trade off 了 quality 與 latency，此外也證明了這問題是 NP-Hard 的。 他們首先提出了一項 minimum-cost-flow based 的算法來解 offline問題，接者探討線上環境，他們提出了兩種 GREEDY 的算法來解，最後也使用了真實與合成資料來驗證 effectiveness 跟 efficiency。



## 2. Problem Definition

**a.Basic**

[Task]
t =< lt,ε >

- lt: 地點
- ε: 錯誤容忍率

Task 基本上是 yes/no 的問題，有兩個假設：

- 在執行前已知有多少個 Task
- 有錯誤容忍率

===

[Worker]
w =< ow, lw, pw, K >

- ow: 出現序列
- lw: 地點
- pw: 歷史準確度
- K: 一次出現能接多少回答


Worker 則有以下兩個假設：

- Worker 的歷史準確度皆高於 66%
  - 低於 66% 的會被視為 spam 被平台排除
- 每位 Worker 的容量是相同的，其數字是根據調查報告的結果
  - 以這篇 paper 的資料來說，若超過這數字會被視為不同 worker

===

[Predicted Accuracy]

透過歷史準確度，以及 Worker 與 Task 當下的位置來計算準確度。

![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538137445329_image.png)


dmax 是 worker能回答該問題的最大距離。

===

[Task Completion]

透過 Weighted Majority Voting 整合不同 worker 的回答，

![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538137325383_image.png)


當錯誤率達到一定值時是為任務完成。


![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538138192024_image.png)


每個任務至少需要被 回答 ⌈δ⌉ 次才能達到錯誤率，而本篇也假設 

- worker 收到任務時會快速回答問題(不會快速回答的都被排除了)
- 所有的問題都可以達到該錯誤率。

===

[Task Latency]

根據完成任務時，最後一位回答問題的 worker 出現順序而定。

**b.Offline define**

限制：

- 任務被指派後便無法更改
- 問題回答容量限制
- 錯誤率限制

此外 offline LTC 問題是 NP hard


**c.Online define**

限制

- Worker 出現資訊未知
- Worker 一出現必須立刻指派任務

此外平台也假設 worker 不會在同一地點待很久


## 3. Offline

output: 任務與worker 的 pair 以及任務完成延遲率

Approximation ratio: 算法延遲時間 / Optimal 延遲時間


**Theorem 2**
先計算 max 延遲時間的上下邊界

upper bound:

![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538140485205_image.png)


lower bound:

![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538140477322_image.png)



- 要找到 Optimal arrangement 是 polynomial time

**算法簡述**
The idea of our proposed algorithm is to iteratively select a batch of workers and make an local optimal arrangement for these workers.
不斷取出 一個 batch 的 workers 再計算local optimal 的 arrangement。

將問題轉為 Minimum cost flow (MCF) 問題後，再將 theorem 2 的 lower bound 作為 batch 的 大小。


**算法細節**

GF = (NF,EF)

- NF = W′ ∪ T ∪ {st,ed}
  - st: source, ed: sink
- Edge: eF(w,t) ∈ EF from w to t 
  - eF (w, t).cost = −Acc∗(w, t)
  - eF (w, t).capacity = 1


- Apply Successive Shortest Path Algorithm (SSPA) to calculate the minimum cost flow。
  - suitable for 
    - large-scale data
    - many-to-many matching with real-valued arc costs [16].


![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538372416251_image.png)


δ: minimal accumulated Acc∗ of each task
Q: a heap to maintain the largest Acc∗ for each worker
m: set as the lower bound of the maximum latency

**範例**
ε = 0.2, 
δ = 2ln(1/0.2) = 3.22
m = |T|⌈δ⌉/K = 6




## 4. Online

**LAF (Largest ACC First)**

- **基本概念**
  - Greedy select the tasks with the highest Acc∗ for every worker.
  
- **算法細節**
  - 當 worker 出現時，前 K 大 ACC*的 Task 就會被 assign 到該worker。


![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538406026983_image.png)



**AAM (Average & Max)**

- **基本概念**
  - hybrid greedy algorithm
  - AAM 在一般的任務使用 LGF (Largest Gain First)，當遇到困難的任務時則切換到 LRF (Largest Remaining First)。
    - 由於算法的瓶頸可能卡在某些困難的任務，因此偵測困難的任務並提早安排這些任務是很重要的


- **算法細節**
  - maintain
    - the average number of workers needed to finish all tasks
    - the maximum number of workers needed in any remaining task (“maximum” for short).
    - 算法將根據這兩個值切換策略
  - 狀況
    - 目前平均大於剩餘最大：
      - 採用LGF策略
        - LGF 一開始會丟高準確率的 worker，但如果這個任務只差一點就可以達到我們要的準確率，就不執著於丟準確率高的worker給他。


    - 剩餘最大所需worker大於平均：
      - 採用LRF策略
        - 現在有困難的任務擋著造成瓶頸，因此優先解掉這些任務。


![](https://d2mxuefqeaa7sj.cloudfront.net/s_70488196B028D68A55A499646996D4944E02102CB021525F72DF283FFA56BEC1_1538405393623_image.png)






# 3. 論文架構
----------



Abstract

----------


- Spatial crowdsourcing 的一點好處
  - 比方說搜集資料...
  - 搜集資料的需求
  - 這些需求傳統上做不到，因為沒考量到
- 這篇我們訂了xx問題
- 證明這問題是 np-hard
- 我們首先提出xx 算法運作於現下環境
- 接下來我們考慮線上環境, which...
- 我們提出兩個算法運作於線上環境
- 最後，我們用真實與模擬資料驗證了算法效果與效率。

Intro

----------


- 近年狀況，平台簡介，顯示方式簡介，平台需求，如何 trade-off...
- 問題的挑戰
- 範例情境
- offline & online 環境的解說
- 問題定義，現存研究主要領域，本篇貢獻




Problem

----------


- 各項參數設定（定義）
  - 設定原因與參數來源
- Offline 環境的限制與 np-hard 證明
- Online 環境的限制


Offline

----------


- 因為這個問題是 NP 問題，所以我們提出什麼算法基於什麼
- 跟最佳解比起來，這個算法的 xx 值是多少，我們也先根據 xx 理論來計算大小邊界
  - xx 理論的計算與推導
- 算法基本概念，細節舉例，xx 值的計算，複雜度分析

Onine

----------


- 這篇我們提出兩個算法，在這環境中...，我們採用 greedy 的方式...
- 我們首先提出...基本上是，接下來為了避免局部最佳，我們更提出了其他算法
- 算法概念，舉例，複雜度分析


Exp

----------


- 實驗設定
  - 資料集來源，資料集參數
    - 資料集沒有的參數怎麼來
  - 評估哪些算法(比較)，評估哪些項目？
  - 實驗在什麼環境？ 用什麼語言？ 跑幾次？
- 實驗結果
  - 改變參數對效果與效率的影響

Related Work

----------


- 這篇 work 跟以下三個領域的研究有關
- xxx 是個重要的議題，因為... 一般來說有兩種解法... 分別是... 但我們的 work 同時要關注xxx 與 yyy 所以前述方法行不通
- --
- 對於 yyy 問題，平台需要...，現在的研究是用金錢來吸引(AMT)，但我們的work回答問題的是志願者
- 因此一般的做法不適用，最接近的 work 是，他們提出三種做法
- 但他們假設 worker 永遠都在，但現實環境中不會這樣
- --
- 這領域近來來越來越熱門，主要的研究領域是task assign & quality control，但延遲時間其實是個被小看的問題，這篇論文就我們所知是第一篇解探討 quality 跟 latency work 的 paper。



Conclusion

----------


- 這篇 work 我們 identify 了這問題
- 在線下環境，where...，我們提出了xxx算法
- 在線上環境，where...，我們提出了xxx算法
- 最後，我們透過合成資料與真實資料的密集實驗驗證了效果效率與拓展性。

