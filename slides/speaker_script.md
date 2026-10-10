# Oral Defense — Speaker Script (中 / EN)
**Machine Learning Augmented Simulation and Analysis of Dynamical Systems**
Presenter: Guo Yue · Supervisor: Li Qianxiao · NUS Department of Mathematics

*How to use:* one section per slide, in slide order. Each has a 中文 version and an English version — pick one, or read 中文 to remember the logic and English to rehearse the delivery. Bold lines are the one-sentence "anchor" to memorize; the rest is padding you can shorten on the day.

> **A phrase I reuse for every system:** a *dynamical system* = **a `state` that changes over time**, plus a **rule (an equation)** that says how it changes. So on every technical slide I first say *"the state is …"* and *"the equation is …"*, then explain the method. That repetition is deliberate — it keeps the whole talk on one thread.

---

## Cover — Title
**中文**
各位老师好，我是郭悦，我的导师是李老师。今天我答辩的题目是《用机器学习增强动力系统的模拟与分析》。一句话概括我这几年做的事情:**动力系统就是一个随时间演化的状态，我们想把它算得又快又准；我做的，是用机器学习去改进"我们怎么表示、怎么计算"这个动力系统，同时保留经典数值方法的保证。**

**English**
Good afternoon everyone. I'm Guo Yue, supervised by Prof. Li Qianxiao. My thesis is *Machine Learning Augmented Simulation and Analysis of Dynamical Systems*. In one sentence: **a dynamical system is a state that evolves in time; we want to compute it fast and accurately. My work uses machine learning to improve *how we represent and compute* that system — while keeping the guarantees classical numerical methods give us.**

---

## Slide 1 / 29 — What is a dynamical system?
**中文**
先从最基础的问题开始:什么是动力系统?**就是一个随时间变化的状态，加上一条决定它怎么变的规则。** 屏幕上四个everyday的例子，都在动:一个分子在热振动——状态是每个原子的位置和速度；一个振子走向极限环——状态是它的位置和幅值；一列波在传播——状态是整根空间上的波形；还有一个受控的单摆——状态是角度和角速度，而且我们还能推它。它们表面很不一样，但数学结构是同一个:一个state，一条演化方程。

**English**
Let me start from the very basics: what is a dynamical system? **It's a state that changes in time, plus a rule for how it changes.** Four everyday examples on the screen, all in motion: a molecule vibrating — its state is the position and velocity of every atom; an oscillator settling onto a limit cycle — state is its position and amplitude; a wave traveling — state is the whole waveform in space; and a pendulum we can push — state is the angle and angular velocity, plus a control. They look different, but the math is the same shape: one state, one evolution rule.

---

## Slide 2 / 29 — Dynamics = choosing a computable form
**中文**
这里想强调一个平时被忽略的点:**只要你想在电脑上真正算一个动力系统，你就必须先把它写成某种"可计算的形式"。** 你得选:用什么方程、用什么坐标/表示、用什么数值格式去推进时间。这三件事——方程、表示、格式——就是我们后面反复讲的"三个旋钮"。这句话是整场报告的主线。

**English**
Here's a point that's easy to overlook: **to actually compute a dynamical system on a computer, you first have to put it into some *computable form*.** You must choose: which equation, which coordinates or representation, and which numerical scheme steps time forward. These three choices — the equation, the representation, the scheme — are the "three dials" I'll keep coming back to. This sentence is the spine of the whole talk.

---

## Slide 3 / 29 — Classically, every dial is set by hand
**中文**
经典的做法是:这三个旋钮**全部靠人手工推导**，而且每一个都带一条定理保证。方程从第一性原理来,比如牛顿定律 \(m\ddot x = -\nabla V\),精确、可解释;表示靠人的洞察,比如把系统写成简正模态,让它解耦;数值格式是手工设计的,比如 Euler、Runge–Kutta、辛格式,有 \(p\) 阶精度、有稳定性。这套传统非常强大。**但它有一个前提:你得能把这个形式亲手写下来。** 这正是我要突破的地方。

**English**
Classically, all three dials are **derived by hand**, and each comes with a theorem. The equation comes from first principles — say Newton's law \(m\ddot x=-\nabla V\) — exact and interpretable. The representation comes from human insight — like rewriting the system in normal modes so it decouples. The scheme is hand-designed — Euler, Runge–Kutta, symplectic — with order \(p\) and stability. This tradition is powerful. **But it has one requirement: you must be able to write the form down by hand.** That requirement is exactly where I push.

---

## Slide 4 / 29 — When the form isn't hand-writable, we learn it
**中文**
现实里,这个"写下来"经常做不到,有两种典型情况。第一种:**我们只有轨迹数据,方程本身是未知的**——我们能看着系统动,但写不出它的方程。第二种:**方程知道,但算一步太贵**——比如分子动力学,每一步的力都是一个量子尺度的计算。这两种情况下,经典手工推导就卡住了。机器学习的作用是:**用一个灵活的、可以从数据里拟合的形式,去替换那个手工形式**——但关键是,我们要把这份灵活性带进科学计算,同时不丢掉经典的保证。那么问题来了:三个旋钮,我们到底学哪一个?

**English**
In practice, "writing it down" often fails, in two typical ways. First: **we only have trajectory data and the equation is unknown** — we watch the system move but can't write its equation. Second: **the equation is known but one step is too expensive** — in molecular dynamics, each force is a quantum-scale calculation. In both cases, hand derivation stalls. Machine learning's role is to **replace the hand-designed form with a flexible one, fit from data** — but the point is to bring that flexibility into scientific computing *without* giving up the classical guarantees. Which raises the question: of the three dials, which one do we learn?

