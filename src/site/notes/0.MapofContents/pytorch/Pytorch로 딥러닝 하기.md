---
{"dg-publish":true,"permalink":"/0-mapof-contents/pytorch/pytorch/","created":"2024-12-07T23:47:47.884+09:00","updated":"2024-12-08T02:46:47.558+09:00"}
---


딥러닝은 다음과 같은 단계로 이루어진다.
우선 문제를 정의하고, 그 문제에 맞는 데이터를 모델에 넣어준다.
그렇다면 모델이 이 데이터에 맞는 가중치 및 편차를 구해준다. (이를 보통 학습이라 부른다.)
이 가중치 및 편차를 사용해 내가 실제로 만나는 데이터들을 분류 / 회귀한다. (이를 테스트라고 부른다.)
우리는 이 모델을 기본 수치 연산을 사용해서 만들 수도 있겠지만, 잘 만들어진 Framework를 이용하는 것이 훨씬 간편할 것이다.
그러므로 현재 현업에서 많이 사용하고 있는 PyTorch가 어떻게 딥러닝 워크플로를 구상하고, 이를 실행시키는 지를 알아보자.

# 데이터 작업하기

우선 PyTorch로 만든 모델에 데이터를 넣어주려면(input) 이를 PyTorch의 모델이 이해할 수 있는 데이터의 형태로 넣어주어야 할 것이다.
따라서, 우리는 PyTorch에서 쓰이는 텐서에 대해 알아보고, 이들의 집합 (혹은 데이터 객체)을 담는 DataLoader와 Dataset을 알아볼 것이다.

## 텐서(Tensor)

