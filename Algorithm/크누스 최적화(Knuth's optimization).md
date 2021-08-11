# 크누스 최적화(Knuth's optimization)

* 특수한 DP 점화식에 대해 적용할 수 있는 최적화.

## 조건

* 어떤 2차원 배열 C
* DP를 사용하는 2차원 배열 dp

![img](https://t1.daumcdn.net/cfile/tistory/999039465A7D17441F)

* C는 i부터 j까지 합치는데 드는 비용.
* 여기서 비용은 이전 비용에 현재 비용을 누적해서 계산하는 문제이다.



* quadrangle inequeality : 사각부등식으로 (a,b,c) + (b,c,d) <= (a,b,c,d) + (b,c) 가 성립하고
* monotonicity는 단조성으로 (b,c) <= (a,b,c,d) 가 성립하는 배열을 뜻한다.



* 따라서 최적화 기법을 적용할 수 있다.

![img](https://t1.daumcdn.net/cfile/tistory/9943404B5A7D18EC0E)

* dp[[i]][[[][j] 는 i부터 j까지 파일을 합치는데 드는 비용의 최솟값이고 그 최솟값을 만들어주는 k(배열의길이)의 범위는 위의 범위를 만족한다는 것이다. 

## 파일 합치기 문제

![img](https://t1.daumcdn.net/cfile/tistory/996425365A7D359C08)

* i에서 k까지 합치는 경우의 수 중 최솟값 + K+1에서 j까지의 최솟값의 합이 최소가 되어야하고 그 값에 i,j를 합치는 총 비용을 지불해야 k와 k+1이 이어지면서dp[[i]][[[][j] 의 최솟값을 구할 수 있다.