---

## Slide 5 / 29 — Thesis viewpoint: learn the structured components
**中文**
这张流程图就是我整篇论文的地图。左边是"动力系统",右边是我们想要的"预测轨迹",中间是把系统变成轨迹要用到的组件。分两种情况:如果**动力学未知**(只有数据),我们就去**学表示**——这是第一部分;如果**动力学已知**(方程有,但算得贵或想更准),我们有两条路——**学积分器**(第二部分),或者**学方程本身**(第三部分)。所以三个部分,恰好对应学习这三个不同的结构组件。下面我一个一个讲。

**English**
This flowchart is the map for the whole thesis. On the left is the dynamical system; on the right is the predicted trajectory we want; in between are the components that turn one into the other. Two cases: if the **dynamics is unknown** (only data), we **learn the representation** — that's Part I. If the **dynamics is known** (we have the equation but it's expensive, or we want more accuracy), we have two routes — **learn the integrator** (Part II) or **learn the equation itself** (Part III). So the three parts correspond exactly to learning these three structured components. Let me take them one at a time.

---

## Divider — Part I · Learning representations (Parametric Koopman Decompositions)
**中文**
第一部分:学表示。这部分发表在 SIAM Journal on Applied Dynamical Systems (2025),题目是 Learning Parametric Koopman Decompositions for Prediction and Control。

**English**
Part I: learning the representation. Published in the *SIAM Journal on Applied Dynamical Systems* (2025), as *Learning Parametric Koopman Decompositions for Prediction and Control*.

---

## Focus page (Part I) — where we are on the map
**中文**
回到地图:这一部分处在"动力学未知、只有数据"这条线上,我们要学的是**表示**。

**English**
Back to the map: this part sits on the "dynamics unknown, only data" branch, and what we learn is the **representation**.

---

## Slide 6 / 29 — Make a nonlinear system look linear (PK-NN)
**中文**
动机很直接。我们手上是一个**非线性系统,而且只能通过数据观察它**——状态随时间演化,但我们不知道它的方程。麻烦在于:非线性系统很难分析。而**线性系统**有一整套现成的工具箱:谱分析、预测、凸优化的最优控制、稳定性证明——可这些好处只对线性系统成立。于是驱动问题是:**我们能不能学一个"表示",让这个非线性系统在新的坐标下变成线性的?** 一旦变线性,那整套工具箱就都能用了。

**English**
The motivation is direct. We have a **nonlinear system that we can only observe through data** — the state evolves in time, but we don't know its equation. The trouble: nonlinear systems are hard to analyze. **Linear** systems, in contrast, come with a whole toolbox — spectral analysis, forecasting, convex optimal control, stability certificates — but only if the system is linear. So the driving question is: **can we learn a *representation* in which the nonlinear system becomes linear?** Once it's linear, we inherit that entire toolbox.

---

## Slide 7 / 29 — The problem: data-driven prediction & control
**中文**
把问题写清楚。**我们只有一个未知系统的轨迹数据,系统的演化规则是一个未知的转移映射:状态 \(x_{n+1}=F(x_n,u_n)\)。** 这里随时间演化的 state 是 \(x\),\(u\) 是我们能施加的控制或参数;\(F\) 未知。基于它,我们问两个问题:一是**预测**——给定初值和控制序列,预测未来的状态;二是**控制**——给定初值和一个代价,找一串控制让代价最小。数据都是一样的 \(\{x_n,u_n\}\)。两个拦路虎:非线性,和高维。

**English**
Let me state the problem. **We only have trajectory data from an unknown system whose rule is an unknown transition map: the state \(x_{n+1}=F(x_n,u_n)\).** Here the time-evolving state is \(x\), \(u\) is the control or parameter we can apply, and \(F\) is unknown. On top of it we pose two problems: **prediction** — given an initial state and a control sequence, predict future states; and **control** — given an initial state and a cost, find controls that minimize it. Same data \(\{x_n,u_n\}\) for both. Two obstacles: nonlinearity and high dimensionality.

---

## Slide 8 / 29 — Koopman: a finite, linear invariant subspace
**中文**
Koopman 的想法很漂亮:**不要直接追踪状态,而去追踪状态的"观测量" \(g(x)\)——也就是状态的函数。** Koopman 算子 \(K\) 会把任意观测量线性地往前推一步:\(g(x_{n+1})=(Kg)(x_n)\)。代价是 \(K\) 作用在无穷维的函数空间上。所以我们只保留一小组精心挑的字典 \(\Psi=(\psi_1,\dots,\psi_m)\),让它张成一个有限的不变子空间;在这个子空间里 \(K\) 就变成一个有限矩阵 \(K\),得到 **\(\Psi(x_{n+1})=K\,\Psi(x_n)\)**。一句话:**用非线性的观测量,把非线性动力学变成线性演化;深度学习负责处理高维和挑字典。**

**English**
The Koopman idea is elegant: **instead of tracking the state, track *observables* \(g(x)\) — functions of the state.** The Koopman operator \(K\) pushes any observable forward one step, linearly: \(g(x_{n+1})=(Kg)(x_n)\). The price is that \(K\) lives on an infinite-dimensional function space. So we keep only a small, well-chosen dictionary \(\Psi=(\psi_1,\dots,\psi_m)\) spanning a finite *invariant* subspace; on it, \(K\) becomes a finite matrix, giving **\(\Psi(x_{n+1})=K\,\Psi(x_n)\)**. In one line: **nonlinear observables turn nonlinear dynamics into linear evolution; deep learning handles the high dimension and picks the dictionary.**

