---
title: 一阶逻辑FOL
published: 2026-07-26
image: cover.jpg
category: 数学知识
---
# 核心部件
- **常量与变量：** 常量是具体对象（如 `张三` $\rightarrow a$），变量是占位符（如 $x$）。
- **谓词（Predicate）：** 表达属性或关系。比如 $Student(x)$ 表示“$x$是学生”，$Love(x, y)$ 表示“$x$爱$y$”。
- **全称量词（$\forall$）：** 任意、所有、每一个。
- **存在量词（$\exists$）：** 存在、至少有一个、有些。
- **项**：指代某个具体个体的表达式，他没有真假，比如张三的爸爸
- **公式**：指表达陈述、有真假潜力的逻辑命题，包括原子公式和通过原子公式析取与合取得到的合式公式（EFF）
- **作用域**：量词后面紧跟的那个完整的WFF就是该量词的作用域
- **约束变量（Bound Variable）：** 如果一个变量 $x$ 位于形如 $\forall x$ 或 $\exists x$ 的量词的作用域内，且该变量与量词的变量名相同，则称该变量在当前位置的出现是**被约束的**。
- **自由变量（Free Variable）：** 没被任何量词约束的变量称为**自由变量**。
- 任何合法的**函数表达式**，在 FOL 中都【只能】属于“项”，因为函数只能充当映射器的概念，在论域里抓住一个或者几个个体，然后通过某种规则，指向论域里的另一个个体
- 任何合法的**谓词表达式**（当它的参数位置被填满时），在 FOL 中都【只能】属于原子公式，充当判定器的作用，输出是真或者假。
注意：全称量词 $\forall$ 永远搭配 蕴含符 $\rightarrow$
例句： “所有写 Python 的人都能快速自学。”正确公式： $\forall x (WritePython(x) \rightarrow CanLearnFast(x))$大坑： 千万不能写成 $\forall x (WritePython(x) \land CanLearnFast(x))$。
如果用 $\land$（且），这句话的意思就变成了“宇宙中的万事万物都在写 Python，且他们都能快速自学”，这显然荒谬。
存在量词 $\exists$ 永远搭配 合取符 $\land$
例句： “有一些大模型存在幻觉。”正确公式： $\exists x (LLM(x) \land HasHallucination(x))$
大坑： 千万不能写成 $\exists x (LLM(x) \rightarrow HasHallucination(x))$。
因为根据逻辑学，只要前件为假，整个蕴含式就为真。这意味着，只要世界上有一个东西不是大模型（比如一根筷子），这个公式就自动成立了，这根本没有表达出“有大模型存在幻觉”的意思。
# FOL的语义学
语法：语法的本质只是“字符串的拼凑规则”，公式本身是没有生命的。比如公式 $\forall x P(x)$，在没有给定具体物理背景之前，它既不是真，也不是假。
语义的任务，就是给这些冰冷的符号**注入灵魂（赋予含义）**，并定义**在什么情况下一个公式是真（True）还是假（False）**。
一阶逻辑的语义学核心是**模型理论（Model Theory）**，通过以下三个递进的概念来严格定义它：
### 1. 结构与论域（Structure and Domain）
要让 FOL 公式说话，我们首先必须指定一个“世界”。在逻辑学中，这个“世界”被称为一个**结构（Structure）**，通常记为 $\mathcal{M}$。
一个结构 $\mathcal{M}$ 严格由两部分组成：
1. **论域（Domain, 记为 $D$ 或 $|\mathcal{M}|$）：** 一个**非空的**个体集合。它规定了我们的量词（$\forall, \exists$）所能指代的对象的范围。
2. **解释函数（Interpretation Function, 记为 $\cdot^\mathcal{M}$）：** 它负责把公式中的非逻辑符号（常量、函数、谓词）映射到论域 $D$ 的具体现实中。
#### 映射规则的严谨定义：
- 如果 $c$ 是一个**常量符号**，那么它的解释 $c^\mathcal{M}$ 必须是论域 $D$ 中的**一个具体元素**（即 $c^\mathcal{M} \in D$）。
    
- 如果 $f$ 是一个 **$n$ 元函数符号**，那么它的解释 $f^\mathcal{M}$ 必须是论域 $D$ 上的一个**真正的数学函数**（即 $f^\mathcal{M}: D^n \rightarrow D$）。
    
