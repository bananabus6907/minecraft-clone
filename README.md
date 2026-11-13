# 🎮 Minecraft Clone (Ursina 엔진)

> [kairess/minecraft-clone](https://github.com/kairess/minecraft-clone)을 포크해 따라 만들며 Python 3D 게임 엔진 **Ursina**를 익힌 학습용 저장소입니다.
> 게임 코드와 에셋은 원본 튜토리얼 것이며, 이 저장소에서는 원본의 초기 버전 코드를 유지하고 문서만 추가했습니다.

![Minecraft Clone](result.png)

## 무엇을 하는 프로그램인가

| 기능 | 구현 방식 |
|---|---|
| 1인칭 이동 | Ursina 내장 `FirstPersonController` — WASD 이동, 마우스 시점, Space 점프 |
| 블록 놓기 / 부수기 | 블록(`Voxel`)이 `Button`을 상속해 마우스가 올라간 블록을 감지. 왼쪽 클릭 시 `mouse.normal`(클릭한 면의 방향)만큼 옆에 새 블록 생성, 오른쪽 클릭 시 `destroy()` |
| 블록 종류 선택 | 숫자 키로 텍스처 선택, 화면 오른쪽 아래 손(블록)의 텍스처도 함께 변경 |
| 지형 | 시작 시 20 × 20 평지 생성, 블록마다 밝기를 0.9~1.0 사이로 무작위 지정해 단조로움 완화 |
| 하늘 | 안쪽 면까지 그린(`double_sided`) 큰 구에 하늘 텍스처 |
| 효과음 | 마우스 버튼을 누르고 있는 동안 펀치 소리 + 손이 앞으로 움직이는 애니메이션 |

## 조작

| 입력 | 동작 |
|---|---|
| W / A / S / D | 이동 |
| Space | 점프 |
| 마우스 | 시점 회전 |
| 왼쪽 클릭 | 블록 놓기 |
| 오른쪽 클릭 | 블록 부수기 |
| 0, 1 | 잔디 |
| 2 | 돌 |
| 3 | 금 |
| 4 (5~9 포함) | 용암 (목록 범위를 넘는 번호는 마지막 블록으로 고정) |

블록 텍스처는 잔디·돌·금·용암 4종이며, 목록의 0번과 1번이 모두 잔디입니다.

## 실행

```bash
pip install ursina
git clone https://github.com/bananabus6907/minecraft-clone.git
cd minecraft-clone
python main.py
```

## 파일 구조

```
minecraft-clone/
├── main.py                 # 게임 전체 코드 (약 80줄)
├── assets/
│   ├── block.obj / .mtl    # 블록 3D 모델
│   ├── block_texture.psd   # 텍스처 원본
│   ├── grass.png · stone.png · gold.png · lava.png
│   ├── sky.jpg
│   └── punch.wav
└── result.png              # 실행 화면
```

## 핵심 코드

```python
class Voxel(Button):
    def __init__(self, position=(0, 0, 0), texture='assets/grass.png'):
        super().__init__(
            parent=scene, position=position, model='assets/block',
            origin_y=0.5, texture=texture,
            color=color.color(0, 0, random.uniform(0.9, 1.0)),  # 블록마다 밝기 차이
            scale=0.5)

    def input(self, key):
        if self.hovered:
            if key == 'left mouse down':
                Voxel(position=self.position + mouse.normal, texture=blocks[block_id])
            elif key == 'right mouse down':
                destroy(self)
```

## 따라 하며 익힌 것
- Ursina의 `Entity` 구조와 `update()` / `input()` 콜백으로 돌아가는 게임 루프
- 마우스가 가리키는 면의 법선 벡터(`mouse.normal`)로 블록을 붙일 위치를 정하는 방법
- `camera.ui`에 붙인 엔티티로 화면 고정 UI(손) 만들기, 텍스처·모델·사운드 로딩

## 크레딧
- 원본 코드·에셋: [kairess/minecraft-clone](https://github.com/kairess/minecraft-clone)
- 엔진: [Ursina Engine](https://www.ursinaengine.org/)
