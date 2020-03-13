## 입양 시각 구하기 (2)
https://programmers.co.kr/learn/courses/30/lessons/59413

* set을 사용하면 변수처럼 선언하고 값을 부여할 수 있다.
* row마다 `@hour`의 값이 1씩 증가하며
* datetime에서 찾은 HOUR()의 값이 @hour과 같은 경우의 수를 COUNT()연산한다.
* 24보다 작을때까지만 수행한다.
* 포인트: GROUP BY로 SELECT했을 때 나오지 않는 row를 만들어주는 연산이 필요하다. (count를 0으로 표시하기 위해)
    * 아래와 같은 sql문을 수행하면 0개의 row가 있는 GROUP은 결과값에 포함되지 않는다.
    ``` sql
    SELECT COUNT(*)
    FROM ANIMAL_OUTS
    GROUP BY HOUR(datetime)
    ```

#### 정답
``` sql
set @hour := -1;
SELECT 
    (@hour := @hour + 1) HOUR,
    (SELECT COUNT(*) FROM ANIMAL_OUTS WHERE HOUR(datetime) = @hour) COUNT
FROM ANIMAL_OUTS
WHERE @hour < 23;
```

<br><br>


## 우유와 요거트가 담긴 장바구니
https://programmers.co.kr/learn/courses/30/lessons/62284

* FROM절에서 NAME에 "요거트"가 있는 row들을 찾은 후
* 원래 테이블과 inner join해서 NAME에 "우유"와 "요거트"가 모두 있는 테이블을 찾는다.

#### 정답
``` sql
SELECT DISTINCT M.CART_ID
FROM 
    CART_PRODUCTS M,
    (SELECT * FROM CART_PRODUCTS WHERE NAME = "요거트") Y
WHERE M.CART_ID = Y.CART_ID AND M.NAME = "우유" 
;
```

