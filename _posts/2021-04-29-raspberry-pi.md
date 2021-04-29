---
layout: post
title: Raspberry pi & DHT_11 & flask
date: 2021-04-28 22:54:00-0400
description: Raspberry pi & DHT_11 & flask
comments: true
---

<hr>
## <span style="color:#3459eb">Description</span>

이번 과제는 raspberry pi 를 이용하여 DHT11 sensor의 데이터를 불러 읽어서 온도, 습도를 측정하고 이를 flask를 이용해 웹 서버로 올려야 한다.

이번 과제를 해결하기 위해서는 아래 3가지 개념이 중요하다.

* Raspberry Pi
* GPIO
* Flask

<hr>

### <span style="color:#3459eb">Raspberry pi</span>

가장 먼저 Raspberry pi에 대해 알아보자. 라즈베리파이는 아래 그림과 같이 생긴 소형 컴퓨터이다. 하나의 보드 위에 모든 컴퓨터의 구성요소가 모두 들어가 있기 때문에 Single board computers 라고도 불린다.


<hr>
<p align="center">
  <img width="80%" height="80%" src="https://upload.wikimedia.org/wikipedia/commons/f/f1/Raspberry_Pi_4_Model_B_-_Side.jpg">
  <div style="bottom: 10px; right: 19px; font-size: 13px;">
    <p align="right">출처 : 위키피디아</p>
  </div>
</p>
<hr>

