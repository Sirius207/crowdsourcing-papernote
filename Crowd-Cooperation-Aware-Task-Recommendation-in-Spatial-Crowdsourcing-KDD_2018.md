# [Crowd] Cooperation-Aware Task Recommendation in Spatial Crowdsourcing - KDD_2018



# Outline
----------


1. 對方想解什麼問題
2. 問題的限制與挑戰
3. 解決的概略流程 (你會怎麼解)
4. 如何評估
5. 綜合特點 (為什麼會上)
6. ⭐️ 整體價值 
7. 引用 paper 與 延伸方向 (本篇位於 Graph 的哪個位置)
8. English




# 1. 總結
----------



## 1. 對方想解什麼問題

**概括**


- CA-SC 問題
  - CA-SC: 需要多位 Worker 才能拼湊出單一問題之解答
  - 目的為確保 CA-SC 問題能得到最高回答品質 (分數)
> recommends cooperation-aware moving workers to spatial tasks to maximize the overall cooperation quality. 


**情境:**

- 本篇
  - 如何靠多人合作來搜集完一棟大樓各區的 Wifi 訊號強度。
- 其他
  - 多人移動重物
  - 測量公園噪音訊號

**目標：**

- 最大化 Work 總分

**定義**

- Task
  - 需要在 “一定時間” 內 讓 N 個 Worker 移動到 “該地點”



## 2. 問題的限制與挑戰

**挑戰**

- Task 需要不只一個人完成
- worker 與 worker 間有能合作程度

**限制**

- 要求 worker 移動到指定地點
  - 每位 Worker 有 位置，deadline，容量 等限制
- (1) the capacity constraints are satisfied; and
- (2) the working area constraints of workers are satisfied; and
- (3) the deadline constraints of tasks are satisfied; and
- (4) the overall cooperation quality of all the tasks in T is maximize


**已知**

- Worker
  - 與 Worker 的合作狀況 (e.g. W1 → W2: 0.9)
  - 個別移動速度
  - 移動範圍
- Task
  - 容量


## 3. 解決的概略流程

**基本**

- 如何拆解任務，判斷工作量(要多少人)？
- 如何決定要派哪些人？
- 如何整合多位 worker 的工作？



⚠️ **方法流程**


- 找到奈許均衡 (每位 Work 都無法再提高品質)
  - 穩定度：
  - 收斂速度：
  - 品質：


- 但在 large- scale 的 CA-SC 問題上會很慢，所以要用最佳化算法改善效率
  - 如何提升效率？
    - LUB: 確認會改變時再計算
    - TSI: 終止條件(Threshold)



  - 加速賽局計算的方法
    - LUB
      - 不是每位 Worker 的 best response 策略都要重新計算
        - 只在對方的策略可能改變時才重新計算。 (減少不必要的運算)
    - TSI
      - 由於每次計算分數上升率會越來越低，當整體分數上升度小於 Qc 就先行停止計算。


## 4. 如何評估

**資料集**

- Real
  - Gowalla data set[6]
  - Foursquare data set[10]
- Synthetic
  - 產生 Worker 的位置
    - UNIF 與 SKEW distribution

**評估項目**


- C: 總 Cooperation Score
- T: Time    


- Effect (以真實資料評比)
  - Task 工作量 增減 對 C, T 的影響
  - Worker 
    - 移動速度 增減 對 C, T 的影響
    - 移動範圍 增減 對 C, T 的影響


- Effect (以模擬資料評比)
  - GT + TSI 中, Threshold 增減對 C, T 的影響
  - Worker 數目 增減 對 C, T 的影響
  - Task 數目 增減 對 C, T 的影響


**Baselines**

- Greedy
- Random


- --


- GT + LUB
- GT + TSI
- GT + ALL


## 5. 綜合特點 (為什麼會上)

**貢獻 (自述)**

- Prove CA-SC 問題是 NP-Hard
- 使用 GT 搭配兩項最佳化解法來解 CA-SC 問題
- 實驗實際呈現方法效率與效用

**特色**

- 解 Worker 間需要合作 的 Task
  - 此外還有 Task 工作量與完成期限，Worker 移動速度與工作範圍等限制。


