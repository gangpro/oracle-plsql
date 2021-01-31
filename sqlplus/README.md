# sqlpluse 사용법

> 개발환경<br> 
> OS : Macbook Pro, macOS Mojave, version 10.14.3<br>
> JDK : Java SE Development Kit 8u181<br>
> Eclipse : Eclipse Mars 2


## 환경 설정(macOS 버전 설정)

## 환경 설정(Windows 버전 설정)
1. Oracle DBMS가 설치되어 있을 경우
###
    C:\Users\student> notepad ace.bat
    
    sqlplus ace번호/me@000.000.000.000:1521/xe
    
    C:\Users\student> ace
    
    SQL> show user

2. Oracle DBMS가 설치되어 있지 않을 경우 : Instance Client 다운로드 받아서 사용
###
    C:\Users\student> notepad ic.bat
    
    set path=C:\Instructor\Software\instantclient_12_1;%path%
    
    sqlplus ace번호/me@000.000.000.000:1521/xe
    
    C:\Users\student> ic
    
    SQL> show user
    
    SQL> exit
    
    ** 한글 관련 설정
    
    시작 -> 실행 -> regedit -> HKEY_LOCAL_MACHINE -> SOFTWARE
    -> ORACLE -> 오른쪽 빈칸에서 오른쪽 버튼 클릭
    -> 새로 만들기 -> 문자열 값 -> '새 값 #1' 대신 'NLS_LANG' 입력
    -> NLS_LANG 더블 클릭 -> KOREAN_KOREA.KO16MSWIN949
    -> 새로운 명령프롬프트 시작 -> ic 실행 
    
    
    

## print_table 프로시져 활용
        
      print_table.sql 파일은 data 폴더에 있음
        
      C:\Users\student> more ic.bat
    
        set path=C:\Instructor\Software\instantclient_12_1;%path%
        sqlplus ID/PW@000.000.000.000:1521/xe
    
      C:\Users\student> ic
    
      SQL> @...\print_table.sql
    
      SQL> set serveroutput on
    
      SQL> exec print_table('select * from employees')
    
      SQL> exec print_table('select * from employees where employee_id = 101')
    
      SQL> exec print_table('select * from employees where last_name = ''Kochhar''')
    
      SQL> create or replace view v1
           as 
           select * from employees
           where last_name = 'Kochhar'
           and job_id = 'AD_VP';
    
      SQL> exec print_table('select * from v1')
    
      SQL> conn sys/oracle@000.000.000.000:1521/xe as sysdba
    
      SQL> grant execute
           on ace29.print_table
           to public;
    
      SQL> create public synonym pt
           for ace30.print_table;
    
      SQL> exec pt('select * from all_tables')




## SQL*Plus 명령어
- 환경설정 : show, set 
- 결과포맷 : column, ttitle, btitle, break, ...
- 명령편집 : list, input, append, delete, ...
- 스크립트 파일관리 : save, get, start, ...
- 치환변수 : &, &&, define, ...
- 바인드 변수 : var, ...
- 기타     : run, clear, describe, connect, ...

  cf.SQL*Plus Command Summary

     https://docs.oracle.com/cd/E11882_01/server.112/e16604/ch_twelve001.htm#SQPUG023

# 환경설정 : show, set 
    
    SQL> show all
    
    SQL> show sqlprompt
    
    SQL> set sqlprompt "Eddy> "     --SQL> 이름 바꾸기
    
    Eddy> set timing on
    
    Eddy> set linesize 200          --SQL 폭 바꾸기
    
    Eddy> set pagesize 40           --SQL 넓이 바꾸기
    
    Eddy> select * from emp;
    
    Eddy> show colsep               --컬럼 구분 보기
    
    Eddy> set colsep "|"            --컬럼 구분자 "|"으로 세팅
    
    Eddy> list  또는 l               --명령문 미 포함 그전 실행문 실행
    
    Eddy> run   또는 r, /            --명령문 포함 그전 실행문 실행
    
    Eddy> exit                      --SQL 빠져나가기
    
    C:\Users\student> notepad login.sql     --세션 닫으면 초기화 되니 아래와 같이 SQL login 파일 만들기
    
    alter session set nls_language = 'american';
    alter session set nls_territory = 'america';
    
    set lines 400
    set pages 100
    
    set serveroutput on
    
    C:\Users\student> ace
    
    SQL> select * from emp;

