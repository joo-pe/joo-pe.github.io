---
layout: single
title:  "required a bean of type '' that could not be found.'"
---

1. 발생에러

``` text
Parameter 0 of constructor in {class} required a bean of type '{class}}' that could not be found.
```
- 컴파일 중에 오류는 발생하지는 않으나 해당 클래스를 찾지 못하는 경우 발생

2. 발생원인

- bean으로 등록되지 않은 클래스를 사용해서 발생하는 문제


3. 해결방안

- 해당 클래스를 bean으로 등록시켜준다. 
  [bean으로 등록 시키는 방법]
  1. 찾지 못하는 클래스를 @Bean 이나 @Component 를 사용하여 등록한다.
  2. ComponentScan을 이용하여 Bean을 등록한다.
  

  