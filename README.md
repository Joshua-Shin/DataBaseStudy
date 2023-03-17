DataBase 학습을 기록하기 위한 저장소 입니다.
---------------
- 개괄
  - DDL(스키마 조작하는거), DML(인스턴스 조작하는거) DCL(접근권한 조작), TCL(트랜젝션 권한 조작)
  - 스키마, 인스턴스. 테이블에서 맨 윗 열을 스키마로 보고, 그 아래 모든 열들을 인스턴스로 보면 돼.
  - DB 설계 = DB 모델링 : 스키마를 만드는것
  - DB 설계의 4단계 : 요구사항분석 -> 개념적 설계 -> 논리적 설계 -> 물리적 설계
  - 요구사항분석 : 어떤 엔티티를 끌어내야 하는지 업무기술서 같은거 보면서 분석하는거.
  - 개념적 설계 : ERM(=개체 관계 모델), ERD(=개체 관계 다이어그램) 만들기. R은 Relationship.
  - 논리적 설계 : Relational Model 만들기, 테이블을 만든다 생각하면 돼.


- 개념적 설계
  - 엔티티, 관계, 속성
  - 엔티티는 물리적객체 개념적객체를 다 포함한 하나의 정보 단위. 우리나라말로 개체.
  - 속성은 엔티티도 관계도 다 갖고 있을 수 있어.
  - 속성에는 singleValued, multiValued, simple, composite, stored, derived 가 있어
  - Composite 속성의 예는 주소.
  - 차수. degree. 는 unary, binary, tenary.. n-ary 가 있음 하나의 관계에 여러 엔티티가 달려있는거.
  - 대응수. 는 일대일 일대다 다대다. 관계에 매달린 속성은 일대다에서 다 쪽으로 붙일 수 있음
  - 키 속성이 없는 개체를 약한 개체라고 함. Identifying Entity의 키 속성과 약한 개체의 키인 부분 키와 합쳐서 키 속성을 만들 수 있어.
  - 보통은 약한개체는 안만드는게 좋아
  - 상속과 비슷한 개념인데 일반화 라는게 있어.


- 논리적 설계
  - 4가지 제약
    - 도메인 제약, 키 제약, 개체 무결성 제약, 참조 무결성 제약
    - 도메인 제약: 나이 속성에다가 문자열을 쓴다거나 200을 넘기는 수가 들어간다거나 하면 안된다.
    - 키 제약 : 키가 있어야 한다.
    - 개체 무결성 제약 : 키에 해당되는 인스턴스 값은 not null & unique 해야 한다.
    - 참조 무결성 제약 : 참조하는 값이 null 이거나, null이 아니라면 해당 값이 실제로 존재해야 한다.
  - ~~ERD to RM의 7가지 스탭~~
    - ~~강한 개체 테이블 만들고~~
    - ~~약한 개체 테이블 만들고~~
    - ~~1:N 관계인거 1인 릴레이션의 PK를 N인 릴레이션으로 넘겨서 FK 필드 만들어주고~~
    - ~~1:1 관계인거 아무쪽에다가 FK 필드 만들어주고~~
    - ~~N:M 관계인거 새로 테이블 만들어주고 각각의 PK를 가져와서 새 테이블의 Composite Key로 만듦~~
    - ~~N-Ary 관계인거 새로 테이블 만들어주고 역시 N차 M차인애들의 PK를 새 테이블의 Composite Key로 만들고, 1차인애늬 PK는 그냥 속성으로 포함.~~
    - ~~multivalued 속성 따로 테이블 만들어주고~~


