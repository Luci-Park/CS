# 데이터베이스
데이터를 깔끔하게 보관하기 쉽게 하기 위한 방법
* 특징
	1. 독립성: DB의 수정으로 응용 프로그램이 바뀔 일 없음
	2. 무결성: DB내에서 데이터의 유효성을 설정 가능
	3. 보안성: DB접근을 조절하여 모든 데이터의 보안을 보장
	4. 일관성: 형식의 일관성
	5. 중복 최소화

## 종류
**관계형**

	* 스키마(테이블, 행렬 등)이 고정된 db
		- 트랜잭션이 하나의 단위로 기록. 실패시 전체 롤백
		- 정규화: db 설계 시 중복을 최소화해서 구조화하는 프로세스
	* 장점
		- 정렬, 탐색, 분류가 빠름
		- 신뢰성 높음
		- 정규화에 따른 갱신의 비용은 최소화
	* 단점
		- 기존에 작성된 스키마를 수정하기 어려움
		- db 부하 분석이 어려움
		- 수직적 확장만 쉽다.
		- 열의 종류가 많은 빅데이터에는 불리하다.
	* 종류: Mysql, Oracle, Mssql

**비관계형(NoSQL)**
	* 스키마가 존재하지 않는 db
		- map 형식으로 key - value 형식을 지원
		- PK, FK, JOIN 같은 관계를 정의하지 않음
	* 장점
		- 대용량 데이터 처리를 하는데 효율적
		- 읽어오는 속도가 빠르다.
		- 수직, 수평 확장에 뛰어나므로 모든 읽기 쓰기 요청이 가능
		- 키 값 저장이 최적화 되면 응답속도나 처리속도가 빠름
		- 복잡한 데이터 구조를 표현할 수 있음.
	* 단점

		- 데이터 하나 변경할 때 다 수정해야 됨
		- 쿼리 처리가 느린 편
		- 쿼리 언어를 각기 다르게 사용하는 경우가 많아 이식성이 낮음

	* 종류: MongoDB, CouchDB


##Key
데이터베이스에서 튜플을 찾을 때 다른 튜플들과 구별할 수 있는 유일한 기준이 되는 속성(attribute)
	*특성
		* 유일성: 하나의 키값으로 튜플 하나를 골라낼 수 있는 성질
		* 최소성: 키를 두 개 다 사용해야 유일성이 지켜지는 것. 혼합키 중 하나를 뺐는데도 유일성이 지켜진다면 최소성이 지켜지지 않은 것이다.
	* 후보(Candidate)
		+ 기본키가 될 수 있는 키
		+ 유일성
		+ 최소성
	* 기본(Primary)
		+ 후보키 중 하나를 선택한 키
		+ 중복 안됨(무결성 조건 1)
		+ Null 안됨(무결성 조건 2)
	* 대체(Alternate)
		+ 기본키 외 후보키
		+ 보조키라고도 함
	* 슈퍼(Super)
		+ 유일성의 특징을 만족하는 속성혹은 그것의 집합.
		+ 최소성을 만족하진 않음.
	* 외래(Foreign)
		+ 테이블을 연결하는 유일한 기본키	
		+ Relation을 맺고 있는 관계에서 타 릴레이션의 기본키를 참조하고 있다면 그것을 외래키라 부름.

## Transaction
db 변화를 위한 작업의 단위
한 transaction에서 수정이 여러 번 일어나면 모든 것이 다 수정되어야 하나의 transaction이 끝났다고 본다.

**관계형 db transaction 특징(ACID)**

	* 원자성(Atomicity): transaction 모두가 반영되거나 안 되거나
	* 일관성(Consistency): transaction 전후로 db의 상태는 동일해야 한다. 즉 transaction은 db의 제약이나 규칙을 따라야한다.
	* 독립성(Independency): transaction 중 다른 transaction이 연산하지 못함
	* 지속성(Durability): transaction이 완료되면 결과는 영구히 반영되어야 함.
### Data Manipulation Language(DML)
- SELECT(column) FROM(table) WHERE (조건)
- SELECT * = Select all
- WHERE OPERATORS : 
	- 부등호
	- <>(깥지 않은 값만)
	- BETWEEN A AND B
	- LIKE(같은 패턴만) ex: LIKE 's%' => s로 시작하는 값
	- IN(특정 복수의 값만) ex: IN ('Paris', 'London')
- AND, OR, NOT
- ORDER BY(attribute) ASC / DESC
- INSERT INTO Customers(name, address) VALUES ('Helen', 203);
	- 만약 지정 열들 중 없는 열들이 있다면 입력되지 않는 열에는 null이 삽입된다.
- IS NULL, IS NOT NULL
- Wildcards(LIKE와 같이 사용)
	- % : %es% => es가 앞, 뒤, 앞뒤에 포함된 값을 필터링
	- _ : h_t => hot, hat, hit
	- []: h[oa]t => hot, hat but not hit
	- ! : h[!oa]t => hit but not hot, hat
	- - : c[a-c]t => cat, cbt, cct
- Aliases: 이후 쿼리에서 지칭 테이블의 이름을 더 간단하게 하는 것.
``` SQL
SELECT o.OrderID, o.OrderDate, c.CustomerName
FROM Customers AS c, Orders AS o
WHERE c.CustomerName="Around the Horn" AND c.CustomerID=o.CustomerID;
```

- Join: 두 개 이상의 테이블로부터 행을 결합하기 위해 사용
```SQL
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders 
INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
```
	- (INNER) JOIN: 교집합만 
	- LEFT (OUTER) JOIN : 왼쪽 모든 레코드 + 오른쪽의 부합 레코드
	- RIGHT (OUTER) JOIN: 왼쪽 부합 레코드 + 오른쪽의 모든 레코드
	- FULL OUTER JOIN: 양 테이블의 부합 모든 레코드

