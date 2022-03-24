My sql
======
![sakila](https://user-images.githubusercontent.com/74452873/155268107-659cf71c-9f86-46de-b698-69f806f27648.png)


[google](www.google.com)

http://www.jooq.org/img/sakila.png
```
-- ex1) 주석 : (--) /**/
-- ex2) 실행단축키(Command + Enter, Command+Shift+Enter)

-- ex3) DB 생성
create database db01;
-- mysql에서는 schema를 database라고 사용한다
-- 원래 schema는 데이터의 속성, 즉 데이터를 사용할 때 정해놓은 규칙을 의미한다

-- ex4) DB 리스트 확인
show databases;

-- ex5) DB 삭제
drop database db01;

-- ex6) Table 생성
use db01;   -- db01을 사용하겠다는 명령
CREATE TABLE table01 (
  id INT NOT NULL AUTO_INCREMENT,
  name VARCHAR(45), -- char는 미리 메모리를 선언해놓고 쓰는데, varchar는 max값이라서 16으로 선언하고 2만 쓰면 메모리는 2만 쓰게 된다 => 하지만 실행속도가 다르기 때문에 고민해서 선택
  age INT,
  salary INT,
  PRIMARY KEY (id));
  
-- ex7) Table 확인
show tables;
  
-- ex8) columns 확인
desc table01; -- (description)
  
-- ex9) Table 삭제
drop table table01;
  
-- ex10) Create
-- insert into 테이블명 values(값,);
insert into table01 values(null, '이순신', 20, 500);
insert into table01 values(null, '가나다', 30, 300);
insert into table01 values(null, '라마바', 40, 400);
insert into table01 values(null, '사아자', null, 600);
  
-- ex11) Read: 조회, 검색
-- select * from 테이블명;
select * from table01;

-- ex12) Update
-- 옵션 체크: Edit > Preference > SQL Editor > 맨밑에 Safe Updates 체크박스 해제
update table01 set age = 99;
update table01 set age = 100 where name = '라마바';
update table01 set name = '소나무' where id =4;
update table01 set name = '소나무', age = 888 where id =4;
 
-- ex13) Delete
delete from table01 where name = '이순신';
select * from table01;

-- ex14) CRUD
insert into table01 values(null, '이순신', 20, 500);
select * from table01;
update table01 set age = 100 where name = '라마바';
delete from table01 where name = '이순신';

-- ex15)
-- 모두 채울 때(묵시적 방법)
insert into table01 values(null, '이순신', 20, 500);
-- 선택적으로 채울 때(명시적 방법)
insert into table01(id, age) values(null, 20);

-- ex16) WorkBench에서 n개의 row를 삭제할 때
-- 우클릭을 이용하여 ctrl 사용하여 n개 삭제 가능 => 반드시 apply 적용하기alter

-- ex17) row의 개수를 출력
 select count(*) from table01;

-- ex18) sakila db에서 actor 테이블 보고, count하기
select * from actor;
select count(*) from actor;
-- 검색 로우의 개수가 다른것을 검색으로 같이 사용할 수 없다.
select count(*), city_id from city;

-- ex19) 출력 개수 제한
select * from actor limit 6; -- 0번부터 6개
select * from actor limit 0, 6; -- 0번부터 6개

-- ex20) 선택적으로 row 검색
select first_name from actor;

-- ex20) 선택컬럼에 산술식을 사용할 수 있다
select name, salary*0.1 from table01;

-- ex21) 컬럼명에 별칭 사용
select name as 이름, salary as 급여 from table01;

-- as는 생략할 수 있다
select name 이름, salary 급여 from table01;

-- 별칭사이에 공백이 들어 가는 경우 백틱 사용
select name `이 름`, salary `급 여` from table01;

-- 단순 쿼리 정리
select name `이 름`, salary * 12 `연       봉` from table01;

-- ex22) 컬럼 연결이 필요한 경우(컬럼 + 컬럼 >> 컬럼컬럼)
select concat(name, salary) 이름급여 from table01;
select concat(name, '님의 급여는', salary, "만원입니다") 이름급여 from table01;
select concat('님의 급여는', concat(salary, "만원입니다")) 이름급여 from table01;

-- ex23) 단순 산술식이나 날짜를 얻고 싶을 때
select 3 * 5 from dual;   -- dual이라는 row가 한개뿐인 가상 테이블을 생성하여 결과를 보여줌.
select sysdate() from dual;
select 9+2, 9-2, 9*2, 9/2, 9%2 from dual; -- /는 몫값이 아니라 실제값이 나온다.

-- ex24) null 검색
select * from table01 where salary = null;  -- 아 주 조 심 해 야 함.(안 됨)
select * from table01 where salary is null;
select * from table01 where salary is not null; 
select name from table01 where salary is not null;
select name `급여를 받는 사람` from table01 where salary is not null;

-- ex25) 직원들의 연봉을 출력
CREATE TABLE table02(
id INT NOT NULL auto_increment,
name varchar(16),
salary int,
bonus int,
primary key(id));


insert into table02 values(null, '호랑이1', 100, 20);
insert into table02 values(null, '호랑이2', 200, null);
insert into table02 values(null, '호랑이3', 300, 40);
insert into table02 values(null, '호랑이4', 400, 30);
insert into table02 values(nul.l, '호랑이5', 500, null);
select salary * 12 + bonus from table02;   -- 원하는 결과가 나오지 않는 쿼리. ( 보너스가 null인사람은 연봉도 null로 나와버림.)
select salary * 12 + ifnull(bonus, 0) from table02;    -- 만약 bonus가 null이라면 0을 넣어줘라 ~ 라는 뜻.


-- ex26) table02의 bonus가 null인 컬럼을 0으로 수정(갱신)하세요.
update table02 set bonus = 0 where bonus is null;
select * from table02;

-- ex27) 4.5점만점의 학점을 100점기준으로 환산 결과를 출력.
CREATE TABLE table03 (
id INT NOT NULL AUTO_INCREMENT,
name varchar(16),
result float,
primary key(id));

insert into table03 values(null, '호랑이1' , 4.5);
insert into table03 values(null, '호랑이2' , 3.7);
insert into table03 values(null, '호랑이3' , 2.7);
insert into table03 values(null, '호랑이4' , 4.2);
insert into table03 values(null, '호랑이5' , 2.2);
select * from table03;
select result * 100/ 4.5 `100점대 환산` from table03;

-- ex28) 중복제거가 된 결과를 나타내기.
-- 나라, 지역, 학과, 등
CREATE TABLE table04 (
id INT NOT NULL AUTO_INCREMENT,
name varchar(16),
contury varchar(15),
primary key(id));

insert into table04 values(null, '호랑이1', '한국');
insert into table04 values(null, '호랑이2', '중국');
insert into table04 values(null, '호랑이1', '중국') ;
insert into table04 values(null, '호랑이2', '한국');
insert into table04 values(null, '호랑이3', '일본');
insert into table04 values(null, '호랑이4', '한국');
insert into table04 values(null, '호랑이2', '일본');
select * from table04;
select distinct name from table04;
-- 중복제거된 목록
select distinct contury from table04;
-- 중복제거된 목록의 개수 
select count(distinct(contury)) from table04;

-- ex29) 
CREATE TABLE table05 (
id INT NOT NULL AUTO_INCREMENT,
contury varchar(15),
gold_num int,
silver_num int,
primary key(id));
drop table table05;

insert into table05 values(null, '한국', 10, 10);
insert into table05 values(null, '중국', 5, 9);
insert into table05 values(null, '일본', 5, 8);
insert into table05 values(null, '미국', 4, 3);
insert into table05 values(null, '독일', 4, 7);


-- select * from table05 order by 컬럼;
-- asc 는 오름차순 ( 생략 가능 ) : 오름차순 , 순차정렬
select * from table05 order by gold_num asc;
select * from table05 order by gold_num desc;   -- 내림차순 desc.
select * from table05 order by 3 desc;  
select * from table05 order by gold_num desc, silver_num desc;  

-- ex30) 정렬
CREATE TABLE table06 (
id INT NOT NULL AUTO_INCREMENT,
name varchar(10),
dept varchar(10),
salary int,
primary key(id));
insert into table06 values (null, '호랑이1', '경영', 500);
insert into table06 values (null, '호랑이2', '개발', 150);
insert into table06 values (null, '호랑이3', '개발', 300);
insert into table06 values (null, '호랑이4', '개발', 700);
insert into table06 values (null, '호랑이5', '영업', 500);
insert into table06 values (null, '호랑이6', '경영', 350);
insert into table06 values (null, '호랑이7', '개발', 250);
insert into table06 values (null, '호랑이8', '영업', 450);
insert into table06 values (null, '호랑이9', '개발', 550);
select * from table06;
select * from table06 order by dept asc, salary desc; -- 부서별로 오름차순 -> 동일한 결과에 대하여 salary로 내림차순.
select name, avg(salary) 평균 from table06 group by name;  

-- ~~별로 (나라별로, 부서별로, 성별로, 학과별로 ) 
-- 1. order by ( 일반적으로 단순 검색에 해당 )		2. group by( 통계자료를 정렬 하고 싶을 때 ) 



-- ex31 정렬)
CREATE TABLE table07 (
id INT NOT NULL AUTO_INCREMENT,
name varchar(15),
kor int,
eng int,
mat int,
primary key(id));

insert into table07 values ( null, '호랑이1', 66 , 78 , 80 );
insert into table07 values ( null, '호랑이2',80 , 79 , 92 );
insert into table07 values ( null, '호랑이3',98 , 94 , 100 );
insert into table07 values ( null, '호랑이4',77 , 67 , 82 );
insert into table07 values ( null, '호랑이5',75 , 86 , 90 );
select * from table07;
select name, kor+eng+mat total
from table07 order by total desc;

-- ex32) 대소 비교( =, !=, >, <=, <, <=)
select * from table07 where kor >= 80;
select name, (kor+eng+mat)/3 evg from table07 where (kor+eng+mat)/3 >= 80;
select name, (kor+eng+mat)/3 evg from table07 aa where avg >= 80;   -- where 에서는 select에서 정의된 별칭을 사용할 수 없다.

-- ex33) and, or 사용 alter
CREATE TABLE table08 (
id INT NOT NULL AUTO_INCREMENT,
name varchar(15),
dept varchar(10),
salary int,
primary key(id));

insert into table08 values(null,'호랑이1','경영',100);
insert into table08 values(null,'호랑이2','개발',150);
insert into table08 values(null,'호랑이3','영업',300);
insert into table08 values(null,'호랑이4','개발',700);
insert into table08 values(null,'호랑이5','영업',500);
insert into table08 values(null,'호랑이6','영업',500);
select id, name, dept, salary --   *대신에 컬럼명을 명시하는게 좋음.
from table08
where dept = '영업' and salary > 350;



select id, name, dept, salary --   *대신에 컬럼명을 명시하는게 좋음.
from table08
where dept = '영업' or salary < 160;

select id, name, dept, salary --   *대신에 컬럼명을 명시하는게 좋음.
from table08 
where salary >= 140 and salary <= 350;

-- salary < A or salary > B;
select id, name, dept, salary --   *대신에 컬럼명을 명시하는게 좋음.
from table08 
where salary >= 140 and salary <= 350;


-- ex34)
CREATE TABLE table09 (
id INT NOT NULL AUTO_INCREMENT,
name varchar(15),
salary int,
primary key(id));

insert into table09 values(null,'스타벅스',100);
insert into table09 values(null,'올스타노래방',150);
insert into table09 values(null,'강남만두',300);
insert into table09 values(null,'스타일미용실',700);
insert into table09 values(null,'닭다리스타',500);
insert into table09 values(null,'짬뽕천국',500);

select * from table09 where name like '스타%';

select * from table09 where name like '%스타';

select * from table09 where name like '%스타%';

select count(name) from table09 where name like '스타%';

-- ex36) 1학년 중에서 김씨는 몇명인가 ??
CREATE TABLE table10 (
id INT NOT NULL AUTO_INCREMENT,
name varchar(15),
grade int,
primary key(id));

insert into table10 values(null,'김유신',1);
insert into table10 values(null,'김서방',2);
insert into table10 values(null,'이순신',1);
insert into table10 values(null,'강감찬',3);
insert into table10 values(null,'홍길동',1);
insert into table10 values(null,'김국진',1);
insert into table10 values(null,'김김',2);
insert into table10 values(null,'김최박이',10);
select * from table10;

select * from table10 where name like '김%' and grade = 1;

-- ex 37) 성,이름이 2글자인 사원 검색.
select * from table10 where name like '__';

-- ex38) 성 + 이름이 3글자가 아닌 사람 검색
select * from table10 where name not like '___';

-- ex39) BETWEEN
CREATE TABLE table11 (
id INT NOT NULL AUTO_INCREMENT,
name varchar(15),
dept varchar(10),
salary int,
primary key(id));

insert into table11 values(null,'호랑이1','경영',100);
insert into table11 values(null, 	'호랑이2','개발',150);
insert into table11 values(null,'호랑이3','영업',300);
insert into table11 values(null,'호랑이4','개발',700);
insert into table11 values(null,'호랑이5','영업',500);
insert into table11 values(null,'호랑이6','영업',500);

-- or 2개보다 가독성이 좋아서 BETWEEN을 사용한다.
select *
from table11 
where salary BETWEEN 200 and 600;

-- ex40) in
select *
from table11
where dept = '개발' or dept = '경영';

select * -- 위의 것과 같음.
from table11
where dept in('개발', '경영');

-- ex41)

select *
from table11
where sal > ANY( 300, 200, 250 );
-- 번역 결과 : where sal > 200 );

-- ex1
where salary > ANY( 300, 200, 250 );
-- 번역 결과 : where sal > 200 );

-- ex2
where salary > ALL( 300, 200, 250 );
-- 번역 결과 : where sal > 300 );

-- ex3
where salary < ANY( 300, 200, 250 );
-- 번역 결과 : where sal < 300 );

-- ex4
where salary < ALL( 300, 200, 250 );
-- 번역 결과 : where sal < 200 );


-- ex42)


-- 문제) 20번 부서의 최고 월급 보다 작은 연봉을 받는 직원을 검색하세요.
CREATE TABLE table12 (
id INT NOT NULL AUTO_INCREMENT,
eno int,			-- 부서번호
name varchar(15),   -- 이름
salary int,			-- 급여
primary key(id));

insert into table12 values ( null, 10, 'tiger1', 100) ;
insert into table12 values ( null, 20, 'tiger2', 200) ;
insert into table12 values ( null, 30, 'tiger3', 300) ;
insert into table12 values ( null, 40, 'tiger4', 400) ;
insert into table12 values ( null, 10, 'tiger5', 500) ;
insert into table12 values ( null, 20, 'tiger6', 600) ;
insert into table12 values ( null, 30, 'tiger7', 700) ;
insert into table12 values ( null, 10, 'tiger8', 800) ;
insert into table12 values ( null, 20, 'tiger9', 350) ;
insert into table12 values ( null, 30, 'tiger10', 999) ;

select * from table12 where eno =20;

-- 20번 부서의 급여를 모두 검색.
select salary from table12 where eno = 20;

-- 20번 부서의 최고 급여는 얼마인가?
select max(salary) 덜덜 from table12 where eno = 20; 

-- 20번 부서의 최고 급여를 받는 직원이 이름은 무엇입니까?
select name from table12 where salary = 600;

-- 변형, 20번 부서의 최고 급여자.
select name from table12 where salary = (select max(salary) 덜덜 from table12 where eno = 20);

-- 변형, 20번 부서의 최고 급여자 보다 높은 급여를 받는 사람.
select name from table12 where salary > (select max(salary) 덜덜 from table12 where eno = 20);

-- 변형 , 20번 부서의 최고 급여자 보다 낮은 급여를 받는 사람. ( 부서 = 20 제외 )
select name ,eno from table12 where salary < any(select salary from table12 where eno = 20) and eno != 20;

-- 유사 예제
-- 문제. 컴공과에서 제일 낮은 점수를 받은 학생보다 성적이 낮은 학생들은 누구인가?
/*
컴공 40 
컴공 20 
컴공 50  
중국 80 
중국 70 
중국 10 
일어 40 
일어 05 
*/
-- ex)42
select  min(salary) from table12;
select name  from table12 where salary = (select max(salary) from table12);


-- ex43) join : 2개 이상의 테이블을 합병 시키는 것.
-- 1. 무슨무슨 조인이라는 이름이 8개 정도 된다.
-- 2. 오라클 join문법, Ansi join 문법 . . . 

-- ex44) 교차조인(CROSS JOIN) ": 데카르트 곱, 카타시안 곱.
-- A table의 로우가 3개, B table의 로우가 4개



DROP TABLE tableA;
create table tablea(
id int not null auto_increment,
name varchar(10),
eno int,
salary int,
primary key(id));

insert into tablea values(null, '홍길동', 20, 1000) ;

insert into tablea values(null, '이순신', 10, 2000) ;
insert into tablea values(null, '안중근', 30, 3000) ;
insert into tablea values(null, '임꺽정', 20, 4000) ;

select * from tablea;



DROP TABLE tableb;
create table tableb(
id int not null auto_increment,
eno int,					-- 부서번호
job varchar(10),
primary key(id));

insert into tableb values(null,  10, '장군') ;
insert into tableb values(null,  20, '의적') ;
insert into tableb values(null,  30, '의사') ;
select * from tableb;


-- 교차 조인(일반적인 조인방식)
select * from tablea, tableb;

-- 교차 조인 (Sndi join 방식)
select * from tablea cross join tableb;

-- ex45) 내부 조인 ( 외부조인이 아닌 것 )  . . . 반드시 조건을 같이 쓴 다는 전제가 있다 ! 
select * from tablea
	inner join tableb;
-- 위의 문장은 처음부터 사용을 잘못한 조인.

-- 정상적인 내부 조인 문법  ( 정석 코드 )

-- 등가조인이라고 한다. ( = )
select * from tablea
	inner join tableb
    on tablea.eno = tableb.eno   -- 조인 조건
	where salary >= 3000; 		 -- 필터 조건 , 검색 조건

-- ex46) 일반조인 ( 실전 코드 )
select * from tablea, tableb
where tablea.eno = tableb.eno -- 조인 조건 
and salary >= 3000; 		   -- 필터 조건, 검색 조건


-- 확인 : 교차 조인

-- ex47) 테이블 이름에 별칭을 사용할 수 있다.
-- 정확하게는 별칭 보다는 리네임이다.  -> 이후 t1, t2만 써야함 .
select * from tablea t1, tableb t2
where t1.eno = t2.eno -- 조인 조건 
and salary >= 3000; 		   -- 필터 조건, 검색 조건
-- and t1. salary >= 3000; 		   -- 중복되는 컬럼명이면 앞에 테이블을 붙여주어야함 !

-- id는 컬럼명이 중복되므로 별칭 명시 필수.

-- ex48) 사용을 잘못하고 있다.
select * from tablea
	cross join tableb
	on tablea.eno = tableb.eno;

-- ex49)
-- 조인조건이 없음 - - > 교차 조인이 일어난다.
-- 조인결과를 필터 조인하므로 매우 좋지 않은 쿼리문.
select * from tablea
	inner join tableb
where tablea.eno = tableb.eno;

-- ex50) 이순신의 직업은 무엇인가  ?
-- 등가 조인 사용시
select name, job 
from tablea t1, tableb t2
where t1.eno = t2.eno
and t1.name = '이순신' ;

-- 서브쿼리를 이용하는 방법
select job
 from tableb
 where eno = (
 select eno 
 from tablea 
 where name = '이순신');



-- ex51) ~ 대학에서 2학점인 과목을 검색하고, 이 과목을 담당하는 교수를 검색해라.
drop table tableb;
create table tablea(
id int not null auto_increment,
pno int,				-- 교수번호
name varchar(10), 		-- 교수이름
primary key(id));

insert into tablea values (null, 100, '홍길동');
insert into tablea values (null, 101, '이순신');
insert into tablea values (null, 102, '안중근');
insert into tablea values (null, 103, '임꺽정');
insert into tablea values (null, 104, '강감찬');

-- 과목 테이블
create table tableb(
id int not null auto_increment,
name varchar(10),		-- 과목명
num int, 				-- 학점
pno int,				-- 담당교수번호
primary key(id));

insert into tableb values (null, '국어',4, 103);
insert into tableb values (null, '영어',3, 104);
insert into tableb values (null, '수학',2, 102);
insert into tableb values (null, '사회',1, 101);
insert into tableb values (null, '체육',2, 103);
insert into tableb values (null, '생물',2, 102);

select * from tablea t1
 inner join tableb t2
 on t1.pno = t2.pno
 where num = 2;
 
 
 create table tablea(
id_a int not null auto_increment,
eno int,				-- 학번
name varchar(10),		-- 학생 이름
major varchar(10),		-- 학과명
year int,				-- 학년
primary key(id_a));

 create table tableb(
id_b int not null auto_increment,
eno int, 		-- 학번
cno int,		-- 시험과목번호
result int,		-- 점수
primary key(id_b));

drop table tablea;
drop table tableb;

insert into tablea values ( null, 1000, '홍길동', '국어', 1 );
insert into tablea values ( null, 1001, '이순신', '화학', 1 );
insert into tablea values ( null, 1002, '안중근', '화학', 2 );
insert into tablea values ( null, 1003, '임꺽정', '국어', 2 );
insert into tablea values ( null, 1004, '강감찬', '화학', 1 );


insert into tableb values( null, 1000, 10 , 59 ) ;
insert into tableb values( null, 1000, 20 , 34 ) ;
insert into tableb values( null, 1001, 10 , 80 ) ;
insert into tableb values( null, 1001, 20 , 79 ) ;
insert into tableb values( null, 1001, 30 , 33 ) ;
insert into tableb values( null, 1002, 20 , 48 ) ;
insert into tableb values( null, 1003, 30 , 55 ) ;
insert into tableb values( null, 1004, 10 , 99 ) ;


-- 화학과 1학년 학생들의 점수를 검색하세요.
select name,result from tablea t1
 inner join tableb t2
 on t1.year = 1 and t1.eno = t2.eno
 where major = '화학';
 
 
 
-- ex53) 양쪽테이블에서 컬럼명이 동일한것이 1개이상 있다는 전제 --> 자연조인을 사용 가능.
select * from tablea
	natural join tableb;



-- ex54) Using 조인 : 컬럼명이 어쩔 수 없이 2개 이상 중복된 경우의 조인
-- 컬럼을 콕 찝어서 조인 시킬 수 있다.

select * from tablea
	join tableb using (eno)
where year = 1;

-- 비등가조인 : (=) 을 제외한 비교연산
drop table tableb;
create table tablea(
id_a int not null auto_increment,
name varchar(10),		-- 이름
eno int,				-- 부서번호, 직급번호
salary int,				-- 급여
primary key(id_a));		
-- 등급테이블
create table tableb(
id_a int not null auto_increment,
grade varchar(10),		-- 등급
losalary int,				-- 최소값 설정
hisalary int,				-- 최대값 설정
primary key(id_a));		

insert into tableb values (null, 'A' , 300, 9999);

insert into tableb values (null, 'B' , 200, 299);

insert into tableb values (null, 'C' , 100, 199);

insert into tableb values (null, 'A' , 0, 99);

insert into tablea values (null, '홍길동', 20, 50);
insert into tablea values (null, '이순신', 10, 150);
insert into tablea values (null, '안중근', 30, 250);
insert into tablea values (null, '임꺽정', 20, 350);
insert into tablea values (null, '김서방', 20, 170);
insert into tablea values (null, '강감찬', 20, 370);

-- ex1)
select name, grade from tablea t1
inner join tableb t2
on t1.salary >= t2.losalary and t1.salary >= t2.losalary
and t1.salary <= t2. hisalary;
 
 -- ex2)  ex1과 같은결과이면서 더 널리 쓰이는 방식.

 select name, grade from tablea t1
inner join tableb t2
on salary between t2.losalary and t2.hisalary;
 
-- ex3) 일반 조인
 select name, grade from tablea t1 , tableb t2
where salary between t2.losalary and t2.hisalary; 
 
-- ex4) 
 select name, grade from tablea t1, tableb t2
where salary between t2.losalary and t2.hisalary
and gread = 'A';

-- ex55) 셀프 조인 : 동일 테이블끼리 조인
drop table tablea;
create table tablea(
id_a int not null auto_increment,
name varchar(10),		-- 이름
eno int,				-- 부서번호, 직급번호
mgr int,				-- 사수번호, 멘토번호
salary int,				-- 급여
primary key(id_a));		

insert into tablea values (null, '홍길동', 1000, null, 100); -- 사수 없음
insert into tablea values (null, '이순신', 1001, 1000, 100);
insert into tablea values (null, '안중근', 1002, 1001, 100);
insert into tablea values (null, '임꺽정', 1003, 1002, 100);
select * from tablea;

-- ex1) 
-- 반드시 별칭이 있어야 한다.
select * from tablea t1 , tablea t2;

-- ex2)
select * from tablea t1 , tablea t2
where t1.eno = t2.mgr;

-- ex3) 셀프조인
select * from tablea t1 , tablea t2
where t2.mgr = t1.eno;

-- ex56) 동명이인 검색
drop table tablea;
create table tablea(
id_a int not null auto_increment,
name varchar(10),		-- 이름
primary key(id_a));	

insert into tablea values  ( null , '홍길동' );
insert into tablea values  ( null , '이순신' );
insert into tablea values  ( null , '안중근' );
insert into tablea values  ( null , '임꺽정' );
insert into tablea values  ( null , '김서방' );
insert into tablea values  ( null , '이순신' );
insert into tablea values  ( null , '김서방' );
insert into tablea values  ( null , '강감찬' );
insert into tablea values  ( null , '이순신' );
select * from tablea;

-- ex1) 아이디가 다르면서 동명이인
select * from tablea t1 , tablea t2
where t1.id_a != t2.id_a
and t1.name = t2.name;
-- 중복이 발생함 ...

-- ex2) 중복제거.
select distinct(t1.name) from tablea t1 , tablea t2
where t1.id_a != t2.id_a
and t1.name = t2.name;

-- ex57) 세미조인( != 안티조인 ) : 메인쿼리에서 로우를 하나씩 가져와서
-- 				  				서브쿼리에서 EXISTS로 존재여부를 묻는 조인
/*
메인쿼리		서브쿼리
메뉴테이블		판매테이블
짜장			짜장
짬뽕			짜장
냉면			짬뽕
			짜장
            짬뽕
*/
create table menu(
foodnum int,
name varchar(20)
);

create table sell(
no int, 	-- 판매번호
count int,	-- 판매수량
foodnum int	-- 판매된 음식번호
);

insert into menu values(1, '그거');
insert into menu values(2, '저거');
insert into menu values(3, '이거');
insert into menu values(4, '새거');
insert into menu values(5, '헌거');

insert into sell values(1, 2, 1);
insert into sell values(2, 3, 2);
insert into sell values(3, 4, 2);
insert into sell values(4, 2, 2);
insert into sell values(5, 2, 1);

-- ex1)
select s.foodnum from sell s;

-- ex2)
select *
from menu m
-- where m.foodnum = 1 or m.foodnum = 2;
-- where m.foodnum in(1,2);  -- 윗코드와 동일
-- where m.foodnum in(1, 2, 2, 2, 1); -- 윗코드와 동일
where m.foodnum in(select s.foodnum from sell s); -- 윗코드와 동일

-- ex3) 존재하느냐고 물어봄. 세미조인.
select * from menu m
where exists(
select * from sell s
where m.foodnum = s.foodnum
); 

-- ex4) 안티조인
select * from menu m
where not exists( 
select * from sell s
where m.foodnum = s.foodnum
);


-- ex58) 외부조인(outer join)
drop table tablea;
create table tablea(
id int,
name varchar(20)
);
drop table tableb;
create table tableb(
id int,
age int(20)
);

insert into tablea values(1, 'tiger01');
insert into tablea values(2, 'tiger02');
insert into tablea values(3, 'tiger03');
insert into tablea values(4, 'tiger04');
insert into tablea values(5, 'tiger05');

insert into tableb values(3, 30 );
insert into tableb values(4, 40 );
insert into tableb values(5, 50);
insert into tableb values(6, 60 );
insert into tableb values(7, 70 );


-- ex1)
select *
from tablea t1, tableb t2
where t1.id = t2.id;

-- ex2)
select *
from tablea t1
left join tableb t2
on t1.id = t2.id;

-- ex3) 문제제시 >> 전체 교수명단과 그 교수가 담당하는 과목의 이름을 검색.
/*
교수테이블				과목테이블
이름		과목번호		과목번호		교과목이름
홍길동	1			1			수학
안중근	null		2			영어
홍길동	2
이순신	2
강감찬	null


쿼리 결과는 ?
홍길동		국어
안중근 
*/

-- ex59) group by : ~별로( 그룹별로, ex) 부서별로, 학과별로, 팀별로 )
-- 		 그룹조건 : having 
-- ex)   그룹별 평균(avg)급여. . . . 그룹별 사원수(count), 구룹별 최대연봉(max)
create table tablea(
	eno int,
    salary int
);

insert into tablea values(10, 800);
insert into tablea values(20, 200);
insert into tablea values(20, 400);
insert into tablea values(10, 500);
insert into tablea values(20, 300);

-- ex1) group by를 사용해서 select할때는 컬럼명을 사용하는것이 일반적이다.
select eno 부서번호 , sum(salary) 급여합계
from tablea
group by eno;

-- ex2) 급여가 300이상인 직원들을 그룹별로 검색하세요.
select eno 부서번호 , sum(salary) 급여합계
from tablea
where salary > 300		-- select를 필터링한다.
group by eno;

-- ex3)
select eno 부서번호 , sum(salary) 급여합계
from tablea
where salary > 300
group by eno
having sum(salary)  > 1000; -- 그룹핑된 결과를 필터링한다.


/*
쿼리 작동 우선순위.
① FROM
② ON
③ JOIN
④ WHERE
⑤ GROUP BY
⑥ CUBE | ROLLUP
⑦ HAVING
⑧ SELECT
⑨ DISTINCT
⑩ ORDER BY
⑪ TOP
*/

-- ex60)
-- BN ( binary) 를 걸게 되면 대소문자를 구별해줌.
-- un ( unsigned) 를 걸게 되면 INT이지만 음수를 사용할 수 없다. ( 대신에 양수의 범위가 2배 증가 )
insert into tablea values(null, 'tiger');
insert into tablea values(null, 'Tiger');
insert into tablea values(null, 'Tiger');
insert into tablea values(null, 'TigerLion');


select * from tablea
where name = 'tiger';

drop table tableb;
create table `db01`.`tableb` (
`id` int not null auto_increment,
`name` varchar(45) binary null,
age int(5) zerofill null,
primary key(`id`));

insert into tableb values(null, 'tiger',23);
select * from tableb;


-- 교차조인, 내부조인, 등가조인 	(일반조인, Ansi조인) 
-- 자연조인, Using 조인, 비등가조인
-- 셀프조인, 세미조인(안티조인), 외부조인
```


```jsx
//1.
// file - preference - setting - wheel검색 - Mouse Wheel Zoom 체크
console.log("Hello World");
console.log(20220321);
```

```jsx
//2. 다른파일 열기 - run/open configuration가서 설정
// 독수리 참조
```

```jsx
//3.
//ex3) SNIPPET(File/Prefenrence/User Snippet/javascript/ 7-14라인 주석해제)
console.log('작은따옴표도 문자열로 인식해줌');
```

```jsx
//4. 변수 선언
var a = 10;
console.log(a);
// 요즘엔 var말고 let사용
var a = 10;
console.log(a);
a=20;
console.log(a);

let b = 20;
console.log(b);
b=30;
console.log(b);

const c = 30;
console.log(c);
c=10;
console.log(c); => Uncaught TypeError TypeError: Assignment to constant variable.
```

```jsx
//5. 타입 알아내기 typeof
let a = 10;
let b = 'tiger';
let c = true;
let d = 3.14;

console.log(typeof(a));
console.log(typeof(b));
console.log(typeof(c));
console.log(typeof(d));

let e = [];
let f = {};
//let g = (); 없는 타입
console.log(typeof(e));
console.log(typeof(f));

// 함수모양
let g = function(){};
console.log(typeof(g));

let h = undefined;
console.log(typeof(h));

>>number
string
boolean
number
② object =>[], {}는 둘다  object타입
function
undefined => null같은아이

```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0ce4a98c-aa21-413c-8ab9-f8154f57a736/Untitled.png)

