---
cover: ../.gitbook/assets/dd9d76e1-0c22-41ac-b8d7-1b352b3a9fc6.png
coverY: 0
layout:
  cover:
    visible: false
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# CAN Protocol

# CAN 프로토콜과 OSI 7계층

이 섹션에서는 OSI 7계층을 기반으로 한 CAN (Controller Area Network) 프로토콜의 구조와 각 계층의 역할에 대해 설명합니다.

## CAN 프로토콜 개요

CAN은 본래 자동차 산업을 위해 개발된 ISO (국제표준화기구) 정의 직렬 통신 버스로, 복잡한 배선 대신 두 개의 전선 버스를 사용합니다.

### OSI 모델과의 관계

CAN 통신 프로토콜, ISO 11898은 네트워크 상의 장치 간 정보 전달 방식을 설명하며, OSI (Open Systems Interconnection) 모델의 계층에 따라 정의됩니다.

## 물리 계층

물리 계층은 모델의 가장 낮은 계층으로, 실제 장치 간의 통신을 정의합니다.

* **정의**: 장치 간 연결을 위한 물리적 매체를 통한 실제 통신을 담당합니다.
* **역할**: 전기적, 기계적, 절차적 특성을 정의하여 물리적 데이터 전송을 가능하게 합니다.

## 데이터 링크 계층

ISO 11898 아키텍처는 OSI/ISO 모델의 일곱 계층 중 데이터 링크 계층과 물리 계층을 가장 낮은 두 계층으로 정의합니다.

* **정의**: 네트워크 장치 간의 데이터 전송 및 오류 검출 기능을 담당합니다.
* **역할**: 프레임 구조 및 오류 검출, 오류 수정 기능을 제공합니다.

## 응용 계층

응용 계층은 상위 레벨의 응용 프로그램 특화 프로토콜과의 통신 링크를 설정합니다.

* **정의**: 상위 레벨의 응용 프로그램 특화 프로토콜과의 통신을 담당합니다.
* **예시**: 벤더 독립적인 CAN open 프로토콜 같은 특정 응용 프로그램 프로토콜 지원.
* **역할**: 사용자 및 제조업체 그룹 CAN in Automation (CIA)에 의해 지원되는 프로토콜을 사용하여 통신합니다.

## 결론

CAN 프로토콜은 OSI 7계층 모델에 기반하여 구축되어, 각 계층의 역할을 통해 효율적이고 신뢰할 수 있는 데이터 통신을 가능하게 합니다.