지난 게시글 중에서 [클릭](https://jgkang1210.github.io/blog/2021/arduino-make/) 아두이노는 MCU 라는 개념을 익혔고, 비슷하지만 미묘하게 다른 MPU라는 존재도 있다는 것을 언급한 적이 있다.

여기서 MPU란 CPU와 같은 의미로 쓰이고, 라즈베리파이와 같이 소형으로 제작된 컴퓨팅 유닛에게 부여되는 명칭이라고 했었다.

즉, 라즈베리파이는 아주 작은 컴퓨터라고 볼 수 이며 그림에서 보이는 USB나 HDMI 포트에 모니터와 마우스, 키보드를 연결하기만 하면 그냥 일반적인 컴퓨터처럼 쓸 수 있다.

초기 라즈베리파이는 2012년 출시하였고 메모리가 256MB로 아주 작았지만, 2020년 기준으로 8GB의 램을 가지는 라즈베리파이까지 출시를 하며 이렇게 작은 보드안에 강력한 컴퓨팅 파워를 가지는 SBC가 생기고 있다는 점은 참 신기하다.

작은 컴퓨터 답게 라즈베리파이는 자체적인 OS 인 라즈비안을 탑재하며 이는 리눅스(데비안) 기반이기 때문에 리눅스에 익숙하다면 금방 익힐 수 있다. 물론 자체적으로 Ubuntu 와 같은 다른 운영체제도 설치가 가능하지만 공식 지원은 아니다.

이렇게 작은 컴퓨터가 외부와 상호작용을 하기 위해서는 기본적인 입출력 지원 장치들이 필요할 것이다.

물론 USB를 이용해서 UART 통신으로 외부와의 상호 작용도 가능하지만, 작은 센서 하나를 사용하기 위해서 USB port를 사용하는 것은 큰 자원의 낭비가 될 수 있다. 특히 지난번 아두이노 과제를 수행하였을 때 처럼 버튼 입력을 위한 input pin과 LED 밝기를 제어하기 위한 PWM output pin 정도만 필요하다면 우리는 어떤 기능을 활요하면 좋을까?

라즈베리파이는 이에 대한 해답으로 GPIO를 제공한다.

<hr>
### <span style="color:#3459eb">GPIO</span>

GPIO는 General Purpose Input/Output의 약자로 runtime에 사용 용도가 결정되는 general 한 핀들이라고 생가하면 된다.

Runtime에 사용 용도가 정해진다는 뜻이 무엇일까? 아두이노에서 우리는 이미 GPIO의 기능을 사용해보았다. 예를 들어 우리가 13번 핀을 LED output으로 설정하기 위해서는 13번 핀의 pinmode를 output으로 수정했어야 했다. 하지만 만약 우리가 코드를 수정해서 13번 핀으로 디지털 input을 받고 싶다면 mode만 input으로 바꾸면 된다.

즉, 동일한 핀이여도 runtime(직접 코드를 실행을 했을 때)에 여러가지 기능들을 수행할 수 있게 도와주는 핀들이라고 보면된다.

GPIO는 Integrated circuit GPIO 와 Board-level GPIO로 나뉠 수 있다.

여기서 Integrated circuit GPIO는 MCU나 MPU 같은 유닛에서 지원하는 GPIO들을 말하며, FPGA와 같은 경우는 사용자가 칩 내부의 회로를 변경하는 과정이 있기 때문에 그냥 GPIO가 자연스럽게 쓰인다고 생각해도 된다.

우리의 관심사는 Board-level GPIO이다. 다양한 회로기판에서 GPIO는 존재하는데, 특히 하나의 작은 물건 안에 모든 컴퓨팅 유닛과 동작 유닛이 다 들어가 있는 Embedded system에서는 센서의 값들을 읽고 이를 통해 모터를 제어하거나 하는 출력을 내기 위해서 GPIO 들이 필요하다.

따라서 라즈베리파이도 약 50가지의 GPIO가 존재한다.

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/GPIO-Pinout-Diagram-2.png' | relative_url }}" alt="" title="raspi gpio"/>
    </div>
</div>

<div style="bottom: 40px; right: 19px; font-size: 13px;">
    <p align="right">출처 : https://www.raspberrypi.org/documentation/usage/gpio/</p>
</div>
<hr>

그리고 이러한 GPIO는 칩의 내부적인 회로 구성을 변경할 수 있기 때문에 라즈베리파이에서는 사용자에게 권한을 부여햐여야 GPIO에 접근할 수 있게 해두었으므로 이를 위하여 아래 명령어로 GPIO 그룹에 user를 추가해줘야 한다.

{% highlight linux %}
$sudo usermod -a -G gpio <username>
{% endhighlight %}

혹은

{% highlight linux %}
$sudo su
{% endhighlight %}

명령어로 root로 실행하면 되었다.

<hr>

### <span style="color:#3459eb">Furthermore..</span>

라즈베리파이에서 GPIO사용 및 DHT 11 센서를 사용하기 위해서 몇 가지 사항을 더 고려해줘야 한다.

<hr>
#### DHT11

우선 DHT11 센서는 온습도를 측정해주는 센서이다. 해당 센서를 단독으로 사용시 일정한 저항을 걸어주어야 하지만, 우리가 가지고 있는 DHT11 센서 모듈은 그런것을 고려하지 않고 바로 GPIO에 연결을 해도 된다.

<hr>
#### Virtual env

Python을 사용하다보면 다양한 라이브러리를 import 해서 사용해야 한다. 특히, DHT11과 같은 센서를 라이브러리 없이 바로 사용하기는 쉽지 않다. 따라서 제조사가 제공해주는 라이브러리를 PIP같은 패키지 매니저로 설치를 해서 사용하여야 한다.

하지만 만약 우리가 우리 컴퓨터에 python 3.7이 깔려있고, DHT11 의 패키지의 버전은 1.4.0인 상황에서 추가로 어떤 패키지를 깔아야 하는데 새로운 패키지의 의존성이 기존에 설치해둔 DHT11을 위한 의존성과 충돌할 수 있다.

더 쉽게 생각하면, 우리가 모듈 A를 설치하기 위해서는 모듈 B의 버전 1이 필요하고, 모듈 C를 설치하기 위해서는 모듈 B의 버전 2가 필요하다고 생각해보자.

그렇다면 당연히 모듈 A로 프로그램을 짜다가, 모듈 C를 설치하고 싶으면 모듈 B를 지우고 다시 버전을 바꾸어서 설치해야한다.

이는 당연히 비효율 적이며 파이썬은 이를 해결하기 위해 virtual environment를 제공해주며 이 기능은 굉장히 강력하다.

Virtual environment는 결국 가상적인 공간을 의미하며, 하나의 파이썬으로 모듈 A가 설치된 가상공간을 하나 만들고, 또 모듈 C가 설치된 가상공간 2를 만들어두면 사용자는 자신이 원하는 모듈을 쓰기 위해서 미리 만들어둔 가상 공간에 들어가기만 하면 된다.

이렇게 사용하는 것이 여러가지 패키지 관리 측면에서 유리하므로 가상 환경을 만들어서 사용하였다.

예시는 아래와 같고, 가상환경에 진입한 다음 사용자가 원하는 패키지를 pip를 활용하여 다운받으면 적용이 된다.

{% highlight linux %}
$ python3 -m venv cite201
$ cd cite201
~/cite201$ source bin/activate
(cite201) ~/cite201$
$ pip3 install Adafruit_DHT
{% endhighlight %}

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/rpi1.png' | relative_url }}" alt="" title="raspi tut1"/>
    </div>
</div>
<hr>

#### Setting for BCM2711

이 세팅은 라즈베리파이 4 B 모델 사용자에게 해당되는 팁이다.

우선 DHT11 라이브러리가 공식적으로 다른 라이브러리 변경되면서, 기존에 사용되던 라이브러리는 라즈베리파이 3 모델까지만 지원하는 상태에서 업데이트가 멈췄기 때문에 라즈베리파이 4 사용자는 몇 가지 추가 설정이 필요하다.

먼저 본인의 라즈베리파이 모델넘버를 확인하려면 아래 명령어를 입력해주면 된다.

{% highlight linux %}
$ cat /proc/cpuinfo
{% endhighlight %}

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/rpi3.png' | relative_url }}" alt="" title="raspi tut3"/>
    </div>
</div>
<hr>

그리고 맨 아래 hardware 란을 확인해보면 Raspberry pi 4 B 모델의 이름은 BCM2711이라는 것을 알 수 있다.

이제 DHT11 라이브러리에 가서 BCM2711을 마치 라즈베리파이 3인것 마냥 설정을 변경해주면 된다.

우선 라이브러리의 위치로 이동한다음 platform.py 를 수정하면 된다.

아래 디렉토리만 확인해봐도 라즈베리파이 4에 관련된 내용은 없는것을 알 수 있다.

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/rpi2.png' | relative_url }}" alt="" title="raspi tut2"/>
    </div>
</div>
<hr>

platform.py의 내부를 보면 라즈베리파이 1,2,3 만 인식을 하기 때문에 라즈베리파이 4를 쓰고 싶다면 BCM2711을 라즈베리파이 3과 같이 인식시켜주기 위해 112에서 114번줄의 코드를 추가했고, 디버깅용으로 print("hi")를 넣어뒀는데 이는 무시해도 무방하다.

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/rpi4.png' | relative_url }}" alt="" title="raspi tut4"/>
    </div>
</div>
<hr>

이러면 정말 DHT11 센서 사용을 위한 준비 작업은 끝이다.

<hr>
#### 실제 동작

sudo su 커맨드로 root 권한으로 아무런 문제 없이 gpio 핀을 사용하도록 하였고, cite201 이라는 virtual env에서 test.py를 실행해본 결과 디버깅용도의 메시지 출력 이후 온도와 습도 값이 정상적으로 나오는 것을 확인할 수 있었다.

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/rpi5.png' | relative_url }}" alt="" title="raspi tut5"/>
    </div>
</div>
<hr>

test.py 코드는 아래와 같다.

{% highlight python %}
import Adafruit_DHT
sensor = Adafruit_DHT.DHT11
pin = '4'
humidity, temperature = Adafruit_DHT.read_retry(sensor,pin)
if humidity is not None and temperature is not None:
    print('Temp={0:0.1f}*C Humidity={1:0.1f}%'.format(temperature, humidity))
else:
    print('Failed to get reading. Try again!')
{% endhighlight %}

<hr>

### <span style="color:#3459eb">Flask</span>

플라스크는 아르만 로나하가 만든 웹 프레임워크라고 한다.

Django 와 더불어 python web framework의 양대 산맥이라고 한다.

플라스크를 활용하기 위해선 pip manager로 설치해주면 된다. 아래와 같이 가상환경에 들어가서 Flask를 설치한다.

{% highlight linux %}
kang@raspberrypi:~ $ source cite201/bin/activate
(cite201) kang@raspberrypi:~ $ pip3 install Flask
{% endhighlight %}

이 외에도 위에서 설치한 Adafruit_DHT11 라이브러리 및 관련 설정도 모두 마쳐주어야 한다.

우선 완성된 코드는 [github repo (클릭)](https://github.com/jgkang1210/cite-dht11/tree/master) 에 존재한다.

app.py는 아래와 같다.

flask를 활용하여 app을 제작하였고, DHT11에서 센서값을 받아오기 위해서 위에서의 코드와 같은 코드를 활용하였다.

이 때 11번 줄에 정의된 get_sensor_data 라는 함수를 통해 우리의 앱이 실행될 때마다 센서에서 데이터를 가져올 수 있도록 하였다.

그리고 24번 줄을 보면 string 이라는 변수를 index.html에 보내는 것을 볼 수 있으며, 이때 보내지는 문자열은 sensor data, 즉 DHT11에서 오는 data이다.

26번줄과 27번줄은 라즈베리파이와 같은 인터넷을 사용하는 사용자가 라즈베리파이의 웹서버에 접속하기 위해서는 host_addr 값이 0.0.0.0 이여야하고, 포트 번호는 그냥 기본인 5000으로 설정해둬서 접속하였다.

{% highlight python linenos %}
from flask import Flask, render_template, request
import Adafruit_DHT

# flask setting
app = Flask(__name__)

# sensor setting
sensor = Adafruit_DHT.DHT11
pin = '4'

def get_sensor_data():
    humidity, temperature = Adafruit_DHT.read_retry(sensor,pin)
    data = ''
    
    if humidity is not None and temperature is not None:
        data = 'Temp={0:0.1f}*C Humidity={1:0.1f}%'.format(temperature, humidity)
    else:
        data = 'Failed to get reading. Try again!'

    return data

@app.route('/')
def hello_name():
   return render_template('index.html', string=get_sensor_data())

host_addr = "0.0.0.0"
port_num = "5000"

if __name__ == '__main__':
   app.run(host=host_addr, port=port_num, debug = True)
{% endhighlight %}

또한 template 폴더 아래에 index.html이 있는데 이는 아래와 같다.

위의 코드에서 받아온 string data가 body 에서 출력이 되는 것을 볼 수 있다.

{% highlight html linenos %}
<!DOCTYPE HTML>
<html>
  <head>
    <title>Flask app</title>
  </head>
  <body>
    <h1>{% raw %}{{ string }}{% endraw %}</h1>
  </body>
</html>
{% endhighlight %}

실제 작동 예시를 보자.

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/rpi6.png' | relative_url }}" alt="" title="raspi tut6"/>
    </div>
</div>
<hr>

먼저 sudo su command로 gpio 사용이 가능하게 하고 flask server를 가동시킨다.

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/rpi7.png' | relative_url }}" alt="" title="raspi tut7"/>
    </div>
</div>
<hr>

그러면 로컬에서는 192.168.0.45:5000으로 (사용자마다 ip 주소는 다를 것이다.) 라즈베리파이에 접속해서 온도와 습도값을 읽을 수 있다.

재미로 입김을 마구 불어넣으면 아래와 같이 온도와 습도가 올라간다.

<hr>
<div class="row justify-content-sm-center">
    <div class="col-sm-0 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/rpi8.png' | relative_url }}" alt="" title="raspi tut8"/>
    </div>
</div>
<hr>

웹사이트 제작은 완성이 되었는데 결국 클라이언트가 호출할때마다 백엔드에서 센서데이터를 주는 것이므로 프론트엔드 페이지가 주기적으로 리프레시 되는 기능도 추가해보면 좋을 것 같지만 웹 지식이 부족하므로 DB나 추가 기능은 생략한다..

<hr>