---
layout: post
title:  "다익스트라 알고리즘 기초"
date:   2018-01-05 17:00:00 +0800
categories: IT
tags: Algorithm Dijkstra
comments: 1
---

그래프상에서 단일 출발점에서 모든 지점까지의 최단 거리를 구하는 알고리즘이다. 최단거리 알고리즘인 shortest path problem중의 하나이다. 

<br>
> ##### 응용범위

응용 범위가 넓은 알고리즘 중의 하나로써, 인터넷상에서의 라우팅이나 대중교통을 이용할 경우 환승해서 목적지까지 가는 경로를 탐색할 때 자주 사용된다.


<br>
> ##### 응용가능한 그래프

다익스트라 알고리즘은 음수의 가중치를 가지지 않는 그래프에서 활용된다. 


<br>
> ##### 알고리즘 상세

1. 거리 값(Distance value)을 설정.
시작 노드의 거리 값은 0으로 , 나머지 노드의 거리 값은 무한대로 초기화 한다.

2. 방문 상태(visit state)를 설정.
시작 노드를 current(현재)상태로, 나머지 노드는 Unvisited(미방문)상태로 초기화 한다. 

3. 현재 노드에서 미방문(univisited) 상태의 이웃 노드들에 대해 잠정적인 거리를 계산한다. 예를 들어 현재 노드 A의 거리 값이 6이고, A와 연결된 이웃 노드 B까지의 거리가 2라면, B의 잠정적인 거리는 6 + 2 = 8 이 된다.
   
   3-1. 만약 이 값(8)이 이미 기록되어 있는 잠정적인 거리 값보다 작으면 B의 거리 값을
8로 갱신한다. 

   3-2. 하지만 잠정적인 거리 값 보다 크면 갱신하지 않고, 같을 경우에는 갱신은 자유이다.

1. 3번과 같은 방법으로 인접하고 있는 모든 이웃노드에 대해서 거리 값을 갱신 하였으면, 가장 작은 거리 값을 가지는 노드를 현재 노드로 갱신하고, 방문(visited)상태로 전환한다. 
   
2. 3번으로 다시 돌아가서 모든 미방문(unvisited) 노드가 방문(visited)상태로 전환될 때까지 3-4을 반복한다. 
   
3. 미방문 상태의 노드가 모두 방문상태로 갱신되면 알고리즘 종료.

    ![다익스트람 이미지 없음](/assets/res/2018-1-5-Dijkstra-Algorithm1.png)


<br>
> ##### 코드(C언어)

```c
#include<stdio.h>


#define num 6
#define max 99999


void file_read();
void init();
void algo();


FILE *in;
int dis[num][num] = {0, };
int sht[num], state[num], pre[num];
int start, arrive;


void algo(){
    int i, j, min, tmp, k;

    for(k=start, i=start; k<start+num; k++){ 
        //k는 단순히 정점의수(6)만큼 반복문을 실행하기 위한 변수.
        min = max; 
        
        //min은 다음 턴에 선택할 노드의 잠정적인 거리값중 최소값을 저장할 변수.
        for(j=0; j<num; j++){
            if(i != j){
                if(state[j] == 0 && dis[i][j] < max){
                    //미방문 상태이고 i와 인접하고 있는 노드 선택.
                    if(sht[j] > sht[i] + dis[i][j]){
                        // 알고리즘 분석의 3-1번과정 
                        sht[j] = sht[i] + dis[i][j];
                        pre[j] = i;  

                        //노드 j의 거리 값과 pre값을 갱신
                        if(sht[j] < min){
                            tmp = j; 
                            //tmp는 최소값을 가지는 노드의 번호를 임시 저장하는 변수.
                            min = sht[j];
                        }
                    }else{ // 3-2번 과정  
                        if(sht[j] < min){
                            tmp = j;
                            min = sht[j];
                        }
                    }
                }
            }else{ // 자기 자신과의 비교는 패스.
                continue;
            }
        }

        state[tmp] = 1; //최소값을 가지는 노드를 방문상태로 바꾼다.
        i = tmp; // 최종적으로 최소값을 가지는 노드 번호를 i에 저장한다.  
    }
}

void init(){
    int i, j;
    for(i=0; i<num; i++){
        for(j=0; j<num; j++){
            fscanf(in, "%d", &dis[i][j]);
            if(dis[i][j] == -1) dis[i][j] = max;
        }
    }

    printf("출발점과 도착점을 입력하십시오 : ");
    scanf("%d %d", &start, &arrive);

    for(i=0; i<num; i++){
        if(i == start){
            sht[i] = 0;
            state[i] = 1;
            pre[i] = 0;
        }else{
            sht[i] = max;
            state[i] = 0;
            pre[i] = max;
        }
    }
}

void file_read(){
    int i, j; 
    in = fopen("dij_input.txt", "r");


    if(in == 0){
        perror("File read error\n");
    }
}

int main(void){
    int i;

    file_read();
    init();
    algo();

    for(i=0; i<num; i++){
        printf("i = %d, sht= %d,  pre = %d\n", i, sht[i], pre[i]); 
    }

    printf("노드%d부터 노드%d까지의 최단 거리는 : %d\n", start, arrive, sht[arrive]);
    return 0;
}
```