### Commit & Rollback

**Commit**
트랜잭션이 완료되어 db에 작업이 반영되는 것

**Rollback**
트랜잭션이 중간에 실패하여 작업이 반영되지 않는 것

**Transaction 종류 **
* 명시적 : 시작과 끝을 사용자가 명시적으로 지정.

```SQL
START TRANSACTION;
	INSERT INTO 학생(학번, 이름, 주소, 학년, 나이, 성별, 휴대폰번호, 소속학과) VALUES ("s004", "규니", "규니집", 3, 25, "남", "010-1234-5678", "컴공");
    SELECT * FROM 학생;
ROLLBACK;
SELECT * FROM 학생;
```
ROLLBACK으로 끝나기 때문에 반영은 안된다. COMMIT;으로 바뀌면 반영.

* 자동 완료: SQL문 하나를 하나의 트랜잭션으로 자동 정의하고 SQL 결과에 따라 자동으로 커밋 혹은 롤백하는 트랜잭션
```SQL

DELETE FROM 학생 WHERE 학번 = 's002';
SELECT * FROM 학생;
```

* 수동 완료 : 트랜잭션의 끝만 사용자가 명시적으로 지정하는 트랜잭션
```SQL
SET AUTOCOMMIT = 0;
SELECT @@AUTOCOMMIT;
```
AUTOCOMMIT 이 0이면 커밋이 자동으로 되지 않는다. 즉 ROLLBACK 명령어를 사용할 수 있게 된다.
```SQL
DELETE FROM 학생 WHERE 학번 = 's007';
SELECT * FROM 학생;
INSERT INTO 학생(학번, 이름, 주소, 학년, 나이, 성별, 휴대폰번호, 소속학과) VALUES ("s009", "규니", "규니집", 3, 25, "남", "010-1234-5678", "컴공");
ROLLBACK;
SELECT * FROM 학생;
```

### 격리 수준(Isolation level)
동시에 여러 트랜잭션이 처리될 때, 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있도록 허용 여부를 결정하는 것.

**READ UNCOMMITTED**
각 트랜잭션의 변경 내용이 커밋이나 롤백 여부 상관없이 다른 트랜잭션에서 보이는 것.
불안정해서 잘 안 씀.
Dirty Read

**READ COMMITTED**
커밋된 데이터만 보임. 
커밋 하기 전 sql 중이면 다른 트랜잭션에선 업데이트 전 데이터가 보인다.

**REPEATABLE READ**
반복해서 read operation을 수행하더라도 결과 값이 변하지 않도록 보장함.
트랜잭션에 실행 순서대로 번호를 매겨 자신보다 숫자가 작은 트랜잭션 번호에서 변경한 것만 보게 하는 것이다.
*NOT REPEATABLE READ*의 경우 트랜잭션이 커밋한 것은 다 보게 된다.

**SERIALIZABLE**
읽기 작업할때도 공유 락을 획득하게 하여 아예 한 트랜잭션이 작동할 때 따른 것이 작동 안 되게 하는 것.

### Lock

DBMS(DataBase Management System)에서 트랜잭션 처리의 순차성을 보장하기 위한 방법. 
고립 레벨에 따라 다른 locking 전략을 취할 수 있음.
락은 커밋 혹은 롤백일 때 언락 된다.

#### 종류

##### 공유 락(Shared / Read Lock)
데이터를 읽을 때 사용. Lock이 해제될 때까지 다른 사용자가 볼 수 있지만 변경할 수는 없음.
공유 락을 사용하는 쿼리끼리는 같은 row 접근이 가능하다.

> 보통 SELECT 쿼리는 lock 없이 사용 하나 SELECT ... FOR SHARE같은 일부 SELECT 쿼리에서 사용.

##### 배타적 락(Exclusive / Write Lock)
데이터를 변경할 때 사용. Lock이 해제될 때까지 읽기와 쓰기가 불가능해짐.
배타적 락을 설정한 경우 다른 쿼리는 그 어떤 락도 락을 설정할 수 없다.
>SELECT, UPDATE, DELETE 같은 수정 쿼리에서 나타남.


#### Blocking
락들끼리 race condition이 발생하여 특정 세션이 작업을 진행하지 못하고 멈춰 선 상태를 지칭함.
보통 배타적 락과 다른 락으로 인해 생기는 편.

__해결방안__
1. 트랜젝션을 최대한 빠르게 실행하는 것
2. 트랜젝션을 짧게 하는 것
3. 동일한 데이터를 동시에 변경하는 작업을 애초에 피하는 것
4. 락에 최대시간을 설정하는 것.

#### Dead Lock
- 두개의 트랜젝션 간에 각각의 트랜젝션이 가지고있는 리소스의 락을 획득하려고 할 때 발생.

## 정규화
Anomaly 방지를 위해 데이터들의 형식을 조정하는 작업
무결성을 보장하고 구성을 논리적이고 직관적이게 바꾼다.

_Anomaly 발생 가능성_

	* 삽입: 원하지 않는 자료, 부족한 자료의 삽입
	* 삭제: 자료가 원하는 것만 삭제가 되지 않는 경우
	* 수정: 일부 자료를 수정하지 못하거나 부정확하게 갱신

**1 단계**
각 칼럼이 하나의 값만 갖도록 테이블 분리

**2 단계**
기본키가 복합일 때 최소성을 만족하도록 분리
만약 복합키 중 하나를 빼도 유일성이 유지된다면 안됨

**3 단계**
한 칼럼이 다른 칼럼에 의해 발견할 수 있다면 그 칼럼은 반드시 key여야 한다.