t 그대로 보내준다고 해석할수는 없음, 개르서 Serializable을 implements해주고 난 다음 보내줘야됨

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d9b3acfd-4090-49d2-ba96-f4f8b888477a/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5e04efea-d2a7-4d63-8976-60bd66efeddb/Untitled.png)

값 넣어주고

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/122e49c8-cf15-4f9e-aac6-0d89692e596a/Untitled.png)

뭘한건지는 모르겠음..ㅇㅇ...

```jsx
//6.
// key:value,0000 >> JSON type
let obj = {
    a:10,
    b:"tiger",
    c:true,
    d:[],
    e:{},
    f:function(){}
};

console.log(typeof(obj));
console.log(typeof(obj.a));
console.log(typeof(obj.b));
console.log(typeof(obj.c));
console.log(typeof(obj.d));
console.log(typeof(obj.e));
console.log(typeof(obj.f));

>>object
number
string
boolean
(2)object
function
```

```jsx
//7. 객체타입 안으로 타고들어가기
let obj={
    a:{
        b:{
            c:{
                d:10
            }
        }
    }
}
console.log(typeof(obj.a.b.c.d));
console.log(obj.a.b.c.d);
>>number
10

let obj2={
    a:10,
    b:'tiger',
}
console.log(obj2.a);
console.log(obj2.b);
>>10
tiger
```

