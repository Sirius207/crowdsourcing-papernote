# [Crowd] **Where To: Crowd-Aided Path Selection - VLDB_2014**


# Outline
----------


1. 對方想解什麼問題
****2. 問題的限制與挑戰
3. 解決的概略流程 (你會怎麼解)
4. 如何評估
5. 綜合特點 (為什麼會上)
6. ⭐️ 整體價值 [+[Crowd] Where To: Crowd-Aided Path Selection: 6.-整體價值-(可延伸應用)](https://paper.dropbox.com/doc/Crowd-Where-To-Crowd-Aided-Path-Selection-6.-KKYG3sUXTeGzu8wSvrNDa#:uid=881344659324908047714799&amp;h2=6.-%E6%95%B4%E9%AB%94%E5%83%B9%E5%80%BC-(%E5%8F%AF%E5%BB%B6%E4%BC%B8%E6%87%89%E7%94%A8)) 
7. 引用 paper 與 延伸方向 (本篇位於 Graph 的哪個位置)
8. English



# 1. Summary
----------
## 1. 對方想解什麼問題

**概括**

- 為融合群眾經驗的最佳路徑選擇
- 提出可量化的路徑評估方式。
- 回答： **如何降低選擇難度**

**場域限制：**

- 在最短路徑行不通的情況下


**情境:**
“今天” 該搭什麼“交通工具”才不會“遲到”？(塞車)

- 計程車,公車,地鐵

**目標：**
The essential objective is to make it easier for users to select the best path among a number of candidates.


**Problem Statement**
*Assume we are given a path set* PS *and a budget* B *of the number of* RQ*s. Without exceeding the budget, we aim to design strategies to crowdsource* RQ*s in order to maximally reduce the selection hardness* H(BP)


## 2. 問題的限制與挑戰


- 空間最短路徑行不通
  - 會被塞車, 速限, 車資影響
    - 這些因子又會因時間而有所改變
      - → 因此目前大多改提供多條路徑
      - → 然而太多的路徑卻又讓 user 難以抉擇



- crowd 無法評估完整路徑好壞 (做選擇的速度跟品質都未達標)
  - 問 RQ:
    - 從A到B，我該走哪個節點


- 從問問題到群眾回答，中間是否會間隔太久？
  - 一個路口問一次會有 15s, 時間足夠。 且多半不需要一個路口就問一次。


- Worker 只對幾個路口較熟，不了解其他路口是否會塞車？
  - 問 BRQ: 
    - 為了去B地點，走這個路線對不對



- 群眾有可能回答錯誤的答案


- 如何評估 問題集 的好壞?



## 3. 解決的概略流程

B**asic**

- 在路口 Ask
  - RQ：群眾選擇下一個節點
  - BRQ：選擇這個節點是否適當
- 以演算法決定在一定預算內該如何最有效的分配 ”問題” (包含問什麼)
  - 包含證明


**如何評估選擇難度**

- use Shannon entropy to measure the selection hardness
  - entropy 高低 → user 選擇路線困難與否
  - 選項越多，entropy 也越高(若每個選項機率相同)


![](https://d2mxuefqeaa7sj.cloudfront.net/s_B064E323C20A698417480D287F6DB659E15F21818BFBEF5A3A45BAEF0AE78922_1532962038195_image.png)


**如何評估 ”該路線為最佳路線” 的機率**

- 根據司機歷史行駛資料


⚠️ **方法流程**




- RQ Based
  - 能降低 ”選擇困難” 的 RQ 集選擇
    - 計算 “選擇困難度” 
      - 計算 Vi 是這群 RQ 中的正解機率 (Best answer)
      - 計算 Pj 是這群 RQ 中的正解(best path) 且  Vi 也是這群 RQ 中的 正解(Best answer)
  


  - 如何有效地選擇 Best RQ
    - 計算選擇困難度改變量
      - 選擇困難度 - A路線為最佳解之改變量
    
![](https://d2mxuefqeaa7sj.cloudfront.net/s_B064E323C20A698417480D287F6DB659E15F21818BFBEF5A3A45BAEF0AE78922_1533018512376_image.png)

  - 如何整合有衝突的 群眾回答，並調整分佈
    - 添加錯誤率
![](https://d2mxuefqeaa7sj.cloudfront.net/s_B064E323C20A698417480D287F6DB659E15F21818BFBEF5A3A45BAEF0AE78922_1533018743074_image.png)

  
![](https://d2mxuefqeaa7sj.cloudfront.net/s_B064E323C20A698417480D287F6DB659E15F21818BFBEF5A3A45BAEF0AE78922_1533018806447_image.png)

    - 證明回答的先後沒有影響
    - 可解對於相同回答的不同問題
    - 整合後所有選項的最佳解機率合併會是 1



  - 制定框架
    - 預算限制
    - 首先，根據目前每條路的機率挑選 RQ 集
    - 接著，根據收到的回答調整新的 RQ 分佈
      - loop…


- Multiple Questions: RQ Extension
  - 一次問多個問題
  - 目的： 為了減少延遲與 realtime performance


  - 挑戰：
    - 一次要挑選哪些問題？ (避免問題間的 RQ 太相近)
      - formulate the problem of selecting the best combination of k questions,  
      - then propose an effective sampling-based solution.



- BRQ Based
  - 目的： Worker 更容易回答 (是或否)



## 4. 如何評估

**評估方式**


1. 以合成資料與模擬crowd評估各參數下的效率
2. 使用 Amazon Mechanical Turk, which is a public crowdsourcing platform.


**資料集**

- **合成資料模擬**
  - 設定
    - 探討 k & e
    - 預算 B = 60, 每一 iteration 丟 k 個問題


  - 關於 k
    - k 越小，選擇難度通常越低，但總選擇時間變長
  
  - 關於 e
    - e 越小，收斂越快
    - e 很高時，k方法的效果相對 baseline 好很多
  


- **真實資料模擬**
  - 兩個資料集(高速公路與主要道路，細部街道)，分別有30, 40個PS
    - B = 60
    - 0.03 USD / RQ


- 使用AMT


**評估項目**

- H(BP)
- Effectiveness
  - 若三個地圖都顯示為最佳則視該路為 ground truth.
  - 精準度效果均超過baseline



- Time
  - 時間花越久精度越高
  - 10s 內兩個資料集精度都超過 70%
  - 在意精準度 k 設小1點，在意時間 k 設大一點


**Baselines**

- top k highest entropies
- random




## 5. 綜合特點 (為什麼會上)

**敘述 (自述)**

- 聲稱是第一篇以 crowdsourcing 的方式解最佳路線問題的 paper
- 改進[25]方法，調整建議路線群之分佈而非選出最好的路線。
  - 因為…

**貢獻 (自述)**

- 如何根據有限的預算提出最有效益的問題
- 如何從 noisy 回答中取得最佳路線，並分析群眾準確度與給定的 RQ 的關聯
- 如何一次選擇多條路線來改善時間效率， 並提出更有效的問問題方式 (BRQ)
- 實驗比較



## 6. 整體價值 (可延伸應用)

**方法**

- 降低群眾選擇難度
  - 評估”選擇難度”的方式
  - “問題選項” 的 選擇方式


- 詢問方法
  - 只問下一個路口的單一節點
    - 怎麼決定問哪些路線
  - 一次問多個問題
    - 怎麼決定問哪些問題，問題與問題間的選項又不會太相關
  - 只問這條路對不對


- 彙整衝突回答的方式

**應用**

- 包含群眾經驗的路線規劃

**與自身題目之差異**

- 本篇 為 詢問 路口 之 路徑選擇
- 自身 為 詢問 路段 之 可能車速


## 7. 引用 與 延伸方向 (本篇位於 Graph 的哪個位置)


- **本篇**


- **其他篇**
  - Crowdsourcing 路徑規劃： [CrowdPlanner](https://ieeexplore.ieee.org/document/6816730/?part=1)


- **延伸方向**


## 8. English
- To the best of our knowledge, this is the first work studying the path selection problem with crowdsourcing.
- In this paper… In particular… Furthermore… Finally…
- it is non-trivial to select the best combination of k questions







# 2. Content
----------


## Abstract

With the widespread use of geo-positioning services (GPS), 

- GPS-based navigation systems have become ever more of an integral part of our daily lives. 
- GPS-based navigation systems usually suggest multiple paths for any given pair of source and target, leaving users perplexed when trying to select the best one among them, namely the problem of *best path selection*. 
- Too many suggested paths may jeopardize the usability of the recommendation data, and decrease user satisfaction. 
- Although existing studies have already partially relieved this problem through integrating historical traffic logs or updating traffic conditions periodically, 
  - their solutions neglect the potential contribution of human experience.


In this paper, 

- we resort to crowdsourcing to ease the pain of the best path selection. 
- The first step of appropriately using the crowd is to ask proper questions. 


  - For the best path selection problem, simple questions (e.g. binary voting) over compete paths cannot be directly applied to road networks due to their being too complex for crowd workers. 
  - Thus, this paper makes the first contribution 
    - by **designing two types of questions**, 
      - namely Routing Query (RQ)
      - and Binary Routing Query (BRQ), 
        - to ask the crowd to decide which direction to take at each road intersection. 
  - Furthermore, we propose **a series of efficient algorithms**
    - to dynamically manage the questions
      - in order to reduce the selection hardness within a limited budget. 
  - Finally, we compare **the proposed methods against two baselines**,
    - and the effectiveness and efficiency of our proposals
      - are verified by the results
        - from simulations and experiments on a real-world crowdsourcing platform.


## 1.2

…
One may wonder

- how to obtain the exact probability distribution for each path. 
- In fact, a critical issue in any system that manages uncertainty is whether we have a reliable source of probabilities. 
- Whereas obtaining reliable probabilities for our system
  -  is one of the most interesting areas for future research, 
  - there is quite a bit to build on. 
- For a routing system integrated with different recommendation algorithms, 
- it is possible to train and test the algorithms on a large number of queries such that each algorithm is given a probability based on its performance statistics. 
- In the case where the recommendation relies mainly on large amounts of historical trajectory data [6, 18], the distribution can easily be inferred by mining the frequent paths chosen by experienced drivers.


- For personalized applications, it is also reasonable to allow users to configure the distribution, which reflects his or her personal preferences [17]. 
- Clearly, it would be a difficult task for an end-user to pick the best path from the suggested ones. 
- This problem may be especially severe when the number of candidate paths is large. Inspired by the emerging concept of crowdsourcing for various intrinsic human tasks, we resort to the crowd to facilitate the selection.


## 1.3 Role of the Crowd

The first task to utilize the crowd is to design a proper worker interface
- we need to determine which type of questions should be posted to the crowd. This is typically referred to as HIT (Human Intelligent Task) design. 

It is well-known that crowdsourcing works best when tasks can be broken down into very simple pieces [24]. 

**A** ***complete path*****, which usually contains hundreds of vertices and edges, may be too complex for a crowd. 完整的路段要群眾選太困難**

In fact, we conducted an experiment by asking the crowd to evaluate complete paths, but neither the **latency nor the accuracy is satisfactory**. 
速度慢又不準

On the other hand, asking open-ended questions is not recommended for a crowd, because it may be difficult to integrate multiple suggestions of heterogeneous semantics. 
開放問題又沒用

As a result, we propose to ask the crowd questions regarding directions of an intersection, 

- namely *Routing Query* (RQ). Each RQ consists of
  -  a given vertex vin,
  -  a set of vertices D = {v1, v2, ..., v|D|},
  -  and a target t, 
    - such that ∀vi ∈ D, vi is any consecutive vertex of vin in one of the suggested paths. 
- Intuitively, D indicates the possible directions moving from vin to- wards target t, and the crowd is to choose the best one among them.

As shown in the upper part of Table 1, 
there are four recommended paths from the source s to the target t - P1, P2, P3 and P4, 

- each of which is associated with its probability of being the best path. 


- Figure 1 demonstrates the graph containing them. We assume s is HKUST and t is HKU, v1, v2 and v3 are Hang Hau, Choi Hung and Kowloon Bay respectively, 
  - then a routing query could be “Which direction should I go from HKUST(s) to HKU(t), Hang Hau (v1), Choi Hung (v2) or Kowloon Bay (v3)?”.

Suppose the answer from the crowd is v3 
since both v1 and v2 are usually congested. 

As indicated in Table 1, P1 and P2 go through v1 and v2 respectively, 
hereby can be ruled out. 

Accordingly, both P3 and P4 have a 50% probability of being the best path, so that selecting the path becomes less difficult for the user. 
In this paper, we propose algorithms that

-  **automatically select RQs and interact with the crowd.**  
  - 能自動選路

So the crowd would be performing tasks in the back-end, and users do not need to be involved with issuing tasks.

**One concern with the crowd is the real-timeliness**

  - **are users willing to wait long enough for the crowd to respond?** 
    - **We believe the answer is quite positive.** 

客戶等待時間嗎？ 沒錯


- A car moving at 30 mph = 48 kmph takes 15 seconds to cover one 200m long city block. 
- Even if a path decision had to be made at every intersection, we would have about 15 seconds to run the algorithm. 
- In practice, one rarely needs to consider a turn at every intersection. Therefore the time interval spent waiting for the response from the crowd is much longer. 
- In this paper, we address the problem of real-timeliness in Section 4.1, 
- by exploring the parallel processing power of the crowd. 


- In particular, we ask the most informative k questions, 
- which different workers can pick up concurrently, cutting down the overall human processing time. 
- As shown in our experimental results (Section 5.4), 
- empowered by our technique (k = 10), 
- the crowd can improve the precision of path selection
  - from 25% to 70% within 10 seconds,
  - which is less than half of the time cost without this technique 
    - (i.e. k = 1). 
- Besides, there are also horizontal techniques of realtime crowdsourcing [3, 4], which have shown promise in returning results to users in 500 milliseconds.

Another concern is that

-  **crowd workers may need to know the traffic conditions in all directions to answer an RQ.** 

So when the degree of vertex is large, 
a worker may not be able to provide an accurate answer. 


For instance, for a given road intersection, a worker may be familiar with one or two of the directions, but not all of them. 
Therefore, we consider another type of questions, called *Binary Routing Query* (BRQ), as shown at the bottom of Table 1. 


Each BRQ contains only one direction, and asks the crowd to confirm or disconfirm, such as “From HKUST(v) to HKU(t), should I go via Hang Hau (v1) or not?”. 
This kind of much simpler and intuitive questions, in most circumstances, can be easily answered correctly.
**我該不該走這一條路**

Since each crowdsourcing question is usually associated with a cost, 
we need to design solutions to find the best path by crowdsourcing an optimal set of RQs. 

However, the selection is not trivial due to the following two obstacles: 

- first, the crowd has the probability of returning incorrect answers; 
- second, the RQs generated from a set of candidate paths are naturally correlated, 
  - so the utility of a set of RQs may be difficult to compute. 


- To address this challenge, 
  - we design efficient and effective strategies to adaptively select and issue RQs by utilizing the submodular property of the selection hardness.


## 1.4 Contributions

We summarize our contributions as follows:

First, we address the core optimization problem:

- how to select the most profitable questions with a given fixed budget. 
- We
  - derive a non-trivial property of the utility for each RQ, 
  - and propose an effective strategy to interactively select and publish RQs. 
  - Both the crowd’s error and the correlation of RQs are gracefully handled in the proposed solution.

Second, we indicate how to utilize noisy crowdsourced answers to adjust the probability distribution of the best path, and analyze the functional relations between the crowd’s accuracy and a given RQ.

Third, we consider two different extensions based on the pro-posed framework - 1. we study how to select multiple RQs and pose them concurrently to the crowd, in order to improve the time efficiency; 2. we investigate an easier type of questions, namely BRQ, as a replacement of RQ for high-degree vertices.

Fourth, we conduct extensive experiments on both synthetic and real datasets. The experimental results show the superiority of our proposed methods in comparison with the baselines.


## 2. DEFINITIONS AND PROBLEM STATEMENT

In this section, we introduce the core definitions and related no- tations, then formally state the problem.

DEFINITION 2.1 (CANDIDATE PATH). *Given a source vertex* s *and a target vertex* t *over a directed graph, a candidate path is a sequence of edges* P = {e0, e1, ..., e|P |}*, such that* s *is the head of* e0*,* t *is the tail of* e|P|*, and* ∀ei,ei+1 ∈ P *the tail of* ei *is the head of* ei+1*.*

Notation: For a given vertex v and a candidate path P , we use P→vandP􏰀vtodenote‘P goesthroughv’and‘P doesnot go through v’, respectively.

DEFINITION 2.2 (PATH SET (PS) & BEST PATH (BP)).
*Given a source vertex* s *and a target vertex* t*, let* P S *be a set of suggested candidate paths from* s *to* t*. The best path, denoted by* BP *, is defined as a discrete random variable with* P S *as the sample space. Each candidate path* P ∈ P S *has a probability* P r(P ) *being the best path, and we have* 􏰃P∈PS Pr(P) = 1*.*

*Example:* P1,P2,P3 *and* P4 *are candidate paths from* s *to* t *in Table 1. We have* P1 → v1 *and* P1 􏰀 v2*, indicating that ‘*P1 *goes through* v1*’ and ‘*P1 *does not go through* v2*’, respec- tively. Moreover,* PS = {P1,P2,P3,P4} *is the path set from* s *to* t*. We also have that the probability of* P1 *being the best path is* Pr(P1) = 0.1*.*


## 



## 3. RQ-BASED METHOD

In this section, we present a complete solution to

- select and crowdsource RQs
  -  in order to reduce the selection hardness. 
- First, we use the expected reduction of selection hardness as the metric
  -  to evaluate RQs,
  -  and derive necessary formulas to enable the computation. 
- Second, we study how to efficiently select the best RQ. 
- Third, we present how to utilize conflicting crowdsourced answers. 
- Lastly, we put these together to develop a framework of the RQ-based method,
  - which reduces the selection hardness using a sequence of RQs.



- 如何評估 RQs
- 如何更有效率的選擇
- 如何整合衝突的問題
- 整合流程，降低 sequence 選擇難度


## 

