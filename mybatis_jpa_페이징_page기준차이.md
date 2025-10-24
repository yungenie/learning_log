# Mybatis vs Jpa Page 기준값 차이

MyBatis 와 JPA 의 페이징 offset 기준이 다릅니다.

### ⚙️ 차이 요약

구분시작 기준예시 (`page=1`, `size=10`)비고

**MyBatis / SQL 직접 작성 1부터 시작**
`OFFSET = (page - 1) * size = 0`보통 사람이 보는 "1페이지" 기준

**JPA / Spring Data JPA 0부터 시작**
`PageRequest.of(0, 10)` → 1페이지 내부적으로 0 기반 인덱스 사용



### 💡 예시로 보면
✅ MyBatis
```SELECT * FROM payment
LIMIT #{size} OFFSET #{(page - 1) * size}
```

➡️ `page = 1` → OFFSET 0
➡️ `page = 2` → OFFSET 10
즉, `page`가 **1부터 시작**한다고 보고 계산합니다.

### ✅ JPA (Spring Data)
```
PageRequest pageRequest = PageRequest.of(0, 10);
```

➡️ 첫 페이지를 `0`으로 설정해야 **1페이지 결과**가 나옵니다.
즉, `page`가 **0부터 시작**한다고 보는 구조예요.

### ✅ 결론
항목offset/page 시작 값
MyBatis (SQL)`1` 페이지부터 시작
JPA (`PageRequest`)`0` 페이지부터 시작
그래서 같은 “첫 페이지”를 의미하더라도
MyBatis에서는 `page=1`,
JPA에서는 `page=0` 으로 설정해야 맞습니다.
