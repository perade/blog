---
title: "[번역] Behavior Driven Development(BDD) and Functional Testing"
path: "bdd-and-funtional-testing"
date: 2020-09-17 00:00:00
published: true
tags: ['Translation', 'Development']
canonical_url: false
hasImage: true
description: "BDD와 Funtional Testing이 무엇인지와 그 필요성에 대해 알아본다."
---

> Eric Elliot의 [Behavior Driven Development(BDD) and Functional Testing](https://medium.com/javascript-scene/behavior-driven-development-bdd-and-functional-testing-62084ad7f1f2/) 번역글입니다. 굳이 이 글이 아니더라도 모든 번역글은 역자의 의도와 상관없이 원문의 내용과 다르게 전달될 수 있으니 원문도 같이 보시는 걸 권해드립니다.

단위 테스트(Unit test)는 코드 단위가 애플리케이션의 나머지 부분과 격리되어 테스트되는 방법이다. 이를 통해 특정 함수, 객체, 클래스, 모듈 등을 테스트할 수 있으며, 애플리케이션의 개별 부분이 잘 작동하는지를 알아보는 데 유용하다.

하지만 단위 테스트는 이러한 코드 단위가 모여 전체 애플리케이션이 구성되었을 때에도 잘 작동하는지를 테스트하지는 않는다. 이를 위해서는 복수 개의 협동 테스트(Collaboration test)나 E2E 테스트(End-to-end test, aka System test) 등과 같은 통합 테스트(Integration test)가 필요하다.

시스템 테스트에는 행위 주도 개발(이하 BDD), 기능 테스트(Funtional test)를 포함한 여러 가지 방법론들이 있다.

![image](./images/behavior-driven-development.png)

***

# 행위 주도 개발이란 무엇인가?

BDD는 테스트 주도 개발(Test Driven Development)의 한 분야이다. BDD는 사람이 읽을 수 있는 사용자 요구사항 명세를 소프트웨어 테스트의 기반으로 사용한다. 도메인 주도 설계(Domain Driven Design)와 마찬가지로, BDD의 초기 단계는 이해관계자, 도메인 전문가, 엔지니어 간의 공유된 어휘를 정의하는 것이다. 이 단계에서는 엔티티, 이벤트, 출력 등을 정의하고, 정의한 요소들에 모두가 동의할 수 있는 이름을 지정하는 작업들이 포함된다.

그 후 실무자들은 사용자 인수 테스트(User Acceptance Test)와 같은 시스템 테스트를 작성하는데 사용할 수 있는 도메인 별 언어(Domain Specific Language)를 만들기 위해 이 어휘들을 사용한다.

각 테스트는 영어와 공식적으로 지정된 유비쿼터스 언어(모든 이해관계자가 공유하는 어휘)로 작성된 사용자 스토리를 기반으로 한다.

예를 들어 암호화폐 지갑의 이체 테스트는 아래와 같다.

```
스토리: 이체 후 잔액 변경

지갑 사용자로서
돈을 보내기 위해
지갑 잔액을 업데이트 해야한다.

내 잔액이 $40 이고,
친구 잔액이 $10 임을 감안할 때,
친구에게 $20을 송금하면
내 잔고는 $20이 되어야 한다.
그리고 친구는 $30이 되어야 한다.
```

이 언어는 소프트웨어의 UI나 목표를 달성하는 방법 보다는 고객이 소프트웨어로부터 얻어야 하는 비즈니스 가치에만 초점을 맞춘다는 것에 유의해야 한다. 대신 UX 디자인 프로세스의 시작점으로 사용할 수 있고, 이러한 종류의 사용자 요구사항을 미리 설계하면 실무자들과 고객이 어떤 제품을 만들고 있는지에 대해 동일한 입장을 취할 수 있도록 도울 수 있어 프로세스 후반에 생길 수 있는 많은 재작업들을 줄일 수 있다.

이 단계에서 아래 두 단계로 진행할 수 있다.

1. 설명을 도메인 별 언어로 변환하여 human-readable한 설명에 machine-readable한 코드가 추가되도록 테스트에 구체적인 기술적 의미를 부여한다. (즉 BDD를 계속 진행)
2. 사용자 스토리를 JavaScript, Rust 등과 같은 범용 언어를 활용하여 자동화된 테스트로 변환한다. (기능 테스트로 전환)

보통 어느 쪽이든 블랙박스 테스트로 처리하여 테스트 코드가 테스트 중인 기능의 구현 세부사항에 신경을 안 쓰게끔 하는 것이 좋다. 블랙박스 테스트는 화이트박스 테스트와 달리 구현 세부사항과 결합되지 않으므로 요구사항이 추가/변경되거나 코드가 리팩토링될 때 상대적으로 덜 취약하다.

BDD 지지자들은 [Cucumber](https://github.com/cucumber/cucumber-js/)와 같은 도구를 사용하여 맞춤형 DSL을 만들고 관리한다.

이와 반대로, 기능 테스트 지지자들은 일반적으로 사용자 인터랙션을 시뮬레이션하고 실제 출력과 예상 출력을 비교하여 기능을 테스트한다. 웹 애플리케이션의 경우, 이는 보통 타이핑, 버튼 클릭, 스크롤, 확대/축소, 드래그 등을 시뮬레이션하기 위해 웹 브라우저와 상호작용하는 테스트 프레임워크를 사용하는 것을 의미한다.

