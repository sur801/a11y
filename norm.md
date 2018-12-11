~~_아 md 파일로 쓰는게 git에 올릴 때 더 좋을 것 같았는데 안 되는 게 너무 많네요 github.io 구축을 안 해서 그런가 걍 다음부턴 pdf로 올려야지 새벽에 개뻘짓했네_~~
# DB 정규화
>관계형 데이터베이스의 설계에서 중복을 최소화하게 데이터를 구조화하는 프로세스를 말함.

일단 관계형 데이터베이스 (이하 RDBMS)를 모르니 간단하게 설명하고 넘어갑시다.

<img src = "https://user-images.githubusercontent.com/26535709/48506933-ef711b00-e88d-11e8-87b3-cff1b6d850cf.png" width = 80%>

RDBS는 table로 이루어져 있으며, 이 table은 key와 value의 관계를 나타냅니다. 이렇게 data끼리의 종속성을 relationship으로 표현하는 것이 RDBMS의 특징이죠.

#### 열(column)
각각의 열은 유일한 이름을 가지고 있으며, 자신만의 타입을 가지고 있습니다.
이러한 열은 필드(field) 또는 속성(attribute)이라고도 불립니다.

#### 행(row)
행은 관계된 데이터의 묶음을 의미합니다.
한 테이블의 모든 행은 같은 수의 열을 가지고 있습니다.
이러한 행은 튜플(tuple) 또는 레코드(record)라고도 불립니다.

#### 값(value)
테이블은 각각의 행과 열에 대응하는 값을 가지고 있습니다.
이러한 값은 열의 타입에 맞는 값이어야 합니다.

#### 키(key)
테이블에서 행의 식별자로 이용되는 열을 키(key) 또는 기본 키(primary key)라고 합니다.
즉, 테이블에 저장된 레코드를 고유하게 식별하는 후보 키(candidate key) 중에서 데이터베이스 설계자가 지정한 속성을 의미합니다.
키는 정규화를 할 때 자주 나오므로, 조금만 더 자세하게 봅시다.
- **기본키 (Primary Key)** : 테이블에서 유일하게 식별하기 위해 사용하는 키
- **외래키 (Foreign Key)** : 외래키란 테이블 내의 열 중 다른 테이블의 기본키를 참조하는 열을 외래키라 한다.
- **후보키 (Candidate Key)** : 테이블을 구석하는 열 중에서 유일하게 식별할 수 있는 열
- **대체키 (Alternate Key)** : 후보키 중 기본키를 제외한 나머지 후보키
- **슈퍼키 (Super Key)** : 슈퍼키 또는 합성키라 불린다. 하나의 열이 키로사용되는 것이 아닌 2개 이상의 열이 합쳐서 기본키로 사용하는 것이다.



**RDBMS의 특징**은 다음과 같습니다.
1. Data의 분류, 정렬, 탐색 속도가 빠르다.
2. 오랫동안 사용된 만큼 신뢰성이 높고, 어떤 상황에서도 데이터의 무결성을 보장해 준다.
3. 기존에 작성된 schema를 수정하기가 어렵다.
4. DBMS의 부하를 분석하는 것이 어렵다.


다시 정규화로 돌아와서,
정규화의 목적은 주로 다음 두 가지와 같아요.
1. 불필요한 데이터(data redundancy)를 제거한다.
2. 데이터 테이블의 구성을 논리적이고 직관적으로 한다.

사실 이게 무슨 소린지 완전히는 모르겠다... 간단하게 '중복을 최소화'하고, 'tuple이 굉장히 많을 때 query를 수행하는 시간을 줄이기 위해' 정규화를 한다고 생각해 보자.



## 1NF(First Normal Form : 제 1 정규화)
1NF에서는 각 row마다 column의 값이 1개씩만 있어야 해요. 이를 통해 모든 column이 Atomic Value를 갖게 해줍니다. 예를들어, 다음과 같은 table이 있다고 생각해 봅시다.

<img src =https://user-images.githubusercontent.com/26535709/48507679-f1d47480-e88f-11e8-904d-2f4373f563a1.png width = "50%">

이 table에서 규리라는 name을 가진 entity를 볼까요? 현재 이 entity는 규리라는 하나의 row가 class라는 column에서 값을 2개나 가졌네요. 이런 경우 한 개의 row를 더 만들어주어 column이 Atomic Value를 가지게 해줍니다. 다음과 같이 변하겠죠

<img src="https://user-images.githubusercontent.com/26535709/48507689-f862ec00-e88f-11e8-9afc-1f663400d240.png" width = "50%">

왜 이런 짓을 할까요?? RDBMS는 모든 속성이 Atomic Value를 가지는 특성이 있기 때문에, 최소한 제 1 정규형을 만족해야 relation이 될 자격이 있습니다.


## 2NF(Second Normal Form : 제 2 정규화)
>primary key가 아닌 모든 attribute가 primary key에 FFD 여야 한다.

