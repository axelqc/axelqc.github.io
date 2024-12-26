---
title: "Using JIT to optimize scheduling"
collection: talks
type: "Poster"
permalink: /talks/curenavaca
venue: "Escuela Nacional de Optimización y Análisis Numérico 2023"
date: 2023-07-14
location: "Cuernavaca, Morelos"
---

Mathematical model using just-in-time and linear programming to solve a scheduling problem for an editorial, which maximizes the utility while penalizing late shipping. The model generates the production plan for the given demand in the established period.

Introduction
------
Editorials are crucial for students; when they encounter a problem, it’s as simple as grabbing a specialized book and reading through it to get the answer and the reasons behind it. Editorials for universities must meet deadlines aligned with the calendar to stay competitive and relevant. To accomplish this, they must establish a production plan dealing with delivery dates, inventory costs, dealing with different suppliers, and maximizing their utility. It may seem challenging to make this happen, but an optimal production can be created with a just-in-time methodology.

JIT is the methodology meant to deliver the exact amount of material (books, in this case) in the specified time. This can be achieved with linear programming, where a model is proposed to maximize utility and minimize cost while being flexible to adapt to the company’s needs.



Methodology
------
The proposed model minimizes production and inventory costs.
It was built with two variables:
- $$x_{ijk}$$ represents the quantity of books $$i$$ to produce in period $$j$$ by the supplier $$k$$.
- $$I_{ij}$$ is the quantity of books $$i$$ to inventory in period $$j$$.



And with four parameters:

- $$c_{ij}$$ is the cost to produce book $$i$$.
- $$h$$ is the cost of inventory per book per period.
- $$D_{ij}$$ is the demand from book $$i$$ in period $$j$$.
- $$M$$ represents the production capacity from suppliers.


The result is the following model:


$$ \begin{aligned} & \text{minimize} \quad z = \sum_i \sum_j \sum_k x_{ijk} c_{ij} + \sum_i \sum_j h I_{ij} \\ & \text{subject to} \\ & \quad \sum_k X_{ijk} + I_{ij-1} = D_{ij} + I_{ij}, \quad \forall i, j \\ & \quad \sum_i x_{ijk} \leq M, \quad \forall j, k \end{aligned} $$



The first restriction deals with the established demand on the established delivery date.


The second restriction deals with the maximum capacity of every supplier.

Results 
------
The model returns a calendar for each supplier with the required quantity of books in a given period. For example:
- For supplier 1


| Period | Dates                        | Books                           | Quantity                  |
|--------|------------------------------|---------------------------------|---------------------------|
| 5      | 2023-08-19 to 2023-09-19     | - Book 1<br>- Book 4           | - 138<br>- 249            |
| 6      | 2023-09-20 to 2023-10-24     | - Book 5                        | - 889                     |
| 7      | 2023-10-25 to 2023-11-28     | - Book 25<br>- Book 19<br>- Book 2 | - 112<br>- 807<br>- 52   |




- For supplier 2


| Period | Dates                        | Books                           | Quantity                  |
|--------|------------------------------|---------------------------------|---------------------------|
| 13      | 2024-05-23 to 2024-06-27     | - Book 2            | - 138<br>- 1088            |
| 14     | 2024-06-26 to 2024-08-01   | - Book 13                        | - 1500                     |



It also returns the costs of inventory and production. 

Conclusions
------
The problem was simplified by dealing with periods instead of days, from 60 thousand variables to less than 2 thousand, resulting in a fast model. If any changes are required, it only takes a re-adjustment of the parameters, which is all that is needed, and the model will generate another optimized production plan. Another point in favor is that the model can be applied to any company that relies on suppliers and schedules.

Pictures
------
![talking](https://axelqc.github.io/images/1688690362423.png)

![diploma](https://axelqc.github.io/images/1688690361919.png)



