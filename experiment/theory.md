
<div style="font-family: 'Nunito Sans', sans-serif; font-size: 20px;text-align: justify;">
<h2>Introduction</h2>

The power flow analysis, also known as load flow analysis, is essential for determining the steady-state operating conditions of a power system. It involves calculating the steady-state active (P) and reactive (Q) powers, voltage magnitude (V), and and the angle $\delta$ each bus of the system. In general, only two of them are known and other two remain unknown at a bus. Based on these steady-state known and unknown quantities, each bus in the power system are classified as load bus (or PQ bus), generator bus (or PV bus), and slack bus (or reference bus). The known and unknown quantities of these buses are listed below -<br> 

###  1.	Bus Type – 

| Bus Type                | Explanation                                                                                                      | Known        | Unknown       |
|-------------------------|---------------------------------------------------------------------------------------------------------------------|------------------|-------------------|
| Load bus or PQ bus      | A bus connected only to loads, i.e., P and Q, are known as a load bus. Generally, P and Q are specified for such type of buses. | P and Q      | V and $\delta$ |
| Generator bus or PV bus | A bus connected to a generator is known as a generator bus. In general, P and V of such buses are known. A generator can maintain the voltage on a bus till its reactive power capability which is important for this bus to continue to operate as a PV bus. | P and V      | Q and $\delta$  |
| Slack or reference bus  | One bus in a system is specified as a slack bus whose V and $\delta$ are specified and P and Q are calculated. Here, please note that P is unknown for this bus which takes care of the mismatch in the generation and losses. Generally, the largest generator in the system is considered as the slack bus. | V and $\delta$ | P and Q        |


###  2. Power Flow Equations – 

<br>

$$
\begin{equation}
    

\begin{bmatrix}
I_{1}
\\ I_{2}
\\ \vdots 
\\ I_{i}
\\ \vdots 
\\ I_{n}
\end{bmatrix}\\

=\begin{bmatrix}
 Y_{11}&   Y_{12}&   \cdots &   Y_{1i}&   \cdots &  Y_{1n}\\ 
 Y_{21}&   Y_{22}&   \cdots &   Y_{2i}&   \cdots &  Y_{2n}\\ 
\vdots&   \vdots&   \vdots &   \vdots&   \vdots &  \vdots \\ 
 Y_{i1}&   Y_{i2}&   \cdots &   Y_{ii}&   \cdots &  Y_{in}\\ 
\vdots&   \vdots&   \vdots &   \vdots&   \vdots &  \vdots \\ 
 Y_{n1}&   Y_{n2}&   \cdots &   Y_{ni}&   \cdots &  Y_{nn} 
\end{bmatrix} 

\begin{bmatrix}
V_{1}
\\ V_{2}
\\ \vdots 
\\ V_{i}
\\ \vdots 
\\ V_{n}
\end{bmatrix}
\end{equation}
$$

<br>

$$
\begin{aligned}
    I_{bus} = Y_{bus}V_{bus}
\end{aligned}
$$

Where, $I_{bus}$  is the vector of the injected bus currents and $V_{bus}$ is the vector of bus voltages measured from the reference node. $Y_{bus}$ is known as the bus admittance matrix.

For each bus $i$ :

$$
\begin{equation}
    I_{i} = \sum_{j=1}^{n} Y_{ij} V_{j} = Y_{ii} V_{i} + \sum_{j=1, j \neq i}^{n} Y_{ij} V_{j}
\end{equation}
$$

Complex power injection at bus $i$ is given by, $S_{i}=P_{i}+jQ_{i}=V_{i}I_{i}^{*}$ . Inserting (2) and separating real and imaginary terms, the power flow equations for active power and reactive power are obtained and given in (3) and (4). 

<br>

$$
\begin{equation}
    P_{i}=\sum_{j=1}^{n}|V_{i}||V_{j}||Y_{ij}|cos(\theta_{ij}+\delta_{j}-\delta_{i})
\end{equation}
$$
$$
\begin{equation}
    Q_{i}=-\sum_{j=1}^{n}|V_{i}||V_{j}||Y_{ij}|sin(\theta_{ij}+\delta_{j}-\delta_{i})
\end{equation}
$$

Where,  $|V_{i}|$ and $\delta_{i}$ are the voltage magnitude and angle at bus $i$, and $|Y_{i}|$  and $\theta_{ij}$ are the magnitude and angle of the admittance matrix corresponding to the element at $i^{th}$ row and $j^{th}$ column. For a $‘n’$ bus system, there are total $‘2n’$ load flow equations and $‘2n’$ variables. 


<br>