1NF만 만족시키는 relation에서는 insert, update, delete에 여전히 이상현상이 일어날 수 있습니다.
2NF에서는 table의 모든 column이 FFD(Full Functional Dependency : 완전함수적 종속성)을 가지게 합니다. 규리는 함수적 종속성 자체가 뭔지도 모르니 잠깐 간단하게 알려주고 갑시다.


#### FFD(Full Functional Dependency : 완전함수적 종속성)
먼저 determinant라는 것이 있는데, 이는 주어진 relation에서 다른 attribute들을 고유하게 결정하는 하나 이상의 attribute입니다. 예를들어, 아래 그림과 같은 사원이라는 relation을 봅시다. 여기서 사원번호는 determinant로서 사원이름, 주소, 전화번호라는 다른 attribute들을 고유하게 결정합니다.

<img src = "https://user-images.githubusercontent.com/26535709/48508031-dae25200-e890-11e8-8e0b-8f102e69c73f.png" width = 70%>

여기서 하나의 attribute가 2개 이상의 determinant에 대해 Functional dependency가 있을 때 이를 FFD라고 합니다. 하나만의 determinant에 대해서만 dependency가 있으면 이는 부분 함수적 종속성이라고 합니다.

**완전 함수적 종속 (Full Functional Dependency)**
> 속성집합 Y가 속성집합 X 전체에 대해서만 함수적으로 종속된 경우

**부분 함수적 종속 (Partial Functional Dependency)**
> 속성집합 Y가 속성집합 X의 전체가 아닌 일부분에도 함수적으로 종속된 경우


이런 relation을 예로 들어볼까요? markdown이 지금 왜 표가 안 만들어지는지 모르겠네요. ppt로 표 만들어서 계속 이미지 삽입하기 힘드니까 상상력을 발휘해 봅시다. 

attribute = [학번, 과목코드, 성적, 학부, 등록금]

여기서 {학번, 과목코드}가 primary key라고 할 때, 다음과 같은 함수적 종속성을 예시로 들어봅시다.

- {학번, 과목코드} -> 성적 
- {학번, 과목코드} -> 학부 
- {학번, 과목코드} -> 등록금 
- 학번 -> 학부 
- 학번 -> 등록금 
- 학부 -> 등록금 

현재, 학부와 등록금이 partial function depencency를 가지고 있네요. 이 PFD는 relation을 [학번, 과목코드, 성적] attribute를 가지는 relation과, [학번, 학부, 등록금] attribute를 가지는 relation 2개로 나눠 주면 제거할 수 있습니다.


`학번 -> 학부` 는 학번만으로 학부에 대한 결정을 지을 수 있다는 말이다. 그러나 지금 primary key가 {학번, 과목코드}이기 때문에 아무 소용이 없다. 따라서 학번이라는 determinant를 따로 빼주어 FFD를 만들어 준다. 이때 FFD를 따지는 determinant가 반드시 primary key 일 필요는 없기 때문에 가능하다.


**제 2 정규형을 만족하면 이상현상이 없어질까?**

그렇지 않다.

- insert이상 :
새로운 학부가 생기는 경우 등록된 학생(학번)이 없다면 학번속성이 NULL이 되므로 삽입할 수 없다.

- update 이상 :
컴퓨터공학부 등록금이 400으로 오르는 경우 20800399, 21500399 둘 모두 바꾸어 주지 않으면 데이터 불일치 문제가 발생한다.

- delete 이상 :
21400001 학번을 가진 학생이 자퇴하는 경우, 기계공학부에 대한 정보가 함께 사라진다.

제2정규형에서도 이상현상이 발생하는 이유는 이행적 함수 종속이 존재하기 때문이다. 이행적 함수 종속을 없애주는 과정이 제 3 정규화이다.


## 3NF(Third Normal Form : 제 3 정규화)
2NF에 속하면서, primary key가 아닌 모든 attribute가 primary key에 종속이 되지 않으면 제 3정규형이다.
X-> Z이고, Y->Z라면 X-> Z가 된다. 이 때 Z가 X에 대해 종속되었다고 하며, 이러한 종속성을 TFD(Transitive Functional Dependency : 이행적 함수 종속)이라고 한다. 3NF에선 이러한 TFD를 없앤다.

- 학번 -> 학부 
- 학번 -> 등록금 
- 학부 -> 등록금 

3NF에선 
>1. table이 2NF를 만족하고
>2. table 내의 primary key가 아닌 모든 attribute가 primary key에만 의존해야하며, 다른 후보 키에 의존하지 않아야 한다.

아까 [학번, 학부, 등록금] attribute를 갖던 table을 [학번, 학부], [학부, 등록금]으로 나눠주면 된다.


그러나 3NF를 해도 이상현상은 없어지지 않는다. primary key가 될 수 있는 후보 키가 여러개라면 relation에선 3NF를 만족하더라도 이상현상이 생긴다. 이를 위해 BNF(==strong 3NF)가 나왔다.

출처:
- http://tcpschool.com/mysql/mysql_intro_relationalDB
- https://yaboong.github.io/database/2018/03/09/database-normalization-1/
- http://futurists.tistory.com/14?category=587334
