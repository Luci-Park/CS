# C++
참조: [C++ 이야기](https://wikidocs.net/book/2380)

* [About C++](#about-c)
* [ReferenceSheet](#reference-sheet)
* [Coding Style](#코딩-스타일)
* [자료형](#자료형)

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
```cpp
int n = 0;
int n(0);
int n = {0};
int n{0};
```

각 자료형은 변수 초기화 할 때 자료형의 크기만큼 스택 영역에 만들어진다.

### const
객체를 불변으로 정의하는 예약어.
보통 `int const nNum` 처럼 쓴다. 
