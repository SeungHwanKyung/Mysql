My sql
======

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
