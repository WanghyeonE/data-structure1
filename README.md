# What is data structure?
자료구조(data structure)는 프로그램에서 자료들을 정리하는 구조이다.  
이해를 쉽게 하기 위해서 자료구조의 종류들을 일상생활에서 볼 수 있는 것과 비교해보면 다음과 같은 예시들이 있다.

- List - 수첩에 해야할 일들을 기록  
순서를 가지고 일렬로 나열한 원소들의 모임이다. 순서가 있다는 점에서 집합과는 구별되며, 갈림길 없이 일렬로 나열되어 처음과 끝이 각각 하나씩만 있다는 점에서 그래프(graph)와도 구별된다. 리스트(list)의 종류에는 배열(array)과 연결리스트(linked list)가 있다.

- Stack - 바닥이 막힌 상자에 책을 쌓아 놓은 것  
데이터가 저장소의 끝 부분(Top 혹은 Top pointer)으로 들어오면서 저장되고, 데이터를 삭제 혹은 사용하기 위해 내보내는 데이터 역시 저장소의 끝 부분에서 나간다. 이러한 방식을 후입선출(Last In, First Out)이라고 한다.

- Queue - 물건을 구입하는 줄  
Queue라는 단어에서도 알 수 있듯이 데이터가 들어오는 위치는 가장 뒤(Rear)에 있고, 데이터가 나가는 위치는 가장 앞(Front)에 있다. Stack과는 다르게 먼저 들어오는 데이터가 먼저 나가게 되며 이러한 방식을 선입선출(First In, First Out)이라고 한다.

- Tree - 계층적인 조직(회사의 조직도, 컴퓨터의 폴더)  
부모 노드 밑에 여러 자식 노드가 연결되고, 자식 노드 각각에 다시 자식 노드가 연결되는 재귀적 형태이며 루트 노드를 중심으로 뻗어나가는 모습이 나무의 구조와 비슷하여 'tree'라는 이름이 붙었다.

- Graph - 지도(다대다매칭)  
그래프는 정점(vertex)과 정점들을 연결하는 변(edge)으로 구성이 된다. 변을 화살표로 나타내는 경우에는 해당 방향으로만 이동할 수 있으며, 이러한 그래프를 유향 그래프(directed graph)라고 말한다. 반대로 직선으로 표현되는 경우에는 양방향 모두 이동이 가능하며, 이러한 그래프를 무향 그래프(undirected graph)라고 말한다. 

# 1. linked list
연결리스트(linked list)는 리스트의 한 종류이며 배열과는 다른 특징을 가지고 있다.  

배열은 메모리 낭비를 피하기 위해 자료를 삭제하고 추가할 때마다 자료를 이동시켜야 한다는 불편함이 있지만 연결리스트는 자료를 삭제하거나 추가할 때 노드의 링크만 수정해주면 된다. 또한 메모리를 할당해놓고 자료를 저장하는 배열과는 다르게 동적할당을 통해 필요할 때 메모리를 할당받을 수 있으므로 자료의 삽입 삭제가 빈번한 경우 많이 사용한다.  

한 노드에는 데이터를 저장하는 부분(data)과 다음 데이터의 위치를 나타내는 부분(link)이 있으며 첫 번째 노드인 헤더(header)는 첫 번째 데이터가 어디 있는지 알려주며 마지막 노드의 링크(link)는 다음 데이터가 없으므로 마지막이라는 것을 알려주기 위해 NULL값을 세팅한다.  

    typedef struct node {   // typedef는 설정되어있는 자료형(data type)을 사용자가 알아보기 쉽도록 임의로 이름을 바꾸는 함수
	    void *dataPtr;      // 노드에 어떤 데이터가 저장될 지 알수 없으므로 자료형에 void를 사용
	    struct node* link;  // 노드의 link는 다음 노드의 주소를 저장하므로 재귀방법을 사용
    } NODE;                 // node라는 구조체(struct)를 NODE로 정의
