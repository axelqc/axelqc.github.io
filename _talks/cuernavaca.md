---
title: "Using JIT to optimize scheduling"
collection: talks
type: "Poster"
permalink: /talks/curenavaca
venue: "Escuela Nacional de Optimización y Análisis Numérico 2023"
date: 2023-07-14
location: "Cuernavaca, Morelos"
---

Mathematical model using just-in-time and linear programming to solve a scheduling problem for an editorial which maximizes the utility while penalizing late shipping. The model generates the production plan for the given demand in the established period.

Introduction
------
Editorials are crucial for students, when they encounter a problem it’s as simple as grabbing a specialized book and reading through it to get the answer and the reasons behind it. Editorials for universities must meet deadlines aligned with the calendar to stay competitive and remain relevant. To accomplish this, they must establish a production plan dealing with delivery dates, inventory costs, dealing with different suppliers, and maximizing their utility. It may seem difficult to make this happen, but with just-in-time methodology, an optimal production can be created.

JIT is the methodology meant to deliver the exact amount of material (books in this case) in the specified time. This can be achieved with linear programming, where a model is proposed to maximize utility and minimize cost while being flexible to adapt to the company’s needs.



Methodology
------
The proposed model minimizes production and inventory costs.
It was built with two variables:
| Representation | Meaning | 
|----------|----------|
| $$x_{ijk}$$| Quantity of books $$i$$ to produce in period $$j$$ by the supplier $$k$$ |
| $$I_{ij}$$    | Quantity of books $$i$$ to inventory in period $$j$$ | 

And with four parameters:
| Representation | Meaning | 
|----------|----------|
| $$c_{ij}$$    | Cost to produce book $$i$$   |
| $$h$$    | Cost of inventory per book per period   | 
| $$D_{ij}$$    | Demand from book $$i$$ in period $$j$$   | 
| M    | Production capacity from suppliers| 


The result is the following model:
$$ \begin{aligned} & \text{minimize} \quad z = \sum_i \sum_j \sum_k x_{ijk} c_{ij} + \sum_i \sum_j h I_{ij} \\ & \text{subject to} \\ & \quad \sum_k X_{ijk} + I_{ij-1} = D_{ij} + I_{ij}, \quad \forall i, j \\ & \quad \sum_i x_{ijk} \leq M, \quad \forall j, k \end{aligned} $$

The first restriction is to satisfy the established demand on the established delivery date.
The second restriction deals with the maximum capacity of every supplier.

Results 
------

Conclusions
------

Pictures
------
![talking](https://axelqc.github.io/images/1688690362423.png)

![diploma](https://axelqc.github.io/images/1688690361919.png)



