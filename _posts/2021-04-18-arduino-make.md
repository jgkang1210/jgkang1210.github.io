---
layout: post
title: Arduino making
date: 2021-04-17 15:10:00-0400
description: arduino making with kicad
comments: true
---

<hr>
# <span style="color:#3459eb"> Build Arduino with KiCad </span>

이번 시간에는 아두이노를 KiCad를 이용해서 직접 만들어 보자.

* What is arduino?
* What is KiCad?
* Schematic tutorial
* BOM(bill of materials)

## <span style="color:#3459eb"> What is arduino? </span>

Arduinio 우노는 Atmega328이라는 MCU를 사용한다. 여기서 MCU는 Microcontroller unit의 줄임말이다.

<hr>
<p align="center">
  <img width="60%" height="60%" src="https://upload.wikimedia.org/wikipedia/commons/c/c8/Microcontrollers_Atmega32_Atmega8.jpg">
  <div style="bottom: 10px; right: 19px;font-size: 13px;">
    <p align="right">출처 : 위키피디아</p>
  </div>
</p>
<hr>

위는 우리가 아두이노 보드를 사용해 봤다면 한번쯤 봤을 법한 Atmega microcontroller이다.

Microcontroller라는 명칭은 이름에서 뜻을 유추해볼 수 있다. 마이크로는 그냥 작은 단위라고 생각하면 되고, controller는 무언가를 제어한다는 의미로 생각하면 된다. 그리고 MCU(=Microcontroller unit)는 아주 작은 크기의 MOSFET들로 이루어져있는데 MOSFET이라고 불리는 작은 스위칭이 가능한 반도체가 있다고 생각하면 좋을 것 같다.

컴퓨터를 잘 모르더라도 CPU(Central Processing Unit)라는 이름은 많이 들어봤을 것이다. CPU는 MPU(Micro processor Unit)라고도 불리며, 같은 의미이며 CPU는 컴퓨터에 들어가는 제어장치라고 생각하면 되고, MPU는 주로 임베디드에서 사용되는 용어라고 생각하면 된다.

CPU는 컴퓨터의 뇌라고 많이들 부른다. CPU는 연산 장치와 레지스터 그리고 제어장치로 이루어져 있다. 이번 글은 아두이노를 만드는 법에 대해서 다루는 것이므로 자세한 설명은 생략하고, CPU가 정상적으로 작동하기 위해서는 (뇌가 정상 작동하기 위해서) 다양한 주변 장치가 필요할 것이다. 예를 들면 메모리와 같은 장치들이 필요하다.

이러한 장치들 까지 하나의 칩으로 만들어서 해당 칩만 있으면 제어가 가능하게 만든 반도체를 MCU라고 부른다.

그렇다면 대체 무슨 제어를 하는 것일까? 우리가 생각할 수 잇는 모든 제어가 가능하다. 예를 들면 LED 깜빡이기와 같은 간단한 작업부터 복잡한 계산이나 소숫점 아래의 정확도를 요구하는 드론의 자세 제어 등등이 있을 것이다.

아두이노는 이러한 MCU를 사용하기 편하게 만들어둔 보드라고 생각하면 편하다. 우리가 MCU에게 명령을 주면 MCU는 열심히 계산해서 우리에게 다시 값을 준다. 그런데 MCU에게 명령을 주기 위해서는 어떻게 해야할까? 다양한 전기 신호를 주어야할 것이다. 아두이노에 탑재된 MCU의 이름은 Atmega 이고, 제조사는 Atmel이다. 생소할 수 있지만, 우리가 주로 사용하는 컴퓨터들에 들어가는 CPU는 Intel에서 만든 다양한 CPU가 들어가는 것 처럼 아두이노에 들어가는 컴퓨팅 유닛의 이름이 Atmega 시리즈라고 생각하면 쉽다.

아두이노는 다양한 input output을 위한 핀들이 있다. 우리는 이 핀들을 이용하여 LED에게 전압을 주기도 하고, 외부 센서들이 주는 전압을 읽어오기도 한다. 그 외에도 다양한 아날로그 신호를 읽을 수 있는 핀들도 있으며 이런 핀들은 ADC 등등이 필요하다.

결국 우리가 컴퓨터에 적절한 위치에 마우스, 키보드를 연결해서 사용하듯 아두이노도 내부에 있는 작은 컴퓨터 유닛인 Atmega MCU에 다양한 선들을 연결해서 외부의 LED, 그리고 센서의 값들을 읽을 수 있다고 생각하면 된다.

이러한 아두이노는 Atmega 칩은 구입이 가능하고, 회로도도 모두 공개되어 있기 때문에 직접 만들 수 있으며 이는 회로도를 참고하며 올바른 위치에 선을 잘 연결해주기만 하면 된다.

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/arduino_orig_sch.png' | relative_url }}" alt="" title="arduino original sch"/>
    </div>
</div>
<hr>

위는 아두이노 공식 홈페이지에 가면 다운 받을 수 있는 회로도이다. 굉장히 장치들이 많아보이는데 우리는 이 중에서 몇 가지만 추려서 구현해보려고 한다.

## <span style="color:#3459eb"> What is KiCad? </span>