## 6. 整體價值 (可延伸應用)


- 方法
  - NP-Hard 的 基本 prove (reducing from k-set packing problem(k-SP))
  - 賽局理論在 CA-SC 問題的應用 
  - 如何拆解任務，判斷工作量(要多少人)？
  - 如何決定要派哪些人？
    - 根據 位置指定 哪些 worker
  - 如何整合多位 worker 的工作？
  - 產生合成資料之方式
    - UNIF 與 SKEW distribution



- 應用
  - 也能改善 Uber 這類共享派遣之效率？
  


- 差異
  - 對車速問題比較不需要 考慮 “worker 合作情況”



## 7. 引用 與 延伸方向 (本篇位於 Graph 的哪個位置)

**本篇**

- 位置
  - 解 Spatial CrowdSourcing 中的 CASC 問題
    - 而目前 存在研究[4,5,9,19,25] 皆不討論 worker 須合作之情況
  - 特性
    - SAT, “batch-based Task” Recommendation
  - 考量
    - Worker
      - 活動位置
    - Task
      - 容量(工作量)
      - 完成期限
  - 最大化 cooperation score

**其他篇論文**


- WST: 由 Worker 選擇 Task ----- [7]:


- SAT: 由 Server 指派 Task ------[4,5,9,19,25]:
  - “Online Task” Recommendation - [19, 22]:
    - Info:
      - one by one assign Task
  - “batch-based Task” Recommendation - [4,5,9,25]:
    - Info:
      - 一次推大量 Task 給大量 Worker
    - Goal:
      - [9]: 最大化 推薦的 Task 數
      - [5]: 最大化 推薦的 Reliable 和 Diversity Score
      - [25]: 最大化 Task 被 Worker 接收的次數
  

**延伸方向**



## 8. English


- Through extensive experiments, we demonstrate the efficiency and effectiveness of our proposed approaches over both real and synthetic datasets.
- Specifically, we make the following contributions.
- 2.preliminaries



# 2. Content
----------


## Abstract

we consider an important spatial crowdsourcing problem, 

- namely cooperation-aware spatial crowdsourcing (CA-SC), 
  - where spatial tasks 
      - (e.g., collecting the Wi-Fi signal strength in one building) 
    - are time-constrained
    - require more than one worker to complete
      - thus the cooperation among recommended workers
        - is essential to the result. 
      
- Our CA-SC problem is to recommend workers to spatial tasks
  -  such that the overall cooperation quality is maximized. 


- We prove that the CA-SC problem is NP-hard 
  - by reducing from the k-set packing problem, thus intractable. 
  
- To tackle the CA-SC problem, 
  - we propose one *game theoretic* (GT) approach 
      - with two optimization methods 
    - to quickly solve the CA-SC problem
    - achieve high total cooperation quality scores.


- Through extensive experiments, 
  - we demonstrate the efficiency and effectiveness of our proposed approaches
    - over both real and synthetic datasets.


## 1. Introduction

With the popularity of the smart devices and the development of high-speed wireless networks, 

- people nowadays are convenient to participant in spatial tasks that are close to their current locations through online services and then contribute to the tasks in physical world, 
  - such as taking photos/videos, 
  - cleaning rooms, 
  - and moving heavy stuff.



- To support these activities, a new framework, namely *spatial crowdsourcing* [9], 
  - is proposed and attracts much attention from both academia and industry (e.g., TaskRabbit [1]). 
- Specifically, a spatial crowdsourcing system
  - recommends moving workers to spatial tasks
    - under the constraints of the locations, deadlines and capacities
      - [4, 5, 9, 17, 20, 21].

In spatial crowdsourcing, 

- some tasks need more than one worker to complete, 
  - such as moving heavy stuff, 
  - collecting the Wi-Fi signal strength in one building, 
  - and measuring noise levels in a park [12]. 
- When workers are recommended to such kind of tasks, 
  - they need to cooperate with each other, 
    - then the relationship between the workers may play a crucial factor to affect the quality of the work dramatically. 