```jsx
//8. ','로 print해줄수있음
let apple=10;
console.log(apple, typeof(apple));

// 기존의 메모리삭제되고
// string타입의 메모리가 생성된다
apple='tiger';
console.log(apple, typeof(apple));
```

```jsx
//9. var, let의 차이는 지역성의 유지 여부
{
    var a = 10;
    let b = 20;
}
console.log(a); => 10
console.log(b); => Uncaught ReferenceError ReferenceError: b is not defined

=> var는 지역성을 유지하지않음 : {}안에 있는데 밖에서 console이 찍힘
=> let은 지역성을 유지함 : {} scope내에서만 console이 찍힘
```

```jsx
//10.
```

```jsx
//11. undefined, null
// 타입이 정해지지 않았다. 
// 하지만 타입이 undefined, null이다.
// ????.,.??
// null: 변수를 선언하고 'null'이라는 빈값을 할당한 경우로 타입은 'object'이다.
// undefined: 변수를 선언하였지만, 값을 할당하지 않은 상태로 타입을 확인해보면 'undefined'이다.

let a = undefined;
let b = null;

console.log(typeof(a));
console.log(typeof(b));

```

```jsx
//12. 문자열 연결
let str = '호랑이';
str+='와 고양이';
console.log(str);
>> 호랑이와 고양이

console.log(s+n);
console.log(Number(s)+n);
console.log(String(n)+200);
>> 100100
200
100200

// 문자열 > 숫자
//1. Number(str)
//2. ParseInt(str)
let s1 = '100';
let s2 = '200';
console.log(Number(s1)+1); >> 101
console.log(parseInt(s2)+1); >> 201

// 알수없는 문자가 하나 끼어있을 경우
let s1 = '100원';
let s2 = '200원';
console.log(Number(s1)+1); >> Nan
console.log(parseInt(s2)+1); >> 201

// 슈가코드
let s3 = '10';
let s4 = +'10'; // 문자앞에 +를 추가해주면 타입이 number로 변함
console.log(typeof(s3),typeof(s4)); >>string number
```

