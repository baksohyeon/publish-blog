---
category: 질문 목록/Solved Problems/Error
share: true
title: 0 containers and 9 tests were Method or class mismatch 에러 해결
---

# 0 Containers and 9 Tests Were Method or Class Mismatch 에러 해결

**제목**: java stream.reduce() 사용법
**질문 날짜**: 2022.10.30.Sunday
**태그**: Java
**관련 글**: [1주차](../../../../1%EC%A3%BC%EC%B0%A8.md)

---

## 질문 내용

![Pasted image 20221030152957.png](../../../Attachments/Pasted%20image%2020221030152957.png)

## 질문 관련 서치 내용

[org.junit.platform.launcher.core.EngineDiscoveryOrchestrator lambda$logTestDescriptorExclusionReasons$7INFO: 0 containers and 4 tests were Method or class mismatch](https://choonsik-lab.tistory.com/entry/orgjunitplatformlaunchercoreEngineDiscoveryOrchestrator-lambdalogTestDescriptorExclusionReasons7INFO-0-containers-and-4-tests-were-Method-or-class-mismatch?category=961003)

검색해본 결과 Test 메서드중 1개만 실행 했을 때 INFO 메세지가 발생한다고 한다. 모든 Test를 진행하면 위와같은 메세지가 발생하지 않으므로 신경쓰지 않아도 된다. 
**위 INFO 메세지를 보고싶지 않다면 하기와 같이 설정을 변경하면 된다.** 
Settings -> Build, Execution, Deployment -> Build Tools -> Gradle 에서 Build and run using과 Run tests using의 속성을 Intellij IDEA로 변경
출처: https://choonsik-lab.tistory.com/entry/orgjunitplatformlaunchercoreEngineDiscoveryOrchestrator-lambdalogTestDescriptorExclusionReasons7INFO-0-containers-and-4-tests-were-Method-or-class-mismatch?category=961003 [Choonsik's Lab:티스토리]

## 질문을 정리하면서 새롭게 얻은 해답이 있는가?

위 INFO 메세지를 보고싶지 않다면 하기와 같이 설정을 변경하면 된다.

Settings -> Build, Execution, Deployment -> Build Tools -> Gradle 에서 Build and run using과 Run tests using의 속성을 Intellij IDEA로 변경
