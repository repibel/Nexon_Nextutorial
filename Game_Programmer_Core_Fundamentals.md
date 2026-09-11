# 🎮 게임 프로그래머 핵심 기본기 종합 정리 노트

이 문서는 게임 프로그래머가 갖춰야 할 필수 기본기(**자료구조, 알고리즘, 게임 수학 & 선형대수학, 렌더링 파이프라인**)를 한눈에 파악하고 복습할 수 있도록 요약·정리한 서머리 노트입니다.

---

## 📌 목차
1. [게임 프로그래머의 5대 기본기 개요](#1-게임-프로그래머의-5대-기본기-개요)
2. [자료구조 (Data Structures)](#2-자료구조-data-structures)
3. [핵심 알고리즘 (Algorithms)](#3-핵심-알고리즘-algorithms)
4. [게임 수학 및 선형대수학 (Game Math & Linear Algebra)](#4-게임-수학-및-선형대수학-game-math--linear-algebra)
5. [3D 좌표계 변환 & 렌더링 파이프라인](#5-3d-좌표계-변환--렌더링-파이프라인)
6. [핵심 요약 암기 카드 (Quick Review)](#6-핵심-요약-암기-카드-quick-review)

etc. https://share.gemini.google/KqC9FQqNsRK7 / gemini상에서 확인 가능한 세부 정보
---

## 1. 게임 프로그래머의 5대 기본기 개요

| 영역 | 핵심 키워드 | 주요 목표 |
| :--- | :--- | :--- |
| **언어 & 메모리** | C++, C#, 포인터, GC, RAII | 메모리 효율성 및 실행 성능 극대화 |
| **자료구조 & 알고리즘** | Vector, Hash, A*, Big-O, CPU Cache | 시간/공간 복잡도 최적화, 최단 경로 및 탐색 |
| **게임 수학 & 선형대수학**| 벡터, 행렬, 내적, 외적, 쿼터니언 | 3D 공간 내의 이동, 회전, 변환 및 시야 판정 |
| **컴퓨터 구조 & OS** | CPU Cache, Multi-threading, Race Condition | 프레임 저하 없는 멀티코어 최적화 및 동기화 |
| **그래픽스 & 엔진** | 렌더링 파이프라인, Shader, Draw Call | 3D 장면의 2D 화면 출력 및 그래픽 최적화 |

---

## 2. 자료구조 (Data Structures)

### 2.1 주요 자료구조 비교

| 자료구조 | 메모리 구조 | 접근 시간 | 삽입/삭제 | 게임 내 주요 활용 사례 |
| :--- | :--- | :---: | :---: | :--- |
| **배열 / 동적 배열**<br/>(Array / Vector) | 연속된 메모리 | $O(1)$ | $O(N)$ | 인벤토리 슬롯, 프레임 순회 객체 목록, CPU 캐시 적중률 우수 |
| **연결 리스트**<br/>(Linked List) | 노드 간 포인터 연결 | $O(N)$ | $O(1)$ | 수시로 추가/삭제되는 이펙트/버프 목록 |
| **해시 테이블**<br/>(Hash Table / Map) | Key-Value 매핑 | $O(1)$ | $O(1)$ | 플레이어 ID 조회, 아이템 데이터베이스 검색 |
| **트리**<br/>(Tree) | 부모-자식 계층 구조 | $O(\log N)$ | $O(\log N)$ | 스킬 트리, AI 판단 구조(Behavior Tree), 씬 그래프 |
| **공간 분할 트리**<br/>(Quad/Oct-tree) | 공간 영역 4/8등분 | $O(\log N)$ | - | 2D/3D 대규모 충돌 검사 최적화, 시야 절두체 컬링 |

### 2.2 핵심 중요 포인트
* **Big-O 표기법**: 데이터량($N$) 증가에 따른 연산 속도 변화 측정 ($O(1) < O(\log N) < O(N) < O(N \log N) < O(N^2)$).
* **CPU 캐시 우호성 (Cache Friendliness)**: 이론상 연결 리스트의 삽입/삭제가 빠르더라도, **연속된 메모리 배열(Vector)**이 CPU 캐시 미스(Cache Miss)를 줄여 실무에서는 훨씬 뛰어난 성능을 보임.

---

## 3. 핵심 알고리즘 (Algorithms)

### 3.1 5대 필수 알고리즘

1. **정렬 (Sorting)**
   * **버블 정렬 ($O(N^2)$)**: 인접 원소 비교.
   * **퀵 정렬 / 병합 정렬 ($O(N \log N)$)**: 기준점(Pivot) 중심 분할 정구.
   * *활용*: UI Depth 정렬, Z-Sorting, 랭킹 시스템.
2. **탐색 (Searching)**
   * **선형 탐색 ($O(N)$)**: 순차 비교.
   * **이진 탐색 ($O(\log N)$)**: 정렬된 데이터에서 중앙값을 기준으로 절반씩 제거하며 탐색.
3. **그래프 탐색 (BFS / DFS)**
   * **DFS (깊이 우선 탐색)**: 스택/재귀 활용. 한 방향으로 끝까지 탐색.
   * **BFS (너비 우선 탐색)**: 큐 활용. 인접한 노드부터 넓게 탐색 (최단 거리 탐색에 유리).
4. **길 찾기 (Pathfinding)**
   * **A* (A-Star)**: 출발지 경로 비용($G$) + 목적지 예상 거리 휴리스틱($H$) = $F$ 값이 가장 낮은 노드를 우선 탐색하는 게임 길찾기 표준 알고리즘.
5. **충돌 감지 및 상태 제어**
   * **SAT (분리축 정리)**: 회전된 다각형(OBB) 충돌 검사.
   * **FSM (유한 상태 머신)**: 대기, 이동, 공격 등 상태 전환 제어.

---

## 4. 게임 수학 및 선형대수학 (Game Math & Linear Algebra)

### 4.1 벡터 (Vector)

* **위치 벡터 vs 방향 벡터**: 위치는 공간상의 점, 방향은 크기가 1인 단위 벡터(Normalized Vector) 사용.
* **벡터의 내적 (Dot Product, $\mathbf{a} \cdot \mathbf{b} = |\mathbf{a}||\mathbf{b}|\cos	heta$)**
  * 양수: 전방 / $0$: 직각 / 음수: 후방
  * *활용*: **몬스터 시야각 판정**, 조명/광원 연산(Lighting).
* **벡터의 외적 (Cross Product, $\mathbf{a} 	imes \mathbf{b}$)**
  * 두 벡터에 동시에 수직인 제3의 벡터 생성.
  * *활용*: **3D 법선 벡터(Normal Vector)** 계산, 경사면 몸체 기울기 계산.

### 4.2 행렬 (Matrix)

* **공간 변환의 도구**: 크기(Scale), 회전(Rotation), 이동(Translation) 변환 수행.
* **행렬 결합**: $T 	imes R 	imes S$ 형태로 곱해 하나의 최종 변환 행렬로 단축.
* **부모-자식 관계**: $	ext{자식 월드 위치} = 	ext{부모 월드 행렬} 	imes 	ext{자식 로컬 행렬}$.

---

## 5. 3D 좌표계 변환 & 렌더링 파이프라인

### 5.1 3대 좌표계 변환 행렬

```
[로컬 공간] (Local Space)
   │  
   ▼  월드 행렬 (World Matrix: 위치, 회전, 크기 배치)
[월드 공간] (World Space)
   │  
   ▼  뷰 행렬 (View Matrix: 카메라 시점 기준 재배치)
[뷰 공간] (View Space / Camera Space)
   │  
   ▼  투영 행렬 (Projection Matrix: 원근감 적용 및 NDC 압축)
[투영 공간] (Projection Space)
```

### 5.2 GPU 렌더링 파이프라인 4단계

1. **정점 셰이더 (Vertex Shader)**: 정점에 **월드 × 뷰 × 투영 행렬**을 곱해 2D 위치 계산.
2. **라스터라이저 (Rasterizer)**: 3D 도형을 화면 단위인 **2D 픽셀(Fragment)**로 분할 및 시야 외 잘라내기(Clipping).
3. **픽셀 셰이더 (Pixel Shader)**: 텍스처 입히기 및 조명 연산을 통한 **최종 색상(RGB)** 결정.
4. **출력 병합 (Output Merger)**: 깊이 버퍼(Z-Buffer) 검사로 가려진 픽셀 버림 및 최종 화면(Frame Buffer) 출력.

---

## 6. 핵심 요약 암기 카드 (Quick Review)

* 💡 **Vector vs Linked List**: 순회 연산이 많으면 캐시 효율이 좋은 `Vector`가 유리!
* 💡 **A* 알고리즘 핵심 공식**: $F = G + H$ ($G$: 시작점~현재 거리, $H$: 현재~목적지 예상 거리)
* 💡 **벡터 내적 활용**: $\mathbf{a} \cdot \mathbf{b} > 0$ 이면 전방 시야 내부!
* 💡 **행렬 연산 순서**: SRT (Scale $
ightarrow$ Rotation $
ightarrow$ Translation)
* 💡 **렌더링 3단계 행렬**: Local $
ightarrow$ **World** $
ightarrow$ **View** $
ightarrow$ **Projection**
* 💡 **Z-Buffer**: 카메라 기준 물체 깊이값을 비교하여 뒤에 가려진 픽셀을 버리는 기술!
