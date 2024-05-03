# ffmpeg를 이용해 포맷 변환하기

## ffmpeg

* 멀티미디어 프레임워크
* 비디오 및 오디오 변환, 스트리밍, 레코딩 등 다양한 작업을 수행하는데 사용한다.&#x20;

### Install (mac)

```
brew install ffmpeg
```



### 포맷 변환

```shell
ffmpeg -i input.mov -c:v libvpx-vp9 output.webm
```

* input.mov 파일을 VP9 코덱을 사용하여 output.webm 비디오로 변환

{% hint style="warning" %}
VP9은 품질과 압축률이 뛰어난 특징을 갖지만, 인코딩 시간이 길어질 수 있다.
{% endhint %}



### 해상도 조절

```sh
ffmpeg -i input.mov -vf scale=-1:720 -c:v hevc_videotoolbox -allow_sw 1 -alpha_quality 0.9 -tag:v hvc1 -q:v 35 -vf premultiply=inplace=1 output.mp4
```

* input.mov 비디오를 outpuo.mp4로 변환
* `-vf scale=-1:720` : 비디오 필터를 사용하여 해상도를 조절
  * 높이를 720픽셀로 설정
  * aspect-ratio를 유지하기 위해 -1로 자동설정
* `-c:v hevc_videotoolbox` : 비디오 코덱으로 hevc\_videotoolbox를 사용
* `allow_sw 1` : 하드웨어 가속이 사용 불가능할 때, 소프트웨어 인코딩을 허용함
* `alpha_quality 0.9` : 알파 채널(투명성)의 품질을 설정, 0.9는 높은 품질을 나타냄
* `tag:v hvc1` : 비디오 스트림에 hvc1을 추가함. 이는 일부 장치와 플레이어에서 HEVC 콘텐츠의 호환성을 향상시키기 위해 사용
* `q:v 35` : 비디오의 품질을 설정.&#x20;
  * 낮은 값은 높은 품질을, 높은 값은 낮은 품질을 나타냄&#x20;
  * 일반적으로 0-51 범위를 사용하며, 23이 기본값
* `vf premultiply=inplace=1` : 비디오 필터를 사용하여 알파 채널을 사전에 곱하도록 설정
  * `inplace=1`은 현장에서 곱셈 작업을 수행
