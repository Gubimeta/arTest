# arTest

**웹 AR을 간단하게 테스트해 보는 코드입니다.**

2026년 읽걷쓰 기반 대학연계 AI STEAM 진로체험 프로그램을 위해 거비메타에서 샘플로 만들었습니다.

휴대폰 브라우저에서 카메라를 켜면, 카메라 영상 위에 3D 물고기들이 떠다닙니다.
앱 설치도, 마커 인쇄도 필요 없습니다.

👉 **[바로 열기](https://gubimeta.github.io/arTest/)**

> 카메라 권한이 필요하므로 반드시 `https://` 주소로 접속해야 합니다.
> 휴대폰(iOS Safari / Android Chrome)에서 열어야 자이로센서가 동작합니다.

---

## 무엇을 테스트하는 코드인가

별도의 AR 엔진이나 SDK 없이, **브라우저 기본 기능만으로** AR 비슷한 경험을 어디까지 만들 수 있는지 확인해 보는 예제입니다.

| 요소 | 사용한 것 |
|---|---|
| 카메라 영상 | `getUserMedia` — HTML `<video>` 를 배경에 깔기 |
| 3D 렌더링 | [A-Frame](https://aframe.io) (three.js 기반) |
| GLB 애니메이션 | [aframe-extras](https://github.com/c-frame/aframe-extras) 의 `animation-mixer` |
| 시선 추적 | 휴대폰 자이로센서 (`DeviceOrientationEvent`) |
| 터치 조작 | Pointer Events + three.js 레이캐스팅 |

**빌드 과정이 없습니다.** HTML 파일 하나와 GLB 파일들을 정적 호스팅에 올리면 끝입니다.

### 조작 방법

| 동작 | 결과 |
|---|---|
| 휴대폰을 돌리기 | 시선이 따라 움직임 (물고기는 제자리에) |
| 물고기를 한 손가락으로 끌기 | 물고기를 원하는 곳으로 옮김 |
| 빈 곳을 좌우로 쓸기 | 물고기 무리 전체를 회전 |
| 두 손가락 핀치 | 물고기 크기 조절 |
| 하단 🐟 버튼 | 마릿수 늘리기 / 줄이기 |

주소 뒤에 `?n=20` 을 붙이면 그 마릿수로 시작합니다.

### 한계

자이로센서는 **어느 방향을 보는지**만 알고 **어디로 걸어갔는지**는 모릅니다.
그래서 제자리에서 둘러보는 건 자연스럽지만, 앞으로 걸어가면 물고기가 따라옵니다.
바닥에 고정된 것처럼 걸어서 다가가려면 WebXR의 hit-test나 8th Wall 같은 상용 엔진이 필요합니다.

---

## 파일 구성

```
arTest/
├── index.html
├── Fish1.glb
├── Fish2.glb
└── Fish3.glb
```


## 3D 모델 출처

3D 모델은 **[Poly Pizza](https://poly.pizza)** 에서 받았습니다.

Poly Pizza는 Google Poly 서비스 종료 후 그 모델들을 보존하고 있는 무료 3D 모델 아카이브입니다.

| 파일 | 출처 |
|---|---|
| Fish1.glb | https://poly.pizza/m/BEcU9rjiAq |
| Fish2.glb | https://poly.pizza/m/3GPUntjwqCa |
| Fish3.glb | https://poly.pizza/m/JGFwp6xWgk |

---

## 라이선스

이 저장소의 **코드**는 MIT 라이선스입니다.
**3D 모델(`*.glb`)은 위 출처 표기를 따르며, 이 라이선스가 적용되지 않습니다.**
