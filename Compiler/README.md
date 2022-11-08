# 컴파일러

고급언어 -> 컴파일러 -> 어셈블리어 -> 어셈블러 -> 기계어 -> 링커 -> 실행파일 -> 로더 -> 실행

## 자동 생성 함수
 - 기본 생성자
 - 기본 소멸자
 - 복사 생성자
 - 복사 대입 연산자

 ```cpp
 class Empty
{
  // 정의하지 않았을때 자동으로 생성되는 함수들
  // public, inline 함수로 생성된다.
  public: 
    Empty() {...}
    Empty(const Empty& rhs) {...}
    ~Empty() {...}

    Empty& operator = (const Empty& rhs) {...}
};
```

