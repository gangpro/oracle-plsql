# 5장. 제어 구조 작성
> 제어 구조 

## 제어구조
* Selection : if, case
* Iteration : basic loop, while loop, for loop
* Sequnce : goto, null

* https://docs.oracle.com/cd/B28359_01/appdev.111/b28370/controlstructures.htm#LNPLS00401

## 5-28.

    begin
        for i in 1..3 loop
        --i := 100;     --에러
        p.p(i);
        end loop;
    end;
    /

## 문제. 구구단 만들기

    begin
        for i in 1..9 loop
        p.p(2 * i);
        end loop;
    end;
    /
    
     begin
        for i in 2..9 loop
            for j in 1..9 loop
        p.p(i||' * '||j||' = '||i * j);
            end loop;
        end loop;
    end;
    /   
