# switch jump table

## 서론

과연 switch 에서 jmp tblae로 case를 건너 뛰는 조건이 무엇인지?에 대한 고민을 가지고 간단하게나마 분석해봤다.

switch 분기 5 이하, jump table X, 최적화 없음.

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled.png)

switch 분기 6개 이상, jump table, 최적화 O2

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%201.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%201.png)

switch 분기 6개 이상,  0~5 순서대로, jump table O, 최적화 없음.

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%202.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%202.png)

switch 분기 6개 이상, 순서대로 X,  jump table O, 최적화 없음.

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%203.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%203.png)

switch 분기 6개 이상, 순서대로 O, 3 다음에 55 순서,  jump table O, 최적화 없음.

분기 차이 벌려봄 55도 그냥 점프함.

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%204.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%204.png)

switch 분기 6개 이상, 순서대로 X,  jump table O, 최적화 없음.

분기 차이 벌려보고 중간에 200 넣어서 정렬되지 않았는데도 200도 그냥 점프함.

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%205.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%205.png)

 switch 분기, 분기 사이를 벌리니깐 jump table 못만듬. 6 66까지는 되던 넘이었음.

666, 6666도 안됨

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%206.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%206.png)

그럼 몇까지 

254까지 됨.

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%207.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%207.png)

255부터 안되네 규칙을 정확히는 몰겠다.

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%208.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%208.png)

값의 범위인가 보다.

1000부터 1005까지는 되네 

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%209.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%209.png)

1254 됨

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%2010.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%2010.png)

1255 안됨

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%2011.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%2011.png)

예상

## 결론

일단 case 항목이 6개이상에 최저값과 최대값 사이의 범위가 255개의 안에 들어가는 경우 

jmp table을 만들어서 처리하고 안들어 가는 경우
위 조건에 맞지 않는 경우 단순 비교로 들어간다.

내일 면접이라 노가다는 여기까지만... 

## 후기

![switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%2012.png](switch%20jump%20table%209ecede9b1c394591bacc2d725e51b0ac/Untitled%2012.png)

5개가 넘어야 한다는 조건을 말해주신 분이 있었음.
이건 테스트가 끝나고 발견했다.

[https://www.gpgstudy.com/forum/viewtopic.php?t=20359](https://www.gpgstudy.com/forum/viewtopic.php?t=20359)

## 추후 실험 목표

255개 이상의 case를 만들면 다 적용 되는지 궁금하다.

case의 갯수를 255 → 65535개 혹은 그 이상까지 늘어 나는지 등이 궁금하다