---

## Slide 9 / 29 — A less trivial example: a nonlinear limit cycle
**中文**
举个不平凡的例子,一个有极限环的系统,Stuart–Landau。**它的状态是 \((x,y)\),方程是 \(\dot x=(1-x^2-y^2)x-\omega y,\ \dot y=\omega x+(1-x^2-y^2)y\)。** 直接看很非线性。但换成极坐标 \(r^2=x^2+y^2\),它就解耦成 \(\dot r=r(1-r^2)\) 和 \(\dot\theta=\omega\):**幅值 \(r\) 非线性地趋近 1,相位 \(\theta\) 匀速旋转。** 右边的图就是这个极限环。这个例子的好处是:它有清楚的结构,我们能亲眼看到 Koopman 坐标该长什么样。

**English**
Here's a less trivial example — a system with a limit cycle, Stuart–Landau. **Its state is \((x,y)\), and the equation is \(\dot x=(1-x^2-y^2)x-\omega y,\ \dot y=\omega x+(1-x^2-y^2)y\).** Looks very nonlinear. But in polar form, \(r^2=x^2+y^2\), it decouples into \(\dot r=r(1-r^2)\) and \(\dot\theta=\omega\): **the amplitude \(r\) relaxes nonlinearly toward 1, and the phase \(\theta\) rotates uniformly.** The plot on the right is that limit cycle. This example is useful because it has clean structure — we can see exactly what the Koopman coordinates should look like.

---

## Slide 10 / 29 — Koopman coordinates separate decay and rotation
**中文**
现在挑对观测量。取 \(\psi_a=\frac{1}{r^2}-1\) 表示幅值,\(\psi_c=\cos\theta,\ \psi_s=\sin\theta\) 表示相位。在这三个观测量上,演化就**完全线性**了:\(\dot\Psi=A\Psi\),一步映射 \(\Psi_{n+1}=K\Psi_n\),\(K=e^{A\Delta t}\)。你能直接读出结构:\(\psi_a\) 指数衰减,\((\psi_c,\psi_s)\) 匀速旋转且模长为 1,而 \(\psi_a=0\) 恰好对应 \(r=1\),也就是极限环。**要点:观测量空间里演化是线性的,而且没有增加系统的内在维数——还是二维的不变流形。**

**English**
Now pick the right observables. Take \(\psi_a=\frac{1}{r^2}-1\) for the amplitude, and \(\psi_c=\cos\theta,\ \psi_s=\sin\theta\) for the phase. On these three, the evolution becomes **exactly linear**: \(\dot\Psi=A\Psi\), one step \(\Psi_{n+1}=K\Psi_n\) with \(K=e^{A\Delta t}\). You can read the structure right off: \(\psi_a\) decays exponentially, \((\psi_c,\psi_s)\) rotate uniformly on the unit circle, and \(\psi_a=0\) means exactly \(r=1\), the limit cycle. **The point: evolution is linear in observable space, with no increase in the intrinsic dimension — still a 2-D invariant manifold.**

---

## Slide 11 / 29 — Finite-dimensional closure is not generic
**中文**
但要诚实:这么干净是特例。换一个非线性单摆:**状态是 \((\theta,v)\),方程 \(\dot\theta=v,\ \dot v=-\sin\theta\)。** 如果从一个有限字典 \(\Psi=(\theta,v,\sin\theta,\cos\theta)\) 出发,你去对时间求导,会不断冒出新的观测量——\(v\cos\theta\)、\(v^2\sin\theta\)……字典永远闭合不了。**结论:Koopman 算子永远是线性的,但它的不变观测量空间一般是无穷维的。所以有限维的 Koopman 模型通常是近似,而不是精确闭合。** 这也正是为什么我们要用神经网络去"学"一个好用的有限字典。

**English**
But let's be honest — that clean case is special. Take a nonlinear pendulum: **state \((\theta,v)\), equation \(\dot\theta=v,\ \dot v=-\sin\theta\).** Start from a finite dictionary \(\Psi=(\theta,v,\sin\theta,\cos\theta)\), differentiate in time, and new observables keep appearing — \(v\cos\theta\), \(v^2\sin\theta\), and so on. The dictionary never closes. **Takeaway: the Koopman operator is always linear, but its invariant observable space is generally infinite-dimensional. So finite Koopman models are usually approximations, not exact closures.** That's precisely why we let a neural network *learn* a good finite dictionary.

---

## Slide 12 / 29 — PK-NN: learn Ψ and K(u) jointly
**中文**
这就是我们的方法,PK-NN。**对参数化/受控系统,矩阵 \(K\) 依赖于控制 \(u\);PK-NN 用神经网络同时学一套共享字典 \(\Psi\) 和一个完全非线性的算子 \(K(u)\)**,模型是 \(\Psi(x_{n+1})=K(u_n)\,\Psi(x_n)\)。和已有方法比:相比 DMD/EDMD,我们加入了参数 \(u\);相比线性/双线性控制模型,我们对 \(u\) 是**完全非线性**的;相比固定基的参数化,我们是用神经网络去学 \(u\) 里的非线性。训练就是最小化一步预测在观测量空间的误差——那个求和式子。**一句话:字典和随控制变化的算子,一起端到端学出来。**