- Existing studies [4, 9, 11, 17], 
  - however, do not concern the cooperation relationship of workers.


With the consideration of the cooperation among workers 

  - that are recommended to same tasks, in this paper, 
- we study an important problem in spatial crowdsourcing, 
  - namely *cooperation-aware spatial crowdsourcing* (CA-SC), 
    - which recommends *cooperation-aware moving workers* to *spatial tasks* to maximize the overall cooperation quality. 
- We will first illustrate the CA-SC problem
  -  with a motivation example of
    -  collecting the Wi-Fi signal strength in one building.

*Example 1.1.* In the example of cooperation-aware spatial crowd- sourcing problem shown in Figure 1, 

- there are two spatial tasks, t1 and t2, 
- and four cooperation-aware workers, w1 ∼ w4. 


- The locations of the tasks and workers are shown in Figure 1(a), 
  - where workers are represented by yellow triangles and tasks are indicated with blue circles, 
  - and the dash circles show the working areas of workers. 
- Specifically,
  - worker w2 can accept tasks t1 and t2
  - worker w1 only prefers task t1.


-  Assume each task needs two workers
  -  to complete the job of
    -  collecting the Wi-Fi signal strength in one building. 
- In addition, based on the historical cooperation records, 
  - we estimate the cooperation qualities of worker pairs 
      - shown in Figure 1(b) with Equation 1, 
    - where each straight line indicates a cooperation relationship 
      - between the connected two workers
        - and the number close to it denotes the cooperation quality of the two workers.

Given the information above, the crowdsourcing system needs to recommend 

- each task with two workers under the working areas
  -  constraints of workers with a goal of
    -  completing the task with high quality. 


- As in spatial crowdsourcing, 
  - workers need to physically move to the required location of the recommended task, 
  - where they will cooperate with the other workers recommended to the same task. 
  - The cooperation qualities among the workers belonging to the same task will affect the result quality dramatically. 
    - → The system needs to determine a group of workers for each task such that they can cooperate well. 
  - In this example, we can recommend workers w1 and w2 to task t1, 
  - and workers w3 and w4 to task t2 resulting in the total cooperation score of 0.2 (calculated by Equation 3). 
  - However, we can achieve a better recommendation by 
    - dispatching workers w and w4 to task t1, 
      - and workers w2 and w3 to task t2, 
        - whose total cooperation quality score is 1.8 
          - (obviously higher than 0.2).

In general, tasks and workers are changing all the time, thus we consider the CA-SC as a real-time process and handle it with batch-based task recommendation process, which means the platform periodically recommends the available workers to unfinished tasks. 
Specifically, in CA-SC, the workers may keep moving and the new tasks may appear continually. 
During the last batch processing of a set of tasks, 

- new tasks and workers may also come. 


- As a result, we need to determine the recommended sets of workers for the tasks within a very short period of time (e.g., 2 seconds), 
  - which is unnoticed by human.

In this paper, we first prove that CA-SC is NP-hard by reducing from the k-set packing problem (k-SP) [23]. 
Thus, the CA-SC problem is not tractable and very hard to achieve the optimal result for problems with real world scales 

  (e.g., hundreds of tasks and thousands of workers). 
  

Existing studies [4, 5, 9, 19, 25] of task recommendation in spatial crowdsourcing 

- do not take the cooperation scores of workers into consideration, 
  - thus cannot be used directly on our CA-SC problem. 


- In this paper, we propose one novel game theoretic approach with two optimization methods to handle it.


----------

Specifically, we make the following contributions.
• We formally define the *cooperation-aware spatial crowd-sourcing* (CA-SC) problem proved NP-hard in Section 2.

• We propose one game theoretic approach with two optimiza-
tion methods in Section 3.

• We conduct extensive experiments on real and synthetic data
sets, 

  - and show the efficiency and effectiveness of our game

theoretic approach in Section 4.

Section 5 reviews previous studies. 
Finally, Section 6 concludes this paper.


## 
## 3. THE GAME THEORETIC APPROACH

