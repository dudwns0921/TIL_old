# SQL 활용

# 1. 조인

# 1-1. EQUI JOIN

## 1) EQUI JOIN

- 조인은 여러 개의 릴레이션을 사용해 새로운 릴레이션을 만드는 과정
- 조인의 가장 기본은 교집합을 만드는 것이다.

```sql
select * from emp, dept
where emp.deptno = dept.deptno;
```



## 2) INNER JOIN

- EQUI JOIN과 마찬가지로 ISO 표준 SQL로 INNER JOIN이 있다. 

```sql
select * from emp inner join dept
on emp.deptno = dept.deptno;
```



## 3) INTERSECT 연산

- intersect 연산은 두 개의 테이블에서 교집합을 조회한다.

```sql
select deptno from emp
intersect
select deptno from dept;
```



# 1-2. NON-EQUI JOIN

- Non-EQUI는 두 개의 테이블 간에 조인하는 경우 "="을 제외한 비교 연산자를 사용한다.
- 정확하게 일치하지 않는 걸 찾을 때 사용한다.



# 1-3. OUTER JOIN

- 두 개의 테이블 간에 교집합을 조회하고 한 쪽 테이블에만 있는 데이터도 포함시켜서 조회한다.

```sql
select * from emp, dept
where emp.deptno (+) = dept.deptno;

select * from emp left outer join dept
on emp.deptno = dept.deptno;

--공통되지 않은 데이터를 가져오고 싶은 테이블에 (+)을 작성하거나 방향에 맞게 left outer join 또는 right outer join을 작성한다.
```



# 1-4. CROSS JOIN

- cross join은 조건구 없이 2개의 테이블을 하나로 조인한다.
- 조인구가 없기 때문에 카테시안 곱이 발생한다.
- 현장에서는 쓸 일이 없으니 어떤 건지만 알아두자.



# 1-5. UNION

## 1) UNION

- union 연산은 두 개의 테이블을 하나로 만드는 연산이다.
- 두 개의 테이블의 칼럼 수, 칼럼의 데이터 형식 모두가 일치해야 한다.
- 하나로 합치면서 중복된 데이터를 제거하는데, 이 때 정렬 과정을 발생시킨다.

## 2) UNION ALL

- union과 달리 중복이나 정렬을 유발하지 않고, 단순히 두 테이블을 합친다.



# 1-6. MINUS

- 두 개의 테이블에서 차집합을 조회한다. 
- 먼저 쓰는 테이블에만 있는 데이터를 조회한다.



# 2. 계층형 조회

- 계층형 조회는 오라클 데이터베이스에서 지원하는 것으로 계층형으로 데이터를 조회할 수 있다.

```sql
select level, empno, mgr, ename
from emp
start with mgr is null
-- 시작 조건을 의미. 이 경우에는 manger, 즉 관리자가 없다는 뜻이므로 가장 높은 직급에서부터 조회를 시작한다는 의미
connect by  prior empno=mgr;
-- 조인 조건을 의미. 해당 사원의 사원번호가 다른 사원의 상사 사원번호와 같을 경우 조인시켜 조회하는 것이다.
```

## > CONNECT BY 키워드

| 키워드                   | 설명                                                         |
| ------------------------ | ------------------------------------------------------------ |
| LEVEL                    | 검색 항목의 깊이를 의미. 즉, 계층구조에서 가장 상위 레벨이 1이 된다. |
| CONNECT_BY_ROOT + 칼럼명 | 해당 칼럼의 계층구조에서 가장 최상위 값을 표시               |
| CONNECT_BY_ISLEAF        | 계층구조에서 최하위를 표시<br />1또는 0으로 표시되며 최하위일 경우 1, 즉 true로 표시된다. |
| SYS_CONNECT_BY_PATH      | 계층구조의 전체 전개 경로를 표시. 가장 많이 사용하는 키워드  |
| NOCYCLE                  | 순환구조 발생 지점까지만 전개된다.                           |
| CONNECT_BT_ISCYCLE       | 순환구조 발생 지점을 표시                                    |



# 3. 서브쿼리

- 서브쿼리는 select문 내에 다시 select문을 사용하는 sql문이다.
- 서브쿼리의 형태는 from구에 select문을 사용하는 인라인 뷰와 select문에 서브쿼리를 사용하는 스칼라 서브쿼리 등이 있다.
- where문에 서브쿼리를 사용하면 서브쿼리라고 한다.