**English**
This is our method, PK-NN. **For parametric or controlled systems the matrix \(K\) depends on the control \(u\); PK-NN learns, jointly, one shared dictionary \(\Psi\) and a fully nonlinear operator \(K(u)\)**, giving \(\Psi(x_{n+1})=K(u_n)\,\Psi(x_n)\). Compared with prior work: versus DMD/EDMD we add the parameter \(u\); versus linear or bilinear control models we are **fully nonlinear** in \(u\); versus fixed-basis parametrizations, a neural net learns the nonlinearity in \(u\). Training just minimizes the one-step error in observable space — that sum on the right. **In one line: the dictionary and the control-dependent operator are learned together, end to end.**

---

## Slide 13 / 29 — We solve both: forward prediction & optimal control
**中文**
学到这个线性表示以后,两个任务都能做,而且都很直接。**预测**:把状态抬到观测量空间(lift),用 \(K(u_n)\) 一步步线性推进,再线性读出 \(\hat y=B\Psi\)。**控制**:因为在观测量空间里是线性的,我们就能在这个"抬起来的"空间里做模型预测控制(MPC),优化控制序列 \(\{u_n\}\) 去跟踪一个参考。**关键:预测和控制用的是同一个学到的线性模型,不用换框架。**

**English**
Once we've learned this linear representation, both tasks follow, and both are simple. **Prediction**: lift the state into observable space, step forward linearly with \(K(u_n)\), then read out linearly, \(\hat y=B\Psi\). **Control**: because it's linear in observable space, we can run Model Predictive Control in that lifted space, optimizing the control sequence \(\{u_n\}\) to track a reference. **The key: prediction and control use the *same* learned linear model — no change of framework.**

---

## Slide 14 / 29 — Prediction under strong nonlinearity in u
**中文**
第一个实验,预测。系统是 Van der Pol–Mathieu:**状态 \((x_1,x_2)\),方程 \(\dot x_1=x_2,\ \dot x_2=(2-2x_1^2)x_2-(1+2\mu u^2-\mu)x_1+u\)**;这里的 \(\mu\) 控制了"控制量 \(u\) 进入得有多非线性"。图上是误差随步长的变化,随着 \(\mu\) 增大——也就是非线性越来越强。**结论:因为我们的 \(K(u)\) 对 \(u\) 是完全非线性的,\(\mu\) 变大时精度依然保持,而且能泛化到训练时没见过的参数。** 这正是线性/双线性模型做不到的。

**English**
First experiment: prediction. The system is Van der Pol–Mathieu: **state \((x_1,x_2)\), equation \(\dot x_1=x_2,\ \dot x_2=(2-2x_1^2)x_2-(1+2\mu u^2-\mu)x_1+u\)**, where \(\mu\) controls how *nonlinearly* the control \(u\) enters. The plot shows error versus step size as \(\mu\) grows — i.e., as nonlinearity increases. **Result: because our \(K(u)\) is fully nonlinear in \(u\), accuracy holds up as \(\mu\) grows, and it generalizes to parameters unseen in training** — which is exactly what linear or bilinear models cannot do.

---

## Slide 15 / 29 — Optimal control — forced KdV (MPC)
**中文**
第二个实验,控制,而且是一个高维的例子:受迫的 KdV 方程。**这里随时间演化的状态是整根空间上的波形 \(\eta(x,t)\);方程是 \(\partial_t\eta+\eta\partial_x\eta+\partial_{xxx}\eta=\sum_i v_i(x)\sin(\pi u_i)\)**,控制 \(u\) 非线性地进入。我们的目标是跟踪一个参考 \(r_n\)(动量 \(\int\eta^2\,dx\)),做法是在抬起来的空间里做 MPC,最小化跟踪误差加上控制能量。模型从抬起的状态线性读出 \(\hat r_n=c^\top\Psi(x_n)\)。图里:**我们的方法(实线)紧跟阶梯参考(虚线),而基线 M2/M3 明显滞后。**

**English**
Second experiment: control, and a high-dimensional one — the forced KdV equation. **Here the time-evolving state is the whole spatial waveform \(\eta(x,t)\); the equation is \(\partial_t\eta+\eta\partial_x\eta+\partial_{xxx}\eta=\sum_i v_i(x)\sin(\pi u_i)\)**, with the control \(u\) entering nonlinearly. Our goal is to track a reference \(r_n\) (the momentum \(\int\eta^2\,dx\)) by running MPC in the lifted space, minimizing tracking error plus control energy. The model reads the reference off the lifted state linearly, \(\hat r_n=c^\top\Psi(x_n)\). In the figure: **our method (solid) follows the step reference (dashed), while the baselines M2/M3 lag.**

---

## Divider — Part II · Learning the numerical mechanism (Personalized ODE Integrators)
**中文**
第二部分:学积分器。这部分发表在 SIAM Journal on Scientific Computing (2022),题目是 Personalized Algorithm Generation: A Case Study in Learning ODE Integrators。

**English**
Part II: learning the numerical mechanism. Published in the *SIAM Journal on Scientific Computing* (2022), as *Personalized Algorithm Generation: A Case Study in Learning ODE Integrators*.

---

## Focus page (Part II) — where we are on the map
**中文**
回到地图:现在换到"动力学已知"这条线;方程有了,我们要学的是**积分器**——也就是推进时间的数值格式。

**English**
Back to the map: now we're on the "dynamics known" branch; the equation is given, and what we learn is the **integrator** — the numerical scheme that steps time forward.

---

