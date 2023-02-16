---
title: Midterm 1 - CPSC 204
---
## Midterm Overview

## Single Server Process
``` mermaid
flowchart LR
    input --> buffer
    subgraph process
    buffer --> server
    end 
    server --> fin
```

### Assumptions
- FIFO queue
- process is stable: all inputs are processed with no waste
    - $\lambda < \mu$ or $\rho < 1$ (utilization < 100%)
    - why can't the assumption be $=$?
- units arrive independently of one another
- no limit on queue length

### Randomness
- $A_i$: Inter-arrival time, how long before the next person comes. It tells you the difference between you and the next person.
    - If you show up at 2:00PM and the next person at 5:00PM, then $A$ would be 3 hours.
- $S_i$: Service time, how long does the person need to get through the server?

### Variables
- $\lambda$: average arrival/input rate (units/time)
    - $\dfrac{1}{\lambda}$: average customer inter-arrival rate
- $\mu$: average capacity rate of the server (units/time)
    - $\dfrac{1}{\mu}$: average processing time
- $\rho = \dfrac{\lambda}{\mu}$: server utilization
- $T = T_q + T_s$: average flow time
    - $T_q$: average waiting time (time in queue)
    - $T_s$: average processing time (by server)
- $I = I_q + I_s$: inventory in process
    - $I_q$: number of people in line
    - $I_s$: number of people being processed

### Formulas
- $I = \lambda T$, $I_q = \lambda T_q$, $I_s = \lambda T_s$
- $T_s = \dfrac{1}{\mu}$
- $I_s = \dfrac{\lambda}{\mu}$

### Queue
What might impact $I_q$?

- $\mu$ since fast service can move the queue faster
- $\lambda$ since more people coming in can increase the line

### PK Formula[^1]
$$I_q \cong \dfrac{\rho^2}{1 - \rho} \cdot \dfrac{C^2_{A}+C^2_{S}}{2}$$

where 

- $C_A$: coefficient of variation of inter-arrival time
- $C_S$: coefficient of variation of service time

where $CV = \dfrac{Standard Deviation}{Mean}$ which can be written as $CV = \dfrac{\sigma[X]}{E[X]}$

NOTE: if exponontially distributed, we have $\sigma[X] = E[X]$

#### Modified Formula

$$I_q \cong \dfrac{\lambda^2}{\mu(\mu - \lambda)} \cdot \dfrac{C^2_{A}+C^2_{S}}{2}$$

where $\dfrac{C^2_{A}+C^2_{S}}{2}$ is the variance

## PK Shortcuts
### G/G/1 queue

$$
I_q \cong \dfrac{\rho^2}{1 - \rho} \cdot \dfrac{C^2_{A}+C^2_{S}}{2}
$$

- **G**: arrivals follow a general distribution
- **G**: service times follow a general distribution
- **1**: there is a single processor (server)

### M/M/1 queue
If we assume that $C_A$ and $C_S$ are for exponential distributions then

$\sigma[X] = E[X]$ and $C_A = C_S = 1$

$$
I_q \cong \dfrac{\rho^2}{1 - \rho}
$$

- **M**[^2]: arrivals follow a exponential distribution
- **M**[^2]: service times follow a exponential distribution
- **1**: there is a single processor (server)

### M/D/1 queue

- **M**[^2]: arrivals follow a exponential distribution
- **M**[^2]: service time is deterministic (standard deviation: $\sigma[X] = 0$)
- **1**: there is a single processor (server)

Where $C_A = 1$ and $C_S = 0$

$$
I_q \cong \dfrac{\rho^2}{1 - \rho} \cdot \dfrac{1}{2}
$$

[^1]: $\cong$ means congruence. In most cases it's an equation, but in special cases, there are numbers that make the formula messy. The full equation is more complex. 
[^2]: M is short for Markovian, or exponentially distributed
