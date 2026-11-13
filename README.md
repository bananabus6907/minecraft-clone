# 🎮 Minecraft Clone

Python Ursina 게임 엔진을 사용한 3D 복셀 기반 마인크래프트 클론 게임입니다.

## 프로젝트 개요

마인크래프트의 핵심 메커니즘을 Python으로 재현한 학습용 프로젝트입니다. 블록 배치/제거, 1인칭 시점 이동, 텍스처 시스템 등을 구현했습니다.

![Minecraft Clone](result.png)

### 주요 기능

- **블록 시스템**
  - 5가지 블록 타입 (잔디, 돌, 금, 용암)
  - 마우스 클릭으로 블록 배치/제거
  - 숫자 키로 블록 타입 선택

- **플레이어 제어**
  - 1인칭 시점 (First Person Controller)
  - WASD 이동
  - 마우스 시점 회전
  - 점프 기능

- **게임 환경**
  - 스카이박스 (하늘 텍스처)
  - 20x20 초기 지형
  - 실시간 3D 렌더링

- **효과음**
  - 블록 배치/제거 시 펀치 사운드

## 기술 스택

### 언어 및 엔진
- **Python 3**
- **Ursina Engine** - 3D 게임 엔진

### 주요 라이브러리
- `ursina` - 게임 엔진 코어
- `FirstPersonController` - 1인칭 컨트롤러

## 파일 구조

```
minecraft-clone/
├── main.py              # 메인 게임 로직 (78줄)
├── assets/              # 게임 리소스
│   ├── block (obj)      # 3D 블록 모델
│   ├── grass.png        # 잔디 텍스처
│   ├── stone.png        # 돌 텍스처
│   ├── gold.png         # 금 텍스처
│   ├── lava.png         # 용암 텍스처
│   ├── sky.jpg          # 하늘 텍스처
│   └── punch (audio)    # 펀치 사운드
├── result.png           # 스크린샷
└── README.md
```

## 설치 및 실행

### 필수 요구사항

```bash
# Python 3.6 이상
python --version

# Ursina 엔진 설치
pip install ursina
```

### 실행

```bash
python main.py
```

## 조작 방법

### 키보드
- **W, A, S, D**: 전후좌우 이동
- **Space**: 점프
- **1~5**: 블록 타입 선택
  - 1: 잔디
  - 2: 잔디 (중복)
  - 3: 돌
  - 4: 금
  - 5: 용암

### 마우스
- **마우스 이동**: 시점 회전
- **왼쪽 클릭**: 블록 배치
- **우클릭**: 블록 제거

## 핵심 코드 구조

### 블록 클래스

```python
class Voxel(Button):
    def __init__(self, position=(0,0,0), texture='grass.png'):
        super().__init__(
            parent=scene,
            position=position,
            model='assets/block',
            texture=texture,
            color=color.color(0, 0, random.uniform(0.9, 1.0)),
            scale=0.5
        )
    
    def input(self, key):
        if self.hovered:
            if key == 'left mouse down':
                # 클릭한 면에 새 블록 생성
                Voxel(position=self.position + mouse.normal, 
                      texture=blocks[block_id])
            elif key == 'right mouse down':
                # 블록 제거
                destroy(self)
```

### 초기 지형 생성

```python
# 20x20 평지 생성
for z in range(20):
    for x in range(20):
        voxel = Voxel(position=(x, 0, z))
```

### 손 UI

```python
hand = Entity(
    parent=camera.ui,
    model='assets/block',
    texture=blocks[block_id],
    scale=0.2,
    rotation=Vec3(-10, -10, 10),
    position=Vec2(0.6, -0.6)
)
```

## 기술적 특징

### 1. 복셀 (Voxel) 시스템
- 3D 공간의 블록 단위 조작
- 면(normal) 기반 블록 배치

### 2. 엔티티 시스템
- Ursina의 Entity 상속
- Button 컴포넌트로 클릭 감지

### 3. 동적 객체 생성/제거
- 런타임 블록 생성
- `destroy()` 함수로 메모리 관리

### 4. 3D 렌더링
- 스카이박스 구현
- 텍스처 매핑
- 실시간 조명 효과

## 학습 내용

### 1. 게임 개발 기초
- 게임 루프
- 입력 처리
- 엔티티 관리

### 2. 3D 그래픽스
- 3D 좌표계
- 텍스처 로딩
- 모델 렌더링

### 3. Python OOP
- 클래스 상속
- 메서드 오버라이딩
- 이벤트 핸들링

### 4. Ursina 엔진
- Entity 시스템
- FirstPersonController
- 리소스 관리

## 확장 아이디어

- [ ] 인벤토리 시스템
- [ ] 더 많은 블록 타입
- [ ] 저장/불러오기 기능
- [ ] 멀티플레이어
- [ ] 크리에이티브/서바이벌 모드
- [ ] 동물/몬스터 추가
- [ ] 낮/밤 사이클

## 프로젝트 의의

- **학습 목적**: 게임 개발의 기본 개념 이해
- **간결한 구현**: 80줄 미만의 코드로 핵심 기능 구현
- **확장 가능성**: 추가 기능을 쉽게 구현할 수 있는 구조

## 참고 자료

- [Ursina Engine 공식 문서](https://www.ursinaengine.org/)
- [Ursina Minecraft Tutorial](https://www.ursinaengine.org/minecraft_clone.html)

## 라이선스

Educational Project

## 크레딧

- **텍스처 및 사운드**: 외부 에셋 활용
- **엔진**: Ursina Engine
- **참고**: Ursina 공식 마인크래프트 튜토리얼

## 작성자

이승준
