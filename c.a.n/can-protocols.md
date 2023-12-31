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

**CAN**은 국제 표준화 기구(ISO)에 의해 정의된 직렬 통신 버스로, 원래 자동차 산업을 위해 복잡한 배선 대신 두 개의 전선 버스로 대체하기 위해 개발되었습니다.

CAN 통신 프로토콜, **ISO 11898**은 네트워크 상의 장치들 사이에서 정보가 어떻게 전달되는지를 설명하며, **OSI(Open Systems Interconnection) 모델의 계층적 구조에 따라 정의**됩니다.

실제 장치 간의 통신은 모델의 물리 계층에 의해 정의됩니다. ISO 11898 아키텍처는 OSI/ISO 모델의 일곱 계층 중 가장 낮은 두 계층인 **데이터 링크 계층**과 **물리 계층**으로 정의합니다.

<figure><img src="https://instrumentationtools.com/wp-content/uploads/2022/09/CAN-Protocol-with-the-OSI-Model.png?ezimgfmt=ng:webp/ngcb2" alt=""><figcaption></figcaption></figure>

**응용 계층**은 상위 수준의 애플리케이션 특정 프로토콜로의 통신 링크를 설정합니다. 이러한 프로토콜 중 하나로는 벤더 독립적인 CAN open 프로토콜이 있으며, 이는 국제 사용자 및 제조업체 그룹인 CAN in Automation (CIA)에 의해 지원됩니다.

***

## \[ Physical Layer ]&#x20;

{% hint style="info" %}
**물리 계층**은 모델의 가장 낮은 계층으로, 실제 장치 간의 통신을 정의합니다.
{% endhint %}

## 1.Bus Length vs Signaling Rate

<table data-full-width="true"><thead><tr><th align="center">Bus Length(m)</th><th align="center">Signaling Rate(m)</th></tr></thead><tbody><tr><td align="center">40</td><td align="center">1</td></tr><tr><td align="center">100</td><td align="center">0.5</td></tr><tr><td align="center">200</td><td align="center">0.25</td></tr><tr><td align="center">500</td><td align="center">0.10</td></tr><tr><td align="center">1000</td><td align="center">0.05</td></tr></tbody></table>

**CAN 네트워크의 최대 버스 길이는 신호 속도에 따라 달라집니다.** 전송 거리가 증가함에 따라 신호 속도는 감소합니다. 주요 제한 요인은 케이블 대역폭 제한으로, 이는 신호 전환 시간을 저하시키고 기호 간 간섭을 일으킵니다. 1000미터 길이의 버스에서 안전하게 사용할 수 있는 신호 속도는 약 50kbps입니다.&#x20;

$$Signaling Rate (Mbps) × Bus Length (m) ≤ 50$$

이러한 안전 여유를 통해 시스템 변화가 발생해도 통신이 중단되지 않습니다. 노드 수가 많고 케이블 길이가 긴 경우, 더 높은 품질의 케이블이나 CAN 버스 중계기 사용이 필요할 수 있습니다. 그러나 실제로는 거의 모든 종류의 케이블이 짧은 거리에서는 어느 정도까지는 작동합니다.

## 2. Cables

비차폐된 120-Ω 케이블이 많은 응용 프로그램에서 사용되지만, CAN 트랜시버를 사용하는 데이터 전송 회로는 광범위한 **공통 모드 전압 범위**에서 견고한 연결이 필요한 작업에 사용됩니다. 따라서, 이러한 전자적으로 가혹한 환경에서는 Belden Cable 3105A와 같은 **차폐 케이블이 권장**됩니다.&#x20;

<div align="left">

<figure><img src="https://ko.kbs-connector.com/Content/uploads/2021625326/202105141506056da5e6410dcf4c08b90874c7ed9f9731.jpg" alt="" width="375"><figcaption><p><strong>차폐 케이블</strong></p></figcaption></figure>

 

<figure><img src="https://ko.kbs-connector.com/Content/uploads/2021625326/202105141506267a7e22a284df48f0aa3ac620f68bf717.jpg" alt="" width="375"><figcaption><p><strong>비차폐 케이블</strong></p></figcaption></figure>

</div>

