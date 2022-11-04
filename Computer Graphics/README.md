# Computer Graphics

## Geometry

## Rotation
오일러(Euler)은 축들을 순서대로 독립적으로 계산하기 때문에 겹쳐지는 짐벌락 현상이 발생함.

__짐벌락(Gimbal Lock)__
두 회전 축이 같은 평면으로 겹치는 현상. 축이 한번 겹치면 이후 회전에 영향을 줄 수 있다.

### Quaternion(쿼터니언)
* 각 축을 한꺼번에 계산하기에 짐벌락 문제가 생기지 않음
* (x, y, z, w)로 이루어짐
* 180보다 큰 값은 표현할 수 없음.
* float 4개만으로 연산을 하기 때문에 4*4의 회전 행렬보다 연산 수가 적어 속도가 빠르다.

_Interpolation_
두 점 사이의 어떤 점의 위치를 알아내기 위한 것.

1. LERP(Linear intERPolation) : 두 점을 지나는 직선 위에서의 점

2. SLERP(Spherical Linear intERPolation) : 두 점을 지나는 원 위에서의 점
