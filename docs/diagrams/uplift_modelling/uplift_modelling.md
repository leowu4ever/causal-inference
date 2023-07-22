<!-- data generation process -->
```mermaid
%%{init: {'theme':'default'}}%%

flowchart
subgraph  
direction BT
x1((x1 \n income))
x2((x2 \n avg \n order))
x3((x3 \n yrs since \n registration))
y((y \n profit))
t((t \n promotion))
x1 --a--> y
t --TE--> y
x2 --b--> y
x3 --c--> y
end
```

<!-- slearner -->
```mermaid
%%{init: {'theme':'default'}}%%
flowchart
subgraph S_learners
direction LR
subgraph training
direction TB
subgraph all_samples
direction TB
X
t
y
end
input_train[input]
input_train-->m0
target_train[target]
X-->input_train
t-->input_train
y-->target_train
target_train-->m0
m0[(model M)]
end

subgraph predicting
direction TB
input[X]
model[(model M)]
t0[X, t=0]
t1[X, t=1]
t0-->model
t1-->model
input-->t0
input-->t1
y0[y0]
y1[y1]
model --predicts-->y0
model --predicts-->y1
cate[treatment effect/uplift = y1 - y0]

y0-->cate
y1-->cate
end
training --> predicting
end
```

<!-- tlearner -->
```mermaid
%%{init: {'theme':'default'}}%%
flowchart
subgraph T_learners
subgraph training
subgraph all_samples
X
t
y
end
subgraph control_samples,t=0
x_control[X]
y_control[y]
end

subgraph treated_samples,t=1
x_treatment[X]
y_treatment[y]

end
all_samples --> control_samples,t=0
all_samples --> treated_samples,t=1
m0[(control model M_0)]
m1[(treatment model M_1)]
input_train_control-->m0
input_train_treatment-->m1
target_train_control-->m0
target_train_treatment-->m1
input_train_control[input]
input_train_treatment[input]
target_train_control[target]
target_train_treatment[target]

x_control-->input_train_control
y_control-->target_train_control
x_treatment-->input_train_treatment
y_treatment-->target_train_treatment

end

subgraph predicting
input[X]
m0_pred[(control model M_0)]
m1_pred[(treatment model M_1)]
input --> m0_pred
input --> m1_pred
y0[y0]
y1[y1]
m0_pred --predicts-->y0
m1_pred --predicts-->y1
cate[treatment effect/uplift = y1 - y0]

y0-->cate
y1-->cate
end
training --> predicting
end
```

<!-- stage1 -->
```mermaid
%%{init: {'theme':'default'}}%%
flowchart
subgraph stage_1
direction TB
subgraph  
mt[(treatment model, M_t)]
my[(outcome model, M_y)]
t_pred[treatment predicted, t_pred]
y_pred[outcome predicted, y_pred]
t_res[treatment residual, t_res]
y_res[outcome residual, y_res]
mt-->t_pred
my-->y_pred
t_pred--t_true - t_pred-->t_res
y_pred--y_true - y_pred-->y_res
input[input]
target_y[target]
target_t[target]
target_y-->my
target_t-->mt
input-->mt
input-->my
end
feature[feature, x]
treatment --> target_t
feature --> input
treatment[treatment, t]
outcome[outcome, y]
outcome --> target_y
end
```

<!-- stage2 -->
```mermaid
%%{init: {'theme':'default'}}%%
flowchart
subgraph  
direction TB
subgraph stage_2
direction TB
t_res
y_res[y_res]
target
y_res-->target
t_res[t_res]
t_res-->input
input
feature[feature, x]
feature-->input
model[(CATE model, M_cate)]
input-->model
target-->model
cate[treatment effect/uplift for each sample]
model-- output -->cate
end
end
```