## Newton-Raphson method for power flow analysis  – 
The Newton-Raphson method is an iterative technique used to solve nonlinear algebraic equations. The Newton-Raphson method is based on the Taylor series expansion and can handle large power systems effectively for load flow analysis. The method involves calculating the Jacobian matrix, power mismatch vector, and applying corrections to the bus voltages iteratively until the minimization of the power mismatches. The advantages of the Newton-Raphson methos are listed below – 

•	The quadratic convergence in the Newton-Raphson method makes it faster than the Gauss-Seidel method. 

•	Due to the faster convergence of the Newton-Raphson method, it solves large and complex power systems effectively.  

Here’s the general procedure for a Newton -Raphson method: 


1.	Formulation of power flow equations: The power flow equations are non-linear equations relating bus voltages the power injections at each bus. For a bus i, the real and reactive power injections are given by - 

$$
\begin{aligned}
    P_i=\sum_{j=1}^{N}\left|V_i\right|\left|V_j\right|\left|Y_{ij}\right|\cos{\left(\theta_{ij}+\delta_j-\delta_i\right)}
\end{aligned}
$$

$$
\begin{aligned}
Q_i=-\sum_{j=1}^{N}\left|V_i\right|\left|V_j\right|\left|Y_{ij}\right|\sin{\left(\theta_{ij}+\delta_j-\delta_i\right)}
\end{aligned}
$$

2.	Initial guess: For a N-bus power system, assume bus 1 as the slack bus, bus 2 to bus M are the PV buses, and bus M+1 to N are the PQ buses. An initial guess is made for the voltage magnitudes and angles for all buses except the slack bus. For PV buses, voltage magnitude is fixed, and angle is guessed. The unknown quantities are 2N-M-1. 

3.	Mismatch vector: Applying Taylor series expansion (till first order) for the real and reactive power flow equations – 

$$
\begin{equation}
P_i^{spec}=P_i\left(\delta_i^{\left(0\right)},{V_i}^{\left(0\right)}\right)+\frac{\partial P_i}{\partial\delta_2}\Deltaδ_2+\frac{\partial P_i}{\partial\delta_3}\Deltaδ_3+ ... +\frac{\partial P_i}{\partial V_{M+1}}\Delta V_{M+1}+\frac{\partial P_i}{\partial V_{M+2}}\Delta V_{M+2}+ ... +\frac{\partial P_i}{\partial V_{N}}\Delta V_{N} ,    \forall i=2,...N.
\end{equation}
$$

$$
\begin{equation}
Q_i^{spec}=Q_i\left(\delta_i^{\left(0\right)},{V_i}^{\left(0\right)}\right)+\frac{\partial Q_i}{\partial\delta_2}\Deltaδ_2+\frac{\partial Q_i}{\partial\delta_3}\Deltaδ_3+ ... +\frac{\partial Q_i}{\partial V_{M+1}}\Delta V_{M+1}+\frac{\partial Q_i}{\partial V_{M+2}}\Delta V_{M+2}+ ... +\frac{\partial Q_i}{\partial V_{N}}\Delta V_{N} ,    \forall i=2,...N.
\end{equation}
$$

Rearranging the terms and writing them into matrix form, then - 

$$
\begin{equation}
\left[\begin{array}{cccccccc}
\frac{\partial P_2}{\partial \delta_2} & \frac{\partial P_2}{\partial \delta_3} & \cdots & \frac{\partial P_2}{\partial \delta_N} & \frac{\partial P_2}{\partial V_{M+1}} & \frac{\partial P_2}{\partial V_{M+2}} & \cdots & \frac{\partial P_2}{\partial V_N} \\
\frac{\partial P_3}{\partial \delta_2} & \frac{\partial P_3}{\partial \delta_3} & \cdots & \frac{\partial P_3}{\partial \delta_N} & \frac{\partial P_3}{\partial V_{M+1}} & \frac{\partial P_3}{\partial V_{M+2}} & \cdots & \frac{\partial P_3}{\partial V_N} \\
\vdots & \vdots & & & \vdots & & & \vdots \\
\frac{\partial P_N}{\partial \delta_2} & \frac{\partial P_N}{\partial \delta_3} & & \frac{\partial P_N}{\partial \delta_N} & \frac{\partial P_N}{\partial V_{M+1}} & \frac{\partial P_N}{\partial V_{M+2}} & & \frac{\partial P_N}{\partial V_N} \\
\frac{\partial Q_{M+1}}{\partial \delta_2} & \frac{\partial Q_{M+1}}{\partial \delta_3} & \cdots & \frac{\partial Q_{M+1}}{\partial \delta_N} & \frac{\partial Q_{M+1}}{\partial V_{M+1}} & \frac{\partial Q_{M+1}}{\partial V_{M+1}} & \cdots & \frac{\partial Q_{M+1}}{\partial V_{M+1}} \\
\vdots & \vdots & & & \vdots & & \vdots \\
\frac{\partial Q_N}{\partial \delta_2} & \frac{\partial Q_N}{\partial \delta_3} & \cdots & \frac{\partial Q_N}{\partial \delta_N} & \frac{\partial Q_N}{\partial V_{M+1}} & \frac{\partial Q_N}{\partial V_{M+2}} & \cdots & \frac{\partial Q_N}{\partial V_N}
\end{array}\right]\left[\begin{array}{c}
\Delta \delta_2 \\
\Delta \delta_3 \\
\vdots \\
\Delta \delta_N \\
\Delta V_{M+1} \\
\Delta V_{M+2} \\
\vdots \\
\Delta V_N
\end{array}\right]=\left[\begin{array}{c}
P_2^{\text {spec }}-P_2^{\text {cal }} \\
P_3^{\text {spec }}-P_3^{\text {cal }} \\
\vdots \\
P_N^{\text {spec }}-P_N^{\text {cal }} \\
Q_{M+1}^{\text {spec }}-Q_{M+1}^{\text {cal }} \\
Q_{M+2}^{\text {spec }}-Q_{M+2}^{\text {cal }} \\
\vdots \\
Q_N^{\text {spec }}-Q_N^{\text {cal }}
\end{array}\right]
\end{equation}
$$