```jsx
//13.
// 산술, 관계, 논리 연산 동일
// 증가, 감소 연산자 동일
// +=, 복합대입연산자 동일
// true, false
// 4대 제어문 동일
// 삼항연산 동일

//제곱승(2의 3승)
console.log(Math.pow(2,3));
console.log(2**3);
```

```jsx
//14. 출력 뒤에 변수를 선언하면?
console.log(result);
let result=1;
>> Uncaught ReferenceError ReferenceError: Cannot access 'result' before initialization
let a = 10;
let b = 0x10; //16진수 표기
let c = 0o777; //8진수 표기
let d = 0b0101010101; //2진수 표기
console.log(a,b,c,d);
//함수
function apple(){
    return 100;
}
//반복문, 여기서 주의할점은 int가 아니라 let!!
for (let index = 0; index < array.length; index++) {
    const element = array[index];
}
```

```jsx
//15. 
// 값이 같은가?
console.log(10==10); >>true
// 값이 같은가? 타입이 같은가?
console.log(10===10); >>true

console.log(10=='10'); >>true
console.log(10==='10'); >>false
console.log(10===10.0); >>true
console.log(7/4); >>1.75
console.log(7%4); >>3
console.log('한글'); >>한글
console.log("한글'과'컴퓨터"); >>한글'과'컴퓨터
let t = 10;
console.log(`한${t}글`); >>한10글
```