## Slide 16 / 29 — An integrator built for a specific system (RK-NN)
**中文**
动机是这样的。经典求解器,比如 Runge–Kutta,**一套系数要对"所有可能的 \(f\)"都成立**——它是最坏情况下最优的、非常鲁棒。但鲁棒是有代价的:如果你其实是在**反复求解同一类 ODE**,那套最坏情况的系数从来没为你这一类调过,于是**你每一次求解都在为你根本碰不到的系统交税**。而且没有哪个经典方法能躲开这一点,因为阶条件天生就是"对所有 \(f\)"。驱动问题:**我们能不能学一个专门为"我们真正在解的这一类"定制的积分器?**

**English**
Here's the motivation. A classical solver like Runge–Kutta fixes **one set of coefficients that must work for *every possible* \(f\)** — it's worst-case optimal, very robust. But robustness has a price: if you're really **solving the same family of ODEs over and over**, those worst-case coefficients were never tuned to your family, so **every single run you pay a tax for systems you never touch**. And no classical method avoids this, because the order conditions are "for all \(f\)" by construction. Driving question: **can we *learn* an integrator specialized to the family we actually solve?**

---

## Slide 17 / 29 — Personalized ODE integrators (known dynamics)
**中文**
把问题形式化。**这里方程是已知的 ODE \(\dot y=f(y)\),随时间演化的状态是 \(y\);一个积分器 \(I\) 用已知的 \(f\) 把状态推进一步:\(y_n\to y_{n+1}\)。** 问题是:该怎么优化这个 \(I\)?经典做法是让 \(I\) 对一大类系统 \(\mathcal F\) 的**最坏情况**最优:\(\sup_{F\in\mathcal F}\|L(I,F)\|=O(h^\alpha)\)。我们改成让 \(I\) 对我们真正求解的那一类 \(\mu\) 的**期望**最优:\(\mathbb E_\mu\|L(I,F_\mu)\|=O(h^\alpha)\)。**从"对所有 \(f\)取上确界"换成"对这一类取期望"——这就是一个个性化的积分器。**

**English**
Let me formalize it. **Here the equation is a known ODE \(\dot y=f(y)\); the time-evolving state is \(y\); and an integrator \(I\) uses the known \(f\) to advance the state one step: \(y_n\to y_{n+1}\).** The question is: how should we optimize \(I\)? Classically, \(I\) is tuned for the **worst case** over a big class \(\mathcal F\): \(\sup_{F\in\mathcal F}\|L(I,F)\|=O(h^\alpha)\). We instead optimize \(I\) over the family \(\mu\) we actually solve, in **expectation**: \(\mathbb E_\mu\|L(I,F_\mu)\|=O(h^\alpha)\). **Swap the "sup over all \(f\)" for an "expectation over this family" — and you get a personalized integrator.**

---

## Slide 18 / 29 — Classical RK: coefficients fixed by hand
**中文**
先看经典 RK 是怎么定系数的,因为我们的方法就长在它上面。一个 RK 步是 \(\hat y_{n+1}=y_n+h\sum_i b_i k_i\),各级 \(k_i=f(y_n+h\sum_j a_{ij}k_j)\)。把精确解按"初等微分"展开:\(y_{n+1}=y_n+hf+\frac{h^2}{2}f'f+\frac{h^3}{6}(f''ff+f'f'f)+\cdots\)。RK 也按同样的初等微分展开——**要对所有 \(f\) 都对上,每一项就给出一个阶条件**,比如 \(\sum b_i=1\)、\(\sum b_i c_i=\tfrac12\),等等。匹配到 \(h^3\),数值步和精确步就只差一个截断误差 \(O(h^4)\)。**记住这张表:经典 RK 的系数,就是"让每一项对所有 \(f\) 相等"逼出来的。**

**English**
Let's see how classical RK fixes its coefficients, because our method sits right on top of it. An RK step is \(\hat y_{n+1}=y_n+h\sum_i b_i k_i\), with stages \(k_i=f(y_n+h\sum_j a_{ij}k_j)\). Taylor-expand the exact step in *elementary differentials*: \(y_{n+1}=y_n+hf+\frac{h^2}{2}f'f+\frac{h^3}{6}(f''ff+f'f'f)+\cdots\). RK expands in the same differentials — **matching each term *for all \(f\)* gives one order condition per term**, e.g. \(\sum b_i=1\), \(\sum b_i c_i=\tfrac12\), and so on. Matched through \(h^3\), the numerical step differs from the exact one only by the truncation error \(O(h^4)\). **Remember this table: classical RK coefficients are forced by "make each term equal for all \(f\)."**

---

## Slide 19 / 29 — RK-NN: learn the coefficients θ
**中文**
我们的方法,RK-NN,想法很简单:**把 Butcher 表里的系数变成可学习的参数 \(\theta\),放进一个 \(m\)-级的 RK 里。** 数据怎么来?我们从目标家族 \(F_\mu\) 里采样,取初值 \(y_0\)、步长 \(h\in(0.01,0.1)\),而**目标 \(y_1=\varphi_h(y_0)\) 就是用已知 \(f\) 算出来的、精确的一步解**。训练的损失有两块:第一块是一步误差,而且用经典 RK 的误差去做归一化——所以我们是在"相对经典有多好"这个尺度上学;第二块是一个 Taylor 正则项 \(\mathcal R\),用来**强制保留 \(\alpha\) 阶精度**。**一句话:系数从数据里学,但阶数由正则项守住。**