- 세팅
  - Oracle, SQL Developer 사용.
  - 맥에서는 Oracle 설치 못함. -> Docker 사용해서 Oracle 설치
  - 맥 m1에서는 Docker로 oracle 컨테이너 생성 및 설치 못함
  - 오픈 소스 컨테이너 런타임인 Colima를 사용해 oci-oracle-xe 이미지를 x86/64 환경으로 띄워서 설치
  - 그러니까 Docker Desktop으로 컨테이너에 설치한게 아니라 Colima로 설치한거임. 다만 Docker Engine은 사용하고 있는 상태인듯.
  - 실제로 Docker Desktop 실행해보면 컨테이너와 이미지 리스트에 oracle 안보임.
  - SQL Developer는 쉽게 설치 가능.
  - Oracle이 사용 가능 상태 되면, SQL developer 실행해서 oracle과 연동.
  - commit하면, 컨테이너에 데이터가 저장되는듯.
  - 당연히 새로운 컨테이너 만들어서 다시 시작하면 해당 데이터는 없겠지.
  - 터미널 종료 해도 컨테이너는 자동 종료 안됨. 컴 종료해야 자동 종료 되는듯?
  - 터미널에서 docker ps -a 치면 컨테이너 리스트과 실행상태 볼 수 있음
  - docker stop 컨테이너ID, docker start 컨테이너ID
  - 당연히 컨테이너에서 실행중인 상태여야 sql developer에서 접속 가능해.
  - 컴 종료 후에 다시 시작하는 법
    - colima 시작 : colima start --memory 4 --arch x86_64
    - 종료된 컨테이터 조회 : docker ps -a 
    - docker start CONTAINER_ID
    - 10초후 sql developer 실행
  - [참조](https://shanepark.tistory.com/400)


- 데이터 모델링의 이해
  - 이론에서는 개념적 설계는 ERD 그리는거고, 논리적 설계는 스키마 및 테이블을 만드는거였는데,
  - 실무에서는 개념적 설계나 논리적 설계의 구분이 좀 모호하고, 물리적 설계 단계에서 테이블을 만들게 됨
  - 차이의 주요한 원인은 ERD를 그리는 방식 때문인듯. 
  - 이론에서는 peter chen 방식을 쓰고, 사실 이는 테이블로 옮기기에 변경해야 될 사항들이 좀 많아.
  - 실무에서는 IE 방식 (= crow's foot)을 쓰고 이는 이미 테이블의 스키마 내용을 다 명시하고 있는거나 마찬가지라, 개념적 설계 논리적 설계 구분이 모호.
  - peter chen 에서 사용한 다이아몬드 모양으로 그린 관계에는 속성이 붙을 수 있으나, IE에는 관계에는 속성이 안붙어. 다이아몬드 모양으로 따로 만들지도 않고.
  - 일대다, 일대일에서는 일의 반대편으로 일의 PK를 넘겨. 만약 관계에 속성이 붙었다면, 해당 속성은 같은 방향으로 넘어가.
  - 다시 말해서 일이 있는쪽을 부모 엔티티, 일의 반대쪽을 자식 엔티티라 하고, 부모 엔티티의 PK가 자식 엔티티의 새로운 필드로 들어가면서 FK라 표기하게 됨.
  - 이때 FK가 자식 식별자의 PK가 되어버린다면, 이를 식별자관계라 하고, 그렇지 않다면 비식별자관계라 함.
  - 식별자 관계일때는 부모엔티티가 없다면 자식엔티티도 없게 되겠지. 강한 의존관계.
  - 자식 식별자가 고유의 ID를 새로 만들어서 식별자 관계였던것을 비식별자 관계로 만들 수도 있어.
  - 현재 듣고 있는 인프런 JPA 강의 보면 엔티티 마다 다 고유의 ID값을 주는걸 봐서는 실무에서는 비식별자관계를 많이 사용하는듯.
  - 다만, 이럴 경우 조회할때 조인을 많이 해서 성능의 이슈가 생길 수도 있어
  - 물론, 식별자 관계로만 이어가면 식별자가 composite으로 계속 쌓이고 쌓여서 이것도 문제가 생길 수 있고.
  - 실무에서는 다대다 관계는 무조건 새로운 테이블 만들어서 푸는듯


- 데이터 모델과 성능
  - **데이터베이스 정규형**
    - 제1정규형 : 한칸에 하나의 데이터만 있는상태, & 동일한 유형의 속성이 여러 칼럼 단위에 반복되는 경우가 아닌 상태.
    - 제2정규형 : 부분함수 종속이 있는 상태(= composite primary key가 있을때, 각 키가 결정 짓는 속성들이 다른 상태)를 다른 테이블로 나눠서 해결한 상태
    - 제3정규형 : 이행함수 종속이 있는 상해(key가 아닌 일반 필드가 다른 속성을 결정짓고 있는 상태)을 해결한 상태
    - BCNF : 모든 '결'정자가 후보키인 정규형
    - 4NF : '다'치종속이 성립하는 경우 테이블의 모든 속성이 함수적 종속 관계를 만족
    - 5NF : 모든 '조'인종속이 테이블의 후보키를 통해서만 성립되는 정규형
  - 함수적 종속성을 확인하고, 부분함수 종속을 제거하는것을 2차 정규화, 이행함수 종속을 제거하는것을 3차 정규화라 함
  - 제3정규화 정도가 이론적으로도 실무적으로도 적합함.
  - 비정규화는 아직 정규화를 하지 않은 상태이고,
  - 반정규화는 정규화 해놓은걸 성능 이슈 때문에 다시 돌리는거야.
  - 반정규화를 한다고 해서 꼭 정규형 수치가 낮아지는건 아니고, 넓은 의미에서는 정규화가 아닌, 테이블 구조 변경을 말하기도 하는듯.
  - 예를 들어 테이블 수직 분할. 딱히 정규형이 낮아지는것이 아닌데도 반정규화 종류중 하나임.
  - 성능 이슈는 삽입 갱신 삭제에서 나타나지 않고, 상황에 따라 조회할때 나타남
  - 너무 정규화된 상태면 조인을 여러번 해야된다든지 계산을 계속해야된다든지 하는 이유 때문에 생길 수 있어
  - 정규화 되지 않은 테이블에는 이상 현상. 삽입, 삭제, 갱신 이상이 나타날 수 있어.
  - 반정규화를 하기전에, 트렌젝션이나 이런저런것들을 분석하고 검토한뒤, index 조정을 하기도 하고 이런저런걸 한뒤, 안되면 하는거야.
  - 칼럼과 테이블을 중복 저장, 병합, 테이블 수직 분할, 수평분할 등등의 방식이 있어.
  - 테이블에서 PK의 경우 Index 테이블이 자동으로 만들어지고 트리 구조로 저장되기에 순서가 정렬되어있어.
  - PK가 composite key일 경우 어떤 요소를 먼저 두나에 따라 정렬 우선순위가 정해짐. 
  - 따라서, 조회를 많이 하는 애를 composite key중 위로 두는게 좋아
  - FK로 조회를 많이 한다면, 해당 FK의 index 테이블을 수동으로 만드는 방법도 있어.
  - 다대다로 연결되어있을때, 이를 1대다 다대1로 풀어내기 위해 새로운 엔티티를 만들잖아 그걸 연관엔티티라 함


- SQL 종류
  - DDL : create, drop, alter, truncate : drop 후 create
  - DML : select, insert, delete, update
  - DQL : 일부에서는 DML의 select 문을 따로 분류하기도함
  - DCL : commit, rollback, savepoint, grant, revoke
  - TCL : 일부에서는 DCL의 commit과 rollback를 따로 분류하기도 함


- SQL 기본 DML
  - desc 테이블명
  - select from where order by
  - distinct 칼럼명. 중복 제거 해서 보여줌. 
  - =, !=, and, or, not, between a and b, in(a, b), like '%석'
  - insert : 테이블에다가 새로운 행을 넣는거.
    - insert all은 언제나 select 문과 같이 쓰이기에 맨 마지막에 껍데기 select 문을 같이 넣어줘야해.
    - ```
      insert into 테이블명(칼럼명1, 칼럼명2, 칼럼명3) values(칼럼1값, 칼럼2값, 칼럼3값);

      insert all into 테이블명(칼럼명1, 칼럼명2, 칼럼명3) values(칼럼1값, 칼럼2값, 칼럼3값) 
                 into 테이블명(칼럼명1, 칼럼명2, 칼럼명3) values(칼럼1값, 칼럼2값, 칼럼3값)
      select * from dual
      ```
    - insert into 테이블명1(칼럼명1, 칼럼명2, 칼럼명3) select 칼럼명1, 칼럼명2, 칼럼명3 from 테이블명2;
      - 테이블2의 칼럼1,2,3을 테이블1의 칼럼1,2,3으로 삽입함
    - insert into 테이블1 select * from 테이블2;
      - 테이블2 전체를 테이블1로 복사
  - delete from 테이블명 where 조건문
  - update 테이블명 set 칼럼명 = 값 where 조건문


- SQL DDL
  - 타입: char(10), varchar2(10), number(8, 2), date
  - drop table 테이블명 cascade constraints; 연관된 제약조건들까지 다 날려라.
  - ```
    create table 테이블명 (
    칼럼명1 타입 not null,
    칼럼명2 타입,
    ...
    constraint 제약조건명 primary key(칼럼명),
    constraint 제약조건명 foreign key(칼럼명) references 테이블명(PK칼럼명)
    );
    ```
    제약조건 걸때,
    그냥 칼럼 선언할때, 칼럼명1 타입 references 테이블명(PK칼럼명) 이라고 하는게 제일 깔끔. 제약조건명을 명시하고 싶다면,
    칼럼명1 타입 constraint 제약조건명 primary key 라고 해도 되고.
  - select * from all_constraints; 하면 모든 제약조건들 확인할 수 있어.
  - FK 제약조건 옵션
    - 칼럼에다가 FK 제약조건 걸때, 뒤에 옵션 붙일 수 있어
    - (on delete, on update) + (retrict, no action, cascade, set null) 조합.
    - team_id number(10) references team(team_id) on delete cascade;
    - player 테이블에 있는 필드 team_id가 team 테이블을 참조하는 FK 인데, 만약 team 테이블의 어떤 레코드를 삭제하려 할때, 참조무결성에 위배되지 않게 하기 위해 player 테이블에 있는 관련 레코드도 삭제 해버리겠다. 라는 뜻. 
    - 옵션 안붙이거나 retrict, no action은 삭제 못하게 하겠다는뜻
    - set null은 값을 null로 만들겠다는뜻
  - alter table 테이블명 add(칼럼명 타입)
  - alter table 테이블명 drop column 칼럼명
  - alter table 테이블명 rename column 칼럼명1 to 칼럼명2
  - alter table 테이블명 modify (칼럼명 not null)
    - 지금까지 입력된 값들이 문제 되지 않은 선에서 칼럼의 제약조건들을 수정할 수 있음.


- Function
  - Lower, upper, length, substr, concat, ||, ascii, chr
  - substr("문자열", "시작위치", "길이") : 시작위치는 1부터 시작.
  - abs, sign, celi, flood, round, trunc, power
  - trim(' abc ') => 'abc'
  - ltrim(' abc ') => 'abc '
  - ltrim('aabbcc', 'a') => 'bbcc'
  - rtrim(' abc ') => ' abc'
  - rtrim('aabbcc', 'c') => 'aabb'
  - to_char : 숫자 또는 날짜를 문자열로, to_number : 문자열을 숫자로, to_date: 문자열을 날짜로
  - extract(year from sysdate)
  - to_char(sysdate, 'yyyy.mm.dd.hh24.mi.ss')
  - ```
    case
      when position is not null then position
      else '없음'
    end as 포지션
    ```
  - nvl(position, '없음)
  - nvl은 null값이 산술 연산할때 무조건적으로 null이 되어버리기에 null값이 될만한 애들을 산술연산할때 무조건적으로 적용
  - sal * 12 + com 을 nvl(sal, 0) * 12 + nvl(com, 0) 으로.
  - nullif(A, B) : 두개 값이 같으면 null, 다르면 A 반환
  - COALESCE(a1, a2, a3, a4,... aN) :a1 부터 aN까지 첫음으로 null 이 아닌 값을 리턴, 모든 값이 null 이면 null 리턴
  - Q. PLAYER 테이블에서 PLAYER_NAME의 맨 마지막 문자를 ‘*’로 대체한 ‘비식별화’ 칼럼을 출력하시오. (concat, subsrt, length)
  - Q. PLAYER 테이블에서 PLAYER_ID를 활용하여 전체 선수를 4개의 그룹(0 ~ 3)에 배정하시오. (mod)

- TCL
  - 트랜잭션 : 논리적 최소 단위
  - *트랜잭션 특성 4가지 ACID*
    - Atomicity : 원자성. 트랜잭션에 있는 모든 연산이 성공하든지 아니면 다 실패 하든지. all or nothing
    - Consistency: 지속성. 트랜잭션 시작전에 문제가 없었다면, 올바르게 마친후에도 문제가 없어야 한다.
    - Isolation: 고립성. 트랜잭션 도중에 다른 트랜잭션이 끼어들 수 없음.
    - Durability : 영구성. commit 되면, DB에 영구히 저장.
  - commit, rollback, savepoint


- DCL
  - create user myid identified by myps;
  - alter user myid identified by newps;
  - grant create user to myid;
  - revoke create user from myid;
  - drop user myid cascade;
  - myid에 있는 player 테이블을 newid가 조회할 수 있는 권한을 주고 싶을때
  - admin 계정으로 로그인 한 상태라면, grant select on myid.player to newid;
  - myid 계정으로 로그인 한 상태라면, grant select on player to newid;
  - 권한들을 role에 묶어서 해당 role만 grant 하면 편리함.
  - 이미 DB에서 만들어놓은 role들이 있고, 그 중 connect, resource, dba 등이 있어.
  - 그래서 시작할때 admin 계정으로 로그인한후 해당 롤들을 새로 생성한 계정으로 준거야.
  - 해당 롤에는 create session, create table 과 같이 당연히 있어야 하는 권한들을 포함 여러가지가 있으며 dba는 관리자 권한과 비슷.


- Join
  - 순수 관계 연산 : select, project, divide, join
    - select의 개념은 where절에서
    - project의 개념은 select절에서
    - divide는 이제 안쓰는 개념
    - join은 처음에 natural join만 제안됐는데, 여기에서 이것저것 발전되어서 다양한 join이 생김
  - Join
    - equi join, no equi join : '='가 있냐 없냐
    - 명시적 조인, 암시적 조인 : join 키워드가 있냐 없냐
    - inner Join
      - inner join : 조건절에 부합하는 애들만을 연결해줌. 
        - 따로 join 키워드와 조건을 명시하지 않으면 두 테이블이 교차조인이 되어버림. 모조리 교차조인된 애들중에 조건절로 선택한 결과가 inner join이라 생각해도 될듯
      - natural join : 두 테이블에서 동일한 이름과 동일한 데이터 타입을을 갖는 모든 컬럼들을 equi join 해버림
        - 따로 조건을 명시할 수 없고, 조인의 대상이 되는 칼럼에 테이블명이나 별칭을 접두사로 명시할 수 없음.
      - using : inner join와 natural join의 중간 형태 느낌인데. 조건을 따로 명시하지만, 접두사로 조건의 컬럼명을 명시할 수 없음. 괄호 필수.
    - outter join : 조건에 부합하지 않아 두 테이블에 동시에 대응되지 않은 칼럼들도 빼버리지 않고 대응되지 않는 속성값을 null로 표시하여 나타냄.
      - left join, right join, full join
    - cross join
    - self join : 동일한 테이블 내에서 자기가 자기 테이블을 조인함. 테이블 식별을 위해 별칭을 반드시 사용.
    - 조인 개념 확인 핵심 문제 <br> <img width="1440" alt="스크린샷 2023-03-17 오후 5 02 11" src="https://user-images.githubusercontent.com/93418349/225863246-1dc16ee5-bee8-47bc-a966-f6b5cf817df2.png">
    - 답: 3개, 12개, 4개, 4개, 5개
    - ```
      select *
      from player p join team t
      on p.team_id = t.team_id
      where p.age >= 18
      order by p.age;
      ```
      이게 표준이고 이게 제일 많이 쓰이고, 조인 조건과 where 조건문을 구분해서 보기도 좋아.
    - on은 조인 조건문을 명시하는곳이야. 어떠한 공통의 값으로 조인을 할지만 쓰는게 아니라, 필터링 조건도 걸 수 있어. 
    - 조건문을 on에다 쓸 경우 일단 필터링을 한 다음 조인을 하겠지만, on에 안쓰고 where에다가 조건문을 쓰면, 조인된 결과물을 가지고 where에서 거르는것이기때문에, 되도록 join 조건은 on에다가 쓰는게 낫지 않을까.
  
  
  - Q. emp 테이블로부터 사원의 사번과 이름, 그리고 매니저의 사번과 이름을 출력, 단 매니저가 없는 사원의 정보도 출력
        <details>
        <summary>정답</summary>

        ```
        select e.empno, e.ename, m.empno, m.ename
        from emp e left join emp m
        on e.mgr = m.empno;
        ```
        </details>
  
- Set Operation
  - 일반 집합 연산 : union, intersect, minus(except), product
    - 합집합, 교집합, 차집합, 곱집합
  - union은 합집합, union all은 합집합의 개념이 아니라 그냥 다 더함.
  - 집합 연산을 위해서는 두개의 select절의 데이터 타입이 상호 호환 되어야 돼. 반드시 동일한 데이터 타입이여야 한다는 의미가 아니야.
  - union all을 제외하고는, 집합 연산이 이루어질때 각 테이블 내에서 중복이 되는 행들을 제거하고 연산이 일어남. 연산이 일어나고 중복행들을 제거하는 걸수도 있고.
  - 집합 연산 마지막 줄에 나오는 order by는 첫번째 select 문에 명시한 칼럼으로만 선택할 할 수 있으며, 해당 칼럼이 별칭을 지정했을 경우, 별칭이 아닌 원 칼럼 이름을 쓸 경우 오류 발생.


- 계층형 질의 (실무에는 안쓰이는데, sqld 시험에는 나옴)
    - 순환관계 데이터 모델(사원, 관리자가 주어졌을때 셀프 조인을 할 수 있는 데이터)의 경우 계층형 구조를 갖게 됨.
    - 즉 동일한 테이블에 관리자의 관리자의 관리자의 관리자로 위로 올라가면 대빵이 있는것처럼, 상위 하위 데이터가 존재하는 계층형 구조가 있어.
    - start with (mgr is null): 시작 노드 지정. mgr이 null인 행부터 순환시작해라.
    - connect by prior (자식칼럼 = 부모칼럼) : 순환 방향 지정. 부모에서 자식으로 내려오는 순방향.
    - level : depth 표현. 시작노드의 층을 1로 보고 그 다음 순환 방향에 맞춰 그 다음층을 +1로 보면 됨. 같은 트리여도 순방향 역방향에 따라 level이 달라지겠지
    - connect-by-isleaf : 순환 방향에 맞춰 가장 끝 노드면 1, 아니면 0. 순방향이면 리프노드일때 1, 역방향이면 루트노드일때 1
    - connect-by-root (empno) : 어느 시작노드에서 시작됐는지 표시.
    - sys-connect-by-path (empno, '/') : 시작노드부터 현재노드까지의 경로를 표시하고 각 노드마다 '/' 로 구분. 얘는 진짜 괄호임.




- Sub query
  - 단일행 서브트리, 다중행 서브트리. 단일열 서브터리, 다중열 서브트리.
  - 다중행 서브트리에다가 단일행 연산자(=, <, >) 사용하면 안되고,
  - 단일행 서브트레아다가 다중행 연산자(in, all, ans/some, exists)는 사용할 수는 있지만, 안사용하는게 좋고.
  - view : 가상 테이블. 실제 데이터를 저장하는게 아니라, sql문을 text로 저장해뒀다가, view를 사용하면 그 저장된 sql문을 쏴주는거임.
  - view 장점 
    - 독립성 : 애플리케이션이 테이블을 직접 접근하지 않고 view를 접근하게 해놓은 상태인데, 테이블 구조를 바꾼다면, 애플리케이션을 수정하지 않고 view만 수정하면 돼
    - 편리성 : 보기 깔끔해
    - 보안성 : DB 테이블을 넘기는것보다 보여주지 말아야할것은 걸러서 view 담아서 view를 넘겨주면 보안에 좋겠지.
  - create view 해서 저장해놓은 뷰를 정적뷰. 그냥 from 절에다가 서브쿼리 처럼 사용하면 그 sql문 쓰고 사라지기에 동적뷰라 함.
  - Q. 신장이 가장 큰 선수의 정보 조회하여라.
    <details>
    <summary>정답</summary>

    ```
    select player_name, height 
    from player 
    where height = (select max(height) from player);
    ```
    </details>
  - Q. 사번 7499인 직원의 매니져를 사번 7369인 직원의 매니져로 변경하여라.
    <details>
    <summary>정답</summary>
    
    ```
    update emp 
    set mgr = (select mgr from emp where empno = 7369)
    where empno = 7499;
    ```
    </details>
    
  - Q. 부서별로 최고 급여를 받는 사원의 사원명, 부서번호, 급여를 출력하여라
    <details>
    <summary>연관 서브쿼리를 활용한 정답</summary>
    
    ```
    select ename, deptno, sal
    from emp m
    where sal = (select max(s.sal)
                    from emp s
                    where m.deptno = s.deptno);
    ```
    </details>
    <details>
    <summary>group by를 활용한 정답</summary>
    
    ```
    select ename, deptno, sal
    from emp
    where (deptno, sal) in (select deptno, max(sal)
                            from emp
                            group by deptno);
    ```
    </details>  
  - Q. 부서의 평균 급여보다 더 높은 급여를 받는 사람의 이름과 급여를 출력하라
    <details>
    <summary>정답</summary>
    
    ```
    select ename, sal
    from emp m
    where sal > (select avg(sal) 
                from emp s 
                where m.deptno = s.deptno);
    ```
    - 얘는 위의 문제 처럼 Group by를 사용할 수 없는데, group by를 활용한 서브 쿼리의 경우 다중행 서브쿼리가 되며, 다중행 연산자중 이상을 표현할 수 있는 연산자가 없음.
    - 그래도 이해가 안간다면 그룹바이로 만든다음에 서브쿼리 찍어봐.
    </details>
  - Q. 사원과 부서 테이블로부터 사원번호, 사원명, 부서번호, 부서명을 추출한 뷰 V_EMP_DEPT를 작성하시오.
    <details>
    <summary>정답</summary>
    
    ```
    create view V_EMP_DEPT as
    select empno, ename, dept.deptno, dept.dname
    from emp JOIN dept
    on emp.deptno = dept.deptno;
    ```
    </details>
    

- Multi-Row Function
  - 집계 함수 : sum, avg, count, max, min
  - group by로 그룹화 한것에 대해 집계를 내릴 수 있어.
  - 연산할때 null 포함안되고, 특히 avg 구할때 분모로 안들어감
  - group by 절이 없지만 select 문에 집계 함수가 들어간다는것은 from에 명시된 테이블을 하나의 그룹으로 본다는 의미로 해석해도 될듯
  - **실행 순서 및 프로세스**
    - from : 테이블 선택
    - where : 조건에 알맞는 행들만 선택
    - group by : 행 그룹화
    - having : 조건에 알맞는 그룹들만 선택
    - select : 칼럼별로 연산이 필요할 시 연산
    - order by : 선택된 데이터들을 정렬
  - Q. 포지션별 키의 평균을 출력하되, 해당 포지션의 키의 최대값이 190cm 이상인 경우에만 출력
    <details>
    <summary>정답</summary>
    
    ```
    select round(avg(height), 2) 포지셔별평균키
    from player
    group by position
    having max(height) >= 190;
    ```
  - Q. 부서이름, 직무, 부서의 직무별 직원수, 부서의 직무별 급여합을 출력하라.
    <details>
    <summary>정답</summary>
    
    ```
    select dname, job, count(*) 직원수, sum(sal) 급여합
    from emp join dept
    on emp.deptno = dept.deptno
    group by dept.dname, emp.job;
    ```
  - Q. 키가 가장 작은 3명의 선수를 출력하라.
    <details>
    <summary>정답</summary>
    
    ```
    select player_name, height, rownum, orgno
    from (select player_name, height, rownum as orgno from player order by height)
    where rownum < 4;
    ```
  - Q. 키가 가장 큰 3명의 선수를 출력하라.
    <details>
    <summary>정답</summary>
    
    ```
    select player_name, height, rownum, orgno
    from (select player_name, height, rownum as orgno from player where height is not null order by height desc)
    where rownum < 4;
    ```
  
                     
- Multi-Row Function (고급)
  - 집계 함수 : rollup, cube, grouping sets + over [partition by 절][order by 절][windowing 절]
    - rollup(칼럼1, 칼럼2) : 칼럼1 그룹핑 집계, 칼럼1+칼럼2 그룹핑 집계, 전체 그룹핑 집계
    - cube(칼럼1, 칼럼2) : 칼럼1 그룹핑 집계, 칼럼2 그룹핑 집계, 칼럼1+칼럼2 그룹핑 집계, 전체 집계 // 모든 조합에 따라 그룹핑해서 집계
    - grouping sets(칼럼1, 칼럼2) : 칼럼1 그룹핑 집계, 칼럼2 그룹핑 집계
    - 주의점 : partition by, order by, windowing 절 사이사이에 습관적으로 ',' 찍지마! 
    - grouping(칼럼) : 해당 칼럼이 그룹핑된 집계값을 나타낸 행이라면 숫자1, 아니라면 0표시. 이걸 통해 case문 작성해서 null값으로 나오는걸 조금 더 깔끔하게 표현할 수 있어.
  - rank(), dense_rank(), row_number()
  - windowing 절 : rows between 1 preceding and 1 following;, range between 50 preceding and 150 following;, range unbounded preceding;
  - 행 순서 윈도우 함수 : first_value, last_value, lag, lead
  - 비율 윈도우 함수 : ratio_to_report, percent_rank, cume_dist, ntile
  - Q. 부서이름, 직무, 부서의 직무별 직원수, 부서의 직무별 급여합을 구하여라
    <details>
    <summary>정답</summary>
    
    ```
    select dname, job, count(*) 직원수, sum(sal) 급여합
    from emp join dept
    on emp.deptno = dept.deptno
    group by rollup(dname, job)
    order by dname, job;
    ```
  - Q. Job, ename, sal, 전체 급여 순위, JOB 내에서의 급여 순위를 출력하라
    <details>
    <summary>정답</summary>
    
    ```
    select job, ename, sal, rank() over (partition by job order by sal desc) as JOB에서의_순위, rank() over (order by sal desc) as 전체급여순위
    from emp
    order by sal desc;
    ```
  - Q. 각 직원이 속한 집업 내에서 급여의 최대값을 함께 출력하라
    <details>
    <summary>정답</summary>
    
    ```
    select job, ename, sal, max(sal) over (partition by job) job_max
    from emp
    order by job, ename;
    ```
  - Q. job = "saleman"인 모든 직원에 대해서, 급여 기준 본인 바로 윗 사람의 급여와 아랫사람의 급여를 출력하는 질의를 완성하시오. 없다면 0으로 채우고.
    <details>
    <summary>정답</summary>
    
    ```
    select ename, sal,
        lag(sal, 1, 0) over (order by sal desc) as 윗사람급여, lead(sal, 1, 0) over (order by sal desc) as 아랫사람급여
    from emp
    where job = 'SALESMAN';
    ```
  - Q. 전체 사원을 급여 순으로 정렬하고, 급여 기준 4개의 그룹으로 분리하는 질의를 자성하시오.
    <details>
    <summary>정답</summary>
    
    ```
    select ename, sal, ntile(4) over (order by sal desc)
    from emp
    order by sal desc;
    ```
      

- PL/SQL
  - 절차형 SQL. 지금까지 배운 sql문들은 서로 이어지지 않으니까, 변수나 상수를 두어 이어지게 만드는거야.
  - 직접 코딩할 정도까지는 아니더라도 주요 3가지 요소인 '프로시저', '함수', '트리거'를 구분할 줄은 알아야돼.
  - 프로시저 : 명시적인 return이 없고, in 변수 out 변수는 있어서, out변수에다가 담으면 돼. 직접 execute 키워드로 호출함.
  - 함수 : 명시적인 return이 있음. 직접 호출함
  - 트리거 : 직접 호출하는것은 아니고, DML이 변경될때 해당 조건에 맞는 상황이면 실행이됨. 트리거를 통해 실행되는 트랜잭션도 하나의 덩어리로 볼 수 있기에 rollback을 하게 되면 전체가 롤백됨.
      따라서 트리거 몸통에 commit과 rollback은 따로 쓸 수 없어. 중간에 커밋되면, 전체 롤백 못하잖아. 
     - after, before : 트리거를 언제 실행할것인가
     - new, old : 트리거 이후 값인가 이전 값인가
     - for each row : 각각의 행마다 할것인가 전체로 할것인가.


- sql 최적화 기본
  - 지금까지는 어떠한 데이터를 select할것인가였는데, 여기서는 어떻게 select 할것인가. 에 대한 내용
  - optimizer, index, join 기법
  - optimizer : 규칙기반, 비용기반이 있어. 
      - 규칙기반 : 쭉 1번부터 14번까지인가 쭉 우선순위가 적혀있는 규칙을 기반으로 탐색 선택을 하게 됨.
      - 비용기반 : 시간이 얼마나 걸렸는가 통계를 기반으로 index를 찾게 되고, 사실 대부분 이걸 쓰게 됨. 너무도 상황이 다양한데 그것에 일반화된 규칙을 적용하기에는 한계가 있기에.
  - index : B-tree 구조로 저장됨. 리프노드 끼리 양방향 이동이 가능해서, 범위 탐색할때도 유용하네.
  - join 기법 : 튜닝을 위한 big3 join 기법이 있어
      - NL join : 선행테이블에서 조건에 맞는 행 선택하고, 그 행 별로 후행테이블과 매칭해서 맞나 안맞나 확인하는거. 마치 2중 for문 처럼. 소규모 데이터에 유리한 기법
      - sort merge join : 선행테이블 정렬, 후행테이블 정렬, 그 다음에 조건에 맞게 병합. 정렬하는데 비용이 들기에 소규모 데이터에는 불리. non equi join도 가능.
      - hash join : 해쉬 함수 이용함. 대규모 데이터에 유리. equi join 에만 가능.

      
- fetch
  - oracle 12부터 작동, order by 절 뒤에 붙음. rownum으로 페이징하는거 안해도 됨!
  - fetch next 10 rows only; : 위에서 10개 열 보여줌
  - offset 10 rows fetch next 10 rows only; : 위에 10개 열 스킵하고 그 다음 10개열 보여줌
  - fetch next 10 rows with ties: : 맨 마지막열과 동률인 애들도 다 보여줌.
  - https://rimkongs.tistory.com/158      
      
      
- [정처기 공부할때 추가로 정리한거](https://github.com/Joshua-Shin/JungchugiSummary/blob/main/README.md#3%EA%B3%BC%EB%AA%A9-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EA%B5%AC%EC%B6%95)

- SQL 코테 보기전에 주의할점
  - 컴파일러 언어 설정이 oracle인지 mysql인지 확실히 확인!
  - 클로즈 북이면 일단 외워
    - offset 10 rows fetch next 10 rows only;
    - not in : not exists랑 null을 대하는 로직이 다른데, id값은 어차피 null이 아니게 주여지니까 그냥 사용해도 상관 없음
    - to_date('2022-02-01', 'yyyy-mm-dd')
    - to_char(date, 'yyyy-mm-dd')
  - Q. 문자열이 알파벳+숫자로 구성되어있다. 숫자부분을 4자리 숫자보다 적을 경우 앞에 0을 붙여 4자리를 완성하여 최종 문자열을 출력하라. 
    - 예) asdfqwer123 -> asdfqwer0123, asdfqwer12 -> asdfqwer0012
    - A. select regexp_replace(name, '[^a-zA-Z]') || lpad(regexp_replace(name, '[^0-9]'), 4, '0') from table;
      - lpad(문자열, 전체길이, 채울문자) : 문자열이 전체 길이에 부족한 만큼 왼쪽에 채울문자를 채워줌.
