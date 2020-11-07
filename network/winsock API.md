# winsock API

## listen()

- listen() 을 실행하지 않는다면?

![winsock_API%204366176be36f45ac8bb0d6a610e7b3a4/Untitled.png](winsock_api/Untitled.png)

client는 연결 시도를 진행하면 server는 RST와 ACK로 응답을 한다. 
client는 이 과정을 5회 진행후 error_code 10061를 return 한다.

**WSAECONNREFUSED**

10061 (0x274D) No connection could be made because the target machine actively refused it.
[https://docs.microsoft.com/en-us/windows/win32/debug/system-error-codes--9000-11999-](https://docs.microsoft.com/en-us/windows/win32/debug/system-error-codes--9000-11999-)

- LISTEN Queue의 크기를 0으로 만들어도 1개는 접속 할 수 있다.
단 2개는 안들어가서 연결을 취소 시킨다.

## accept()

- accept() 하지 않아도(= listen()까지만 진행한 상황.) client에서 connect()를 호출하면 3way-handshaking를 진행한다.
즉, accept()는 listen()의 backlog 큐의 갯수를 줄이고 상대방에 대한 SOCKET을 만들고 이를 얻기 위해 호출하는 것이다.

![winsock%20API%204366176be36f45ac8bb0d6a610e7b3a4/Untitled%201.png](winsock_api/Untitled%201.png)

## shutdown()

서버와 클라이언트 둘다 shutdown()을 시도할 수 있지만, 먼저 shutdown()을 시도한 측에 TIME_WAIT이 남는다.

![winsock%20API%204366176be36f45ac8bb0d6a610e7b3a4/Untitled%202.png](winsock_api/Untitled%202.png)

client측에 TIME_WAIT이 남는것은 별문제가 안되지만 server측에 남는것은 포트의 갯수 등으로 인한 자원에 문제가 발생할 것으로 생각된다.

## closesocket()

기본 소켓에서는 FIN을 보내는 동작을 진행한다.

linger 옵션 사용시 closesocket()을 통해 TIME_WAIT을 없앨 수 있다. (= RST 를 보내도록 할 수 있다. )