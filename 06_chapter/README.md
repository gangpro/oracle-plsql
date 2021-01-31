# 6장. 조합 데이터 유형 작업
> Composite Data Type

## 분류
    * PL/SQL Record
    * PL/SQL Collection -> Associative array (or index-by table) : PL/SQL Table
                        -> VARRAY (variable-size array)
                        -> Nested table
* https://docs.oracle.com/cd/E11882_01/appdev.112/e25519/composites.htm#LNPLS005

## 6-21. Collection Methods
* https://docs.oracle.com/cd/E11882_01/appdev.112/e25519/composites.htm#LNPLS00508

## %rowtype 활용 예제
    create or replace procedure p1(a in jobs.job_id%type, b out jobs%rowtype)
    is
    begin
        select * into b
          from jobs
         where job_id = upper(a);
    end;
    /
    
    --아래 함수에서 위 프로시져 호출
    create or replace function f1(j in jobs.job_id%type)
        return jobs.job_title%type
    is
        r jobs%rowtype;
    begin
        p1(j, r);
        
        return r.job_title;
    end;
    /
    
    exec dbms_output.put_line('abc')
    exec dbms_output.put_line(f1('ad_pres'))
    exec dbms_output.put_line(f1('st_man'))