To quickly achieve an recommendation with a high total cooperation quality score, in this section, we propose a game theoretic solution that can iteratively adjust a valid recommendation of the workers to tasks until the *Nash equilibrium* is met [13]. Intuitively, in a Nash equilibrium recommendation, any single worker cannot improve his/her cooperation quality score by unilaterally switching from the recommended task to other tasks when other workers stay in their recommended tasks. In addition, usually a Nash equilibrium recommendation can result in a high total cooperation quality score since each worker is recommended to his/her “best” task in the stable recommendation, which is confirmed in our experimental study in Section 4. Next, we first introduce the basics of game theory, then introduce the game theoretic approach with two optimization methods.

**3.1**
Game theory is the study of mathematical models of conflict and cooperation between intelligent rational decision-makers, which is widely used in economics, political science, and computer science [8]. In *strategic games*, there are a set of *players* N competing or cooperating for some resources in order to optimize their individual objective functions (*utilities*). For each player i ∈ N , he/she can choose one *strategy* si (i.e., conducting task tx ) out from the set of his/her possible strategies Si , and has a *utility function* Ui , whose value depends on the strategy of player i as well as the strategies of other players. The input of the utility function Ui is a given *joint strategy* S ∈ S, where S is the Cartesian product of the actions of allplayers(i.e.,S=S1 ×S2 ×···×S|N|).Letsi bethestrategyof player i in the joint strategy S and s−i be the joint strategies of all other players except for player i. A strategic game has a pure Nash equilibrium [15] S∗ ∈ S, if and only if for every player i ∈ N we have the following conditions:

In other words, in a Nash equilibrium no player can improve his/her utility by unilaterally changing his/her strategy when other players persist in their current strategies.

One common used framework to search a Nash equilibrium S∗ ∈ S for a given strategic game G = ⟨N,S,{Ui}i∈N 􏰅→ R⟩ is the *best- response framework* [16], which first randomly selects a strategy for each player, then iteratively selects the “best” strategy for each player i based on the current strategies of other players until a Nash equilibrium is found (i.e., no one will change the selected strategy).

There are several issues related to the best-response framework: a) Stability: whether the best-response frame can find a Nash equi- librium; b) Convergence: how fast it can converge; c) Quality: how good is the found solution. We will first propose one game theoretic approach to solve our CA-SC problem, then answer the three issues related to the approach one-by-one.

**3.2**
In this section, we first model a CA-SC problem instance as a strate- gic game G then propose a game theoretic approach (GT) based on the best-response framework to find a Nash equilibrium joint strat- egy for the strategic game G, where every worker is recommended to his/her “best” task such that a high total cooperation quality score is achieved. Specifically, we model each worker wi as a player i, whose target is to select a “best” task with the highest cooperation score (utility) for him/her. For each player wi , his/her strategy set Si indicates all the possible strategies that he/she can choose (e.g., all the valid tasks he/she can conduct). Then, a joint strategy S for the strategic game G corresponds to an recommendation R for the CA-SC problem instance.

We first define the utility function Ui of worker wi in the joint strategy S as the cooperation quality score increase:

where si is the strategy of worker wi and s−i indicates the joint strategy of other workers except for worker wi . In addition, Wj is the recommendedworkersoftasktj (includingworkerwi),andQ(Wj) indicates the cooperation quality calculated with Equation 2. Then, we propose a basic game theoretic approach, shown in Algorithm 1, to achieve a Nash equilibrium for a set of workers W(φ) and a set of tasks T(φ) at timestamp φ. Specifically, we first randomly recommend a set of aj workers to each task tj (lines 1 - 2). Next, we iteratively adjust each worker’s strategy to his/her best-response strategy that maximizes his/her utility function Ui (as defined in Equation 5) according to the other workers’ current joint strategy until a Nash equilibrium is found (i.e., no worker will change his/her strategy in the best-response framework) (lines 3 - 6). Here, each iteration of the WHILE loop (lines 3-6) is call a *round*.

