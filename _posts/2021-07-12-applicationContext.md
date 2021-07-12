---
layout: single
title:  "JUnit 실행시 발생하는 ApplicationContext"
---


1. JUnit  실행시 발생현황

```java

Failed to load ApplicationContext
java.lang.IllegalStateException: Failed to load ApplicationContext
```

2. 해결방안

 1) @WebAppConfiguration 어노테이션 삽입.
 2) @SpringBootTest(classes = {}) 에 찾지못하는 bean을 등록해주기.