**English**
Our method, RK-NN, is simple: **make the Butcher-tableau coefficients learnable parameters \(\theta\) inside an \(m\)-stage RK.** Where's the data from? We sample the target family \(F_\mu\), initial states \(y_0\), and steps \(h\in(0.01,0.1)\); the **target \(y_1=\varphi_h(y_0)\) is the accurate one-step solution computed from the known \(f\)**. The loss has two parts: a one-step error, *normalized by the classical RK error* — so we learn on the scale of "how much better than classical" — plus a Taylor regularizer \(\mathcal R\) that **enforces order \(\alpha\)**. **In one line: coefficients are learned from data, but the order is held fixed by the regularizer.**

---

## Slide 20 / 29 — RK-NN keeps order 3 yet beats RK3
**中文**
结果。两个真实的二维非线性系统,状态都是 \(y=(u,v)\)。一个是 **Van der Pol:\(\dot u=v,\ \dot v=\mu(1-u^2)v-u\)**,一个极限环,非线性由 \(\mu\) 定;另一个是 **Brusselator:\(\dot u=A+u^2v-(B+1)u,\ \dot v=Bu-u^2v\)**,一个自催化反应。我们训一个 3 级、\(\alpha=3\) 的 RK-NN,针对每个家族、在 \(h\in(0.01,0.1)\) 上训练。**结论:它保持三阶精度,但在这一类问题上比经典 RK3 更准。** 代价是:出了这个步长范围它就退化——这是"专门定制"的必然,也是设计使然。

**English**
Results. Two real 2-D nonlinear systems, state \(y=(u,v)\) in both. One is **Van der Pol: \(\dot u=v,\ \dot v=\mu(1-u^2)v-u\)**, a limit cycle with nonlinearity set by \(\mu\); the other is **Brusselator: \(\dot u=A+u^2v-(B+1)u,\ \dot v=Bu-u^2v\)**, an autocatalytic reaction. We train a 3-stage, \(\alpha=3\) RK-NN per family over \(h\in(0.01,0.1)\). **Result: it keeps third-order accuracy, yet beats classical RK3 on these families.** The price: outside that step range it degrades — the flip side of specialization, and by design.

---

## Slide 21 / 29 — Highlight: "breaking" the order barrier
**中文**
这是我最喜欢的一个小结果。用一个 2 级格式,\(k_2=f(y+h\theta_1 k_1)\),\(\hat y_{n+1}=y+h(\theta_{c1}k_1+\theta_{c2}k_2)\)。经典地,2 级只能凑到二阶:两个二阶条件解出来就是 Heun,\(\theta_1=1,\theta_{c1}=\theta_{c2}=\tfrac12\);而三阶需要两个独立条件,2 级凑不齐——这就是**阶障碍**。但如果我们只针对一个特殊家族 \(f=y^2\):这时三阶的两项恰好都正比于 \(y^4\),**合并成一个条件**。把精确解和我们的格式都展开到 \(O(h^4)\),对齐 \(h^3\) 项就只需 \(\theta_{c2}\theta_1^2=1\),可解,给出 \(\theta_1=2,\theta_{c2}=\tfrac14,\theta_{c1}=\tfrac34\)。**结论:在这一类问题上,2 级格式拿到了三阶的收敛斜率——机制上"绕过"了阶障碍。**

**English**
This is my favorite little result. Take a 2-stage scheme, \(k_2=f(y+h\theta_1 k_1)\), \(\hat y_{n+1}=y+h(\theta_{c1}k_1+\theta_{c2}k_2)\). Classically, 2 stages only reach order 2: solving the two second-order conditions gives Heun, \(\theta_1=1,\theta_{c1}=\theta_{c2}=\tfrac12\); order 3 needs two independent conditions that 2 stages can't meet — the **order barrier**. But specialize to one family, \(f=y^2\): now the two third-order terms are both \(\propto y^4\), so they **merge into one condition**. Expand the exact step and our scheme to \(O(h^4)\); matching the \(h^3\) term needs only \(\theta_{c2}\theta_1^2=1\), which is solvable, giving \(\theta_1=2,\theta_{c2}=\tfrac14,\theta_{c1}=\tfrac34\). **Result: on this family, a 2-stage scheme attains a third-order convergence slope — it *breaks past* the order barrier.**

---

## Divider — Part III · Learning the continuous description (Integrator-aware Learning)
**中文**
第三部分:学方程本身。这部分是在投的稿件,Integrator-aware Learning of Modified Equations。

**English**
Part III: learning the equation itself. A manuscript in process, *Integrator-aware Learning of Modified Equations*.

---

## Focus page (Part III) — where we are on the map
**中文**
回到地图:还是"动力学已知"这条线,但这次我们不动积分器,去学**方程**(那个连续的场)——而且是"考虑了积分器"地去学。

**English**
Back to the map: still the "dynamics known" branch, but this time we leave the integrator alone and learn the **equation** — the continuous field — in an *integrator-aware* way.

---

## Slide 22 / 29 — Cheap forces, but tiny steps (ModEqLearn)
**中文**
动机来自分子动力学。现在的 ML 势(比如 MACE)让**每一步的力算得很便宜**。但分子运动需要**非常小的时间步 \(\Delta t\)(飞秒量级)**,所以一条长轨迹要走**上百万步**——瓶颈从"力贵不贵"变成了"步数多不多"。你想加大 \(\Delta t\) 来提速?那么每一步的离散误差会**沿着轨迹累积**,能量和结构会漂移。屏幕上是 ethanol、naphthalene、aspirin 三个分子。驱动问题:**我们能不能学一套力,让它在更大的 \(\Delta t\) 下依然准?**

