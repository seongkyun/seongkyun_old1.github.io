---
layout: post
title: CH7. 큐 (Queue) 1
category: data_structure
tags: [data structure, 자료구조]
comments: true
---

# CH7. 큐 (Queue) 1
- 큐를 마지막으로 선형 자료구조가 끝나며, 8단원부터는 비선형 자료구조를 다룸
  - 따라서 챕터 7까지 학습 완료 후 선형자료구조에대한 전반적인 복습이 필요
- 선형자료구조와 다르게 비선형자료구조 파트는 조금 더 어려울 수 있기에 선형자료구조에대한 확실한 이해가 필요함

- 스택과 큐의 경우 이론은 어렵지 않으나 스택의 활용범위가 다양해서 계산기구현등의 까다로운 적용예시가 있었음
  - 큐는 그렇지 않음..

## 7-1 큐의 이해와 ADT 정의
- 큐는 FIFO(First-in, First-out) 구조의 자료구조로, 선입선출의 특성을 가짐
  - 일종의 줄서기와 동일
    - 스택은 프링글스처럼 하나의 입구로 넣고 빼는 방식
    - 큐는 종이컵분배기처럼 입구와 출구가 따로있어 먼저 넣은 데이터가 먼저 빠져나오게 됨
- 큐의 기본연산
  - 큐에서 데이터를 넣는 연산 -> __enqueue__
  - 큐에서 데이터를 빼는 연산 -> __dnqueue__
- 큐는 운영체제 관점에서 보면 프로세스나 쓰레드의 관리에 활용되는 자료구조
  - 이렇게 운영체제의 구현에도 자료구조가 사용이 되며, 운영체제의 학습 이전에 자료구조에 대한 이해가 선행되어야 함

### 큐의 ADT 정의
- 다음의 ADT를 이용하여 _배열 기반의 큐_ 또는 _연결 리스트 기반의 큐_ 를 구현 할 수 있음

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig1.png" alt="views">
<figcaption> </figcaption>
</figure>
</center>

## 7-2 큐의 배열 기반 구현
- 연결리스트/배열을 이용한 큐의 구현 중 배열기반의 큐 구현이 갖는 의미가 연결리스트보다 더 큼

### 큐의 구현 논리
- 입구가 있고 출구가 따로 존재하므로, 입구와 출구에 대한 코드 적용 시 별도의 방법을 고안해야 함
  - `F`, `R` 포인터 변수를 설정해서 `F`를 출구쪽, `R`을 입구쪽으로 설정

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig2.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>

### 가장 기본적인 배열 기반 큐의 문제점
- 문제점
  - 배열의 길이가 정해진 경우, R포인터를 더 오른쪽으로 옮길 수 없는 상황에서 문제가 발생
  - 먼저 들어온 데이터가 참조되어 빈 공간인데도 활용하지 못하는 상황이 발생
- 이러한 문제를 해결하기 위해 맨 마지막 배열을 맨 앞의 배열과 연결(인덱스 조절하여 설정이 가능)하는 __원형 큐__ 를 사용

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig3.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>

### 원형 큐의 소개
- 전제조건: R과 F는 배열의 끝에 오면 맨 앞으로 다시 가게 된다.

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig4.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>

### 원형 큐의 단순한 연산
- Enqueue: `R` 포인터가 이동 후 해당 자리에 새로 들어온 데이터를 저장
- Dequeue: `F` 포인터가 가리키던 자리의 데이터를 반환 후 이동

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig5.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>

### 원형 큐의 단순한 연산의 문제점
- `F`, `R`이 같은 위치를 가리키면 데이터가 하나 저장된 상태
- `R` 포인터 바로 다음이 `F` 포인터인 경우 원형 큐는 다 찬 상태거나 텅 빈 상태이거나 둘중 하나임
  - 즉, 원형 큐가 빈 상태인지, 다 차 있는 상태인지 구분 할 수 없음
- __따라서 F와 R의 관계를 다시 정의하고 구분하여 원형 큐가 다 차있는 상태인지 빈 상태인지를 구별해야 함__

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig6.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>

### 원형 큐의 문제점 해결
- 텅 비어있는 상태를 초기화 상태로 정의하고, 초기화 상태에서 `F`와 `R`은 같은 배열 index를 갖도록 설텅
- 초기화 된 상태, 즉 `F`와 `R`이 같은 공간을 가리키는 상태를 Empty 상태로 정의
  - 해당 상태에 해당 index에는 정보를 넣지 못함
    - 메모리 손실!
    - 하지만 일반적으로 배열의 길이가 매우 길어지므로 한 칸의 메모리 손실은 매우 적으며, 이로인한 코드상의 이점이 훨씬 크므로 의미있음!
- `R` 바로 다음자리가 `F`가 되는 경우 Full 상태로 정의

- __즉, 다음과 같은 상대적인 `F`와 `R`의 관계를 통해 Empty와 Full을 구분__
  - `R == F`: Empty
  - `R+1 = F`: Full
- 참고로 절대위치가 아닌 상대위치임!

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig7.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>

### 원형 큐의 구현: 헤더파일
- 포인터 변수인 `front`(F)와 `rear`(R)은 배열의 index를 나타냄
- F와 R의 위치를 알고 배열의 끝에 도달했을 때 다시 0번 인덱스로 보내주는 helper function 정의 필요

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig8.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>

### 원형 큐의 구현: Helper Function
- `front`, `rear` 값이 이동 시 가져야 할 올바른 index값을 반환하도록 정의
  - `front`나 `rear`의 값(배열의 index)이 맨 마지막일 경우 처음 값인 0을 반환하고
  - 아니면 다음 index를 반환하도록 정의

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig9.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>

### 원형 큐의 구현: 함수의 정의1
- 각각 초기화 함수와 비어있는지를 구분하는 구분하는 함수
- 초기화 값이 0이 아닌 다른값이어도 동작에는 전혀 문제가 없음!
  - 원형 꼴이므로

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig10.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>
  
### 원형 큐의 구현: 함수의 정의2
- Enqueue는 원형큐가 꽉 찬 경우를 제외하고 다음 인덱스값을 얻은다음 update된 인덱스 자리에 새로운 값을 저장함
- Dequeue는 원형큐가 텅 빈 경우를 제외하고 다음 인덱스값을 얻은다음 update된 인덱스 자리의 데이터를 반환
  - 둘 다 포인터 이동 후 en/de queue 연산 실행
  
<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig11.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>  

### 원형 큐의 실행

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-06-01-data_structure/fig12.PNG" alt="views">
<figcaption> </figcaption>
</figure>
</center>  
