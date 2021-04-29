---
layout: post
title: Arduino making(2)
date: 2021-04-28 21:49:00-0400
description: arduino making with kicad
comments: true
---

<hr>
# <span style="color:#3459eb"> Build Arduino with KiCad </span>

이번 시간에는 아두이노를 KiCad를 이용해서 마무리 해보겠다.

* Gerber format
* 3D model

<hr>

## <span style="color:#3459eb"> Gerber format </span>

Gerber format 이란 PCB 제작을 위해 공용적으로 사용되는 하나의 데이터 포맷이다.

PCB란 지난시간에 알아보았듯 인쇄하듯이 기판위에 다양한 부품들을 올려놓을 수 있게 만든 기술이였다.

따라서 PCB 위에 정확히 어떤 위치에 어떤 부품이 올라갈 것인지를 확인하기 위해서는 gerber format에 맞추어 설정해줘야 한다.

또한 PCB에 부품의 위치 뿐 아니라 다양한 드릴들이 기판에 구멍을 뚫어야 하기 때문에 드릴을 뚫을 위치부터 구멍의 크기 등등도 모두 설정해주어야 한다.

이를 gerber format이라 하고 아래 그림과 비슷한 형태로 생기게 된다.

<hr>
<p align="center">
  <img width="60%" height="60%" src="https://upload.wikimedia.org/wikipedia/commons/4/42/Openbiosprog-spi_PCB_Kicad_0.1_%284935009830%29.png">
  <div style="bottom: 10px; right: 19px;font-size: 13px;">
    <p align="right">출처 : 위키피디아</p>
  </div>
</p>
<hr>

이번에는 연습 삼아 4 layer로 아두이노를 제작해 보았고, gerber file은 추후에 업데이트 할 것이다.

<hr>

## <span style="color:#3459eb"> 3D model </span>

KiCad의 강력한 기능 중 하나는 PCB 기판의 gerber format을 토대로 3d 모델을 만들어 준다는 것이다.

3D 모델의 장점은 미리 구멍의 위치와 USB 핀의 방향등을 검토해볼 수 있고, 또 이를 export하여 회로기판 내부의 열전달 해석 등등에도 활용할 수 있다.

물론 지난 시간에 만들었던 BOM 파일을 모두 제대로 설정했을 경우에 한해서 3D 모델이 완성이 된다. (올바른 부품 없이 모델 제작은 당연히 안 된다.)

아두이노를 특별한 모양으로 만들고 싶어서 데일 밴드 형태로 만들어 보았고, 밴두이노 같은 이름을 지어주려고 한다.

사진은 아래와 같다.

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/arduino_made_3d.png' | relative_url }}" alt="" title="arduino made"/>
    </div>
</div>
<hr>

실제로 업체에 맡기면 뽑을 수 있긴한데 돈이 많지 않기 때문에 우선 실제 출력은 조금 미루기로 한다...ㅜㅜ