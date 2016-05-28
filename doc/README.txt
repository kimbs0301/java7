자바 1.7 발표자료

io이론
http://guysheep.tistory.com/86

서적
http://www.acornpub.co.kr/book/java7
http://blog.naver.com/incunv?Redirect=Log&logNo=220517027062

nio2
http://knight76.tistory.com/entry/Java7-jdk7-%EC%86%8C%EA%B0%9C-%EC%B6%9C%EC%8B%9C%EA%B8%B0%EB%85%90-3-NO2

try-with-resources 구문
http://okky.kr/article/231496

Type Inference
String in Switch
Automatic Resource Management
Fork/Join Framework
Underscore in Numeric literal
Catching Multiple Exception Type in Single Catch Block
Binary Literals with Prefix “0b”
G1 Garbage Collector
http://www.jpstory.net/2014/06/java-7-features/

http://www.mimul.com/pebble/default/2009/10/06/1254816960000.html

G1 Garbage Collector
http://logonjava.blogspot.kr/2015/08/java-g1-gc-full-gc.html
http://sarc.io/index.php/java/255-g1-garbage-collector
http://knight76.tistory.com/entry/G1-GC-%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-java-7-update-25-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EC%9E%A5%EC%95%A0-%ED%95%B4%EA%B2%B0-%EC%82%AC%EB%A1%80-%EA%B3%B5%EC%9C%A0
http://www.oracle.com/technetwork/articles/java/g1gc-1984535.html
http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/index.html#overview
http://imp51.tistory.com/120
대용량 서비스에서 혹시 G1 GC를 쓰는 서버 애플리케이션의 경우, 반드시 java 8 update 20 또는  java 7 update 60 이상의 버전을 쓰도록 권고한다.

ecc 암호화
http://javacan.tistory.com/177
https://gist.github.com/ymnk/fec39e033394ee2ec47c

http://linuxism.tistory.com/934

nio2
http://freeminderhuni.blogspot.kr/2013/09/7-nio2-1.html
https://docs.oracle.com/javase/tutorial/essential/io/fileio.html
http://java.boot.by/ocpjp7-upgrade/ch06.html
http://andreinc.net/2013/12/09/java-7-nio-2-how-to-use-seekablebytechannel-interface-for-random-access-to-files-raf/
http://andreinc.net/2013/12/05/java-7-nio-tutorial-the-path-class/
http://andreinc.net/2013/12/05/java-7-nio-2-tutorial-file-attributes/
http://andreinc.net/2013/12/06/java-7-nio-2-tutorial-writing-a-simple-filefolder-monitor-using-the-watch-service-api/
http://andreinc.net/2013/12/06/java-7-nio-2-recursive-folder-walks/

files
http://dodocap.tistory.com/180
http://horajjan.blog.me/220485134540
http://freeminderhuni.blogspot.kr/2013/09/7-nio2-4.html

nio2 file channel
http://debop.blogspot.kr/2013/04/nio2-asynchronousfilechannel.html

nio1 socket
http://story.cosmossoftwareresearchers.com/98
http://zeroit.tistory.com/236

async io
https://github.com/dublintech/async_nio2_java7_examples
http://freeminderhuni.blogspot.kr/2013/10/7-nio2-7-seekablebytechannel.html
http://freeminderhuni.blogspot.kr/2013/10/7-nio2-8-api.html
http://freeminderhuni.blogspot.kr/2013/10/7-nio2-9-api.html
http://freeminderhuni.blogspot.kr/2013/10/7-nio2-10.html

클래스 셈플 확인
http://www.programcreek.com/java-api-examples/index.php?api=java.nio.file.attribute.PosixFilePermission



NIO에서 도입된 3가지 기술

버퍼(Buffer)
NIO에서 Buffer클래스를 도입했다.
1.4이전 버전에서 모든 메모리는 JVM 힙 영역을 통해서 관리했다.
결국, IO 과정에서 항상 비효율적인 복사 과장을 거치고, 블록킹이 된다.
하지만 1.4 부터는 시스템 메모리를 직접 사용할 수 있는 Buffer 클래스(DirectByteBuffer)가 도입 되었다.

채널(Channel)
NIO에서는 스트림의 향상된 버전인 Channel을 도입했다.
채널은 스트림처럼 읽거나 쓰는 단방향에서부터, 읽고 쓰는 양방향 통신이 가능한 세가지 형식이 존재한다.
또한 운영체제에서 제공해주는 다양한 네이티브IO 서비스들을 이용할 수 있게 해준다.

셀럭터(Selector)
셀럭터는 네트워크 프로그래밍의 효율을 높이기 위한 것이다.
기존에는 클라이언트 하나당 스레드 하나를 생성해서 처리해야 했는데,
사용자가 늘어나면 스레드가 많이 생성됨으로 인해 급격한 성능 저하를 가져왔다.
따라서 셀렉터는 하나의 스레드에서 다수의 동시 사용자를 처리할 수 있는 기술이다.
셀렉터를 사용해서 nonblocking 입출력 구현이 가능하다.


1. 채널들을 관리할 Selector를 얻는다.
Selector selector = Selector.open();
2. ServerSocketChannel를 얻는다.
ServerSocketChannel server = ServerSocketChannel.open();
3. 내부 소켓을 얻는다.
ServerSocket socket = server.socket();
4. binding 한다.
SocketAddress addr = new InetSocketAddress(포트번호);
socket.bind(addr);
5. ServerSocketChannel을 Selector에 등록시킨다.
ServerSocket는 OP_ACCEPT동작만 할 수 있다.
server.register(selector, SelectionKey.OP_ACCEPT);
6. 클라이언트의 접속을 대기한다.
이때 접속이 되면 accept()에 의해 상대방 소캣과 연결된 SocketChannel의 인스턴스를 얻는다.
이 채널는 읽기(OP_READ),쓰기(OP_WRITE),접속(OP_CONNECT) 행동을 지원한다.
SocketChannel socketChannel = serverChannel.accept();
7. 접속된 SocketChannel를 Selector에 등록한다.
socketChannel.register(selector, 소켓채널의 해당행동);
8. 채널이 취할 수 있는 3가지 동작(읽기(OP_READ),쓰기(OP_WRITE),접속(OP_CONNECT) )에 대해서 검사한다.
이때는 다음과 같은 메서드를 이용해서 체크를 한다.
isConnectable() : true이면 상대방 소켓과 새로운 연결이 됐다는 뜻이다.
이때 Non-Blocking I/O 모드일 경우에는 연결과정이 끝나지 않는 상태에서 리턴될 수도 있다.
그러므로 SocketChannel 의 isConnectionPending()메서드가 true를 리턴하는지 아닌지를 보고
true를 리턴한다면 finishConnection()를 명시적으로 불러서 연결을 마무리짓도록 한다.
isReadable() : true이면 읽기(OP_READ) 동작이 가능하다는 뜻으로 SocketChannel로부터
데이터를 읽는 것에 관한 정보를 받을 준비가 되었다는 뜻이다.
따라서 버퍼에 읽어들이기만 하면된다.
isWritable() : true이면 쓰기(OP_WRITE) 동작이 가능하다는 뜻으로
SocketChannel에 데이터를 쓸 준비가 되었다는 뜻이다.
따라서 버퍼의 데이터를 이 채널에 write()하면 된다.
