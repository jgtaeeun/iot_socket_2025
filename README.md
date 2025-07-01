# iot_socket_2025
## vmware, putty 시작하기
- vm workstation - restart guest
- putty configuration - ubuntu load(ip - 192.168.74.128)
- id - reed
- pass - 1234
- <img src='./images/putty와 vm ubuntu연결 22번 포트.png' width=500>

## 99일차(6/30)
#### vmware, ubuntu, PuTTy 설치
- ubuntu 디스크이미지 파일 다운로드
    1. https://ubuntu.com/ 
    2. Download Ubuntu - Download Ubuntu Desktop -Ubuntu 24.04.2 LTS download
        - <img src='./images/ubuntu디스크이미지파일.png' width=500>

- PuTTy 다운로드
    1. https://www.putty.org/ 
    2. Download PuTTY - 64-bit x86 : putty-64bit-0.83-installer.msi

- vmware 가상머신 설치
    1. https://support.broadcom.com/ - 회원가입 및 로그인
    2. VMware Cloud Foundation-  My Downloads 이동 
        - <img src='./images/vmwarer 가상머신 설치 .png' width=500>
    3. Free Software Downloads available HERE 클릭
    4. VMware Workstation Pro - VMware Workstation Pro 17.0 for Windows - 17.6.3
    5. 이용약관 동의 - 다운로드 버튼 클릭 - 주소정보 입력
    6. VMware-workstation-full-17.6.3-24583834 응용프로그램파일을 관리자권한으로 실행 - 다운로드
    7. VMware Workstation 아이콘 실행 - create a new virtual machine - **installer disc image file을 다운로드한 ubuntu로 하기**
    8. next, finish 계속 해서 설치 마무리
        - <img src='./images/ubuntu 가상환경 설치.png' width=500>
    9. 우분투 기본 설정하기 
        - 언어, 계정(reed, 1234), 최소 설정 등  [참고사이트](https://movefun-tech.tistory.com/23)
        - 설치 후 다시 시작
        - 로그인- 왼쪽하단버튼- cmd 검색해서 실행
        - ifconfig 설치 [참고사이트](https://yigongyikong.tistory.com/866)
            ```
            ~$ sudo apt install net-tools
            ~$ ifconfig
            
            ```
            - <img src='./images/inet.png' width=500>
        - openssh-server 설치
            ```
            ~$ sudo apt install openssh-server
            ~$ netstat -lnpt | grep :22
            ```
    10. Putty configuration
        - 다운로드한 putty 실행파일 실행 , configuration 설정
            - <img src='./images/puttyconfiguration설정.png' width=500>
            - <img src='./images/putty로그인.png' width=500>
        

#### 준비학습

|호출순서|함수|
|:--:|:--:|
|소켓생성|socket()|
|소켓주소할당|bind()|
|연결요청대기|listen()|
|연결허용|accept()|
|데이터송/수신|read/write()|
|종료|close()|


#### 문법 with Putty  [./소켓/chapter1]
1. gcc 컴파일러 
    - gcc 컴파일러 설치
        ```
        sudo apt install gcc
        ```
    - C 파일 작성
        ```
        touch main.c
        nano main.c
        cat main.c
        ```
    - 실행명령
        ```
        gcc main.c -o main.out
        ./main.out
        ```
    - <img src='./images/gcc로 c파일 실행.png' width=500>

2. 자주 사용하는 기본 명령어
    - <img src='./images/자주사용하는기본적인명령어.png' width=500>
    - <img src='./images/nano 설정.png' width=500>
    - <img src='./images/명령어1.png' width=500>
    - <img src='./images/명령어2.png' width=500>
    - <img src='./images/명령어3.png' width=500>
    - <img src='./images/명령어4.png' width=500>

    - 파일/디렉토리 명령어 연습
        - <img src='./images/파일연습1.png' width=500>
        - <img src='./images/파일연습2.png' width=500>
        - <img src='./images/파일연습3.png' width=500>
        - <img src='./images/파일연습4.png' width=500>
        - <img src='./images/파일연습5.png' width=500>

3. 리눅스에서 파일을 열거나 생성
    - 기본형태
        ```C
        int open(const char *pathname, int flags[, mode_t mode]);
        ```
        - pathname: 열고자 하는 파일 경로
        - flags: 파일을 어떻게 열지 설정하는 옵션 (읽기, 쓰기, 생성 등)
        - mode: 새 파일을 만들 때 퍼미션 설정 (예: 0644), O_CREAT가 있을 때만 사용됨

    - 플래그
        |플래그|의미|
        |:--:|:--:|
        |O_RDONLY|읽기 전용으로 연다|
        |O_WRONLY|쓰기 전용으로 연다|
        |O_RDWR|읽기 + 쓰기 모두 가능하게 연다|
        |O_CREAT|파일이 없으면 새로 만든다|
        |O_APPEND|파일 끝에 내용을 추가해서 쓴다|
        |O_TRUNC|파일 열 때 기존 내용을 지우고 빈 상태로 연다|
        - 플래그들은 #define된 상수입니다. 예를 들어, O_RDONLY는 내부적으로 그냥 0이에요.
        - 플래그는 비트 연산(|)으로 조합할 수 있어요.

    - 파일 디스크립터
        |이름|의미|번호|
        |:--:|:--:|:--:|    
        |stdin|표준 입력 (키보드 등)|0|
        |stdout|표준 출력 (화면 등)|1|
        |stderr|표준 에러 출력|2|
        - fileno() 함수는 FILE* 스트림 (예: stdin, stdout, stderr)이 내부적으로 사용하는 파일 디스크립터 번호(int)를 반환합니다.
        ```c
        #include <unistd.h>
        int main() {
        printf("stdin FD: %d\n", fileno(stdin));   // 0
        return 0;
        }

        ```
        - open()은 성공하면 파일 디스크립터(file descriptor)를 반환합니다. 이건 파일을 나타내는 양수(보통 3 이상) 숫자입니다.
        - 0 이상 :	성공 
        - -1	: 실패 
            ```c
            #include <stdio.h>
            #include <fcntl.h>
            #include <stdlib.h>
            #include <unistd.h>


            int main()
            {
                    int fd;
                    fd = open("a.txt", O_CREAT|O_RDONLY, 0644);
                    if (fd <0)
                    {
                            perror("could not open a.txt");
                            exit(1);
                    }
                    else
                    {
                            printf("open success!\n");
                            printf("file descripter(0이상이어야지 성공) : %d\n" , fd);
                    }
                    close(fd);
                    return 0;

            }
            ```
    - 권한 확인
        ```cs
            ls -l
        ```
        - <img src='./images/권한 확인.png' width=500>
        - ---sr-s--- 에서 소유자와 기타 사용자 모두 읽기, 쓰기, 실행 권한이 없음 →즉, 파일 소유자도 읽거나 쓸 수 없는 상태입니다.
        - 이 때문에 프로그램에서 permission denied 오류가 나는 겁니다.
4. 파일 권한(퍼미션) 형식

    |위치|의미|
    |:--:|:--:|
    |1번째 문자|파일 종류|
    |2~4번째 문자|소유자(user) 권한|
    |5~7번째 문자|그룹(group) 권한|
    |8~10번째 문자|기타 사용자(other) 권한|
    - <img src='./images/파일권한1.png' width=500>
    - <img src='./images/파일권한2.png' width=500>   
    - 권한 확인
        ```c
        //  -: 일반 파일
        // rwx: 소유자는 읽기, 쓰기, 실행 가능
        // r-x: 그룹은 읽기, 실행 가능 (쓰기 불가)
        // r--: 기타 사용자는 읽기만 가능
        -rwxr-xr--
        ```
    - 권한 변경
        - <img src='./images/권한변경테스트.png' width=500>
        - <img src='./images/파일권한3.png' width=500>
5. 성공, 실패 메시지 출력

    |상황|함수|출력대상|
    |:--:|:--:|:--:|    
    |성공 메시지|printf(), puts() 등|stdout (표준 출력, 화면)|
    |에러 메시지|perror(), fprintf(stderr, ...)|stderr (표준 에러 출력)|

6. 리틀 엔디안 vs 빅 엔디안 (Little Endian vs Big Endian)

    |개념|빅엔디안 (Big Endian)|리틀엔디안 (Little Endian)|
    |:--:|:--:|:--:|
    |의미|큰 바이트가 앞|작은 바이트가 앞|
    |저장 순서 (메모리 or 전송)|상위 바이트 → 하위 바이트|하위 바이트 → 상위 바이트|
    |예: 0x12345678|12 34 56 78|78 56 34 12|
    |사용처|네트워크 전송, 일부 CPU|x86/x86-64 (인텔/AMD) 등|

    - 네트워크에서는 시스템마다 엔디안 방식이 다르기 때문에, 모든 데이터는 "빅엔디안"으로 전송하자는 규칙이 생겼습니다.
    - 이를 "네트워크 바이트 오더(Network Byte Order)" = Big Endian 이라고 부릅니다.
    - 변환
        |함수|의미|목적|
        |:--:|:--:|:--:|
        |htonl(x)|Host TO Network Long|리틀엔디안 → 빅엔디안 변환|
        |ntohl(x)|Network TO Host Long|빅엔디안 → 리틀엔디안 변환|
        - 대부분 PC/서버는 리틀엔디안 (작은 바이트가 먼저)
        - 하지만 네트워크에서는 빅엔디안이 표준
        - 그래서 데이터를 보내거나 받을 때는 변환이 필요함

    1. 리틀 엔디안
    ```c
    #include <stdio.h>

    void main()
    {
        int n =  0x1234567;         // 16진수: 0x01234567
        char *pn = &n;              // int형 변수 n의 주소를 char 포인터로 저장

        printf("1st : %p , %#x\n" , &(*pn),  *pn);
        printf("2nd : %p , %#x\n" , &(*pn),  *(pn+1));
        return 0;
    }
    ```
    - <img src='./images/리틀엔디안.png' width=500>
    - <img src='./images/메모리해석.png' width=500>
 
    2. 리틀 엔디안 -> 빅 엔디안 변환
        ```c
        #include <stdio.h>
        #include <arpa/inet.h>  // htonl, ntohl 함수가 선언된 헤더

       
        void  main()
        {

                unsigned short port = 0x1234;
                unsigned int ip = 0x12345678;

                unsigned short network_port;
                unsigned int network_ip;

                network_port = htons(port);
                network_ip = htonl(ip);

                printf("ip:%#x", ip);
                printf(" port : %#x\n", port);
                printf("conversion ip: %#x", network_ip);
                printf("conversion port : %#x\n", network_port);

        }


        ```
        - <img src='./images/빅엔디안 기준.png' width=500>
        

7. 프로토콜 체계, 소켓 타입(전송방식)
    |인터넷프로토콜 체계|
    |:--:|
    |PF_INET<br/>IPv4 인터넷프로토콜 체계|
    |PF_INET6<br/>IPv6 인터넷프로토콜 체계|

    |소켓 타입|표현|
    |:--:|:--:|
    |TCP(연결지향형 소켓)|SOCK_STREAM|
    |UDP(비연결지향형 소켓)|SOCK_DGRAM|

    ```C
    #include <sys/socket.h>
    #include <netinet/in.h>

    //PF_INET은 IPv4 프로토콜을 사용하겠다는 뜻이고,
    //SOCK_STREAM은 TCP 소켓 타입을 의미합니다.
    int sockfd = socket(PF_INET, SOCK_STREAM, 0);

    ```
## 100일차(7/1)
#### 문법 with Putty  [./소켓/chapter2]
- 문자열 정보 처리
    ```c
    #include <arpa/inet.h>
    ```
    - printf("%#x", ...) 등으로 출력하면: 호스트 바이트 순서, 즉 보통의 PC(리틀 엔디안 시스템) 기준으로 정수 해석 결과가 출력됩니다. 
    1. inet_addr() -  문자열로 된 IPv4 주소를 네트워크 바이트 오더(빅엔디안)로 변환
        - 매개변수 -  cp: 점으로 구분된 문자열 형식의 IPv4 주소 ("192.168.0.1" 등)
        - 반환값 - 변환된 32비트의 IPv4 주소 (네트워크 바이트 순서)
        - 오류가 발생하거나 잘못된 주소일 경우 INADDR_NONE (-1) 반환
        ```C
        #include <arpa/inet.h>
        void main()
        {

                char* addr = "192.168.0.1";
                unsigned int cv_addr = inet_addr(addr);
                if (cv_addr == INADDR_NONE) perror("Error");
                else printf("addrss : %#x\n",cv_addr );   //addrss : 0x100a8c0
                //printf("%#x", ...) 등으로 출력하면: 호스트 바이트 순서, 즉 보통의 PC(리틀 엔디안 시스템) 기준으로 정수 해석 결과가 출력됩니다.
               

        }
        ```
        - <img src='./images/inetaddr.png' width=500>
    2. inet_aton() - 문자열 IP → struct in_addr
        - 매개변수1 - cp: 점으로 구분된 문자열 형태의 IPv4 주소 (예: "192.168.1.1")
        - 매개변수2 - inp: 변환된 IP 주소를 저장할 struct in_addr 타입 포인터
        - 반환값 - 변환 성공 시 1 , 변환 실패 시 0
        ```C

        void main()
        {

                const char* addr ="192.168.0.1";
                struct sockaddr_in addr_inet;

                if (!inet_aton(addr, &addr_inet.sin_addr))
                {
                        perror("conversion failed");
                }
                else
                {
                        printf("conversion addr : %#x\n",addr_inet.sin_addr.s_addr); // 0x100a8c0

                }
        }

        ```
        - <img src='./images/inetaton.png' width=500>
    - 결론

    |의미|값 (16진수)|설명|
    |:--:|:--:|:--:|
    |호스트 바이트 순서|0x100a8c0|inet_addr() 결과를 리틀엔디안으로 printf 해석|
    |네트워크 바이트 순서|0xC0A80001|실제 IP: 192.168.0.1 (정상적 변환 결과)|

- 구조체에서 사용되는 타입 정의(type definition)

|타입 이름|실제 타입|의미|주로 쓰이는 곳|
|:--:|:--:|:--:|:--:|
|sa_family_t|unsigned short|주소 체계 (IPv4 주소 체계를 의미하는 AF_INET은 정수 값 2입니다)|sockaddr, sockaddr_in, etc.|
|in_addr_t|uint32_t|IPv4 주소 (32비트)|inet_addr(), in_addr.s_addr|
|in_port_t|uint16_t|포트 번호 (16비트)|sockaddr_in.sin_port, htons() 등|
   
- IPv4 관련 구조체-  소켓 프로그래밍, IP 주소 처리, 네트워크 통신에서 매우 자주 사용
    ```c
    #include <netinet/in.h>
    ```
    1. struct in_addr
        - IPv4 주소를 저장하는 구조체
        - 보통 inet_aton(), inet_pton() 등에서 변환된 IP 주소를 저장할 때 사용 
        ```C
        struct in_addr {
            in_addr_t s_addr;  // 32비트 IPv4 주소 (네트워크 바이트 순서)
        };
        ```

    2. struct sockaddr_in
        - IPv4 주소용 소켓 주소 구조체
        - struct sockaddr를 기반으로 IP와 포트 정보를 담음
        ```C
        struct sockaddr_in {
            sa_family_t    sin_family; // AF_INET
            in_port_t      sin_port;   // 포트 번호 (네트워크 바이트 순서)
            struct in_addr sin_addr;   // IPv4 주소
            unsigned char sin_zero[8]; // 구조체 크기 맞춤용 패딩
        };
        ```
        - <img src='./images/구조체와 타입정의.png' width=500>
        - <img src='./images/2바이트인 이유.png' width=500>
    3. struct sockaddr 
    ```c
    struct sockaddr {
        sa_family_t sa_family;
        char        sa_data[14];
    }; 
    ```
    4. memset -  구조체를 0으로 초기화할 때 자주 사용됩니다.
        ```c
        #include <string.h>
        void *memset(void *ptr, int value, size_t num);
        ```
        - 매개변수 설명

        |매개변수|설명|
        |:--:|:--:|
        |ptr|값을 채울 메모리의 시작 주소|
        |value|채울 값 (하위 1바이트만 사용됨)|
        |num|채울 바이트 수|

        ```c
        #include <stdio.h>
        #include <arpa/inet.h>
        #include <string.h>
        #include <stdint.h>

        //구조체 in_addr 타입처럼 사용자가 만든 구조체
        struct myin_addr {

                uint32_t s_addr;
        };

        //구조체 sockaddr_in 타입처럼 사용자가 만든 구조체
        struct mysockaddr_in
        {
                unsigned short sin_family;
                struct myin_addr sin_addr;
                uint16_t sin_port;
        };

        int main(void){

                struct mysockaddr_in myaddr;
                memset(&myaddr, 0, sizeof(myaddr));

                printf("sin_family: %d\n", myaddr.sin_family);   //0
                printf("sin_addr: %d\n", myaddr.sin_addr.s_addr);  //0
                printf("sin_port: %d\n", myaddr.sin_port);   //0
                return 0;
        }

        ```
#### TCP 서버-클라이언트 프로그램의 기본 구조 with Putty [./소켓/chapter2/base_server.c] [./소켓/chapter2/base_client.c]
- 서버에 있는 문자열을 클라이언트가 수신
- 서버
    ```c
    #include <stdio.h>
    #include <stdlib.h>
    #include <unistd.h>
    #include <string.h>
    #include <arpa/inet.h>


    int main(int argc, char** argv)
    {
            int server_fd, client_fd; //서버 소켓 파일 디스크립터 ,  클라이언트 소켓 파일 디스크립터 (accept 후 생성)
            struct sockaddr_in server_addr, client_addr;  //서버와 클라이언트 주소 정보를 담는 구조체
            socklen_t client_addr_len;   //클라이언트 주소 구조체 크기
            const char* message = "Hello World~\n";   // 클라이언트에 보낼 문자열

            if ((server_fd = socket(PF_INET, SOCK_STREAM, 0)) == -1)   //socket() 함수로 TCP 소켓 생성 (PF_INET=IPv4, SOCK_STREAM=TCP)
            {
                    perror("socket failed");
                    exit(1);
            }
            memset(&server_addr, 0, sizeof(server_addr));  
            server_addr.sin_family = AF_INET;   //AF_INET은 IPv4)
            server_addr.sin_addr.s_addr = INADDR_ANY;   //모든 네트워크 인터페이스에서 들어오는 연결 허용
            server_addr.sin_port = htons(atoi(argv[1]));  // 서버실행 ./base_server 9090



            if (bind(server_fd, (struct sockaddr*)&server_addr, sizeof(server_addr)) == -1 )  //소켓에 IP 주소와 포트 번호 할당(바인딩)
            {
                    perror("bind failed");
                    close(server_fd);
                    exit(1);
            }

            if(listen(server_fd, 5) == -1)  //클라이언트 연결 대기 상태로 변경 (listen 호출) , 큐 길이 5 (동시 접속 대기 최대 5명)
            {
                    perror("listen failed");
                    close(server_fd);
                    exit(1);
            }
            client_addr_len = sizeof(client_addr);   //동시에 5명의 클라이언트가 접속하더라도, 각각의 accept() 호출에서 client_addr_len은 항상 한 명에 대한 주소 크기만 사용됩니다.
            if( (client_fd = accept(server_fd, (struct sockaddr*)&client_addr, &client_addr_len)) == -1 )  //accept는 새로운 소켓(client_fd) 생성 (클라이언트와 통신용)
            {
                    perror("accept failed");
                    close(server_fd);
                    exit(1);
            }
            if (write(client_fd, message, strlen(message)) == -1 ) perror("write failed");

            close(client_fd);
            close(server_fd);
            return 0;
    }


    ```
- 클라이언트
    ```c
    #include <stdio.h>
    #include <stdlib.h>
    #include <arpa/inet.h>
    #include <string.h>
    #include <unistd.h>

    int main(int argc, char** argv)
    {

            int sock_fd;    //클라이언트 소켓 디스크립터
            struct sockaddr_in  server_addr;   //서버의 주소 정보를 저장할 구조체
            char buf[1024];   //서버에서 받는 데이터를 저장할 버퍼
            int recv_len;   //실제로 받은 바이트 수를 저장할 변수


            if((sock_fd = socket(PF_INET, SOCK_STREAM, 0) ) == -1 )
            {
                    perror("socket failed");
                    exit(EXIT_FAILURE);
            }

            memset(&server_addr , 0, sizeof(server_addr));
            server_addr.sin_family = AF_INET;
            server_addr.sin_addr.s_addr = inet_addr(argv[1]);  // 클라이언트 실행 ./base_server 127.0.0.1 9090     //ip주소
            server_addr.sin_port = htons(atoi(argv[2]));    // 포트 번호

            if (connect(sock_fd, (struct sockaddr*)&server_addr, sizeof(server_addr)) == -1 )  //서버에 연결 시도
            {
                    close(sock_fd);
                    exit(EXIT_FAILURE);
            }

            recv_len=recv(sock_fd, buf, sizeof(buf)-1, 0);  //서버에서 메시지 수신 , 최대 1023 바이트까지 받고, recv_len에 받은 길이 저장

            if (recv_len == -1 )
            {
                    printf("receive failed");
            }
            printf("Message from server : %s %ld %s \n",buf, sizeof(buf), buf[14]);
            close(sock_fd);
            return 0;


    }


    ```
- TCP 클라이언트 실행
    - <img src='./images/server,client tcp통신.png' width=500>
    - 127.0.0.1:9090 요청은 보통 자기 자신 컴퓨터에서 실행 중인 서버(9090번 포트)에 접근
    - C언어에서 문자열(string)은 **널 문자('\0')**로 끝나는 문자 배열이에요. 문자열의 실제 문자 개수 + 널 문자('\0') 1개 를 저장할 공간이 필요해요.
    - <img src='./images/argv개념.png' width=500>

####  TCP 서버-클라이언트 분석

- TCP 서버

    |호출순서|함수|
    |:--:|:--:|
    |소켓생성|socket()|
    |소켓주소할당|bind()|
    |연결요청대기|listen()|
    |연결허용|accept()|
    |데이터전송|recv(), send()|
    |종료|close()|

    1. socket함수
        ```c
        #include <sys/types.h>
        #include <sys/socket.h>

        int socket(
            int domain, 
            int type, 
            int protocol
        );
        ```
        - domain - 주소 체계 지정 , IPv4기반의 AF_INET 
        - type - 소켓 타입 지정 , SOCK_STREAM, SOCK_DGRAM
        - protocol - 사용할 프로토콜 지정 , 대개는 0을 넣는다. 

    2. bind함수
        ```c
        #include <sys/types.h>
        #include <sys/socket.h>

        int bind(
            int sock,
            const struct sockaddr *addr,
            socklent_t addrlen
        );
        ```
        - sock - 클라이언트 접속을 수용하는 소켓, 지역 IP주소와 지역 포트 번호가 아직 결정되지 않은 상태
        - addr - 소켓 주소 구조체, 통신 프로토콜에 따른 주소 정보를 담고 있다. **TCP/IP의 경우 sockaddr_in 또는 sockaddr_in6 구조체를 사용하되, 지역IP주소와 지역포트번호로 초기화하여 전달한다.(struct sockaddr*로 형변환)**
        - addrlen - 소켓 주소 구조체의 길이(바이트 단위)
    3. listen함수
        ```c
        #include <sys/types.h>
        #include <sys/socket.h>
        int listen(
           int sock,
           int backlog
        );
        ```
        - sock - 클라이언트 접속을 수용하는 소켓, 이전에 bind()함수를 호출하여 지역IP 주소와 지역 포트번호를 설정한 상태
        - backlog - 서버가 당장 처리하지 않더라도 접속 가능한 클라이언트의 개수, 클라이언트의 접속정보는 연결 큐에 저장되는데 backlog는 이 연결 큐의 길이를 나타낸다.

    4. accept함수
       ```c
        #include <sys/types.h>
        #include <sys/socket.h>
        int accept(
           int sock,
           struct sockaddr *addr,
           socklen_t *addrlen
        );
        ```
        - sock - 클라이언트 접속을 수용하는 소켓,  이전에 bind()함수를 호출하여 지역IP 주소와 지역 포트번호를 설정하고, listen()함수로 TCP 상태를 LISTENING으로 변경한 상태
        - addr - 소켓 주소 구조체를 전달하면, 접속한 클라이언트의 주소정보로 채워진다.
        - addrlen - 운영체제에 따라 int 또는 socklen_t 타입의 변수를 준비하고 addr이 가리키는 소켓 주소 구조체의 크기로 초기화 하여 전달
    5. recv, sent함수 (데이터전송함수) 
    6. close함수
        ```c
        #include <unistd.h>
        int close(int fd);
        ```

- TCP 클라이언트

    |호출순서|함수|
    |:--:|:--:|
    |소켓생성|socket()|
    |서버에 접속|connect()|
    |데이터전송|recv(), send()|
    |종료|close()|
    
    1. socket함수

    2. connect함수
        ```c
        #include <sys/types.h>
        #include <sys/socket.h>
        int connect(
            int sock,
            const struct sockaddr *addr;
            socklen_t addrlen
        );
        ```
        - sock - 서버와 통신하는 소켓
        - addr - 소켓 주소 구조체를 서버주소로 초기화하여 전달한다.
        - addrlen - 소켓 주소 구조체의 길이(바이트 단위)
        - client_addr이 필요없는 이유-  운영체제가 자동으로 지역 IP주소와 지역 포트번호를 할당해준다. 
    3. send함수(송신)
        ```c
        #include <sys/types.h>
        #include <sys/socket.h>
        ssize_t send(
            int sock,
            const void *buf,
            size_t len,
            int flags
        );
        ```
        - sock - 서버와 연결된 소켓
        - buf - 보낼 데이터를 담고 있는 응용 프로그램의 버퍼의 주소
        - len - 보낼 데이터의 크기(바이트 단위)
        - flags - send()함수 동작을 바꾸는 옵션, 거의 항상 0을 사용

    4. recv함수(수신)
        ```c
        #include <sys/types.h>
        #include <sys/socket.h>
        ssize_t recv(
            int sock,
            void *buf,
            size_t len,
            int flags
        );
        ```
        - sock - 서버와 연결된 소켓
        - buf - 받은 데이터를 저장할 응용 프로그램의 버퍼의 주소
        - len - 운영체제의 수신 버퍼로부터 복사할 최대 데이터 크기, 이 값은 buf가 가리키는 응용 프로그램 버퍼보다 크지 않아야 한다.
        - flags -recv()함수 동작을 바꾸는 옵션
        ```c
            recv_len=recv(sock_fd, buf, sizeof(buf)-1, 0);  //서버에서 메시지 수신 , 최대 1023 바이트까지 받고, recv_len에 받은 길이 저장
        ```   
    5. close함수

#### 클라이언트-서버 구조에서 양방향 통신
- 전체 동작 흐름 요약
    1. 서버 실행
        - 서버는 지정된 포트에서 bind(), listen()을 통해 연결 대기
        - 클라이언트의 연결 요청이 오면 accept()를 통해 연결 수락
    2. 클라이언트 실행
        - 서버의 IP와 포트로 connect() 시도
        - 연결이 성공하면 통신 시작
    3. 양방향 통신
        - 클라이언트: 사용자의 입력을 받아 서버로 send() , 서버의 응답을 recv()로 받아 출력
        - 서버: 클라이언트가 보낸 메시지를 recv()로 수신,  받은 데이터를 다시 클라이언트로 send() (에코)
        - `한 번의 메시지 주고받기마다 끝나고, 이전 메시지는 저장하거나 쌓아두지 않음.`
    4. 종료
        - 클라이언트에서 "Q" 또는 "q" 입력 시 루프 종료 → 서버 소켓과 클라이언트 소켓 종료

- 서버
    ```c
    #include <stdio.h>
    #include <stdlib.h>
    #include <unistd.h>
    #include <string.h>
    #include <arpa/inet.h>


    #define BUFFER_SIZE 1024

    int main(int argc, char* argv[])
    {
            int server_fd, client_fd;
            struct sockaddr_in server_addr, client_addr;
            socklen_t client_addr_len;
            char buffer[BUFFER_SIZE];
            ssize_t bytes_read;

            if (argc !=2)
            {
                    printf("%s <port>\n", argv[0]);
                    exit(1);
            }

            server_fd = socket(PF_INET , SOCK_STREAM,0);
            if (server_fd == -1 )
            {
                    perror("socket failed");
                    exit(1);
            }

            memset(&server_addr, 0, sizeof(server_addr));
            server_addr.sin_family = AF_INET;
            server_addr.sin_addr.s_addr = INADDR_ANY;
            server_addr.sin_port =  htons(atoi(argv[1]));

            if (bind(server_fd, (struct sockaddr*)&server_addr, sizeof(server_addr)) == -1)
            {
                    perror("bind failed");
                    close(server_fd);
                    exit(1);
            }

            if (listen(server_fd, 5) == -1 )
            {
                    perror("listen failed");
                    close(server_fd);
                    exit(1);
            }
            client_addr_len = sizeof(client_addr);

            if( (client_fd = accept(server_fd, (struct sockaddr*)&client_addr, &client_addr_len)) == -1 )
            {
                    perror("accept failed");
                    close(server_fd);
                    exit(1);
            }
            while((bytes_read = recv(client_fd, buffer, sizeof(buffer)-1 , 0)) !=0 )
            {
                    buffer[bytes_read]= '\0';
                    printf("message from client : %s" , buffer);
                    send(client_fd, buffer, bytes_read, 0);
            }
            close(client_fd);
            close(server_fd);
            return 0;

    }

    ```
- 클라이언트
    ```c
    #include <stdio.h>
    #include <string.h>
    #include <stdlib.h>
    #include <arpa/inet.h>
    #include <unistd.h>

    #define BUFFER_SIZE 1024

    int main(int argc, char** argv)
    {
            int server_fd;
            struct sockaddr_in server_addr;
            char buffer[BUFFER_SIZE];
            int recv_len;

            if (argc !=3)
            {
                    printf("%s <ip> <port> \n" , argv[0]);
                    exit(1);
            }

            if( (server_fd = socket(PF_INET, SOCK_STREAM, 0)) == -1 )
            {
                    printf("socket failed");
                    exit(1);
            }
            memset(&server_addr, 0, sizeof(server_addr));
            server_addr.sin_family = AF_INET;
            server_addr.sin_addr.s_addr = inet_addr(argv[1]);
            server_addr.sin_port = htons(atoi(argv[2]));

            if (connect(server_fd, (struct sockaddr*)&server_addr, sizeof(server_addr))== -1)
            {
                    close(server_fd);
                    exit(1);
            }
            puts("server connected....");

            while(1)
            {       fputs("Exit(Q to quit) : " , stdout);
                    fgets(buffer, BUFFER_SIZE-1, stdin);

                    if(!strcmp(buffer, "Q\n")  || !strcmp(buffer, "q\n") )
                    {
                            break;
                    }

                    send(server_fd, buffer, strlen(buffer), 0);
                    recv_len = recv(server_fd, buffer, sizeof(buffer)-1, 0);
                    if (recv_len == -1 )
                    {
                            printf("receive failed");
                    }
                    buffer[recv_len]='\0';
                    printf("Message from server : %s\n" , buffer);

            }

            close(server_fd);
            return 0;
    }

    ```

- TCP 클라이언트 실행
    - <img src='./images/tcp 클라이언트,서버 양방향통신.png' width=500>
    - <img src='./images/누적메시지 하려면.png' width=500>

## 101일차(7/2)