```jsx
//16.
let str = `<h3>호랑이</h3>`; // '\n', '\t'
let first = 'tiger';
let last = 'lion';

console.log(`My name is ${first+last}`);

let a = 3;
let b = 4;
console.log(`${a+b}`);
```

```jsx
//17.
//1. 실행시간에 필요에 따라서 키값을 추가할 수 있다.
let obj={
    a:10,
}
obj.b=20;
console.log(obj.a);
console.log(obj.b);

//2.
let obj2 = {
    a:10,
}
obj2['b'] = 20;
console.log(obj2['a']);
console.log(obj2['b']);

//3.
let obj3 = {
    a:10,
    b:20,
}
obj3['b'] = 30;
console.log(obj3.b);

//4.
let obj4 = {
    a:10,
    b:20,
    myfunc:function(){
        return this.a+this.b;
    }
}
obj4['b'] = 30;
console.log(obj4.myfunc(), obj4['b']);

//5. key값이 충돌될 수 있는 위험을 방지하기 위해서 symbol문법이 나타남
let obj5 = {
    a:10,
    monkey:20,
    myfunc:function(){
        return this.a+this.monkey;
    }
}
let monkey = Symbol('monkey');
obj5[monkey]=999;
console.log(obj5.myfunc());
console.log(obj5['monkey']) >>20
console.log(obj5[monkey]); >>999

let obj6={
    a:20,
    b:30,

}
console.log(4*5); >>20
console.log(4>5); >>false
console.log(1+true); >>2
console.log(0==''); >>true
!(x||y)==!x&&!y
let a=1, b=2, c=3;
```