**English**
The motivation comes from molecular dynamics. Modern ML potentials — like MACE — make **the force per step cheap**. But molecular motion needs a **tiny time step \(\Delta t\), on the femtosecond scale**, so a long trajectory takes **millions of steps** — the bottleneck shifts from force cost to *step count*. Want to take a bigger \(\Delta t\) to go faster? Then the per-step discretization error **accumulates along the trajectory**, and energy and structure drift. On screen: ethanol, naphthalene, aspirin. Driving question: **can we learn forces that stay accurate at a larger \(\Delta t\)?**

---

## Slide 23 / 29 — Key idea ①: Duality with learning the integrator
**中文**
第一个关键想法,是它和第二部分的**对偶**。第二部分是:**给定一个系统,学积分器**——固定方程,调格式,让离散映射逼近真流。这一部分反过来:**给定一个积分器,学方程**——固定格式,调那个喂进去的场 \(f_\theta\),让同一个离散映射逼近真流。两张图结构一样,只是"哪个是学的、哪个是固定的"对调了。**一句话:同一个"离散映射对不上真流"的缺口,你既可以动格式来补(第二部分),也可以动方程来补(这一部分)——这就是对偶。**

**English**
The first key idea is a **duality** with Part II. Part II was: **given a system, learn the integrator** — fix the equation, tune the scheme so the discrete map approximates the true flow. This part flips it: **given an integrator, learn the equation** — fix the scheme, tune the field \(f_\theta\) you feed in so the *same* discrete map approximates the true flow. The two diagrams have the same structure; only "what's learned vs. what's fixed" is swapped. **In one line: the *same* gap between the discrete map and the true flow can be closed by tuning the scheme (Part II) *or* by tuning the equation (this part) — that's the duality.**

---

## Slide 24 / 29 — Key idea ②: modified equations
**中文**
第二个关键想法,是把这件事讲严谨的工具:**后向误差分析**。它说:**一个数值积分器,其实是在精确地求解一个"被修改过的"微分方程**——这个修改后的场 \(\mathcal M_h(f)\) 依赖于积分器和步长 \(h\):\(\mathcal M_h(f)=f+h f^{(1)}+h^2 f^{(2)}+\cdots\)。反过来用:**给定我们想复现的真场 \(f_{\text{true}}\),去找一个要喂给积分器的场 \(f_\theta\),让积分器的数值流恰好等于真流。** 结论一句话:**积分器真正应该瞄准的目标,不是 \(f_{\text{true}}\),而是它的"逆修改" \(\mathcal M_h^{-1}(f_{\text{true}})\)。**

**English**
The second key idea is the tool that makes this rigorous: **backward error analysis**. It says: **a numerical integrator is actually solving a *modified* differential equation exactly** — the modified field \(\mathcal M_h(f)\) depends on the integrator and the step size \(h\): \(\mathcal M_h(f)=f+h f^{(1)}+h^2 f^{(2)}+\cdots\). Used in reverse: **given the true field \(f_{\text{true}}\) we want to reproduce, find a field \(f_\theta\) to feed the integrator so its numerical flow matches the true flow exactly.** One-line conclusion: **the integrator's true target isn't \(f_{\text{true}}\), it's the *inverse-modified* field \(\mathcal M_h^{-1}(f_{\text{true}})\).**

---

## Slide 25 / 29 — Method: ModEqLearn
**中文**
方法就直接落在上一张的结论上。**我们训练一个场 \(f_\theta\),让它的"修改方程"匹配真场:最小化 \(\mathbb E_{y\sim\mathcal D}\|\mathcal M_h(f_\theta)(y)-f_{\text{true}}(y)\|^2\)。** 数据是:从定义域里采样一批状态 \(y\sim\mathcal D\),每个配上它那里的真场 \(f_{\text{true}}(y)\);训练是在目标步长 \(h\) 上做的。图里对比了两种训练:经典是"逐点把场对上",我们是"考虑了积分器再对场"。**一句话:不要让场在每个点上对得最准,而要让它经过积分器之后、跑出来的轨迹最准。**

**English**
The method sits right on the previous conclusion. **We train a field \(f_\theta\) so that its *modified equation* matches the true field: minimize \(\mathbb E_{y\sim\mathcal D}\|\mathcal M_h(f_\theta)(y)-f_{\text{true}}(y)\|^2\).** The data: sample states \(y\sim\mathcal D\) over the domain, each paired with the true field \(f_{\text{true}}(y)\) there; training is done at the target step size \(h\). The figure contrasts two trainings: classical matches the field *pointwise*; ours matches it *after accounting for the integrator*. **In one line: don't make the field most accurate at each point — make the *trajectory it produces through the integrator* most accurate.**

---

## Slide 26 / 29 — Case study: molecular dynamics (Hamiltonian · leapfrog)
**中文**
把它落到分子上。**分子是一个哈密顿系统:状态是位置 \(q\) 和动量 \(p\);能量 \(H(q,p)=\tfrac12 p^\top M^{-1}p+V(q)\);动力学由它的偏导给出:\(\dot q=M^{-1}p,\ \dot p=-\nabla V(q)\)。** 数值上用辛的 leapfrog(Störmer–Verlet)推进。分子 ML 学的是势能 \(V_\theta\)。经典 MACE 是**逐点**把 \(V_\theta\) 拟合到能量和力上;我们(ModEqLearn)则用 leapfrog 的**修改哈密顿量** \(\tilde H^{(2)}_\theta\) 来训练——让修改后的场匹配真场。**关键一句:一个逐点完美的 \(V_\theta\),在大步长的 leapfrog 下也可能漂移——这正是我们要补上的缺口。**