{% hint style="info" %}
"**공통 모드 전압**"은 이 두 선이 동일하게 영향을 받는 전압 변화를 말합니다. 예를 들어, 두 선 모두 동시에 2V 증가하면, 이는 공통 모드 전압 변화입니다. "공통 모드 전압 범위"는 이러한 공통 모드 변화가 **시스템에서 안정적으로 처리될 수 있는 전압 범위**를 의미합니다. 따라서 이 범위 내에서는 외부 간섭에 의한 영향이 최소화되며, 신호 전송의 정확성이 유지됩니다.
{% endhint %}



## 3. Grounding

<figure><img src="https://danfosseditron.zendesk.com/hc/article_attachments/10832372580509" alt=""><figcaption></figcaption></figure>

CAN 통신에서 접지(Ground)를 연결하는 주된 이유는 **전기적 노이즈와 간섭을 최소화하고, 시스템의 전기적 안정성을 높이기 위함**입니다. CAN 시스템은 차량과 같이 전기적 노이즈가 많은 환경에서도 안정적으로 작동해야 하며, 접지는 이러한 노이즈로부터 시스템을 보호합니다. 또한, 접지 연결은 CAN 네트워크의 다양한 구성 요소 간에 공통의 참조 전압을 제공하여, 신호 무결성을 유지하는 데 도움이 됩니다. **연결하지 않아도 통신이 가능**할 수 있지만, 안정적인 통신을 위해서는 접지 연결이 권장됩니다.\


## 4.Line Terminations

CAN (Controller Area Network) 통신에서 양쪽 끝에 **120-Ω의 저항을 설치하는 이유는 전송선로의 반사파(reflections)를 최소화하기 위해서**입니다. 이러한 반사파는 신호의 왜곡이나 간섭을 일으킬 수 있으며, 특히 높은 데이터 전송 속도에서 더욱 문제가 됩니다.

CAN 네트워크는 다수의 노드(node)가 트위스티드-페어 케이블을 통해 연결되는 버스 구조를 가지고 있습니다. 이러한 구조에서 신호는 케이블을 따라 양쪽 방향으로 전파됩니다. 케이블의 끝에 도달하면, 신호는 반사되어 다시 돌아옵니다. 이 **반사된 신호는 원래 신호와 중첩되어 간섭을 일으킬 수 있습니다.**

120-Ω의 저항을 케이블의 양쪽 끝에 설치함으로써, 케이블의 특성 임피던스와 일치시키게 됩니다. 특성 임피던스란 케이블이 전기 신호를 전송할 때 나타내는 저항값을 의미합니다. 케이블의 특성 임피던스와 종단 저항이 일치하면, 신호는 **케이블 끝에서 반사되지 않고 저항에 의해 흡수**됩니다. 이렇게 함으로써 신호의 반사를 최소화하고, 데이터 전송의 신뢰성을 높일 수 있습니다.&#x20;

<figure><img src="../.gitbook/assets/Screenshot from 2023-12-31 17-31-49.png" alt=""><figcaption><p>120Ohm 설치 여부 결과 사진</p></figcaption></figure>

## 5. Connectors

**일반적인 CAN 통신 커넥터 (창고용)**

<figure><img src="../.gitbook/assets/Screenshot from 2023-12-31 17-40-04.png" alt=""><figcaption><p>9-pin DSUB</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot from 2023-12-31 17-42-09.png" alt=""><figcaption><p>5-Pin Mini-Connector</p></figcaption></figure>

### \[ Data Link Layer ]

ISO 11898 아키텍처는 OSI/ISO 모델의 일곱 계층 중 데이터 링크 계층과 물리 계층을 가장 낮은 두 계층으로 정의합니다.

* **정의**: 네트워크 장치 간의 데이터 전송 및 오류 검출 기능을 담당합니다.
* **역할**: 프레임 구조 및 오류 검출, 오류 수정 기능을 제공합니다.

### \[ Appliacation Layer ]

응용 계층은 상위 레벨의 응용 프로그램 특화 프로토콜과의 통신 링크를 설정합니다.

* **정의**: 상위 레벨의 응용 프로그램 특화 프로토콜과의 통신을 담당합니다.
* **예시**: 벤더 독립적인 CAN open 프로토콜 같은 특정 응용 프로그램 프로토콜 지원.
* **역할**: 사용자 및 제조업체 그룹 CAN in Automation (CIA)에 의해 지원되는 프로토콜을 사용하여 통신합니다.

{% embed url="https://danfosseditron.zendesk.com/hc/en-gb/articles/360042232992-CAN-bus-physical-layer" %}

{% file src="../.gitbook/assets/slla270.pdf" %}
Control Area Network Physical Layer Requirements
{% endfile %}