```jsx
let str = 'apple';
console.log(str.length);
>>5
```

```jsx
//18.date함수
let n = new Date();
console.log(typeof(n));
console.log(typeof(Date));
console.log(typeof n);
console.log(typeof Date);

console.log(n.getFullYear(), '년');
console.log(n.getMonth()+1, '월'); => 0월부터 시작
console.log(n.getDate(), '일');
console.log(n.getDay(), '요일'); => 일요일이0

console.log(n.getHours(), '시');
console.log(n.getMinutes(), '분');
console.log(n.getSeconds(), '초');
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4c65754b-68e1-41fb-8157-28990855d272/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1fc91889-cd90-4b4f-b81b-2e34245ef20c/Untitled.png)

그러나....

```jsx
//19. Date()를 이용하여 1초동안 for문의 반복횟수
let start = new Date().getTime();
console.log(start);
for (var i = 0; new Date().getTime<start+1000; i++) {
    
}
console.log(i);
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fad5fa5f-8ce5-4e50-9e9a-0035418f66d7/Untitled.png)

난 왜 0밖에 안나올까...

⇒ 했더니 getTIme() 여기서 괄호를 안해줬었음....ㅠ

```jsx
//20.
//1.
let start1 = new Date().getTime();
console.log(start1);

while (new Date().getTime()<start1+1000);
console.log('end');

//2.
// 함수이름이 없이 함수를 작성하는것은 보통 '즉시실행함수'라고 함.
(function(num){
    console.log(num);
})(100);

//3.

(function(num){
    console.log("start2", num/1000);
    let start2 = new Date().getTime();
    while (new Date().getTime() < start2 + num);
})(10000);
// 10초 뒤에~
console.log('end');
```

```jsx
//21. if문
if(true){
    console.log('100');
}
// 짧은 if문
// true + and
true && console.log('101');

if(!false){
    console.log('102');
}
// false + or
false || console.log('103');

// let num = 10;
// if(false && num++){

// }
// console.log(num);
```

```jsx
//22.
//1.
function f1(){
    console.log('1');
}
f1();
// 호이스팅
f11();
function f11(){
    console.log('11');
}
//2.
let f2 = function(){
    console.log('2');
}
f2();
// 호이스팅안됨
//f22();
let f22 = function(){
    console.log('22');
}

//3. 람다함수
let f3 = () =>{
    console.log('3');
}
f3();

//4. 즉시 실행 함수(IIFE)
//(function(){})() // 기본틀
(function(){
    console.log('4');
})();

//5. IIFE + 람다
//(()=>{})() // 기본틀
(()=>{
    console.log('5')
})();

//6. 람다 인수 전달
((num)=>{
    console.log(num);
})(6);

//7. 리턴 처리
let result = ((num)=>{
    return num*10;
})(6);
console.log(result);

//8. 첫자가 대문자로 출발하는데
// 생성자 함수(==class)
function F4(){
    this.name = '호랑이';
    this.age = 100;

    this.f1 = function(){
        console.log(this.name);
    }
    this.f2 = ()=>{
        console.log(this.age);
    }
}
let obj = new F4();
obj.f1();
obj.f2();
console.log(obj.name, obj.age);
```

```jsx
//23. 함수형태
function t1(){
    console.log('t1');
}
t1();

function t2(num){
    console.log('t2');
}
t2();

function t3(){
    return 100;
}
t3();
console.log(t3());

function t4(num){
    return num*100;
}
t4();
console.log(t4(2));
```

```jsx
//24.
//1.
let count = {
    num: 0,
    increase: function(){
        this.num++;
        console.log(this.num);
    }
}
console.log(count.num);
count.increase();

//2.
let person={
    name:'tiger',
    sayHello:function(){
        console.log(`${this.name}`);
    }
}
person.sayHello();
console.log( person );
console.log('end');

//3. 빈객체
let empty={

}

//4. 
let obj1 = {
    'name':'tiger',
    'age':10,
}
console.log(obj1.name, obj1.age);

//5.
let obj2 = {};
let key = 'hello';
obj2[key] = 'world';
console.log(obj2.hello);
console.log(obj2);

//6.
let obj3={
    0:10,
    1:20,
    '2':30
}
console.log(obj3[0]); //.0안되고 .'0'안되고

//7. 가장 나중에 정의된 속성으로 된다
let obj4={
    age:10,
    age:20
}
console.log(obj4);
console.log(obj4.age, obj4['age']);

//8.
let obj5={
    a:10,
}
//속성 갱신
obj5.a=20;

//속성 생성
obj5.b = 30;

//속성 삭제
delete obj5.a;
console.log(obj5);

//9.
let x=3, y=4;
let obj6={
    x:x,
    y:y
}
console.log(obj6);

//10. xx: yy:을 생략할 수 있음
let xx=5, yy=6;
let obj7={
    xx,
    yy
}
console.log(obj7);

//11.
let xxx=7, yyy=8;
let obj8={xxx,yyy,}
console.log(obj8);

//12.
let prefix = 'prop';
let ct = 0;
let obj9= {};
obj9['prop-0']=0;
obj9[prefix + '-' + ++ct]=0;
for (let i = 0; i < 5; i++) {
    obj9[prefix+'-'+i]=i;
    
}
console.log(obj9);

//13. 12번연장 ct를 쓰나봄
let obj10 = {
    [`${prefix}-${ct}`]: 99,
}
console.log(obj10);

//14.
let obj11 = {
    // 일반함수형태로 정의된 함수는 new를 이용해서 객체로 생성할 수 있다.
    // 생성자함수로서의 역할을 할 수 있다
    f1:function(){
        console.log('1');
    },
    // 메소르로 정의된 함수는 new를 이용해서 객체로 생성할 수 없다.
    // 생성자함수로서의 역할을 할 수 없다.
    f2(){
        console.log('2');
    }
}
new obj11.f1();
//new obj11.f2();

>>0
1
tiger
{name: 'tiger', sayHello: ƒ}
end
tiger 10
world
{hello: 'world'}
10
{age: 20}
20 20
{b: 30}
{x: 3, y: 4}
{xx: 5, yy: 6}
{xxx: 7, yyy: 8}
No debugger available, can not send 'variables'
{prop-0: 0, prop-1: 1, prop-2: 2, prop-3: 3, prop-4: 4}
{prop-1: 99}
1
```