**English**
Let's ground it in molecules. **A molecule is a Hamiltonian system: the state is positions \(q\) and momenta \(p\); the energy is \(H(q,p)=\tfrac12 p^\top M^{-1}p+V(q)\); and the dynamics come from its partials: \(\dot q=M^{-1}p,\ \dot p=-\nabla V(q)\).** Numerically we step with the symplectic leapfrog (Störmer–Verlet). Molecular ML learns the potential \(V_\theta\). Classical MACE fits \(V_\theta\) to energies and forces **pointwise**; ours (ModEqLearn) trains through leapfrog's **modified Hamiltonian** \(\tilde H^{(2)}_\theta\) — matching the modified field to the true one. **Key line: a pointwise-perfect \(V_\theta\) can still drift in leapfrog at a large step size — that's exactly the gap we close.**

---

## Slide 27 / 29 — Results: stable long-time integration
**中文**
结果,一眼就能看出来。左边是乙烷 \(C_2H_6\) 在**大步长**下的 rollout 轨迹:真值、经典、MACE 预训练、和我们的方法。前几个在大步长下会散掉或漂移,**我们的方法保持稳定**。右图是定量的:**存活时间随步长的变化**——我们的方法(紫色)在更大的步长下依然能存活到满程,而基线很快掉下去。**一句话:考虑了积分器去学方程,换来的是大步长下的长时间稳定。**

**English**
The results are visible at a glance. On the left, rollout trajectories of ethane \(C_2H_6\) at a **large step size**: ground truth, classical, MACE-pretrained, and our method. The first ones blow up or drift at large steps; **ours stays stable**. On the right, the quantitative version: **survival time versus step size** — our method (purple) survives to full length at larger steps, while the baselines drop off quickly. **In one line: learning the equation in an integrator-aware way buys long-time stability at large step sizes.**

---

## Slide 28 / 29 — Structural metric: bond-distance distribution
**中文**
不只是"没散",结构也对。我们用一个**键长分布**的指纹来量:交错和重叠两种构象有不同的原子间距分布 \(\psi(r)\),我们比较预测和真值分布的差 \(\Delta\psi\)——越小越好,"---"表示轨迹已经崩了。表里可以看到:在越来越大的步长下,基线大量出现崩溃(---),而**我们的方法在更大的步长下依然给出有限的、较小的 \(\Delta\psi\)**。**一句话:我们的方法不仅跑得久,连分子的构象统计都保持对。**

**English**
It's not just "doesn't blow up" — the *structure* is right too. We measure it with a **bond-distance** fingerprint: staggered and eclipsed conformations have distinct interatomic-distance distributions \(\psi(r)\), and we compare predicted vs. true via \(\Delta\psi\) — lower is better, and "---" means the trajectory has already blown up. In the table, as the step size grows the baselines blow up all over (---), while **ours still gives finite, small \(\Delta\psi\) at larger steps**. **In one line: our method not only runs longer, it keeps the molecule's conformational statistics correct.**

---

## Slide 29 / 29 — Summary: three works, one framework
**中文**
总结一下,三个工作其实是同一件事的三个组件:**把一个动力系统从"系统"算到"轨迹",你需要表示、积分器、方程这三样;每一部分学其中一个,同时守住经典的那部分保证。** Part I / PK-NN 学**表示**(Koopman 坐标),守住"演化是线性的";Part II / RK-NN 学**积分器**(为一类问题定制),守住"精度阶数";Part III / ModEqLearn 学**方程**(考虑积分器),守住"辛/几何结构"。中间那条对偶把 II 和 III 连起来:**同一个"离散映射对不上真流"的缺口,要么调格式,要么调方程。** 三把旋钮,各学一个,经典保证都还在。

**English**
To summarize, the three works are three components of one thing: **to compute a dynamical system from "system" to "trajectory," you need a representation, an integrator, and an equation; each part learns one of them, while keeping the classical guarantee on the rest.** Part I / PK-NN learns the **representation** (Koopman coordinates) — keeping linear evolution; Part II / RK-NN learns the **integrator** (tailored to a family) — keeping the order of accuracy; Part III / ModEqLearn learns the **equation** (integrator-aware) — keeping symplectic/geometric structure. The duality in the middle links II and III: **the same "discrete map vs. true flow" gap can be closed by tuning the scheme *or* the equation.** Three dials, learn one each, and the classical guarantees stay intact.

---

### One-line spine (memorize this and the talk reconstructs itself)
**中文:** 动力系统 = 随时间演化的 state + 一条方程 → 要算它就得选**表示 / 积分器 / 方程**这三个旋钮 → 经典全靠手推、还得能写下来 → 写不下来或太贵就**学一个旋钮、守住其余保证** → I 学表示(保线性)、II 学积分器(保阶数)、III 学方程(保辛),II↔III 对偶。

**English:** A dynamical system = a state evolving in time + one equation → to compute it you pick three dials: **representation / integrator / scheme(equation)** → classically all hand-derived and must be writable → when it isn't (or is too costly), **learn one dial, keep the guarantees on the rest** → I learns representation (keep linear), II learns the integrator (keep order), III learns the equation (keep symplectic), with II↔III dual.
