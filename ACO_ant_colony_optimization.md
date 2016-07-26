## 蚁群算法特点

系统性：处于一定的相互关系中并与环境发生关系的各组成部分的综合体

分布式计算：每只人工蚂蚁在问题空间的多个点同事开始相互独立地构造问题解，而整个问题的求解不会因为某只人工蚂蚁无法成功获得解而受到影响。

自组织：从抽象意义讲，自组织就是在没有外界作用下是的系统熵增加的过程（即系统从无序到有序的进化过程）。自组织性大大增强了算法的鲁棒性。

反馈作用：
正反馈：信息素；负反馈：随机选路

TSP问题的蚁群算法：

选路公式:

\[
    p^k_{ij}(t)=\left\{
         \begin{aligned} 
            &\frac{[\tau_{ij}(t)]^\alpha \cdot [\eta_{ij}(t)]^\beta}{\sum_{s\in A_k}[\tau_{is}(t)]^\alpha \cdot [\eta_{is}(t)]^\beta} ,\quad j \in A_k\\
            & 0 ,\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad otherwise\\ 
         \end{aligned} 
         \right.
\]

路径启发函数：

\[
    \eta_{ij}=\frac{1}{d_{ij}}
\]

信息素更新规则：

\[
    \tau_{ij}(t+1)=(1- \rho) \cdot \tau_{ij}(t)+\Delta\tau_{ij}(t)\\
    \Delta\tau_{ij}(t)= \sum_{k=1}^m\Delta\tau_{ij}^k(t)
\]

信息素增量策略：

+ Ant-Cycle 模型
\[
    \Delta\tau_{ij}^k(t)=\frac{Q}{L_k}
\]

+ Ant-Quantity 模型
\[
    \Delta\tau_{ij}^k(t)=\frac{Q}{d_{ij}}
\]

+ Ant-Density 模型
\[
    \Delta\tau_{ij}^k(t)=Q
\]