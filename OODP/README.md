# 객체지향
## 호출 순서
### 생성자
부모 -> 자식 순으로 호출

### 소멸자
* 소멸자에 virtual 키워드가 없을 때
	* 부모형 포인터로 자식을 동적할당 받은 경우 자식 소멸자가 호출되지 않는다.
	* 자식형 포인터로 자식을 동적할당 받은 경우 자식 -> 부모 순으로 호출된다.
* 소멸자에 virtual 키워드를 붙이면 자식 -> 부모 순으로 호출된다.
	- 부모에만 붙이면 된다!
```cpp
class cParent{
	public:
		cParent(void){
			cout << "부모 클래스 생성자 호출"<< endl;
		}
		virtual ~cParent(void){
			cout << "부모 클래스 소멸자 호출" << endl;
		}
};

class cChild: cParent{
	public:
		cChild(void){
			cout << "부모 클래스 생성자 호출"<< endl;
		}
		~cChild(void){
			cout << "부모 클래스 소멸자 호출" << endl;
		}
};