Equation (7) is represented by – 



$$
\begin{equation}
\left[\begin{array}{ll}
J_1 & J_2 \\
J_3 & J_4
\end{array}\right]\left[\begin{array}{l}
\Delta \delta \\
\Delta V
\end{array}\right]=\left[\begin{array}{l}
\Delta P \\
\Delta Q
\end{array}\right]
\end{equation}
$$

Where,
$$

\begin{aligned}
&\begin{aligned}
& \left\lvert\,[\delta]=\left[\begin{array}{lll}
\delta_2 & \delta_3 \cdots \delta_N
\end{array}\right]\right., \quad[V]=\left[\begin{array}{lll}
V_{M+1} & V_{M+2} \cdots V_N
\end{array}\right], \quad[P]=\left[\begin{array}{lll}
P_2 & P_3 \cdots P_N
\end{array}\right], \quad \text { and } \quad[Q]= \\
& {\left[\begin{array}{ll}
Q_{M+1} & Q_{M+2} \cdots Q_N
\end{array}\right] .[\Delta \delta]=\left[\begin{array}{ll}
\Delta \delta_2 & \Delta \delta_3 \cdots \Delta \delta_N
\end{array}\right]_{(N-1) \times 1}^T,[\Delta V]=\left[\Delta V_{M+1} \quad \Delta V_{M+2} \cdots \Delta V_N\right]_{(N-M) \times 1}^T,} \\
& {[\Delta P]=\left[\begin{array}{ll}
\Delta P_2 & \Delta P_3 \cdots \Delta P_N
\end{array}\right]_{(N-1) \times 1}^T \text {, and }[\Delta Q]=\left[\begin{array}{ll}
\Delta Q_{M+1} & \Delta Q_{M+2} \cdots \Delta Q_N
\end{array}\right]_{(N-M) \times 1}^T .}
\end{aligned}\\
&\left\lvert\,[\delta]=\left[\begin{array}{lll}
\delta_2 & \delta_3 \cdots \delta_N
\end{array}\right]\right., \quad[V]=\left[\begin{array}{lll}
V_{M+1} & V_{M+2} \cdots V_N
\end{array}\right], \quad[P]=\left[\begin{array}{lll}
P_2 & P_3 \cdots P_N
\end{array}\right], \quad \text { and } \quad[Q]=\\
&\left[\begin{array}{ll}
Q_{M+1} & Q_{M+2} \cdots Q_N
\end{array}\right] .[\Delta \delta]=\left[\begin{array}{ll}
\Delta \delta_2 & \Delta \delta_3 \cdots \Delta \delta_N
\end{array}\right]_{(N-1) \times 1}^T,[\Delta V]=\left[\Delta V_{M+1} \quad \Delta V_{M+2} \cdots \Delta V_N\right]_{(N-M) \times 1}^T,\\
&[\Delta P]=\left[\begin{array}{ll}
\Delta P_2 & \Delta P_3 \cdots \Delta P_N
\end{array}\right]_{(N-1) \times 1}^T \text {, and }[\Delta Q]=\left[\begin{array}{ll}
\Delta Q_{M+1} & \Delta Q_{M+2} \cdots \Delta Q_N
\end{array}\right]_{(N-M) \times 1}^T .\\
&\text { Where, } \quad\left[J_1\right]=\left[\frac{\partial P}{\partial \delta}\right]_{(N-1) \times(N-1)}, \quad\left[J_2\right]=\left[\frac{\partial P}{\partial V}\right]_{(N-1) \times(N-M)}, \quad\left[J_3\right]=\left[\frac{\partial Q}{\partial \delta}\right]_{(N-M) \times(N-1)}, \quad \text { and } \quad\left[J_4\right]=\\
&\left[\frac{\partial Q}{\partial V}\right]_{(N-M) \times(N-M)}
\end{aligned}
$$