사실 위에서 본 회로도는 실제 제품의 반쪽짜리다. 혹은 실제 제품 만들기 과정의 30%라고 해도 과언이 아니다.

실제 제품들은 위의 회로도를 바탕으로 아래와 같은 설계도를 그리고 그 위에 실제 부품들을 납땜한다.

<hr>
<p align="center">
  <img width="80%" height="80%" src="https://upload.wikimedia.org/wikipedia/commons/6/67/PCB_design_and_realisation_smt_and_through_hole.png">
  <div style="bottom: 10px; right: 19px;font-size: 13px;">
    <p align="right">출처 : 위키피디아</p>
  </div>
</p>
<hr>

왼쪽에 보이는 설계도를 만드는 과정을 Electronic cad 작업이라고 보면 된다. 다양한 3D 설계를 위해 solidworks를 사용하는 것처럼 다양한 PCB(Printed circuit board)를 위해서 다양한 ECad 툴이 있고 그 중 무료인 KiCad를 사용할 것이다.

PCB(Printed Circuit Board)란 인쇄 회로 기판을 말하며 회로 부품을 위에 올릴 수 있도록 인쇄해둔 기판이라고 보면 된다. 흔히 볼 수 있는 초록색 기판에 다양한 전자 부품들이 마구 부착되어있는것을 볼 수 있는데 이런것을 PCB 라고 부른다.

다양한 방식으로 PCB에 부품을 연결 할 수 있는데, 우선 PCB에 구멍을 뚫어서 납땜을 하는 Thru hole 방식도 있고, PCB 표면에 납땜을 할 수 있게 만들어서 다양한 전자부품을 붙이는 SMD(Surface mount device) 방식도 있다.

SMD 기술은 Thru hole 방식보다 더욱 고 집적도의 회로기판을 설계할 수 있게 만들어 준 기술이다. 생각해보면 구멍을 뚫은 부분은 한 개의 부품만 들어갈 수 있지만, SMD방식으로 표면에 붙이면 윗면과 아랫면을 모두 사용할 수 있으니까 더욱 효율적일 것이라고 직관적으로 받아들일 수 있다.

## <span style="color:#3459eb"> Schematic tutorial </span>

실제 KiCad 사용방법은 추후 추가 예정.

[회로도 링크(클릭)](https://postechackr-my.sharepoint.com/:b:/g/personal/jgkang1210_postech_ac_kr/EUrbfpFbYqxKsxsQANsEo3YBLU50qTuwaw5MVbwzZlAtzA?e=dtagtf)



<hr>
***전체회로도***
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-0 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/arduino_made.png' | relative_url }}" alt="" title="arduino made sch"/>
    </div>
</div>

<hr>
***전압 레귤레이터***
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-0 mt-md-0">
            <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/arduino_regulator.png' | relative_url }}" alt="" title="arduino original sch"/>
    </div>
</div>

<hr>
***Atmega 328***
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-0 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/arduino_atmega.png' | relative_url }}" alt="" title="arduino original sch"/>
    </div>
</div>
<hr>

<hr>
***Debug, USB***
<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/arduino_debug.png' | relative_url }}" alt="" title="arduino original sch"/>
    </div><div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/arduino_usb_power.png' | relative_url }}" alt="" title="arduino original sch"/>
    </div>
</div>
<hr>

핵심적인 부분만 최대한 만들었다.

Atmega 내부에 클락이 있지만 외부 크리스탈을 쓰는게 좋아 보여서 외부 크리스탈을 넣었고, 그 외에는 아두이노에 안정적인 전압을 주기위한 voltage regulator등을 넣었다. 부트로더는 제작하지 않고, USB to UART 칩을 사용하기로 결정했다.

따라서 리셋을 위한 스위치등도 선정이 되었다.

이러한 회로들에 대한 레퍼런스나 각각의 회로도를 만드는 방법은 각 소자의 datasheet를 보면 전부 친절히 나와있으니 참고하면 좋을 것 같다.

## <span style="color:#3459eb"> BOM(bill of materials) </span>

위에서 만든 PCB도 사실 반쪽이다. 판만 있으면 아무 동작을 안한다. 실제 부품들을 구입해서 판에 붙이기 위해서는 무슨 부품들이 필요한지 알아야 한다. 이러한 부품 선정은 사실 쉽지 않다.

그래서 직접 만들어 보면서 느끼는게 제일 좋은 방법같고 나도 잘하진 못한다. 그래서 그냥 내가 이번 아두이노를 제작하기 위해 선정한 부품들만 리스트로 올리고자 한다.

[BOM (클릭)](https://postechackr-my.sharepoint.com/:x:/g/personal/jgkang1210_postech_ac_kr/EY0i4hP2TXxJvRG4WtRsDwoBpVo8AYFaTW5uqxlSaFcI9w?e=n5McRX)

<hr>
BOM 리스트
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-0 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/arduino_bom.png' | relative_url }}" alt="" title="arduino original sch"/>
    </div>
</div>
<hr>

정말 다양한 부품들이 들어가고 각각의 부품에 무엇을 쓸 것인지 부터 구매링크가 모두 있다.

우선 이번 시간은 여기서 마치고 다음 기회에 실제 PCB 제작을 위한 ECad 과정 및 실제 납땜을 통한 아두이노 제작을 해보겠다.