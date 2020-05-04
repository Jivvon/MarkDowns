### breakpoint

- b [line number]
- info b
- dis b 1 : disable
- del 2 : delete
- del : 모든 중단점 삭제
- b 9 if y == 13 : 9번째 줄에 y == 13일 때 breakpoint 설정
- cond 8 i == 456 : 8번 breakpoint에 조건 i == 456을 걸어준다
- ignore 8 455 : 8번 breakpoint는 455번 동안 무시된다
- until : 반복문 안에서 breakpoint가 걸렸는데 한번에 빠져나오기
- finish : 함수를 실행하고 return 값을 보여준다

### etc

- print x : 변수 x의 값 확인 ($2 = 12)
- display x /undisplay : 변수 x의 값을 매 단계마다 보여줌
- continue



[참고](https://kldp.org/node/71806)

