```mermaid
flowchart
subgraph covariates
x0[x_0]
x1[x_1]
x2[x_2]
xi[x_i]
end
x0-->y
x1-->y
x2-->y
xi-->y
y[Y]
t[T]
t-- TE, varies with x_1 -->y
x2-->t
```