# 3-1. 단일 행 서브쿼리와 다중 행 서브쿼리

| 서브쿼리 종류    | 설명                                                   |
| ---------------- | ------------------------------------------------------ |
| 단일 행 서브쿼리 | 비교 연산자인 =,<,<=,>,>=,<>를 사용한다.               |
| 다중 행 서브쿼리 | 다중 행 비교 연산자인 IN, ANY, ALL, EXISTS를 사용한다. |



# 3-2. 다중 행 서브쿼리

## 1) IN

- 메인 쿼리의 비교조건이 서브쿼리의 결과 중 하나만 동일하면 참이 된다. (OR 조건)

```sql
--부서별로 가장 급여를 많이 받는 사원의 정보를 출력하시오.(IN 연산자 이용)
select empno, ename, sal, deptno
from emp
where sal in (select max(sal) from 
emp
group by deptno);

/* in 뒤의 쿼리가 서브쿼리이다.
이 때 서브쿼리가 where문 뒤에 사용됐으므로 그대로 서브쿼리라고 한다.
또한 그 결과값이 여러 행을 반환하므로 다중 행 서브쿼리에 해당한다.

부서별로 가장 급여를 많이 받는 사원의 정보를 출력하기 위해서는 먼저 각 부서 급여의 최댓값을 알아야 한다.
그 다음에 각 부서 최대 급여를 받는 모든 사원의 정보가 필요하므로 or 조건에 해당하는 IN 연산자를 사용한다. */
```



## 2) ANY

in과 같이 or 조건에 해당한다.

하지만 in은 >와 같은 비교 연산자를 사용할 수 없기에 비교 연산자를 사용해야 할 경우가 생기면 any를 사용한다.



## 3) ALL

- 메인쿼리와 서브쿼리의 결과가 모두 동일하면 참이 된다.

```sql
--30번 부서 소속 모든 사원들이 받는 급여보다 더 많은 급여를 받는 사원의 정보를 출력하시오.
select empno, ename, sal, deptno
from emp
where sal > all(select sal from
emp
where deptno=30);

/* 30번 부서의 모든 사원들보다 급여를 많이 받아야 하므로 먼저 30번 부서 사원들의 모든 급여를 가져온다.
그 다음에 all 연산자를 사용해서 가져온 모든 급여보다 클 경우에만 조회한다. */
```



## 4) EXISTS

- 서브쿼리의 결과를 만족하는 값이 존재하는지 여부를 판단
- 결과값은 True와 False가 반환된다.
- 튜닝 시 성능 향상에 도움이 된다.

```sql
select empno, ename, sal, deptno
from emp
where exists (select sal from
emp
where deptno=40);

/* 40인 부서번호는 없기 때문에 서브쿼리의 결과를 만족하는 값은 존재하지 않는다.
따라서 false를 반환하고 조회되는 행은 없다. */
```



# 3-3. 스칼라 서브쿼리

- 스칼라 서브쿼리는 select문에 사용되는 서브쿼리로, 반드시 한 행과 한 칼럼만을 반환한다.



# 3-4. 연관 서브쿼리

- 연관 서브쿼리는 서브쿼리 내에서 메인 쿼리 내의 칼럼을 사용하는 것을 의미한다.



# 4. 그룹 함수

# 4-1. ROLLUP

- rollup은 group by 칼럼에 대해서 subtotal을 만들어준다.
- group by구에 칼럼이 두 개 이상 오면 순서에 따라서 결과가 달라진다.

```sql
select decode(deptno, null, '전체합계', deptno), sum(sal) from emp
group by rollup(deptno);

/* rollup을 통해 부서별 사원들 급여의 합과 그 합들을 합친 전체합계가 함께 출력된다.*/

select job, deptno, sum(sal) from emp
group by rollup(job, deptno);

CLERK	10	1300
CLERK	20	1900
CLERK	30	950
CLERK		4150

select deptno, job, sum(sal) from emp
group by rollup(deptno, job);

10	CLERK	1300
10	MANAGER	2450
10	PRESIDENT	5000
10		8750
```



# 4-2. GROUPING

- grouping 함수는 rollup, cube, grouping sets에서 생성되는 합계값을 구분하기 위해 만들어진 함수
- 소계, 합계 등이 계산되면 grouping 함수는 1을 반환한다.
- decode 함수를 사용해서 원하는 데이터를 좀 더 명확하게 확인할 수 있다.

