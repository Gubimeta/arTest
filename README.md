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
├── check-size.html
├── Fish1.glb
├── Fish2.glb
└── Fish3.glb
```

---

## 크기 설정
 
GLB는 제작 도구마다 단위가 달라서, 같은 배율을 주면 어떤 모델은 티끌만 하고 어떤 모델은 방을 가득 채웁니다.
이 프로젝트는 세 파일을 미리 분석해 **실제 길이(미터)로 지정**하도록 만들어 두었습니다.
 
`index.html` 상단의 설정 블록만 고치면 됩니다.
 
```js
var MODELS = [
  { src:'#fish1', len:0.40, raw: 7.87713, center:[0,  0.31180, -0.81436], animated:true  },
  { src:'#fish2', len:0.30, raw:95.56850, center:[0,  0.00000,  0.00000], animated:false },
  { src:'#fish3', len:0.55, raw:10.19428, center:[0, -0.50150, -1.56104], animated:true  }
];
```
 
- `len` — 화면에 나올 실제 길이 (미터). **여기만 바꾸면 됩니다.**
- `raw` / `center` — GLB 원본의 실측값. 모델을 교체하지 않는 한 그대로 두세요.
### 모델 실측 데이터
 
| | 원본 가장 긴 변 | Fish1 대비 | 내장 애니메이션 | 제작 도구 |
|---|---|---|---|---|
| Fish1 | 7.877 | 1배 | `Armature\|Swim` | FBX2glTF |
| Fish2 | 95.569 | **12.1배** | 없음 (정적 메시) | obj2gltf |
| Fish3 | 10.194 | 1.29배 | `Armature\|Swim` | FBX2glTF |
 
세 모델 모두 몸통이 Z축을 따라 눕고 머리가 **+Z 방향**입니다.
Fish2는 뼈대가 없어 꼬리를 흔들지 못하므로, 코드에서 몸 전체를 좌우로 살짝 흔들어(`sway`) 보완했습니다.
 
다른 GLB로 교체할 때는 `check-size.html` 을 열면 새 모델의 `raw` 와 `center` 값을 알려줍니다.

---

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
