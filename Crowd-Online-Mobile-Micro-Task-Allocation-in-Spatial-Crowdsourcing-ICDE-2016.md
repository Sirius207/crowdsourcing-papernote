# [Crowd] Online Mobile Micro-Task Allocation in Spatial Crowdsourcing - ICDE 2016


# Outline
----------

為什麼要看這一篇？ 目標為何？

- 此研究情境是”讓worker能透過手機即時接下空間性質任務”，與本身研究性質接近
- 了解這篇 work 的”即時”算法

看前想了解什麼？

- 如何定義 micro-Task
  - 不需專業技能即可達成之 Task: 例如拍張照證明這裡人不多。
- 時間限制多少？ 
  - job 與 Worker 有接單時間限制
- 怎麼達到即時解？
  - 即時的意義是線上動態，本篇是探討在task 與worker 未來出現時間地點都未知的情況下還能有良好的效果的方法。
- 如何決定派給誰？
  - Greedy 跟 global optimal 來比對
- 派多少人？
  - 設定一項任務僅有一位 worker 可接
    - 但一人可接多個 task

看完得到什麼？


- CR 評估算法 (與線下環境最佳解的比率)
- Greedy 的變化思考(拆一半分頭用不同方法解)
- Hungarian algorithm
- 時空間複雜度的lemma計算方式



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


# 1. 總結
----------
## 1. 對方想解什麼問題

**概括**

- GOMA
  - Global online Micro-Task Allocation in spatial crowdsourcing
  - 在線上即時環境下的 Task Assign 問題。

**場域限制：**

- 線上環境：Task 與 Worker 之出現時間皆未知
- Worker 有接案容量，接案範圍，時間死線限制
- Task 有時間死線限制。
  - 一項 Task 僅能指派給一位 Worker

**情境:**

- Tony 想知道某家餐廳現在人多不多，因此發佈一項任務請求worker拍張餐廳等候狀況的照片，回覆時間越快越好。


**目標：**

