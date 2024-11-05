---
{"dg-publish":true,"permalink":"/3-permanents-notes/python/resource/","created":"2024-11-04T13:32:17.817+09:00","updated":"2024-11-04T13:47:19.691+09:00"}
---

## 궁금해 한 이유

데이터 증강을 적용한 딥 러닝 프로그램을 돌리다가 CUDA out of memory가 떴다.
nvidia-smi로 계산해보니 GPU는 거의 사용하지 않고, CUDA memory만 사용한 것을 확인할 수 있었다.
서버에서 CUDA memory와 일반 memory 는 거의 같은 용량을 할당하고 있었고, 그래서 이걸 조사했다.

## 내용

Python의 내장 모듈인 `resource`를 사용하여 메모리 사용량을 모니터링할 수도 있습니다. 이 방법은 주로 Unix 계열 운영체제에서 사용할 수 있습니다.

```python
import resource

def memory_usage():
    rusage = resource.getrusage(resource.RUSAGE_SELF)
    return rusage.ru_maxrss  # 최대 RSS 메모리 사용량 (KB)

if __name__ == "__main__":
    print(f'Initial memory usage: {memory_usage()} KB')
    # 실행할 코드
    large_list = [i for i in range(10**6)]  # 큰 리스트 생성
    print(f'Memory usage after allocation: {memory_usage()} KB')

```