```sql
select job, decode(grouping(job), 1, '전체합계'), deptno, grouping(deptno), sum(sal) from emp
group by rollup(job, deptno);

/* 합계가 구해질 때 1이 아닌 전체합계라는 텍스트로 출력된다.
```



# 4-3. GROUPING SETS

- grouping sets 함수는 group by에 나오는 칼럼의 순서와 관계없이 다양한 소계를 만들 수 있다.
- group by문에 나오는 칼럼 순서에 상관없이 개별적으로 모두 처리한다.



# 4-4. CUBE

- cube 함수에 제시한 칼럼에 대해서 결합 가능한 모든 합계를 계산한다.

```SQL
select job, deptno, sum(sal) from emp
group by rollup(job, deptno);

--전체합계, 직업별 합계, 부서별 합계, 부서별 직업별 합계 모두가 출력된다. 
```



# 5. 윈도우 함수

# 5-1. 윈도우 함수

- 윈도우 함수는 행과 행 간의 관계를 정의하기 위해서 제공되는 함수이다.
- 윈도우 함수를 사용해서 순위, 합계, 평균, 행위치 등을 조작할 수 있다.

**>윈도우 함수 구조**

| 구조         | 설명                                                         |
| ------------ | ------------------------------------------------------------ |
| ARGUMENTS    | 윈도우 함수에 따라서 0~N개의 인수를 설정한다.                |
| PARTITION BY | 전체 집합을 기준에 의해 소그룹으로 나눈다.                   |
| ORDER BY     | 어떤 항목에 대해서 정렬한다.                                 |
| WINDOWING    | 행 기준의 범위를 정한다.<br />ROWS는 물리적 결과의 행 수 이고 RANGE는 논리적인 값에 의한 범위이다. |

**>WINDOWING**

| 구조                | 설명                                                         |
| ------------------- | ------------------------------------------------------------ |
| ROWS                | 부분집합인 윈도우 크기를 물리적 단위로 행의 집합을 지정한다. |
| RANGE               | 논리적인 주소에 의해 행 집합을 지정한다.                     |
| BETWEEN~AND         | 윈도우의 시작과 끝의 위치를 지정한다.                        |
| UNBOUNDED PRECEDING | 윈도우의 시작 위치가 첫 번째 행임을 의미한다.                |
| UNBOUNDED FOLLOWING | 윈도우 마지막 위치가 마지막 행임을 의미한다.                 |
| CURRENT ROW         | 윈도우 시작 위치가 현재 행임을 의미한다.                     |



```sql
select empno, ename, sal, 
sum(sal) over(order by sal rows between unbounded preceding and unbounded following) totalSal
from emp;
```

sal 항목에 대해 정렬하고, 첫째 항부터 마지막 항까지 모두 더한 값을 조회한다.



# 5-2. 순위 함수

- 윈도우 함수는 특정 항목과 파티션에 대해서 순위를 계산할 수 있는 함수를 제공한다.
- 순위 함수는 RANK, DENSE_RANK, ROW_NUMBER 함수가 있다.

**>순위 함수**

| 순위 함수  | 설명                                                         |
| ---------- | ------------------------------------------------------------ |
| RANK       | 특정항목 및 파티션에 대해서 순위를 계산한다.<br />동일한 순위는 동일한 값이 부여된다. |
| DENSE_RANK | 동일한 순위를 하나의 건수로 계산한다.                        |
| ROW_NUMBER | 동일한 순위에 대해서 고유의 순위를 부여한다.                 |

```sql
select empno, ename, sal,
rank() over (order by sal desc) rank,
dense_rank() over (order by sal desc) dense_rank
from emp;
```

dense rank는 동일한 순위를 하나의 건수로 계산해서 그 다음 순위가 +1이 되는 반면에,

rank는 동일한 순위에 동일한 값을 부여할 뿐 하나의 건수로 계산하지 않아 그 다음 순위가 +동일한 순위 건수 가 된다.

row number은 rownum과 같다.

그래서 동일한 순위이더라도 고유한 값을 부여한다.



# 5-3. 집계 함수