- Task有報酬，Worker有達成率，兩者之關聯為Utility。 此研究想在兩者出現時間皆未知的情況下，極大化平均 Utilty。
![](https://d2mxuefqeaa7sj.cloudfront.net/s_D1225A3B109C5B5BC899461487124EE16C7B9FE9AB6CFCC8ADDCAAC3021F8D92_1536916504067_image.png)



![](https://d2mxuefqeaa7sj.cloudfront.net/s_D1225A3B109C5B5BC899461487124EE16C7B9FE9AB6CFCC8ADDCAAC3021F8D92_1536916513330_image.png)




## 2. 問題的限制與挑戰

**限制**

- Task Worker 出現順序未知

**挑戰**

- 運算時間與Greedy不能差異太大
  - 本篇一萬件的時間差異在5s內 (觀察圖表得知)

**已知**

- Task
  - 位置，時間，期限，報酬
- Worker
  - 位置，時間，期限，工作半徑，接案數，成功率


## 3. 解決的概略流程


**方法流程**

TGOA (Two phased based framework)

- 將預計出現的 Task & Worker 列 分成兩部分(phase)，前半部份採用 Greedy，後半部使用 Hungarian algorithm 找出 global optimal 之後，再判斷是否符合(若符合就指派該task 或 worker)。



## 4. 如何評估

**評估方式**

- 測試五十個不同 T, W 出現順序計算平均數值

**資料集來源**

- Real
  - gMission dataset[25]
    - Task
      - description, location, release time, deadline, payoff
    - worker
      - description, location, release time, deadline, maximum activity range, success ratio.
  - EverySender dataset[連結失效]
  - 上述資料集都沒有 capacity, 本篇論文用 uniform distribution 補足


![from paper](https://d2mxuefqeaa7sj.cloudfront.net/s_D1225A3B109C5B5BC899461487124EE16C7B9FE9AB6CFCC8ADDCAAC3021F8D92_1536841228815_image.png)

- Synthetic (如何製作？)
  - 分佈： 均勻與常態都有
  - 出現順序則依照均勻分布


![from paper](https://d2mxuefqeaa7sj.cloudfront.net/s_D1225A3B109C5B5BC899461487124EE16C7B9FE9AB6CFCC8ADDCAAC3021F8D92_1536841250531_image.png)



**Baselines**

- Greedy-RT

**評估項目**

- utility score
- running time
- memory cost
- --
- Scalability (線性時間)

下列參數變化對算法效能的影響

- Cardinality of W
- Cardinality of T
- Capacity
- Radius of Restricted range
- Success ratio
- Deadline
- pay off


算法評估

- Scalability
  - TGOA-Greedy 為線性增長
- Real Dataset

效果

- 改變 W, T 數量, 接單容量時，Utility 會最高，但因為 cubic 時間複雜度會跑更久，記憶體用量也更大。
- 改變接單距離, 成功率, 接單死線 不影響運行時間


## 5. 綜合特點 (為什麼會上)

**貢獻 (自述)**

- 提出應用更廣泛之GOMA問題
- 建構出在不同環境下 Utility 皆大於 Greedy 之算法(CR 1/4)。 但考量到運行時間，後續更提出另一個Utility 與 原先算法接近，但運行時間更低之算法(CR 1/8)。
  

**其他 (判斷)**

- 算法效果跟Greedy比雖然不錯，但算法本身並不複雜，基本上也是Greedy的變化
- 該做的評估(效果，時空間複雜度，拓展性)都有做
- 其對於 CR 與 時空間複雜度 計算得出的 多個 lemma 跟 Theorem 計算過程可以參考。



## 6. 整體價值 (可延伸應用)

**方法**

- 將 Task 與 Worker 資訊拆成兩半分別用 Greedy 跟 Global optimal 判斷是否指派。

**應用**

- 現實生活中動態的 Task 與 Worker 出現環境。
  - 本文僅定義micro-task，成功率與worker專業度無關的任務。 若要多考量 worker 的專業領域 或是 worker 須合作的任務就要再調整。

**與自身題目之差異**


- 或許自身 Task 的出現時間也可以根據歷史資訊預測。
- Task 與 問題不同，不需考慮 assign 失敗的情況或回答不同的情況。
- 自身不需多考慮單一 worker 的接案數量。


## 7.  引用 與 延伸方向 (本篇位於 Graph 的哪個位置)


- 主要考量 動態節點的出現指派，不須整合回答也不用考量worker品質或合作狀況。

延伸方向有什麼paper? 已經做了什麼？

- 目前被 引用 89次(2018-9-16)
- [FTOA](http://delivery.acm.org/10.1145/3140000/3137643/p1334-tong.pdf?ip=140.116.247.113&id=3137643&acc=ACTIVE%20SERVICE&key=AF37130DAFA4998B%2EEBD64A1DDB1C5FF0%2E4D4702B0C3E38B35%2E4D4702B0C3E38B35&__acm__=1537111803_a0ff68b706bb594696600b2b27dfa252): 考量 worker 會移動的情況，試圖減少 worker 的移動距離。
- 往後補充




## 8. English


- To the best of our knowledge, as discussed later, this is the first work that studies the GOMA problem particularly under the random order model and thus we should design efficient algorithms specifically for our problem. We make the following contributions.



- We clarify that  the baseline algorithm performs not well enough in practice since the worst case under the adversarial model happens with a very low probability in real world.


- However, there are two major differences between our GOMA problem and [31].


相同句型…

- We can first see that / We can see that / we can again see that
- we can first observe that / we can observe / we can observe similarly that
- Our first observation is that


- The reason is that,
  - which is reasonable since there are…
  - due to its

相同句型…
Finally, for memory consumption,

- we again observe that the TGOA-based algorithms consume more memory.


- we can see that TGOA consumes the most and the memory consumption of the three algorithms does not vary too much as the success ratio of workers varies.


- TGOA is again the most inefficient and the memory consumption of all the algorithms does not vary much with varying deadline


- we can again see that TGOA is the most inefficient and all the three algorithms do not vary too much in memory when the payoff of tasks varies.



# 2. 內容
----------



## 1. Intro

現存的 spatial crowdsourcing paper 多是在解”任務分配”問題，確保最終 task assign 量最多或 total task worker 契合權重最高[2]。 但這些 paper 都假設環境是線下的，所有的任務跟worker 地理位置都是先前已知的，而這無法應用在即時動態的環境。 比方說以下情境：


> Tony 想知道這家餐廳現在人多不多，所以發了個 拍照 task 到這類平台上期望得到立刻回覆。

這產生了以下問題： 如何即時分配問題到合適的worker？ 如何 model 這類問題？

在線下環境，這類 paper通常會將 task 與 worker 分配在 bipartite graph，再以 maximum weighted bipartite matching的方式來解，但在現下環境，因為task 與 worker 出現的順序未知，所以無法應用。

- 舉個例子，task t1~t6 與 worker u1 ~ u3，每個 worker 有自己的工作範圍，也有自己的最大接任務數量，在已知每個worker與task的座標，以及worker對應task的契合度之下，我們可以用最佳化算法解此問題。
- 但這在線上環境上不見得有效，舉個例子


我們將這種動態問題定義為 GOMA 問題，現存的研究對此多半專注於算法在 worst case 的效能，但現實世界中這種 worst case 出現頻率很低，為此我們本篇在意的是平均效能。 

- 有個類似的研究分支是 online maximum weighted bipartite matching(OMWBM) problem[9,10], 他們 worker 或 task 其中一種的資料是已知的，另一個則會隨機變動。
- 而我們的work中 worker 與 task 的資料都是隨機變動的，為此 可以說 OMWBM 問題其實是一種GOMA 問題。 而本篇是第一篇解決這種問題的 work。

本篇實際貢獻如下


- 定義 GOMA 問題，將 state of art 算法應用於 GOMA 並得到 2eln(1+Umax) 的 competitive ratio。
- 確認 baseline 算法的效果其實不佳(因為worst case 不常出現)。 我們提出了
- 透過實際與合成資料實驗驗證了本篇算法的 effectiveness & efficiency 。


## 2. Problem

A. **問題定義**

參數

- t =<l, a, d, p>
  - 地點，時間，期限，報酬


- w=<l, a, d, r, c, δ>
  - 時間，地點，底線，半徑，容量，歷史達成率


- utility = $$$$$$p_t$$ * δw


- micro-task
  - 拍照，確認價格等不需特殊技能就可完成之Task，因此成功率表示該worker之可靠度


> The GOMA problem is to find an allocation M among the tasks and the workers to maximize the total utility。

限制

- task 與 worker 必須在彼此指定的死線期完成指派
- 任務指派完不可更改
- worker 接收的任務
  - 數量不能超過其容量上限
  - 地點不能超過他的移動範圍

B. **Online Input Models**

Offline 環境的設定少了死線與不可更改指派這兩個限制

CR (competitive ratio) 的定義是 將目前的online演算法與線下最佳解的效果做比較。

本篇採用兩種model進行比較:

- adversarial model:
- random model:


CR 在兩種不同model下的計算方式如下：

Adversarial model

![from paper](https://d2mxuefqeaa7sj.cloudfront.net/s_D1225A3B109C5B5BC899461487124EE16C7B9FE9AB6CFCC8ADDCAAC3021F8D92_1536820854764_image.png)



Random model

![from paper](https://d2mxuefqeaa7sj.cloudfront.net/s_D1225A3B109C5B5BC899461487124EE16C7B9FE9AB6CFCC8ADDCAAC3021F8D92_1536820862189_image.png)


E 是 期望值



**C. Offline Algorithm**

線下算法基本上是將GOMA問題轉換為MWBM問題之後，再用 Hungarian algorithm[12] 來解。



## 3. Related

**Spatial Crowdsourcing**

**OMWBW**
OMWBW 問題是 maximum weighted bipartite matching problem 問題的線上版。

- 輸入值是一個 bipartite graph G = (L, R, E, U)
  - L: left-hand vertices
  - R: right-hand vertices (unknown arrive one by one)
  - E: edge (l,r)
  - U: edge 的權重 U(l,r)
- 目的是最大化 total matched edges 的 weights

根據對 R 不同的出現頻率假設，可分為 adversarial Model 與 random Model:

Adversarial:
R, E, U 與 “R的出現順序”是隨機的，主要 Focus 在 “R 的出現順序” 是 worst case

接近的有[31] 這個work， 他也是 兩邊 vertices 都是隨機，但他只能處理 unweighted cases，此外我們的work還有個貢獻是想出在random model 的算法，這是前者沒有做到的。

Random Model:
R 的出現順序是依照 uniform random permutation, 因此專注於算法的 average or expected performance。

[32] 證明了simple greedy 算法可以將CR從 1/2 提升至 1 - 1/e
而[34] 更將CR提升到了 1/e，然而這些研究都只有一邊的 vertice 是online的，而兩邊vertice都是 online 的GOMA 問題在當時還無法被任何算法直接解決。




## 4. Baseline

**Extend-Greedy-RT**
原先的 Greedy-RT[11] 適用於 OMWBM 問題，在這種情況下只有 task 或 worker 一邊的情況未知。 其概念是先隨機設定一 threshold，如果新出現的點造成的 edge weight 有超過 threshold 再採用。 而本篇延伸這算法，情境上除了兩邊都是未知外，也考慮到task與worker 工作死線的問題使其更接近現實狀況。

新算法的概念如下
$$e^k$$ 為 threshold，如果新出現的點能造成大於 threshold 的 Utility 就採用。
其 Competitive ratio 為 $$\frac{1}{2e[ln(U_max+1)]}$$


![](https://d2mxuefqeaa7sj.cloudfront.net/s_D1225A3B109C5B5BC899461487124EE16C7B9FE9AB6CFCC8ADDCAAC3021F8D92_1537110301512_image.png)



## 5. Framework

本篇論文提出兩種算法： TGOA 與 TGOA - Greedy，後者比前者有效率，但效果稍微降低。

**TGOA**
TGOA 的 CR 值為1/4，概念是將 所有Task 與 worker 依照出現順序分成兩部分並用不同方式計算，由於有歷史資料，我們可得平均一天有多少Task 與 worker ，並藉此得出要分成兩部分的時機。

第一部分採用Greedy，將會 assign Task 給目前滿足限制條件，Utility 又最高的Worker，反之亦然。 第二部分則是當有新的點(Task or Worker)出現時，會先用 Hungarian algorithm 計算 global optimal match $$M_v$$，如果新到的節點滿足 $$M_v$$ 則指派給它，如果不滿足則等待新的節點抵達。

**TOGA - Greedy**
由於前者在時空間複雜度較高(cubic time)，因此多提出TGOA-Greedy，其 CR 值為 1/8，與其者的差異在於將 Hungarian algorithm 換成 Greedy Match。


![](https://d2mxuefqeaa7sj.cloudfront.net/s_D1225A3B109C5B5BC899461487124EE16C7B9FE9AB6CFCC8ADDCAAC3021F8D92_1537109257372_image.png)




## 
# 3.  論文架構


Abstract

----------


- 目前研究領域，以及他們的問題
- 這篇論文提出...
- 用xxx的步驟
- 最終結果如何，使用什麼資料？


Intro

----------


- 近年狀況


- 近年研究專注於...
- 但這些work不適用於...
- 比方說這個情境...
- 因此產生了新問題...


- 過去的研究在xx情境使用xx算法，xx算法的方式是
- 舉個例子（數字）
- 但這樣在這個環境不行，舉個例子


-  為此我們提出了新的
- 我們的貢獻是...

problem statement

----------


- 參數設定
- 問題設定
  - 最大/小化什麼？
  - 限制
- Model
  - offline / online



- 你的 比較算法怎麼算 (如果你的環境是線上的話)


Related

----------



- 過去的B問題，有xx解法，效果xxx，但這問題跟我們的xxx問題不同，現有無法解決。
- 最接近的有xx work ，但它的work 沒辦法處理xxx，這我們有做到，還有多做xxx



Baseline

----------


- baseline 使用 xx 算法，在這問題的流程是..
- 而在我們的問題，我們調整xx算法，有個要注意的是...


- 新算法的流程是... code 第1,2行意思是...
- 舉個實際的例子，流程..

Framework

----------


- 算法簡介
- pseudo code解說
- 計算案例
- Competitive Ratio 計算
- 複雜度分析



Experience

----------


- 實驗設定
  - 資料集來源，資料集參數
  - 評估哪些算法，評估哪些項目？
  - 實驗怎麼做？ 跑幾次？
- 實驗結果
  - 參數影響






