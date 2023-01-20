# C++
참조: [C++ 이야기](https://wikidocs.net/book/2380)

* [About C++](#about-c)
* [ReferenceSheet](#reference-sheet)
* [Coding Style](#코딩-스타일)
* [자료형](#자료형)
* [유리수 자료형](#유리수-자료형-관련)
* [연산 overriding](#연산자-overriding)
* [범위와 수명](#범위와-수명scope-and-lifetime)
## About C++
C++는 자유롭기에 어떤 스타일이던 문제가 되지 않는다. 다만 거기에 대한 책임을 져야 한다.
* C++는 C의 많은 부분을 차지하기에 C 스타일 코딩을 해도 된다. 자료형만 명확히 해라
* C++는 객체지향 언어가 아니라 객체지향을 지원하는 언어이다. 절차지향으로 해도 된다.
* C의 표준 함수를 쓸 필요는 없다. 다만 C의 표준함수가 C++의 표준 함수보다 빠른 경우가 있다.
    * ex) prinf vs cout => ```endl```이 스트림 버퍼를 비우기에 그럴 목적이 아니라면 ```\n```이 낫다.
* ```goto```는 지양하라 많은 사람이 말하지만 책임있게 잘 쓰면 된다.

## Reference Sheet

### 헤더 파일 추가
```cpp
#include <headerfile> //library
#include "headerfile" //user's header
 ```

 ### 배열, 포인터, 참조자
 ```cpp
 int arr[5] = {1, 2, 3, 4, 5};
 int* p = arr;
 int& r = arr[0];
 ```

 ### I/O 연산자
 ```cpp
 fstream file;
 file.open("filename", <file mode constant>);
 
 //read using operator
 file >> var;
 file << var;

//getline
getline(file, line);

//reading and writing binary
file.read(memory_block, size);
file.write(memory_block, size);
file.close();

//File Mode Constants
ios::in //읽기
ios::out //쓰기
ios::ate //SEEK EOF
ios::app //EOF 이어서 쓰기
ios::trunc // 이전 내용 버리기
ios::nocreate // 파일 없으면 실패
ios::noreplace // 파일 있으면 시랲

//C-Style
FILE* pFile;
char buffer[100];
pFile = fopen("myfile.txt", "r");
if(pFile == NULL) perror("Error opening file");
else {
    while(!feof(pFile)){
        if(fgets(buffer, 100, pFile) == NULL) break;
        fputs(buffer, stdout);
    }
    fclose(pFile);
}
```
### 사용자 정의 자료형
```cpp
struct <structure_name>
{
    public:
        member_type member_name;
    protected:
    private:
};

class <class_name>{
    public:
        //method_prototypes
    protected:
        //method_prototypes
    private:
        //method_prototypes
        //data_attributes
}
```

### 객체
```cpp
myStruct m;
m.var1 = value;

myClass c;
var = c.method1(arg);

myStruct* pm = new myStruct;
pm -> var1 = value;

myClass* pc;
var = pc -> method1(arg);
```

## 코딩 스타일
참조: [구글 C++ 스타일](https://google.github.io/styleguide/cppguide.html)
참조: [기타 보편 스타일](https://wikidocs.net/47056)

* 변수, 함수는 앞에 자료형을 할 수 있게 함.
```cpp
int nIntegerNumber;
double dFloatingPointNumber;
int fnInitializeDataset(const std::string& strFileName);
```

* 함수 이름은 "'동사'를 '목적어'한다" 구조로 한다. 전역 일반 함수는 fn 첨자를 붙이고 class멤버함수는 fn 없이 소문자로한다.
```cpp
bool fnInitDataSet(const DataSetType& DataSet); //initialize data set

class DataSetType{
    public:
        bool initDataSet();
}
```

* class나 struct는 대문자로 시작하고 객체함수는 FO, 자료형 클래스의 경우 뒤에 Type를 붙인다. protected, private 클래스는 뒤에 _를 붙인다.

```cpp
class DataType{
    int nID;
    std::string strName;
}

struct DataSetType{
    int nGroupIndex;
    std::vector<DataType> vecData;
private:
    int nAccessCount_;
}

class FODeleteElement{
    public:
    void delete()(Object* obj);
}

```

## 자료형
### 기본 자료형
* bool(1 byte)
* char(1 byte) : ASCII만 가능
* wchar_t(4 byte) : UTF - 8 지원
* int(4 byte) - long (8 byte)
* double(8 byte)
* void

**변수 초기화**
정의된 자료형의 메모리 공간에 값을 설정하는 것.
초기화 할 때 자료형의 크기만큼 스택 영역에 만들어진다.

```cpp
int n = 0;
int n(0);
int n = {0};
int n{0};
```

### const, constexpr
__const__
객체를 불변으로 정의하는 예약어.
보통 `int const nNum` 처럼 쓴다. 물론 `const int nNum`도 괜찮긴하다.
```cpp
//정수 상수를 가르키는 p1
int const * p1 = new int; // == const int * p1 = new int
*p1 = 10; //error(pointer가 지정하는 값은 변경할 수는 없다.)
delete p1;
p1 = new int; //ok(pointer을 다른 주소로 지정할 수는 있다.)

//상수 포인터인 p2
int * const p2 = new int;
*p2 = 10; // ok(pointer가 지정하는 값은 변경할 수 있다.)
delete p2;
p2 = new int;//error(pointer를 다른 주소로 지정할 수 없다.)

//참조자 const는 정의시 초기화 해야 한다.
int const& ref = 10; // == const int& ref = 10;
```

__constexpr__
컴파일타임에 const 여부를 확인하게 하는 키워드. 상수를 지정하고 읽기 전용 메모리에 데이터를 배치할 수 있게 하기 위해 사용된다. 무조건 **리터럴** 값으로 넣어야 한다.
```cpp
const int num = 5;// 상수 정수형에 5 저장
constexpr int var = 10; // 상수표현식 정수형에 10저장

double fnStDev(const vector<double>&);
double fnMean(const vector<double>&);

vector<double> vecDbl{1.3, 2.4, 1., 2.7, 3.2, 1.8, 2.9};
const double stdev = fnStDev(vecDbl);//런타임에 fnStDev의 리턴값으로 상수 정의
constexpr double mean = fnMean(vecDbl); //에러, fnMean이 constexpr가 아니라 정의가 불가능.
```

```cpp
constexpr double square(double x) { return x*x; }

constexpr double area1 = 3.14*square(5);       // 3.14*square(5)는 상수표현식이다.
constexpr double area2 = 3.14*square(var);      // error: var가 변수라면 상수표현식이 아니다. 상수라면 가능하다.
const double area3 = 3.14*square(var);          // 런타임에 계산된 값을 상수로 정의한다.
```

### static, extern
`static`
    * 사용자 정의 자료형에서 사용하는 것
        : 객체가 아닌 사용자 정의 자료형에 속함.
        멤버변수는 객체에 속하지 않고 객체외부에 단 하나만 존재함.
    * 링킹 과정에서 내부, 외부 링크를 위한 용도
        : `static`을 붙이면 내부 링크 => 같은 파일에서만 사용할 수 있게 된다.

`extern`
    * `const`와 `typedef`는 기본적으로 내부링크로 처리 되는데, `extern`으로 외부링크로 전환한다(전역).
__변수의 선언과 정의__
정의: 자료평에 맞는 값을 저장할 수 있는 메모리 공간을 만드는 것.
선언: 컴파일러에게 변수 이름, 자료형, 초기값을 알림. 초기값을 없을 때 선언할 것을 알리기 위해 `extern`을 사용한다. 초기값이 있다면 `extern`을 할 필요는 없다.

```cpp
extern int Number;//int 타입의 Number가 어디서 정의 될것이라는 선언
int nCount; //int 타입 자료를 저장할 수 있는 nCount 메모리 공간을 정의
```


### 사용자 정의 자료형
```cpp
enum {eID_NUM, eID_NAME};
enum class COLOR{ RED, GREEN, BLUE};
class MyClassType{
    public:
    MyClass() = default;
    ~MyClass() = default;
    int nID_;
};

struct MyStructType{
    int nNumber;
    std::string strName;
};

MyClassType MyClass;
MyStructType MyStruct;
int nNumber = eID_NUM;
COLOR Color = COLOR::RED;
```

### typedef, using
자료형의 별칭을 만드는 과정
```cpp
typedef unsigned int uint;
using uchar = unsigned char;
uint n;
uchar uch;
```

### type casting
```cpp
//c style
int n = (double)1.5;
//cpp style
const_cast<new_type>(expression); //상수화 상태 일시 해제
static_cast<new_type>(expression); //기본 자료형 변환
reinterpret_cast<new_type>(expression); //포인터 자료형 변환
dynamic_cast<new_type>(expression); // 상속 구조에서 상위 혹은 하위 자료형으로 변환
```


### auto, decltype, typeinfo
컴파일 타임에 객체의 자료형을 결정하는 것.
`auto`: 초기화할 때 자료 결정
`decltype`: 존재하는 객체의 자료형으로 자료형 결정.
`<typeinfo>`의 `typeid`: 자료형의 id를 받는다.
```cpp
auto n = 10;
decltype(n) d = 3;
auto l1 {0}; //integer type
auto l2 = {0}; // list type
```

## 유리수 자료형 관련

|Float|Double|
|-|-|
|Single Precision|Double Precision|
|32bit 사용|64bit 사용|
|7개 유효숫자까지| 15개 유효숫자까지|

### 엔디언(endian)
컴퓨터의 메모리와 같은 1차원 공간에 여러 개의 연속된 대상을 배열하는 방법
바이트를 배열하는 방법은 _바이트 순서_(Byte Order)라고 한다.

+ 빅 엔디언 : 큰 단위가 앞에 나오는 경우 / MSB부터 차례로 저장하는 방식
	- int s = 0x12345678 => 0x12 0x34 0x56 0x78
	- 사람과 읽는 순서가 동일하여 디버깅이 편함
	- 모토로라 
+ 리틀 엔디언 : 작은 단위가 앞에 나오는 경우 / LSB부터 차례로 저장하는 방식
	- int s = 0x12345678 => 0x78 0x56 0x34 0x12
	- 하위 바이트만 사용할 때 별도의 계산이 필요없다.
	- intel x86, x64, AMD

 + 가상머신은 스택을 사용한다.
 
### 부동 소수점(Floating Point)
- 정규화 과정을 거쳐 부호 + 왼쪽부터 채워넣으면 된다.
- 정규화: 정수부에 1만 남을 때까지 소수점을 왼쪽으로 이동시키고 이동한 칸 수 만큼 n에 두는 것
	- 물리학의 단위 나타내는 것과 동일
	- ex) 111.101 => 1.11101 * 2^2
	- 1은 신경 안 쓰고 표현하기도 한다. 무조건 1이기 때문.
- 지수부의 경우 bias로 지정된 숫자를 더한 다음 넣어야 한다. n이 음수 일때도 있기 때문이다.
	- IEEE 표준에선 bias가 float은 127, double은 1023을 더한다.
- ex) 312.9375 = 100111000.1111 = 1.001110001111 * 2^8
(float) 0 10000111 00111000111100000000000 
=> 부호(1bit) 지수(8bit) 가수(23bit)
(double)  0 10000000111 0011100011110000000000000000000000000000000000000000
 => 부호(1bit) 지수(11bit) 가수(52bit)


## 연산자 overriding
|문법|클래스 내부 정의| 클래스 외부 정의|
|--|--|--|
|+a|T T::operator+() const;|T operator+(const T &a);|
|-a|T T::operator-() const;|T operator-(const T &a);|
|a + b|T T::operator+(const T2 &b) const;|T operator+(const T &a, const T2 &b);|
|a - b|T T::operator-(const T2 &b) const;|T operator-(const T &a, const T2 &b);|
|a * b|T T::operator*(const T2 &b) const;|T operator*(const T &a, const T2 &b);|
|a / b|T T::operator/(const T2 &b) const;|T operator/(const T &a, const T2 &b);|
|a % b|T T::operator%(const T2 &b) const;|T operator%(const T &a, const T2 &b);|
|~a|T T::operator~() const;|T operator~(const T &a);|
|a & b|T T::operator&(const T2 &b) const;|T operator&(const T &a, const T2 &b);|
|a \| b|T T::operator\|(const T2 &b) const;|T operator\|(const T &a, const T2 &b);|
|a ^ b|T T::operator^(const T2 &b) const;|T operator^(const T &a, const T2 &b);|
|a << b|T T::operator<<(const T2 &b) const;|T operator<<(const T &a, const T2 &b);|
|a >> b|T T::operator>>(const T2 &b) const;|T operator>>(const T &a, const T2 &b);|

## 범위와 수명(Scope and Lifetime)
**범위**
변수가 유효한 범위

### 블록 범위(block scope)
가장 기본적인 범위는 블록내 선언 된 지점에서 시작하여 블록의 끝에서 끝나는 것.

### 함수 매개 변수 범위(function parameters scope)
함수의 매개 변수는 함수 정의 종료에서 끝난다.

**수명**
https://maxcomfem.tistory.com/5