## 결과포맷 : column, break, ttitle, btitle, ...
    
    SQL> select department_id, employee_id, last_name, salary
       from employees
       order by department_id;
    
    SQL> ? column                       --컬럼 커멘드 help
    SQL> help column                    --컬럼 커멘드 help
    
    SQL> col last_name for a15          --last_name 컬럼 길이 줄이기
    SQL> r
    
    SQL> col salary for 999,999.99      --salary 컬럼 1000단위 추가
    SQL> r
    
    SQL> break on department_id         --department_id에 중복값 제거
    SQL> /
    
    SQL> break on department_id skip 1  --department_id에 중복값 제거 후 한줄 띄기
    SQL> /
    
    SQL> set linesize 60
    SQL> tti 'Salary|Report'            --제목 만들기
    SQL> r
    
    SQL> set feedback off               --feedback off
    
    SQL> set pages 40
    SQL> bti 'Confidential'             --마지막 줄 추가
    SQL> r
    
    SQL> tti off                        --tti off
    SQL> bti off                        --bti off
    SQL> clear break                    --
    
    SQL> col                            --컬럼
    SQL> col salary                     --salary 컬럼
    SQL> col salary clear               --salary 컬럼 설정값 제거
    SQL> clear col                      --모든 컬럼 설정값 제거
    SQL> col

# 명령편집 : list, input, append, delete, ...

    SQL> select empno
      2  from emp;
    
    SQL> list
    
    SQL> 1
    
    SQL> append , ename, sal    
    
    SQL> l
    SQL> /
    
    SQL> input
      3  where deptno = 30;
    
    SQL> l 2 3

## 스크립트 파일관리 : save, get, start, ...
    save  파일이름 : 버퍼 -> 파일
    get   파일이름 : 파일 -> 버퍼
    start 파일이름 : 파일 -> 버퍼 -> 실행 
    @     파일이름 : 파일 -> 버퍼 -> 실행 
    edit  파일이름 : 파일 생성 or 파일 편집  
    edit           : 버퍼 편집
    
    spool 파일이름 ....  spool off  : 화면 캡쳐
    
    SQL> select empno, ename from emp;
    
    SQL> save s001   <- 같은 이름의 파일을 덮어쓰려면 replace 옵션 추가
    
    SQL> select * from dept;
    
    SQL> list
    
    SQL> get s001
    
    SQL> start s001
    
    SQL> edit s002
    
    select *
    from tab;
    
    SQL> @s002

    SQL> host dir *.sql     --sql안에서 운영체제 파일 검색
    SQL> host dir *.bat     --sql안에서 운영체제 파일 검색
    
## 바인드 변수 : var, ...
    
    var b_sal number
    
    begin
        select sal into :b_sal      --":" 표시
        from emp
        where empno = 7788;
    end;
    /    
    
    print b_sal               --바인드 변수의 값 출력
    
    
    var
    
    select empno, ename, sal
    from emp
    where sal < :b_sal;        --:b_sal 바인드 변수 사용

## 치환 변수 : &, &&, define, accept ...

    select empno, ename, sal
    from emp
    where sal < &sv_sal;       --&sv_sal 치환변수 : 값을 주라고 한다.
    
    set verify off              --치환변수 값의 old와 new feedback 삭제


## 예제 1. 변수 3개 사용
    VARIABLE b_annual_salary NUMBER
    
    SET AUTOPRINT ON                                        --print 자동 설정 변경
    
    DECLARE                                                 --v_empno : PL/SQL 변수
        v_empno emp.empno%type := &sv_empno;                --&sv_empno : 치환 변수
    BEGIN
        select sal*12 + nvl(comm, 0) into :b_annual_salary  --:b_annual_salary : 바인드 변수
          from emp
         where empno = v_empno;
    END;
    /
    
    (Enter value for sv_empno: 7788) 값입력
    
    print b_annual_salary
    
## 예제 2. 리포팅하기
    SQL> exit
    
    C:\Users\student> ace
    
    SQL> ed sal_rep.sql
        
        accept sv_deptno prompt '부서 번호를 입력해 주세요 :'
        
        set linesize 100
        set pagesize 40
        set feedback off
        set verify off
        
        tti 'Salary|Report'
        bti 'Confidential'
        break on deptno skip 1
                
        spool sal_rep.txt
        
        select deptno, empno, ename, job, sal
        from emp
        where deptno in (&sv_deptno)
        order by deptno, empno;
        
        spool off
        
        clear break
        tti off
        bti off
        
        set linesize 400
        set pagesize 100
        set feedback on
        set verify on

    SQL> @sal_rep.sql
    
    SQL> edit sal_rep.txt    














