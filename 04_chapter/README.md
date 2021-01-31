# 4장. 오라클 데이터베이스 서버와 상호 작용
> SQL Statement in Blocks

## 4-4. 

      BEGIN
        - DDL : CREATE, ALTER, DROP, RENAME, TRUNCATE, COMMENT   : 사용 불가
        - DML : INSERT, UPDATE, DELETE, MERGE, SELECT            : 사용 가능
        - TCL : COMMIT, ROLLBACK, SAVEPOINT                      : 사용 가능
        - DCL : GRANT, REVOKE                                    : 사용 불가
      END;

      => 이유 : https://docs.oracle.com/cd/A57673_01/DOC/server/doc/PLS23/ch5.htm#toc043

begin end 안에 데이터가 한개도 없거나 여러개면 select문은 에러 발생
그러나 update, insert의 경우는 에러가 아니다.
다만 select문에 bulkup 또는 명시적 커서를 사용하면 괜찮다.

## 4-16.

    drop table t1 purge;
    
    create table t1 as select * from emp;
    
    begin
        update t1
        set sal = sal*1.1
        where deptno = 10;
        
        p.p('수정된 행은 '||sql%rowcount||'건 입니다.');
        
        delete from t1
        where deptno = 20;
        
        p.p('삭제된 행은 '||sql%rowcount||'건 입니다.');

    end;
    /
