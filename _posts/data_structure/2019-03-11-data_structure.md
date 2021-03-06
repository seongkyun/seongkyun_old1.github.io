---
layout: post
title: CH4. 연결 리스트 (Linked list) 2-1
category: data_structure
tags: [data structure, 자료구조]
comments: true
---

# CH4. 연결 리스트 (Linked list) 2-1
- 제목은2이지만, 실제로는 첫 번째로 다루는 연결 리스트의 이야기가 됨

## 4-1 연결 리스트의 개념적인 이해
- 연결 리스트 공부를 할 때 그림 그려가며 이해하는것이 중요!
- 이해에 선행조건? -> 동적할당에 대한 이해가 되어야 함
  - malloc, free 함수가 호출되는 이유를 알고 있어야 함
- 배열의 가장 큰 장점? -> __데이터의 순차 접근이 가능한 자료구조형태임__
  - 선언만 하면 index 값을 이용한 순차 접근이 가능해짐
- 배열의 단점 -> 기본적으로 크기(길이)가 결정되어야 함
  - 필요할 때마다 만든다 = __동적할당__
- 배열은 순차접근이 가능하나, malloc으로 동적할당만 해놓으면 순차접근이 불가하고 관리가 안됨!
  - __연결 리스트는 방향성을 두고 malloc으로 동적할당 된 리스트를 연결하는 작업!__
- 연결리스트를 이용해 malloc으로 동적할당 된 메모리를 연결해 리스트로 관리가 가능
- __새로운 리스트가 메모리에 동적할당 되더라도, 이를 머리나 꼬리 부분에 새로 붙여서 늘릴 수 있게 하는것이 연결리스트__
- 경우에는 Head나 Tail에 대한 정보만 갖고 있을 수 있음
  - 필요에 따라 모두 갖고 있을 수도, Head만 갖고 있을 수 있음

### Linked가 무엇을 연결하겠다는 뜻인가?
- 연결 방법? -> node에 대한 구조체를 정의해야 함
  - 노드 자체가 구조체 변수가 되어야 함
  - 노드가 해야 할 일
    - 1. 노드에 데이터를 담을 수 있어야 함
    - 2. 연결이 가능해야 함
  - `struct _node * next;`에서 \*next 는 구조체 \_node 를 가리키는 구조체 포인터 변수

```c
typedef struct _node
{
	int data; // 1번 역할
	struct _node * next; // 2번 역할
} Node;
```

- main 함수에 node에 해당하는 변수 선언
  - `Node n1, n2;`
  - n1과 n2는 각각 새로 생성된 노드를 의미
    - n1.data는 데이터를, n1.next는 누군가를 가리키기 위한 변수공간 (n2도 동일)
  - `n1.next = &n2;` 로 n2의 주솟값을 n1.next에 저장(연결)
    - __이를 이용해 n1의 포인터가 n2를 가리키게 됨
  - `*(n1.next)`로 n2를 가리킬 수 있음(순차적 접근)
- 이를 이용해 
  - 1. 연결을 어떻게 하는지
  - 2. 순차적 접근에 대한 것을 배울 수 있음

- "LinkedRead.c"의 분석: 초기화
  - head: 연결 리스트의 머리 부분을 가리키기 위한 포인터 변수
  - tail: 연결 리스트의 꼬리 부분을 가리키기 위한 포인터 변수
  - cur: 순차적 접근에 사용되는 포인터 변수(순차적 조회 시에만 필요)

```c
// NULL 포인터 초기화
Node * head = NULL; // 첫 번째 노드를 가리킴(필수요소)
Node * tail = NULL; // 내가 어디까지 갔는지(순차접근)에 대한 정보를 담음
Node * cur = NULL; // 마지막 노드를 가리키지만 없을 수 있음(필수요소가 아님)

Node * newNode = NULL;
int readData;
```

- "LinkedRead.c"의 분석: 삽입

```c
/**** 데이터를 입력 받는 과정 ****/
while(1)
{
	printf("자연수 입력: ");
	scanf("%d", &readData); // 데이터를 입력하기위한 값을 입력받음
	if(readData < 1)  // 1보다 작은 경우 더 이상 노드를 추가하지 않고 빠져나감
		break;

	/*** 노드의 추가과정(연결과정) ***/
	newNode = (Node*)malloc(sizeof(Node));
	newNode->data = readData;
	newNode->next = NULL;

	if(head == NULL)
		head = newNode;
	else
		tail->next = newNode; // tail이 가리키는 노드 멤버의 next
	tail = newNode;
}
```

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-03-11-data_structure/fig1.jpg" alt="views">
<img src="/assets/post_img/data_structure/2019-03-11-data_structure/fig2.jpg" alt="views">
<figcaption> </figcaption>
</figure>
</center>

- "LinkedRead.c"의 분석: 데이터 조회

```c
if(head == NULL) 
{
  printf("저장된 자연수가 존재하지 않습니다. \n");
}
else 
{
  cur = head; 
  printf("%d  ", cur->data);   // 첫 번째 데이터 출력

  while(cur->next != NULL)    // 두 번째 이후의 데이터 출력
  {
    cur = cur->next;
    printf("%d  ", cur->data);
  }
}
```

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-03-11-data_structure/fig3.jpg" alt="views">
<figcaption> </figcaption>
</figure>
</center>

- "LinkedRead.c"의 분석: 데이터 삭제

```c
if(head == NULL) 
{
  return 0;    // 해제할 노드가 존재하지 않는다.
}
else 
{
  Node * delNode = head;
  Node * delNextNode = head->next;

  printf("%d을(를) 삭제합니다. \n", head->data);
  free(delNode);    // 첫 번째 노드의 삭제

  while(delNextNode != NULL)    // 두 번째 이후의 노드 삭제 위한 반복문
  {
    delNode = delNextNode;
    delNextNode = delNextNode->next;

    printf("%d을(를) 삭제합니다. \n", delNode->data);
    free(delNode);    // 두 번째 이후의 노드 삭제
  }
}
```

<center>
<figure>
<img src="/assets/post_img/data_structure/2019-03-11-data_structure/fig4.jpg" alt="views">
<figcaption> </figcaption>
</figure>
</center>