```jsx
function f1(){
    console.log(typeof arguments);
    console.log(arguments.length);
    let sum=0;
    for(const key in arguments){
        console.log(key, arguments[key]);
        sum+=arguments[key];
    }
    console.log(sum);
}
// f1(10);
// f1(10,20);
f1(10,20,30);
```

```jsx
//25.
//1.
let f1 = function(){
    console.log('1');
    let f2 = function(){
        console.log('2');
    }
    f2();
}
f1();

//2.
let f3 = function(){
    console.log('1');
    return function(){
        console.log('2');
    }
};
f3()();

//3.
let f4 = function(a,b){
    console.log(a+b);
    return function(c,d,e){
        console.log(c+d+e);
    }
}
f4(10,20)(10,20,30);

//3.
let f5 = function(a){
    a();
    return function(c){0.
        c();
    }
}
f5(function(){
    console.log(99);
})(function(){
    console.log(100);
});
```

```jsx
//26.
// cf >> callback function
let f1 = function(cf){
    cf();
}

let f2 = function(cf){
    console.log('f2 call');
}

//1.
f1(f2);

//2.
f1(function(){
    console.log('ex2');
});

//3.
f1(()=>{
    console.log('ex3');
});

//4.
let f3 = function(){
    console.log('1');
    const f4 = function(){
        console.log('2');
    };
    return f4;
};
f3()();

//5.
let f5 = function(){
    console.log('1');
    return function(){
        console.log('2')
    };
};
f5()();
```

```jsx
//27.
//1.
(function(){
    console.log('1');
    return function(){
        console.log('2');
    };
})()();

//2.
(()=>{
    console.log('1');
    return ()=>{
        console.log('2');
    };
})()();

//3.
((cf)=>{
    cf();
    console.log('1');
    return (cf1)=>{
        cf1();
        console.log('2');
    };
})(()=>{
    console.log('tiger');
})(()=>{
    console.log('lion');
});
```

```jsx
//28.
//1.
let c = function(a){return a*10;}
console.log(c(2));

//2.
let c2 = (a)=>{return a*10;}
console.log(c2(3));

//3. lambda에서 인수전달이 1개일 때에는 인수전달하는 괄호는 생략가능
let c3 = a=>{return a*10;}
console.log(c3(3));

//4. lambda에서 return단문장일 때 return 생략가능, 단 {}도 같이 생략
let c4 = a => a*10;
console.log(c4(5));

//5.
let c5 = function(){
    //return 숫자, 문자열, 함수, 객체;
    //return 100;
    //return 'tiger';
    //return ()=>{};
    return {a:10, b:20};
}
let obj01 = c5();
console.log(obj01.a, obj01.b);

//6.
let c6 = ()=>{
    return {a:10, b:20};
};
// return과 {} 빼고 나니까
// 남아있는 스코프가 함수의 {}dlswl
// 객체의 스코프인지 모호한 상황이 발생
let c7 = () => ({a:10, b:20});
let obj02 = c7();
console.log(obj02.a, obj02.b);
```

```jsx
//29.
//1.
(a)=>{};

//2.
((a)=>{})();

//3.
((a)=>{
    console.log(1);
    ((b)=>{
        console.log(2);
    })();
})();

//4.
((a)=>{((b)=>{
    console.log(a+b);
})(10); })(20); 

//6.
let c = (a)=>{(b)=>{console.log(a+b);}};

//7.
let c2 = (a)=>{(b)=>{}};

//8.
let c3 = a => {b=>{}};

//9. 
//let c4 = (a)=> (b) => {};
//let c4 = (a) => {
//    return (b) => { return a+b;}
//}
//let c4 = (a)=>{return (b) => {return a+b;}}
let c4 = a => b => a+b;
console.log(c4(10)(20));
```

```jsx
//31. 클로즈함수 : 
const f1 = function(){
    let a = 100; // 생명연장시켜줌
    return function(){
        console.log(a);
    }
}
f1()(); //return 되는게 함수니까 ()두번
```

> 클로저함수란 ? 함수가 선언된 어휘적 환경의 조합이다.
> 

예시들을 보면, 함수 밖에서 선언된 다른 지역변수를 사용할 수 있는 것 같은데, 중첩된 함수에서는 이를 활용할 수 있다고한다.

```jsx
function makeFunc() {
      var name = "Mozilla";
      function displayName() {
        alert(name);
      }
      return displayName;
    }

    var myFunc = makeFunc();
    //myFunc변수에 displayName을 리턴함
    //유효범위의 어휘적 환경을 유지
    myFunc();
    //리턴된 displayName 함수를 실행(name 변수에 접근)

function makeAdder(x) {
      var y = 1;
      return function(z) {
        y = 100;
        return x + y + z;
      };
    }

    var add5 = makeAdder(5);
    var add10 = makeAdder(10);
    //클로저에 x와 y의 환경이 저장됨

    console.log(add5(2));  // 107 (x:5 + y:100 + z:2)
    console.log(add10(2)); // 112 (x:10 + y:100 + z:2)
    //함수 실행 시 클로저에 저장된 x, y값에 접근하여 값을 계산
```

```jsx
//32. setTimeout
// 비동기함수 : 코드의 실행순서를 지키지 않는 함수
// setTimeout(함수, 정수(시간));
// 출력이 꼭 순서대로 가지는 않음. => 1찍히고 2초뒤에 2가 찍히는데 맨밑에 3 찍히고 2가 찍힘
// 자바랑은 다르게 기본적으로 순서같은거 모르겠고 다같이 출발하나봄 => 멀티가 가능함ㅎㄷ
console.log(1);
// 비동기함수가 없으면 함수가 블로킹
// 비동기 함수를 동기화시킨다는 역할을 하고있음. 어차피 순서를 지맘대로하는데, 이를 교정해줌 = ?

setTimeout(() => {
    console.log(2);
}, 2000);
console.log(3);
```

```jsx
//33. setInterval

console.log(1);
// 차이점은 계속해서 함수를 불러옴
let id = setInterval(() => {
    console.log(2);
}, 1000);

setTimeout(() => {
    clearInterval(id); // setinterval 종료
}, 10000);
console.log(3);
```

```jsx
//34.
//1. 1이 3번이 후두두둑 한꺼번에나옴
// for (var i = 0; i < 3; i++) {
//     setTimeout(() => {
//         console.log(1);
//     }, 1000);
// }
//2.
// for (var i = 0; i < 3; i++) {
//     ((num)=>{
//         setTimeout(() => {
//             console.log(num);
//         }, 1000);
//     })(i);
// }
//3.
// for (let i = 0; i < 3; i++) {
//     setTimeout(() => {
//         console.log(i);
//     }, 1000*i);
//}
//4. 최종본
for (var i = 0; i < 5; i++) {
    ((num)=>{
        setTimeout(() => {
            console.log(num);
        }, 1000*i);
    })(i);
}
```

```jsx
//35. eval() : 문자열을 코드로 해석해서 실행해주는 함수
let str = '';
str += 'let a = 10;';
str += 'console.log(a);';

eval(str);
// client >> 문자열을 만든다(js코드작성) >> server에 날린다 >> 받은 문자열을 eval을 이용해서 실행결과를 만듬
// >> 클라이언트에 전송

//    배열          객체
//1. []             {}
//2. index 사용     속성(key:value)
//3. forEach가능    forEach불가
//4. length가능     length불가
```

```jsx
//36.
let obj = {
    s: 'tiger',
    n:10,
    b:true
}
console.log(obj);
console.log(obj.s, obj.n, obj.b);
console.log(obj['s'], obj['n'], obj['b']);

for (const key in obj) {
    console.log(key, obj[key]);
}

// with키워드 : obj[] 생략가능
with(obj){
    console.log(s,n,b);
}
```