| 집계 함수 | 설명                                    |
| --------- | --------------------------------------- |
| SUM       | 파티션 별로 합계를 계산한다.            |
| AVG       | 파티션 별로 평균을 계산한다.            |
| COUNT     | 파티션 별로 행 수를 계산한다.           |
| MAX와 MIN | 파티션 별로 최댓값과 최솟값을 계산한다. |



# 5-4. 행 순서 관련 함수

- 행 순서 관련 함수는 상위 행의 값을 하위에 출력하거나 하위 행의 값을 상위 행에 출력할 수 있다.
- 특정 위치의 행을 출력할 수 있다.

| 행 순서     | 설명                                                         |
| ----------- | ------------------------------------------------------------ |
| FIRST_VALUE | 파티션에서 가장 처음에 나오는 값을 구한다.<br />MIN 함수를 사용해서 같은 결과를 구할 수 있다. |
| LAST_VALUE  | 파티션에서 가장 나중에 나오는 값을 구한다.<br />MAX 함수를 사용해서 같은 결과를 구할 수 있다. |
| LAG         | 이전 행을 가지고 온다.                                       |
| LEAD        | 윈도우에서 특정 위치의 행을 가지고 온다.<br />기본값은 1이다. |

```sql
select empno, ename, sal,
lead(sal, 0) over (order by sal desc) as pre
from emp;
```

이렇게 하면 현재 항의 값과 같은 값을 가져올 수 있다.

lead는 앞선다는 의미를 가지므로, 해당 칼럼에서 설정한 값만큼 앞선 항의 값을 가져온다고 생각하면 된다.



# 5-5. 비율 관련 함수

| 비율 함수       | 설명                                                         |
| --------------- | ------------------------------------------------------------ |
| CUME_DIST       | 파티션 전체 건수에서 현재 행보다 작거나 같은 건수에 대한 누적 백분율을 조회한다. |
| PERCENT_RANK    | 파티션에서 제일 먼저 나온 것을 0으로 제일 늦게 나온 것을 1로 하여 값이 아닌 행의 순서별 백분율을 조회한다. |
| NTILE           | 파티션별로 전체 건수를 argument 값으로 N등분한 결과를 조회한다. |
| RATIO_TO_REPORT | 파티션 내에 전체 SUM에 대한 행 별 칼럼값의 백분율을 소수점까지 조회한다. |



# 6. 테이블 파티션



# 6-1. 파티션 기능

- 파티션은 대용량의 테이블을 여러 개의 데이터 파일에 분리해서 저장한다.
- 테이블의 데이터가 물리적으로 분리된 데이터 파일에 저장되면 입력, 수정, 삭제, 조회 성능이 향상된다.
- 파티션은 각각의 파티션 별로 독립적으로 관리될 수 있다.
  즉, 파티션 별로 백업하고 복구가 가능하며 파티션 전용 인덱스 생성도 가능하다.
- 파티션은 Oracle 데이터베이스의 논리적 관리 단위인 테이블 스페이스 간에 이동이 가능하다.
- 데이터를 조회할 때 데이터의 범위를 줄여서 성능을 향상시킨다.



# 6-2. RANGE PARTITION

- Range Partition은 테이블의 칼럼 중에서 값의 범위를 기준으로 여러 개의 파티션으로 데이터를 나누어 저장하는 것이다.

ex) 급여 1000~3000 / 2000~4000 ... 



# 6-3. LIST PARTITION

- List Partition은 특정 값을 기준으로 분할하는 방법이다.

ex) 부서번호



# 6-4. HASH PARTITION

- Hash Partition은 데이터베이스 관리 시스템이 내부적으로 해시 함수를 사용해서 데이터를 분할한다.
- 결과적으로 DBMS 가 알아서 분할하고 관리한다는 의미
- Composite Partition이라는 파티션 기법도 존재.
  여러 개의 파티션 기법을 조합해서 사용하는 기법



# 6-5. 파티션 인덱스

- 파티션 인덱스는 4가지 인덱스를 제공한다.
- Oracle 인덱스는 Global Non-Prefixed를 지원하지 않는다.

| 구분               | 주요 내용                                 |
| ------------------ | ----------------------------------------- |
| Global Index       | 여러 개의 파티션에서 하나의 인덱스를 사용 |
| Local Index        | 해당 파티션 별로 각자의 인덱스 사용       |
| Prefixed Index     | 파티션 키와 인덱스 키가 동일              |
| Non Prefixed Index | 파티션 키와 인덱스 키가 다름              |

