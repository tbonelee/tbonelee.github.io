---
layout: post
title: "[Spring-Boot] properties 값 객체로 바인딩"
date: "2022-11-12 18:01:02 +0900"
categories:
  - 자바
---
## `@Value` 어노테이션 대신 사용하는 이유?



[링크](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config.typesafe-configuration-properties.vs-value-annotation)



- 온전한 relaxed binding 허용
	- `@Value` 어노테이션에서는 지원되는 부분이
	 있고 아닌 부분이 있기 때문에 일관적이지 않을 수 있다.
- metadata support
	- properties 파일을 작성할 때 소스 코드에 있는 프로퍼티
	 정보대로 IDE가 자동 완성을 제공할 수 있도록 지원한다.
- 프로퍼티 구조가 여러 단계로 계층화되어 있으면 클래스로
 맵핑하는 것이 직관적일 수 있다.


## IDE support을 위한 설정



 IDE의 자동 완성을 지원하려면 다음의 의존성 추가가 필요하다.
 



```False
dependencies {
    annotationProcessor "org.springframework.boot:spring-boot-configuration-processor"
}
```


[링크](https://docs.spring.io/spring-boot/docs/2.7.3/reference/html/configuration-metadata.html#appendix.configuration-metadata.annotation-processor)



## 작성 예시


- 스프링 빈 생성과 비슷하게 생성자 주입 방식 / 필드 주입
 방식이 존재한다.
	- 객체 생성 시에 온전하게 값을 가지고 있는 것이 안전한
	 방법이라 생각하여 생성자 주입 방식을 사용하는 것이
	 낫다고 생각했고, 여기서는 생성자 주입 방식 예시를
	 작성했다.
- 필수적인 프로퍼티를 누락하거나 잘못된 형식으로 입력했을 때
 앱에서 예외를 던지는 것이 필요하다고 생각해서
 `@Validated` 어노테이션을 추가했다.


### yml 파일



```False
jwt:
  access-secret: "access secret must be at least 256 bits long for JWt HMAC-SHA algorithm"
  refresh-secret: "refresh secret must be at least 256 bits long for JWt HMAC-SHA algorithm"
  access-expiration: 600
  refresh-expiration: 86400
```

### 클래스 파일



```False
package org.project.configuration;

import javax.validation.constraints.NotNull;
import lombok.Getter;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.context.properties.ConstructorBinding;
import org.springframework.validation.annotation.Validated;

@Getter
@Validated
@ConstructorBinding
@ConfigurationProperties("jwt")
public class JwtProperties {

  @NotNull
  private final String accessSecret;
  @NotNull
  private final String refreshSecret;
  @NotNull
  private final Long accessExpiration;
  @NotNull
  private final Long refreshExpiration;

  public JwtProperties(String accessSecret, String refreshSecret, Long accessExpiration,
      Long refreshExpiration) {
    this.accessSecret = accessSecret;
    this.refreshSecret = refreshSecret;
    this.accessExpiration = accessExpiration;
    this.refreshExpiration = refreshExpiration;
  }
}
```