Equation (8) is further simplified as given below – 

$$
\begin{equation}
[J][\Delta X]=[mis]
\end{equation}
$$

Where, $\Delta X=\left[\begin{array}{l}
\Delta \delta \\
\Delta V
\end{array}\right]$  is the correction vector and $mis=\left[\begin{array}{l}
\Delta P \\
\Delta Q
\end{array}\right]$ is the real and reactive power mismatch vector. $\Delta P$ and $\Delta Q$ for each bus are calculated (expect slack bus). The power mismatch vector for real and reactive power is given below, where $P^{spec} / Q^{spec}$ are the scheduled active and reactive power and $P^{cal}/Q^{cal}$ are the calculated active and reactive power at a bus $i$ substituting the initial guess.

$$
\begin{aligned}
[mis]=\left[\begin{array}{l}
\Delta P \\
\Delta Q
\end{array}\right]=\left[\begin{array}{l}
P^{\text {spec }}-P^{c a l} \\
Q^{s p e c}-Q^{c a l}
\end{array}\right]
\end{aligned}
$$

To perform the power flow analysis, an initial guess for the bus voltage magnitude is required. For the normal steady-state operating conditions, the bus voltage magnitudes maintain between 0.95 – 1.05 p.u. Therefore, all the unknown bus voltages are initialized at $1\angle 0^{0}$ p.u., also called as a flat start. Let us assume that the first bus is a slack bus and from bus 2 to ‘M’ bus are the PV buses for a $‘N’$ bus system. The remaining $‘N-M-1’$ buses are the PQ buses. The complete procedure for Newton-Raphson power flow is as given below - 



 <b>Step 1:</b> Initialize $V_{j}^{(0)}=V_j^{(spec)}\angle0°$ for $j=2,3,\ldots,m$ and $V_j^{(0)}=1\angle0°$ for $j=\left(m+1\right),\ \left(m+2\right),\ldots,n$. Set iteration count $k=1$. 

 <b>Step 2:</b> Perform the following operations for $i=2,3,\ldots,m$.

<br>&nbsp;&nbsp;&nbsp;&nbsp;(a)  Calculate:
$$
\begin{equation}
Q_i^{(k)}=-\sum_{j=1}^{n}\left|V_i^{(k-1)}\right|\left|V_j^{(k-1)}\right|\left|Y_{ij}\right|\sin{\left(\theta_{ij}\ {+\ \delta}_j^{(k-1)}-\delta_i^{(k-1)}\right)} 
\end{equation}
$$

<br>&nbsp;&nbsp;&nbsp; (b) If $Q_i^{min}<Q_i^{(k)}<Q_i^{max}$, then assign $|V_i^{(k)}|$ = $V_i^{(spec)}$ and the $i^{th}$ bus id retained as PV bus for $k^{th}$ iteration. 

<br>&nbsp;&nbsp;&nbsp; (c) If $Q_i^{(k)}>Q_i^{max}$, then set $Q_i^{spec}=Q_i^{max}$, and if $Q_i^{(k)}<Q_i^{min}$, then set $Q_i^{spec}=Q_i^{min}$. In both scenarios, the respective bus is converted to PQ bus. The voltage of the respective bus becomes an unknown for the current iteration. It introduces extra unknown quantities and to evaluate this, an extra equation is needed, which is obtained from (10). Therefore, when the $i^{th}$ bus is converted to the PQ bus, the dimensions of both $\Delta P$ and $\Delta Q$ are increased by one. 

<br><b>Step 3:<b>  Compute the vectors $P^{cal
}$ and $Q^{cal}$ with the vectors $\delta^{(k-1)}$ and $V^{(k-1)}$ to form the mismatch vector $\Delta M$.

<br><b>Step 4:<b> Compute $error=max (|\Delta M|)$.

<br><b>Step 5:<b> If $error  \leq \epsilon \hspace{0.25cm}   (pre- spec. \hspace{0.25cm} tolerance)$, then the final vectors are $\delta{(k-1)}$ and $V^{(k-1)}$ and print the results. Otherwise go to step 6. 

<br><b>Step 6:<b> Evaluate the Jacobian matrix with the vectors $\delta^{(k-1)}$ and $V^{(k-1)}$.

<br><b>Step 7:<b> Evaluate the correction vector $\Delta X$ by solving equation (9).

<br><b>Step 8:<b> Update the solution vectors $\delta^{(k)}=\delta^{(k-1)}+\Delta \delta$ and $V^{(k)}=V^{(k-1)}+ \Delta V$. Update $k=k+1$ and go back to step 2. 