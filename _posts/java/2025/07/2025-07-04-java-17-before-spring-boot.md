---
layout: post
title: 스프링 부트 3를 공부하기 전, 정리하는 자바 17 주요 내용
date: 2025-07-04
categories: java
tags: [java17]
---

스프링 부트3 공부하려고 책 읽는데, 자바 17을 사용한다고 하여, 주요 변경사항이 정리되어 있었다.

나도 블로그에 적어놔야지.

# 텍스트 블록

이제 string을 +없이 여러줄로 적을 수 있다.
``` java
String beforeQuery = "SELECT * FROM \"items\"\n" + 
	"WHERE \"status\" = \"ON_SALE\"\n" + 
	"ORDER BY \"price\"; \n";

String after17Version = """
	SELECT * FROM "items"
	WHERE "status" = "ON_SALE"
	ORDER BY "price";
    """;
```

# formatted() 메서드

값을 파싱하기 위해 사용한다.
``` java
String textBlock = """
{
	"id": %d,
	"name": %s
}
""".formatted(2, "juice");
```
텍스트 블록과 같이 사용하여 쿼리 짜기 좋겠다.

# 레코드

데이터 전달을 목적으로 하는 객체를 더 빠르고 간편하게 만들기 위한 기능.

상속할 수 없고 파라미터에 정의한 필드는 private final로 정의됨.

또한 getter를 자동으로 만들기 때문에 애너테이션이나 메서드로 getter 정의 하지 않아도 됨
``` java
record Item(String name, int price) {
	// 이렇게 하면 파라미터가 private final로 정의된다.
	// private final String name;
	// private final int price;
}

Item juice = new Item("juice", 3000);
juice.price(); // 3000
```
이제 vo를 record로 만들면 되는건가.

새로운 개념이라 공부가 더 필요하겠다.

# 패턴 매칭

instanceof 키워드를 조금 더 쉽게 사용할 수 있게 해줌.

이전에는 instanceof 키워드와 형변환 코드를 조합해야 했지만 이제는 바로 형변환 가능.
``` java
// 이전 버전
if (o instanceof Integer) {
	Integer i = (Integer) o;
}

// 17버전
if (o instanceof Integer i) {
	...
}
```

# 자료형에 맞는 case 처리
``` java
static double getIntegerValue(Object o) {
	return switch(o) {
    	case Double d -> d.intValue();
        case Float f -> f.intValue();
        case String s -> Integer.parseInt(s);
        default -> 0d;
    };
}
```