노드를 정의하는 부분이다. node라는 구조체를 NODE타입으로 정의하여 뒤에 노드를 생성할 때 한눈에 쉽게 들어오도록 하였다. 노드 안에는 데이터를 저장하는 부분(dataPtr)과 다음 노드의 위치를 알려주는 부분(link)으로 구성되어 있다.

    NODE* createNode(void* itemPtr)             // void형의 itemPtr를 인수로 가지는 createNode라는 함수를 생성
    {
	    NODE* nodePtr = NULL;                   // NODE타입의 nodePtr노드를 NULL값으로 초기화
	    nodePtr = (NODE*)malloc(sizeof(NODE));  // nodePtr노드를 NODE타입의 크기만큼 메모리 할당
	    nodePtr->dataPtr = itemPtr;             // nodePtr노드의 dataPtr에 itemPtr값 저장
	    nodePtr->link = NULL;                   // nodePtr노드의 link는 NULL값으로 초기화
	    return nodePtr;                         // createNode함수의 반환값
    }  
노드를 생성하는 함수이다. NODE타입의 createNode라는 함수를 생성하였으며 인수로는 itemPtr를 가진다. 연결리스트의 장점을 살리기 위해 malloc함수를 이용해 동적할당을 해주었고 하나의 노드를 만들기 위해 할당받는 메모리의 크기는 sizeof함수를 써서 NODE타입의 크기만큼을 할당받는다. malloc함수를 쓰기 위해서 라이브러리(#include <malloc.h>)를 반드시 포함시켜주고 이렇게 함으로써 메모리 낭비를 최소화 할수 있다. 또한 노드를 생성하는 함수이므로 dataPtr에 itemPtr가 저장되고 다음 노드에 대한 정보는 없으므로 link는 NULL값으로 초기화된다. 함수의 반환값으로 노드인 nodePtr를 반환한다.  

    int main(void)
    {
	    int* newData;
	    int* nodeData;
	    NODE* node;

	    newData = (int*)malloc(sizeof(int));   // 메모리 할당
	    *newData = 7;                          // newData에 데이터(7) 저장
	    node = createNode(newData);            // 1번 노드를 생성하고 노드에 데이터 저장
	    newData = (int*)malloc(sizeof(int));   // 메모리 할당
	    *newData = 75;                         // newData에 데이터(75) 저장
	    node->link = createNode(newData);      // 1번 노드의 링크에 2번 노드 연결
	    nodeData = (int*)node->dataPtr;        // nodeData에 1번 노드에 저장되어 있는 데이터 저장
	    printf("data1: %d\n", *nodeData);      // 1번 노드의 데이터 출력
	    nodeData = (int*)node->link->dataPtr;  // nodeData에 2번 노드에 저장되어 있는 데이터 저장
	    printf("data2: %d\n", *nodeData);      // 2번 노드의 데이터 출력
	    return 0;
    }  
노드를 2개 생성하여 각각의 노드에 7과 75를 저장하고 두 노드를 연결하는 메인문이다.  
우선 malloc함수를 이용한 동적할당을 통해서 newData의 메모리를 할당한다. newData에 int형의 데이터를 저장할 것이기 때문에 할당받을 메모리의 크기는 int형의 크기만큼만 할당받아서 메모리 낭비를 최소화한다. newData에 7이라는 값을 저장하고 createNode라는 함수를 호출한다. 함수를 정의할 때 노드를 생성함과 동시에 함수를 호출하면서 사용한 인수를 바로 노드에 저장하므로 1번 노드에는 7이라는 값이 저장된다. 같은 방법으로 다시 메모리를 할당하고 newData에 75라는 값을 저장한다. 이번에는 2번 노드를 생성하면서 바로 1번 노드의 링크(node->link)에 2번 노드의 주소가 저장되고 2번 노드에는 75라는 값이 저장된다. nodeData에는 1번 노드에 저장되어 있는 int형의 데이터((int*)node->dataPtr)가 저장되며 그 값을 출력한다. 그 다음 nodeData에는 1번 노드의 링크에 저장되어 있는 주소의 데이터 즉, 2번 노드의 데이터(node->link->dataPtr)가 저장되며 그 값을 출력한다.

# 2. factorial
재귀함수의 간단한 예로 factorial을 구현하였다.  

어떤 사건이 자기 자신을 포함하고 다시 자기 자신을 사용하여 정의될 때 재귀적(recursive)이라고 한다. 재귀함수는 반복문과 비교할 수 있는데 반복문으로 변수를 바꿔가는 형식의 명령형 계산은 항상 재귀적인 형식으로 똑같이 구현할 수 있으며 그 반대도 마찬가지지만 함수가 호출될 때마다 호출 스택 메모리를 잡아먹는 경우 퍼포먼스 측면에서 반복문을 사용하는 것이 더 좋다. 이와 같은 예로는 피보나치 수열이 있다. 피보나치 수열은 순환하고 있지만 순환기법을 사용하면 함수 호출 횟수가 비약적으로 증가하기 때문에 실행하는데 매우 오래 걸린다.  

반대로 반복문을 사용하여 구현했을 때 코드가 심하게 복잡해지거나, 로직이 꼬이는 경우 재귀함수로 구현하는 것이 간단하고 훨씬 더 이해하기 쉬운 경우가 있다. 즉 사용자가 문제를 파악하고 함수 호출 횟수를 많이 줄여 빠르게 실행시킬 수 있다고 판단했을 때 재귀함수를 사용하는 것이 매우 효과적이며 그렇지 않다면 일반 반복문을 사용하는 것이 더 좋다.  

	int fact(int n) {
		if (n <= 1)
			return 1;                // 0! 또는 1! = 1
		else
			return n * fact(n - 1);  // 함수의 반환값에 자신이 포함되어 있는 함수 (재귀함수)
	}  
재귀적 방법을 사용하여 factorial을 구현한 함수이다. 하나의 정수를 인수로 가지며 정수가 1이하일 때 1을 반환하고 그렇지 않으면 자신을 다시 호출하여 같은 과정을 반복한다.  

	void main() {
		system("color F0");                                // 출력창의 배경을 흰색으로, 글자를 검은색으로 설정
		int i;
		for (i = 1; i < 6; i++) {                          // 1!부터 5!까지의 모든 수를 출력하기 위한 반복문
			if(i<5)
				printf("%d!=%d\n", i, fact(i));    // 1!부터 4!까지의 값 출력(1!=1, 2!=2, 3!=6, 4!=24)
			else printf("fact(%d)=%d\n", i, fact(i));  // 5!의 값 출력(fact(5)=120)
		}
	}  
factorial의 값을 출력하는 함수의 메인문이다. system("color F0"); 이 부분은 출력 커멘드 창의 배경을 흰색, 글자를 검은색으로 설정하기 위해 추가한 부분으로 사용하기 위해서는 라이브러리(#include <Windows.h>)를 추가해야 한다. i를 1부터 5까지 1씩 증가시키면서 반복문을 실행시키고 i가 5보다 작은 수 일 때는 (1!=1, 2!=2, 3!=6, 4!=24) 이와 같이, i가 5일 때는 (fact(5)=120) 이와 같이 출력을 한다. 이 코드는 5!까지의 값만을 출력하는데 다른 수의 출력을 보고 싶다면 정수형 데이터 a를 scanf함수로 사용자에게 입력받은 뒤 반복문 (i = 1; i < 6; i++) 이 부분을 (i = 1; i < a+1; i++) 이렇게 바꾸고 if(i<5) 이 부분을 if(i<a) 로 바꾸면 사용자가 원하는 정수까지의 factorial값을 출력할 것이다.