- 如果 $P$ 是一个 **$n$ 元谓词符号**，那么它的解释 $P^\mathcal{M}$ 必须是论域 $D$ 上的一个**关系集合**（即 $P^\mathcal{M} \subseteq D^n$）。
### 2. 变量赋值（Variable Assignment）
常量有解释函数来搞定，但变量（如 $x, y$）是变动的，怎么办？我们引入**变量赋值（Assignment）**，通常记为 $v$。

> **严谨定义：** 变量赋值 $v$ 是一个映射函数，它把语言中所有的**变量**映射到论域 $D$ 中的具体元素。

有了结构 $\mathcal{M}$ 和赋值 $v$，我们就能算出任何一个“项（Term）”在当前世界里指代的具体个体是谁，记为 $I_v^\mathcal{M}(t)$：
- 常量的个体值：$I_v^\mathcal{M}(c) = c^\mathcal{M}$
- 变量的个体值：$I_v^\mathcal{M}(x) = v(x)$
- 函数的个体值：$I_v^\mathcal{M}(f(t_1, \dots, t_n)) = f^\mathcal{M}(I_v^\mathcal{M}(t_1), \dots, I_v^\mathcal{M}(t_n))$
### 3. 真值定义：塔斯基真值定义（Tarski's Definition of Truth）
这是整个模型理论的王冠。我们用符号 $\mathcal{M}, v \models \phi$ 来表示：**在结构 $\mathcal{M}$ 和赋值 $v$ 下，公式 $\phi$ 是真的（或者说 $\mathcal{M}, v$ 满足 $\phi$）**。

根据公式的构造，真值是递归定义的（以下为完全确定的标准数理逻辑定义）：

1. **原子公式：**
    
    $\mathcal{M}, v \models P(t_1, \dots, t_n)$ 当且仅当 $(I_v^\mathcal{M}(t_1), \dots, I_v^\mathcal{M}(t_n)) \in P^\mathcal{M}$。
    
2. **联结词（以 $\neg$ 和 $\rightarrow$ 为例，其余类推）：**
    
    - $\mathcal{M}, v \models \neg \phi$ 当且仅当 **不成立** $\mathcal{M}, v \models \phi$。
        
    - $\mathcal{M}, v \models \phi \rightarrow \psi$ 当且仅当 如果 $\mathcal{M}, v \models \phi$，则必有 $\mathcal{M}, v \models \psi$。
        
3. **量词（核心难点）：**
    
    - $\mathcal{M}, v \models \forall x \phi$ 当且仅当 对论域 $D$ 中的**任意**元素 $d$，都有 $\mathcal{M}, v[x \mapsto d] \models \phi$。
        
    - $\mathcal{M}, v \models \exists x \phi$ 当且仅当 论域 $D$ 中**存在至少一个**元素 $d$，使得 $\mathcal{M}, v[x \mapsto d] \models \phi$。
        

> _注：$v[x \mapsto d]$ 表示一个新赋值，它除了把变量 $x$ 强制映射到个体 $d$ 之外，其余变量的映射与 $v$ 完全相同。_
# 消解与合一
### 1. 斯科伦化（Skolemization）与合取范式（CNF）
计算机在做推理前，需要把写好的 FOL 公式化简。

- **面试点：** 怎么消灭存在量词 $\exists$？如果是孤立的 $\exists x P(x)$，用一个常量代入（Skolem Constant）；如果 $\exists$ 在 $\forall$ 的作用域内，比如 $\forall x \exists y Love(x, y)$，就必须用函数代入（Skolem Function），化简为 $\forall x Love(x, f(x))$。
    
- 把所有量词移到最前面，最后把公式化成由 $\lor$（或）连接、外面由 $\land$（且）连接的**子句集（Clausal Form）**。
    

### 2. 合一算法（Unification）与消解（Resolution）
- 计算机怎么推理？比如库里有 `[~Man(x) \/ Mortal(x)]`（只要是人就会死）和 `[Man(Socrates)]`（苏格拉底是人）。
    
- 算法会将变量 $x$ 和常量 `Socrates` 进行合一（Unification）绑定，即替换 $[x \mapsto \text{Socrates}]$。
    
- 然后将 `~Man(Socrates)` 和 `Man(Socrates)` 进行**消解（消消乐）**，最终推导射出 `Mortal(Socrates)`。