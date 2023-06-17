# Operations Research. Linear Programming. 
## Modified Simplex Method (Inverse Matrix) 

[![Generic badge](https://img.shields.io/badge/GO_TO_JUPYTER_NoteBook_FILE-OPEN-blue?style=for-the-badge&logo=jupyter)](https://github.com/OlehOleinikov/oper_research_inv_matrix/blob/main/operres_inv_mtx_exam_consult.ipynb) 

## *F<sub>max</sub> = 5x₁ + 4x₂ + 6x₃*


### *x₁ + x₂ + x₃ ≤ 6*

### *2x₁ + x₂ + x₃ ≥ 9*

### *3x₁ + x₂ + 2x₃ ≥ 11*

### *x₁, x₂, x₃ ≥ 0*


```python
mtx_vars_origin = [[1, 1, 1],
                   [2, 1, 1],
                   [3, 1, 2]]

mtx_vars_balance = [[0, 0,],
                    [-1, 0,],
                    [0, -1]]

mtx_vars_base = [[1, 0, 0],
                 [0, 1, 0],
                 [0, 0, 1]] # ця змінна генерується автоматично

Cb = [0, -10000, -10000] # треба врахувати балансування

b = np.array([6, 9, 11])
C = np.array([5, 4, 6, 0, 0])

mtx_vars_all = np.concatenate((mtx_vars_origin, mtx_vars_balance), axis=1)
mtx_vars_all
```

| x<sub>1</sub> | x<sub>2</sub> | x<sub>3</sub> | x<sub>4</sub> | x<sub>5</sub> |
|---------------|---------------|---------------|---------------|---------------|
| 1             | 1             | 1             | 0             | 0             |
| 2             | 1             | 1             | -1            | 0             |
| 3             | 1             | 2             | 0             | -1            |


## Step example:

```python
step1 = LpInvMtxStep(A=mtx_vars_all,
                     C=C,
                     b=b,
                     Cb = Cb)
print(step1)
```
Iter #: 1

Values amount: 8


Matrix A:

|      |      |      |      |      |
|------|------|------|------|------|
| 1    | 1    | 1    | 0    | 0    |
| 2    | 1    | 1    | -1   | 0    |
| 3    | 1    | 2    | 0    | -1   |


C:

|  |  |  |  |  |
|--|--|--|--|--|
| 5 | 4 | 6 | 0 | 0 |


b:

|  |  |  |
|--|--|--|
| 6 | 9 | 11 |


B:

|  |  |  |
|--|-----|-----|
| 1.0 | 0.0 | 0.0 |
| 0.0 | 1.0 | 0.0 |
| 0.0 | 0.0 | 1.0 |


Basis index:

|   |   |   |
|---|---|---|
| 5 | 6 | 7 |



Basis varnames:

|               |               |               |
|---------------|---------------|---------------|
| x<sub>6</sub> | x<sub>7</sub> | x<sub>8</sub> |


B_inv:

|  |  |  |
|--|-----|-----|
| 1.0 | 0.0 | 0.0 |
| 0.0 | 1.0 | 0.0 |
| 0.0 | 0.0 | 1.0 |

CbB_inv:

|   |        |        |
|---|--------|--------|
| 0 | -10000 | -10000 |



BinvA:

|  |  |  |  |  |
|-----|-----|-----|-----|-----|
| 1.0 | 1.0 | 1.0 | 0.0 | 0.0 |
| 2.0 | 1.0 | 1.0 | -1.0| 0.0 |
| 3.0 | 1.0 | 2.0 | 0.0 | -1.0|


Binv_b:

|   |   |    |
|---|---|----|
| 6 | 9 | 11 |


CbB_inv_b:

|   |
|---|
| -200000 |


CbBinvA_minus_C:

|  |  |  |  |  |
|--|--|--|--|--|
| -50005 | -20004 | -30006 | 10000 | 10000 |


min_elem_order:

| x<sub>1</sub> |
|---------------|
| -50005.0       |



ratio:

|   |   |    |
|---|---|----|
| 6 | 4.5 | 3.66666667 |


| min ratio value (pos): |
|------------------------|
| 3.6666666666666665     |


leave: x<sub>8</sub>


New Cb:

|   |   |   |
|---|---|---|
| 0 | -10000 | 5 |



New B:

|      |      |      |
|------|------|------|
| 1.0  | 0.0  | 1.0  |
| 0.0  | 1.0  | 2.0  |
| 0.0  | 0.0  | 3.0  |


**Result: PLANE STILL CONSIST NEGATIVE VALUE(S)**

## Loop:

```python
res = False
initial_step = LpInvMtxStep(A=mtx_vars_all,
                            C=C,
                            b=b,
                            Cb = Cb)
next_step_data = initial_step.get_next_step()

while not res:
    step = LpInvMtxStep(**next_step_data)
    res = step.get_result()['stop_status']
    next_step_data = step.get_next_step()

print(step)
```

> Iter #: 4
> ...
>Result:
>
>ITERATING STOPPED: 
>
>BEST FUNC: 33.0 
>
>VARS: x<sub>3</sub> = 3.0; x<sub>5</sub> = 4.0; x<sub>1</sub> = 3.0


[![Generic badge](https://img.shields.io/badge/GO_TO_MINIMALIZATION_EXAMPLE_NoteBook_FILE-OPEN-blue?style=for-the-badge&logo=jupyter)](https://github.com/OlehOleinikov/oper_research_inv_matrix/blob/main/operres_inv_mtx_1128.ipynb) 
