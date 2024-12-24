주어진 쿼리에서 오류가 발생하는 이유는 `ROWNUM`이 `ORDER BY`절 뒤에서 처리되기 때문입니다.

`ROWNUM`은 쿼리 결과 집합에서 반환되는 행의 순서를 나타내는 의사열입니다. 이는 쿼리의 결과가 생성된 후에 부여되므로, `ROWNUM`을 사용하여 결과를 제한하기 전에 정렬이나 다른 필터링 조건을 적용해야 합니다.

따라서 `ROWNUM`을 사용하여 결과를 제한하기 전에 서브쿼리를 사용하여 정렬을 수행해야 합니다. 다음은 수정된 쿼리입니다.





`ROWNUM`이 정렬된 결과에 영향을 미치려면 서브쿼리나 윈도우 함수를 사용해야 합니다. 하지만 `ROWNUM`은 `ORDER BY` 절 뒤에서 처리되기 때문에 직접적으로 정렬된 결과에 영향을 주는 것은 불가능합니다.

그러나 Oracle 12c부터는 `FETCH FIRST` 구문을 사용하여 정렬된 결과 집합에서 처음 몇 개의 행을 가져올 수 있습니다. 따라서 다음과 같이 `FETCH FIRST`를 사용하여 처음 10개의 행을 가져오는 것이 가능합니다.

```
WITH fish_len AS (
    SELECT id, length
    FROM fish_info
    WHERE length IS NOT NULL
    ORDER BY length DESC, id
)
SELECT *
FROM fish_len
FETCH FIRST 10 ROWS ONLY;

```
이것은 `FETCH FIRST` 구문을 사용하여 정렬된 결과 집합에서 처음 10개의 행을 가져오는 것으로, `ROWNUM`을 사용하는 것보다 더 간단하고 직관적입니다. 그러나 이 방법은 Oracle 12c 이상에서만 지원됩니다.