- 텐서(tensor)는 배열(array)이나 행렬(matrix)과 매우 유사한 자료구조입니다.
- 텐서는 GPU나 다른 하드웨어 가속기에서 실행할 수 있다는 점만 제외하면 [NumPy](https://numpy.org/) 의 ndarray와 유사합니다.
- 텐서와 NumPy 배열(array)은 종종 동일한 내부(underly) 메모리를 공유할 수 있어 데이터를 복사할 필요가 없습니다. ([NumPy 변환(Bridge)](https://tutorials.pytorch.kr/beginner/blitz/tensor_tutorial.html#bridge-to-np-label) 참고)
- 텐서는 또한 ([Autograd](https://tutorials.pytorch.kr/beginner/basics/autogradqs_tutorial.html) 장에서 살펴볼) 자동 미분(automatic differentiation)에 최적화되어 있습니다.

### 텐서(tensor) 만들기

**데이터로부터 직접(directly) 생성하기**
데이터로부터 직접 텐서를 생성할 수 있습니다. 데이터의 자료형(data type)은 자동으로 유추합니다.

```python
data = [[1, 2],[3, 4]]
x_data = torch.tensor(data)
```

**NumPy 배열로부터 생성하기**
텐서는 NumPy 배열로 생성할 수 있고, 그 반대도 가능합니다.
- ``torch.from_numpy(ndarray)`` : numpy array (ndarray로 정의됨)을 Tensor로 변경합니다.
- ``Tensor.numpy()`` : Tensor 객체를 numpy array로 변경합니다.

```
np_array = np.array(data)
torch_np = torch.from_numpy(np_array)
np_array = torch_np.numpy()
```

**다른 텐서로부터 생성하기**
명시적으로 다시 정의(override)하지 않는다면, 인자로 주어진 텐서의 속성(모양(shape), 자료형(datatype))을 유지합니다.

- ``torch.zeros_like(input)``: 텐서(input)의 속성을 유지하며 0으로 채웁니다.
- ``torch.ones_like(input)`` : 텐서(input)의 속성을 유지하며 1로 채웁니다.

```python
# 모양과 자료형이 유지됨
x_zeros = torch.zeros_like(x_data)
print(f"Zeros Tensor: \n {x_zeros} \n")
x_ones = torch.ones_like(x_data)
print(f"Random Tensor: \n {x_ones} \n")
```

밑의 예제는, 텐서(input)의 자료형이 실수(float type)여야만 동작합니다.
따라서 텐서의 자료형을 다시 정의하여 텐서의 속성을 바꿔주는 것을 보여줍니다.
- ``torch.rand_like(input)``: 텐서(input)의 속성을 유지하며 연속 균등 분포 난수로 변환
- ``torch.randn_like(input)``: 텐서(input)의 속성을 유지하며 표준 정규 분포 난수로 변환
```
# 자료형(dtype) 변경
x_rand = torch.rand_like(x_data, dtype = float)
x_randn = torch.randn_like(x_data, dtype = float)
```


> [!tip]
> 텐서의 자료형이 정수형이라면,
> ``RuntimeError: "check_uniform_bounds" not implemented for 'Long'``
>  에러가 발생합니다.

**출력 텐서의 차원으로 생성하기**
- `shape` 은 텐서의 차원(dimension)을 나타내는 튜플(tuple)로, 아래 함수들에서는 출력 텐서의 차원을 결정합니다.
```python
shape = (2,3,)
rand_tensor = torch.rand(shape)
randn_tensor = torch.randn(shape)
ones_tensor = torch.ones(shape)
zeros_tensor = torch.zeros(shape)
```

### 텐서의 속성

텐서의 속성은 텐서의 모양, 자료형 및 어느 장치에 저장 되는 지를 나타냅니다.
- ``Tensor.shape`` : 텐서의 모양을 보여줍니다.
- ``Tensor.dtype`` : 텐서의 자료형을 보여줍니다.
- ``Tensor.device`` : 텐서가 어느 장치에 저장 되는 지를 알려줍니다.

```python
tensor = torch.rand(3,4)
print(f"Shape of tensor: {tensor.shape}")
print(f"Datatype of tensor: {tensor.dtype}")
print(f"Device tensor is stored on: {tensor.device}")
```

### 텐서 연산

텐서 연산들을 [여기](https://pytorch.org/docs/stable/torch.html) 에서 확인할 수 있습니다.
**NumPy식의 표준 인덱싱과 슬라이싱:**
- 인덱싱(indexing)
    - ``tensor[i]`` :  tensor의 i번째 값을 가져옵니다.
    - 음수 인덱싱이 가능하다.
- 슬라이싱(slicing)
    - `tensor[i:j:k]` : i번째의 값부터 j번째의 값까지 k만큼 건너뛰며 가져옵니다.

```python
tensor = torch.ones(4, 4)
print(f"First row: {tensor[0]}")
print(f"First column: {tensor[:, 0]}")
print(f"Last column: {tensor[..., -1]}")
tensor[:,1] = 0
print(tensor)
```

> [!tip]
인덱싱, 슬라이싱 관련 참고 : https://numpy.org/doc/stable/user/basics.indexing.html

**텐서 모양 변경**
- `Tensor.view(shape)` : Tensor의 shape (shape)로 Tensor를 만들어 줍니다.
- ``Tensor.reshape(shape)`` :  Tensor의 shape (shape)로 Tensor를 만들어 줍니다.

>[!Note] view와 reshape의 차이
>view() 메서드는 메모리가 연속적인 경우만 사용이 가능하다.
>따라서 Tensor.is_contiguous() 메서드로 메모리가 연속적인지 비연속적인지를 확인해주어야 한다.
>또한 Tensor.contiguous() 메서드를 쓰면, Tensor의 메모리가 연속적으로 바뀐다.
>
>reshape() 메모리는 연속적이지 않아도 사용이 가능하다.
>다만 성능 저하의 단점이 있다.

- ``Tensor.transpose(dim1, dim2)`` : Tensor의 dim1 차원과 dim2 차원의 축을 바꿉니다.


>[!Note] reshape와 transpose의 차이
>reshape는 두 차원의 축을 변경하지 않지만, transpose는 두 차원의 축을 변경한다.
>

- ``torch.flatten()`` : Tensor를 평탄화합니다.
    - `torch.flatten()`

```python
tensor = torch.ones(5,5)
view_tensor = tensor.view(-1,1)
k = tensor[:,:3]
view_tensor.is_contiguous()
reshape_tensor = tensor.reshape(-1,1)
```

**텐서의 차원 확장**

- ``torch.unsqueeze(tensor, dim=n)``: n차원인 차원을 1만큼 확장해줍니다.

**텐서의 차원 축소**

- ``torch.squeeze(tensor)``: 차원이 1인 차원을 모두 축소합니다.
    - `dim`옵션을 주게 되면 그 차원이 1인 경우 그 차원만 축소합니다.
      
**텐서 합치기** 

- `torch.cat(tensors)` 

    - 주어진 차원에 따라 일련의 텐서를 연결할 수 있습니다. 
    - 합쳐주려는 차원의 크기가 같아야 한다. (브로드캐스팅 x)

- ``torch.stack(tensors)``

    - 새로운 차원에 텐서를 연결할 수 있습니다.
    - 모든 텐서의 차원이 같아야 합니다.

```python
tensor = torch.ones(4, 4)
t1 = torch.cat([tensor, tensor, tensor], dim=1)
print(t1)
```


>[!tip]
> dim 옵션을 주면 그 차원을 기준으로 연결할 수 있다.
> dim 옵션을 주지 않는다면, 자동으로 dim = 0으로 계산한다.

**산술 연산(Arithmetic operations)**

- `torch.add(a,b)` : a Tensor와 b Tensor를 더하는 연산
- `torch.sub(a,b)` : a Tensor에서 b Tensor를 빼는 연산
- `torch.mul(a,b)` : a Tensor와 b Tensor를 요소별로 곱하는 연산

    - `a * b` 도 같은 연산을 수행한다.

- `torch.div(a,b)` : a Tensor와 b Tensor를 요소별로 나누는 연산
- `torch.pow(a,n)` : a Tensor를 n 거듭제곱 하는 연산

    -  `n` 대신 `1/n` 하면 제곱근이 된다.

- `A @ B` :  A tensor와 B tensor를 행렬 곱하는 연산

```python
a = torch.tensor([[1,2],[3,4]])
b = torch.tensor([[5,6],[7,8]])
print(f"Tensor Add:{torch.add(a,b)}")
print(f"Tensor Sub:{torch.sub(a,b)}")
print(f"Tensor Mul:{torch.mul(a,b)}")
print(f"Tensor Div:{torch.div(a,b)}")
print(f"Tensor Pow 2:{torch.pow(a,2)}")
print(f"Tensor Add:{a@b}")
```

**바꿔치기(in-place) 연산** 

연산 결과를 피연산자(operand)에 저장하는 연산을 바꿔치기 연산(in-place)이라고 부르며, `_` 접미사를 갖습니다.

예를 들어: `x.copy_(y)` 나 `x.t_()` 는 `x` 를 변경합니다.

```python
print(f"{tensor} \n")
tensor.add_(5)
print(tensor)
```

**비교 연산**

반드시 두 텐서의 모양이 같아야 합니다.
함수의 리턴 값은 입력 텐서와 같은 형태의 boolean 텐서로 나옵니다.

- `torch.eq(a,b)` : 대응 원소들이 같은지 비교
- `torch.ne(a,b)` : 대응 원소들이 다른지 비교
- `torch.gt(a,b)` : 대응 원소들이 큰지 비교
- `torch.ge(a,b)` : 대응 원소들이 크고 같은지 비교
- `torch.lt(a,b)` : 대응 원소들이 작은지 비교
- `torch.le(a,b)` : 대응 원소들이 작고 같은지 비교

**논리 연산**

- `torch.logical_and(a,b)` : 논리곱(AND) 연산
- `torch.logical_or(a,b)` : 논리합(OR) 연산
- `torch.logical_xor(a,b)` : 배타적 논리합(XOR) 연산

> [!note]
> 바꿔치기 연산은 메모리를 일부 절약하지만, 기록(history)이 즉시 삭제되어 도함수(derivative) 계산에 문제가 발생할 수 있습니다. 따라서, 사용을 권장하지 않습니다.