`shift+f5` 디버깅 멈춤

```jsx
//37.
const obj = {
    // obj = this <--> a (바인딩)
    a:10,
    b:20,
    f1:function(){
        console.log(this.a, this.b);
    },
    // 람다에서는 this를 사용할 수 없음 그래서 undefined가 나오나봄 ㅠㅠ
    f2:()=>{
        console.log(this.a, this.b);
    },
    f3:function(){
        for (const key in this) {
            console.log(key, this[key]);
        }
    }
}
// obj.f1(); //10 20
// obj.f2(); // undefined undefined
obj.f3();
let c= 'dd';
let apple = {
    a:10,
    b:'tiger',
    [c]:30, // c를 가지는 값이 있을것이다 무조건, let c ='dd' 이기 때문에 key값이 dd임
}
console.log(apple);

const obj1 = {
    a:10
}
obj1.b = 20;
obj1['c'] = 30;
console.log(obj1);

for (let i = 0; i < 3; i++) {
    obj1['lion'+i] = i*10;
}
console.log(obj1);

console.log('end');
```

```jsx
//38.
const obj = {
    a:10,
    b:20
}
console.log(obj);
console.log(Object.keys(obj)); // key값만 뽑아와줌

let a = Object.keys(obj); // a는 어케 가져올까 배열일까, 그냥 다른걸까 확인하기위해
console.log(typeof a); // type은 object
console.log(Array.isArray(a)); // 배열이면 true, 아니면 false

// 값만 얻을 수 있다.
console.log(Object.values(obj));
```

```jsx
//39. 객체 병합
const obj01 = {
    a:10,
    b:20,
}
const obj02 = {
    c:30,
    d:40,
}

//1. assign()
const obj03 = Object.assign(obj01, obj02);
console.log(obj03);
// key가 중첩되면, 후처리

//2. 90% : ...을 전개 연산자
//const obj04 = {obj01, obj02};
const obj04 = {...obj01, ...obj02};
console.log(obj04);

let apple = (banana)=>{
    console.log(banana);
}
apple({...obj01, ...obj02});
```

```jsx
//40.
//1.
let ar = [10,20,30];
console.log(ar);
console.log(ar.length);
console.log(typeof ar);
console.log(Array.isArray(ar));

//2.
let br = Array();
console.log(br.length);

//3.
let cr = Array(10);
console.log(cr);
console.log(cr.length);

//4.
let dr = Array(40,50,60);
console.log(dr);

//5. 모든 타입을 적용할 수 있다.
let er = [10, 'tiger', true, [], {}, function(){}, undefined];
console.log(er);

//for in
for (const index in ar) {
    console.log(index, ar[index]);
};

//for of
for (const value of ar) {
    console.log(value);
}

//forEach 함수를 사용할 수 있다.
// ar.forEach(함수);
// ar.forEach(function(){});
ar.forEach((v,i)=>{
    console.log(v,i);
});
// 기존에 가지고있는 데이터를 가공해서 새로운 데이터를 생성
let br = ar.map((value)=>{
    console.log(value);
    return value*10;
})
console.log(br);

let cr = ar.map(value=>value*10);
console.log(cr);
```

```jsx
//41.
let ar = [10,11,12,14];
let br = ar.map((v)=>{
    return (v%2===0) ? 'Even': 'Odd';
})
console.log(br);
```

```jsx
//42.
let ar = [
    {
        n:'tiger',
        a:10,
    },
    {
        n:'lion',
        a:20,
    },
    {
        n:'cat',
        a:30,
    },
]

console.log(ar);
for (const index in ar) {
    console.log(ar[index].n, ar[index].a);
}

ar.forEach((v,i)=>{
    console.log(v);
})

let br = ar.map((v,i)=>{
    return v.n;
})
console.log(br);
```

```jsx
//43. 배열 함수(*func : 스스로 갱신)
//1.
let ar = [80,20,10,15];
let str = ar.toString();
console.log(str, typeof str);

//2.
let date = new Date();
console.log(date);
console.log(date.toLocaleString());

//3. pop()* => 맨 마지막 요소를 삭제함, *은 "배열자체를 갱신시킨다." 라는 의미를 가지고있음
ar.pop();
console.log(ar);

//4. push()*
let num = ar.push(15);
console.log(ar, num); //num이 4가 나온걸로 봐서는 push하고 난 다음 size를 return시켜주나봄

let br = [77,88,99];
ar.push(br);
console.log(ar);
// >> 배열안에 요소들이 들어간게 아니라(병합X) 배열이 통쨰로 들어가버림
console.log(ar[4]);

//5.
let cr = [10,20,30];
let dr = [40,50];
let er = cr.concat(40);
console.log(er); // 변수안두고 그대로하면 배열이 갱신되지않음.

let fr = cr.concat(dr);
console.log(fr);
fr = cr.concat([88,99]);
console.log(fr);

//6. join : 구분자를 삽입하면서 문자열을 연결한다. 디폴트값은 ','
let gr = ['tiger', 'lion', 'cat'];
console.log(gr.join());
console.log(gr.join(' '));
console.log(gr.join('+'));

//7. reverse()* : 반대로
console.log(gr.reverse());

//8. shift
            //1,2,3,4
        //1,2,3,4
let t1= gr.shift();
console.log(t1);
console.log(gr);
gr.unshift('rose');
console.log(gr);
```

```jsx
//44.
// 주의 : 우연히 정렬된다.
let ar = [80,10,20,15];
console.log(ar);
ar.sort();
console.log(ar);

let br = [52,243,123,33];
console.log(br);
br.sort();
console.log(br);

let cr = [99,5,2,13,55];

// cr.sort((a,b)=>(a>b) ? 1 : -1)
cr.sort((a,b)=>(a-b))
console.log(cr);

//역순
cr.sort((a,b)=>(b-a))
console.log(cr);

//
let dr = [
    {
        n:30,
        s:'월'
    },
    {
        n:20,
        s:'화'
    },
    {
        n:10,
        s:'수'
    },
]
dr.sort((a,b)=>{
    // return a.n-b.n; // 순차정렬
    return b.n-a.n // 역순정렬
})

console.log(dr);
console.log('');
```

```jsx
//45. 배열 : slice
let ar = [1,2,3,4,5,6,7,8,9];
let br = ar.slice(2,4);
console.log(br);

// splice(시작위치, 삭제수, 추가항목, 추가항목,...)
let cr = [1,2,3,4,5,7,5,8];
// cr.splice(1,0,4,5,6);
// console.log(cr);
cr.splice(2,2,4,5,6,7);
console.log(cr);

//indexOf : 첫번째 인덱스만 알려줌
let dr = ['lion', 'tiger', 'dog', 'cat', 'tiger'];
console.log(dr.indexOf('tiger'));
// 못찾으면 -1,
console.log(dr.indexOf('tiger',2)); // 2번째 tiger
console.log(dr.lastIndexOf('tiger')); // 뒤에서부터 찾아주는데 return값은 정상적인 배열index임

let er = [1,30,20,40,50,3];
function f1(data){
    return data < 40;
}
// every 모든데이터가? 모두 true여야 true
console.log(er.every(f1));
// some 어떤데이터가? 하나라도 true면 true
console.log(er.some(f1));

let fr = [1,3,5,7];
console.log(fr.some((value)=>{
    return value%2 === 0;
}));

// 배열에서 map() >>

```

```jsx
//46.
// filter
let ar = [1,30,24,3,4,6,77];
console.log(ar.filter((v)=>{
    return v<30;
}));

let cr = [
    'tiger',
    'lion',
    'dog',
    'cat'
]
console.log(cr.filter((v)=>{
    return v.length>=4;
}));

```