**3.3**
In this section, we analyze the three important issues related to the game theoretic approach (Algorithm 1): a) whether it can find a Nash equilibrium (Stability); b) how fast it can converge (Convergence); c) how good is the found result (Quality).
The Stability of the Approach. We prove that Algorithm 1 can finally result in a Nash equilibrium. To prove the stability of Algo- rithm 1, we first introduce the theory of Potential Games [14]. A strategicgameG=⟨N,S,{Ui}i∈N 􏰅→R⟩iscalledan*exactpoten- tial game* if and only if there exists a *potential function* F (S) : S 􏰅→ R such that:

where si and si′ are the strategies that worker wi can select, and s−i is the joint strategy of the other workers except for worker wi . Intuitively, in an exact potential game, the change in a single player’s utility due to his/her own strategy deviation results in exactly the same amount of change in the potential function. The most important property of potential games is that the best-response framework always converges to a pure Nash equilibrium for potential games with the finite strategies [14].

Next, we prove the stability of the basic game theoretic approach by proving the strategic game of our CA-SC problem is an exact potential game in the following theorem.

PROOF. For our CA-SC problem, we define the utility function Ui of worker wi as Equation 5 and the potential function F as the objective function in Equation 3. Let si∗ and si be the best response strategy and any other response strategy of worker wi for a given joint strategy s−i of the other workers except for worker wi . We note the task selected in strategies si∗ and si as tasks tj and tk , respectively, then we have:

Thus, according to the definition of potential games [14], the strategic game of the CA-SC problem is an exact potential game.

Based on the theory of potential games, with Theorem 3.1, we achieve the conclusion that the best-response based game theoretic approach (shown in 1) can finally result in a pure Nash equilibrium since the strategic game of the CA-SC problem is a potential game and has finite strategies.

The Convergence of the Approach. To answer the convergence speed of the GT approach (Algorithm 1), we need to know how many rounds it needs to find a stable result (a pure Nash equilibrium) and the time complexity of each round.

To estimate the upper bound of the total rounds that the GT approach needs to achieve a pure Nash equilibrium, we consider a scaled version of the problem where the objective function takes integer values. Specifically, for the corresponding potential game ofaCA-SCprobleminstance,⟨N,S,{Ui}i∈N 􏰅→R⟩,weassume that there is an equivalent game with potential function FZ(S) = d · F (S), where d is a positive multiplicative factor chosen such that FZ (S) ∈ Z, ∀S ∈ S. With this scaled potential function, we show that the GT approach executes at most FZ(S∗) rounds, where S∗ is the best strategy the workers can select in this potential CA-SC game (i.e., FZ(S∗) is the product of optimal value of the objective function Q(T(φ))andthepositivemultiplicativefactord).

However, we do not know the optimal joint strategy of the work- ers unless we enumerate all the possible strategies, which is im- practical due to that the CA-SC problem is NP-hard (as proven in Theorem 2.5). Thus, we need to estimate the upper bound of FZ(S∗)
(=d·􏰃tj ∈T(φ) Q(Wj∗),whereWj∗ isthesetofworkersrecommended
to task tj in the optimal joint strategy). We notice that if we can cal-
culate the optimal value of the cooperation quality score of each task
tj , noted as Qmax (tj ), we can estimate the upper bound of the scaled

find aj workers having the highest cooperation quality score for task tj is also a NP-hard problem, which is called the Maximum Weight Connected k-Induced Subgraph Problem and is proved NP-hard by reducing from the CLIQUE problem [2]. Nevertheless, we can estimate the upper bound of the cooperation quality score task tj in the optimal joint strategy with the lemma below.



## 5.







## 6. Conclusion

In this paper, 
we formalize the problem of 

- cooperation-aware spatial crowdsourcing (CA-SC) problem, 
  - which recommends 
    - a set of moving workers to a set of time
    -  and capacity constrained spatial tasks, 
      - such that the tasks can be accomplished with high cooperation qualities. 
- We prove that the CA-SC problem is NP-hard
  - by reducing it from a well-known NP-hard problem, 
    - k-set packing problem (k -SP)


- and then propose a game theoretic approach
  - with two optimization methods to solve it.


- Extensive experiments have been conducted to
  -  show the efficiency and effectiveness
    -  of our CA-SC approaches
      -  on both real and synthetic data sets.

