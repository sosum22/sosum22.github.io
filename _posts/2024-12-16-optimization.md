{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 1. 변수 정의"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "- $x_{ij} \\in \\{0, 1\\} \\quad \\forall i, j \\in \\{1, 2, \\ldots, K\\}$\n",
    "- $y_{ij} \\in \\{0, 1\\} \\quad \\forall i, j \\in \\{1, 2, \\ldots, K\\}$\n",
    "- $z_{ij} \\in \\{0, 1\\} \\quad \\forall i, j \\in \\{1, 2, \\ldots, K\\}$ <br><br>\n",
    "- $rc_{ij} \\in \\{0, 1\\} \\quad \\forall i \\in \\{1, 2, \\ldots, K\\}, \\ j \\in \\{1, 2, \\ldots, n_c\\}$\n",
    "- $rb_{ij} \\in \\{0, 1\\} \\quad \\forall i \\in \\{1, 2, \\ldots, K\\}, \\ j \\in \\{1, 2, \\ldots, n_b\\}$\n",
    "- $rw_{ij} \\in \\{0, 1\\} \\quad \\forall i \\in \\{1, 2, \\ldots, K\\}, \\ j \\in \\{1, 2, \\ldots, n_w\\}$ <br><br>\n",
    "- $I_{c_j} \\in \\{0, 1\\} \\quad \\forall j \\in \\{1, 2, \\ldots, n_c\\}$\n",
    "- $I_{b_j} \\in \\{0, 1\\} \\quad \\forall j \\in \\{1, 2, \\ldots, n_b\\}$\n",
    "- $I_{w_j} \\in \\{0, 1\\} \\quad \\forall j \\in \\{1, 2, \\ldots, n_w\\}$ <br><br>\n",
    "- $T_{x_{i}} \\in \\mathbb{Z} \\quad \\forall i \\in \\{1, 2, \\ldots, K\\}$\n",
    "- $T_{y_{i}} \\in \\mathbb{Z} \\quad \\forall i \\in \\{1, 2, \\ldots, K\\}$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 2. 상수 정의"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "- $D_{x_{ij}}$: shop\\_seq $i$와 $j$ 간의 거리 $\\quad \\forall i, j \\in \\{1, 2, \\ldots, K\\}$\n",
    "- $D_{y_{ij}}$: dlv\\_seq $i$와 $j$ 간의 거리 $\\quad \\forall i, j \\in \\{1, 2, \\ldots, K\\}$\n",
    "- $D_{z_{ij}}$: shop\\_seq $i$와 dlv\\_seq $j$ 간의 거리 $\\quad \\forall i, j \\in \\{1, 2, \\ldots, K\\}$ <br><br>\n",
    "- $C_{\\text{capa}}$:Car capacity, $C_{\\text{n}}$:Car available_number, $C_{\\text{speed}}$:Car speed, $C_{\\text{fc}}$:Car fixed_cost, $C_{\\text{vc}}$:Car variable_cost, $C_{\\text{st}}$:Car service_time\n",
    "- $B_{\\text{capa}}$:Bike capacity, $B_{\\text{n}}$:Bike available_number, $B_{\\text{speed}}$:Bike speed, $B_{\\text{fc}}$:Bike fixed_cost, $B_{\\text{vc}}$:Bike variable_cost, $B_{\\text{st}}$:Car service_time\n",
    "- $W_{\\text{capa}}$:Walk capacity, $W_{\\text{n}}$:Walk available_number, $W_{\\text{speed}}$:Walk speed, $W_{\\text{fc}}$:Walk fixed_cost, $W_{\\text{vc}}$:Walk variable_cost, $W_{\\text{st}}$:Car service_time <br><br>\n",
    "- $BD_{rt_{j}}$: order $j$의 ready\\_time $\\quad \\forall j \\in \\{1, 2, \\ldots, K\\}$\n",
    "- $BD_{dl_{j}}$: order $j$의 deadline $\\quad \\forall j \\in \\{1, 2, \\ldots, K\\}$\n",
    "- $BD_{v_{j}}$: order $j$의 volume $\\quad \\forall j \\in \\{1, 2, \\ldots, K\\}$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 3. 제약식 정의"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "1. capacity 만족 <br><br>\n",
    "- $\\sum_{i=0}^{K-1} rc_{ij} \\cdot BD_{v_{i}} \\leq C_{\\text{capa}} \\quad \\forall j$\n",
    "- $\\sum_{i=0}^{K-1} rb_{ij} \\cdot BD_{v_{i}} \\leq B_{\\text{capa}} \\quad \\forall j$\n",
    "- $\\sum_{i=0}^{K-1} rw_{ij} \\cdot BD_{v_{i}} \\leq W_{\\text{capa}} \\quad \\forall j$ <br><br>\n",
    "2. 모든 bundle은 1개의 rider에 할당\n",
    "- $\\sum_{j=0}^{n_{c}-1} rc_{ij}+\\sum_{j=0}^{n_{b}-1} rb_{ij}+\\sum_{j=0}^{n_{w}-1} rw_{ij} = 1 \\quad \\forall i$ <br><br>\n",
    "3. 동일 군집은 동일 rider에 할당\n",
    "- $x_{il} + rc_{ij} \\leq 1 + rc_{lj} \\quad \\forall i \\neq l, \\forall j$\n",
    "- $x_{il} + rb_{ij} \\leq 1 + rb_{lj} \\quad \\forall i \\neq l, \\forall j$\n",
    "- $x_{il} + rw_{ij} \\leq 1 + rw_{lj} \\quad \\forall i \\neq l, \\forall j$ <br><br>\n",
    "4. 군집 개수는 총 사용하는 rider 수와 같음\n",
    "- $\\sum_{j=0}^{n_{c}-1} I_{c_{j}}+\\sum_{j=0}^{n_{b}-1} I_{b_{j}}+\\sum_{j=0}^{n_{w}-1} I_{w_{j}} = \\sum_{i=0}^{K-1} \\sum_{l=0}^{K-1} z_{il}$ <br><br>\n",
    "5. rider 배차 여부에 대한 지시 변수 정의\n",
    "- $I_{c_{j}} \\leq \\sum_{i=0}^{K-1} rc_{ij} \\leq K \\cdot I_{c_{j}} \\quad \\forall j$\n",
    "- $I_{b_{j}} \\leq \\sum_{i=0}^{K-1} rb_{ij} \\leq K \\cdot I_{b_{j}} \\quad \\forall j$\n",
    "- $I_{2_{j}} \\leq \\sum_{i=0}^{K-1} rw_{ij} \\leq K \\cdot I_{w_{j}} \\quad \\forall j$ <br><br>\n",
    "6. SS, EE 제자리 점프 불가\n",
    "- $x_{il} + y_{il} = 0  \\quad \\forall i =l $ <br><br>\n",
    "7. 양방향 점프 불가\n",
    "- $x_{il} + x_{li}  \\leq 1 \\quad \\forall i \\neq j$\n",
    "- $y_{il} + y_{li}  \\leq 1 \\quad \\forall i \\neq j$\n",
    "- $z_{il} + z_{li}  \\leq 1 \\quad \\forall i \\neq j$ <br><br>\n",
    "8. 한 곳으로만 점프 가능\n",
    "- $\\sum_{l=0}^{K-1} x_{il} \\leq 1, \\sum_{l=0}^{K-1} y_{il} \\leq 1, \\sum_{l=0}^{K-1} z_{il} \\leq 1 \\quad \\forall i$ <br><br>\n",
    "9. 한 곳에서만 점프해서 올 수 있음\n",
    "- $\\sum_{i=0}^{K-1} x_{il} \\leq 1, \\sum_{i=0}^{K-1} y_{il} \\leq 1, \\sum_{i=0}^{K-1} z_{il} \\leq 1 \\quad \\forall l$ <br><br>\n",
    "10. SE는 연결고리 불가능\n",
    "- $\\sum_{p=0}^{K-1} \\{z_{ip} + z_{pi}\\} - z_{ii} \\leq 1 \\quad \\forall i$ <br><br>\n",
    "11. SE 개수는 군집 개수와 같음\n",
    "- $K - \\sum_{i=0}^{K-1} \\sum_{l=0}^{K-1} x_{il} = \\sum_{i=0}^{K-1} \\sum_{l=0}^{K-1} z_{il}$\n",
    "- $K - \\sum_{i=0}^{K-1} \\sum_{l=0}^{K-1} y_{il} = \\sum_{i=0}^{K-1} \\sum_{l=0}^{K-1} z_{il}$ <br><br>\n",
    "12. 시작점 여부\n",
    "- $\\sum_{l=0}^{K-1} \\{z_{li} + y_{li} \\} = 1 \\quad \\forall i$ <br><br>\n",
    "13. 끝점 여부\n",
    "- $\\sum_{l=0}^{K-1} \\{z_{il} + x_{il} \\} = 1 \\quad \\forall i$ <br><br>\n",
    "14. SE가 다른 연결이면, 반드시 다른 군집 (s<->s)\n",
    "- $x_{il} + x_{li} \\leq 1 - \\frac{1}{2} \\sum_{p=0}^{K-1} (z_{ip} + z_{lp}) \\quad \\forall i \\neq l$\n",
    "- $y_{il} + y_{li} \\leq 1 - \\frac{1}{2} \\sum_{p=0}^{K-1} (z_{ip} + z_{lp}) \\quad \\forall i \\neq l$ <br><br>\n",
    "15. SE가 다른 연결이면, 반드시 다른 군집 (s<->e)\n",
    "- $x_{il} + x_{li} \\leq 1 - \\frac{1}{2} \\sum_{p=0}^{K-1} (z_{ip} + z_{pl}) \\quad \\forall i \\neq l$\n",
    "- $y_{il} + y_{li} \\leq 1 - \\frac{1}{2} \\sum_{p=0}^{K-1} (z_{ip} + z_{pl}) \\quad \\forall i \\neq l$ <br><br>\n",
    "16. SE가 다른 연결이면, 반드시 다른 군집 (e<->e)\n",
    "- $x_{il} + x_{li} \\leq 1 - \\frac{1}{2} \\sum_{p=0}^{K-1} (z_{pi} + z_{pl}) \\quad \\forall i \\neq l$\n",
    "- $y_{il} + y_{li} \\leq 1 - \\frac{1}{2} \\sum_{p=0}^{K-1} (z_{pi} + z_{pl}) \\quad \\forall i \\neq l$ <br><br>\n",
    "17. 다른 군집이면 SS, EE 불가능\n",
    "- $r_{c_{ij}} + r_{c_{lj}} = 1 \\implies x_{il} + x_{li} = 0 \\quad \\forall i \\neq l, \\forall j$ <br><br>\n",
    "18. SE에서 S_i -> E_(~i) 일 때, 다른 연결 고리 필요 (S_i로 갈 아이)\n",
    "- $z_{il} = 1 \\implies \\sum_{p=0}^{K-1} x_{pi} = 1 \\quad \\forall i \\neq j$ <br><br>\n",
    "19. SE에서 S_i -> E_(~i) 일 때, 다른 연결 고리 필요 (E_(~i)에서 갈 아이)\n",
    "- $z_{il} = 1 \\implies \\sum_{p=0}^{K-1} y_{lp} = 1 \\quad \\forall i \\neq j$ <br><br>\n",
    "20. 픽업 시간은 ready_time 이후\n",
    "- $T_{x_{i}} \\geq BD_{rt_{i}} \\quad \\forall i$ <br><br>\n",
    "21. 배달 시간은 deadline 이전\n",
    "- $T_{y_{i}} \\leq BD_{dl_{i}} \\quad \\forall i$ <br><br>\n",
    "22. 픽업 순서대로 픽업 시간 할당\n",
    "- $x_{il} = 1 \\land \\sum_{j=0}^{n_{c}-1} rc_{ij} = 1 \\implies T_{x_{l}} \\geq T_{x_{i}} + \\text{round}(D_{x_{il}}/C_{\\text{speed}} + C_{\\text{st}}) \\quad \\forall i \\neq l$\n",
    "- $x_{il} = 1 \\land \\sum_{j=0}^{n_{b}-1} rb_{ij} = 1 \\implies T_{x_{l}} \\geq T_{x_{i}} + \\text{round}(D_{x_{il}}/B_{\\text{speed}} + B_{\\text{st}}) \\quad \\forall i \\neq l$\n",
    "- $x_{il} = 1 \\land \\sum_{j=0}^{n_{w}-1} rw_{ij} = 1 \\implies T_{x_{l}} \\geq T_{x_{i}} + \\text{round}(D_{x_{il}}/W_{\\text{speed}} + W_{\\text{st}}) \\quad \\forall i \\neq l$ <br><br>\n",
    "23. 픽업 시간 이후에 배달 시간 할당\n",
    "- $z_{il} = 1 \\land \\sum_{j=0}^{n_{c}-1} rc_{ij} = 1 \\implies T_{y_{l}} \\geq T_{x_{i}} + \\text{round}(D_{z_{il}}/C_{\\text{speed}} + C_{\\text{st}}) \\quad \\forall i,l$\n",
    "- $z_{il} = 1 \\land \\sum_{j=0}^{n_{b}-1} rb_{ij} = 1 \\implies T_{y_{l}} \\geq T_{x_{i}} + \\text{round}(D_{z_{il}}/B_{\\text{speed}} + B_{\\text{st}}) \\quad \\forall i,l$\n",
    "- $z_{il} = 1 \\land \\sum_{j=0}^{n_{w}-1} rw_{ij} = 1 \\implies T_{y_{l}} \\geq T_{x_{i}} + \\text{round}(D_{z_{il}}/W_{\\text{speed}} + W_{\\text{st}}) \\quad \\forall i,l$ <br><br>\n",
    "24. 배달 순서대로 배달 시간 할당\n",
    "- $y_{il} = 1 \\land \\sum_{j=0}^{n_{c}-1} rc_{ij} = 1 \\implies T_{y_{l}} \\geq T_{y_{i}} + \\text{round}(D_{y_{il}}/C_{\\text{speed}} + C_{\\text{st}}) \\quad \\forall i \\neq l$\n",
    "- $y_{il} = 1 \\land \\sum_{j=0}^{n_{b}-1} rb_{ij} = 1 \\implies T_{y_{l}} \\geq T_{y_{i}} + \\text{round}(D_{y_{il}}/B_{\\text{speed}} + B_{\\text{st}}) \\quad \\forall i \\neq l$\n",
    "- $y_{il} = 1 \\land \\sum_{j=0}^{n_{w}-1} rw_{ij} = 1 \\implies T_{y_{l}} \\geq T_{y_{i}} + \\text{round}(D_{y_{il}}/W_{\\text{speed}} + W_{\\text{st}}) \\quad \\forall i \\neq l$ <br><br>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### 4. 목적 함수 정의"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "$\\begin{aligned}\n",
    "\\min \\quad & \\sum_{i=0}^{K-1} \\sum_{l=0}^{K-1} [x_{il} \\cdot D_{x_{il}}/100 \\cdot \\{\\sum_{j=0}^{n_{c}-1} rc_{ij} \\cdot C_{\\text{vc}} + \\sum_{j=0}^{n_{b}-1} rb_{ij} \\cdot B_{\\text{vc}} +\\sum_{j=0}^{n_{w}-1} rw_{ij} \\cdot W_{\\text{vc}}\\} \\notag \\\\\n",
    "           & + y_{il} \\cdot D_{y_{il}}/100 \\cdot \\{\\sum_{j=0}^{n_{c}-1} rc_{ij} \\cdot C_{\\text{vc}} + \\sum_{j=0}^{n_{b}-1} rb_{ij} \\cdot B_{\\text{vc}} +\\sum_{j=0}^{n_{w}-1} rw_{ij} \\cdot W_{\\text{vc}}\\} \\notag \\\\\n",
    "           & + z_{il} \\cdot D_{z_{il}}/100 \\cdot \\{\\sum_{j=0}^{n_{c}-1} rc_{ij} \\cdot C_{\\text{vc}} + \\sum_{j=0}^{n_{b}-1} rb_{ij} \\cdot B_{\\text{vc}} +\\sum_{j=0}^{n_{w}-1} rw_{ij} \\cdot W_{\\text{vc}}\\}] \\notag \\\\\n",
    "           & + \\sum_{j=0}^{n_{c}-1} \\cdot I_{c_j} \\cdot C_{\\text{fc}} + \\sum_{j=0}^{n_{b}-1} \\cdot I_{b_j} \\cdot B_{\\text{fc}} + \\sum_{j=0}^{n_{w}-1} \\cdot I_{w_j} \\cdot W_{\\text{fc}}\n",
    "\\end{aligned}\n",
    "$"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": []
  }
 ],
 "metadata": {
  "language_info": {
   "name": "python"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
