# 헷갈리는 c언어 정리

## fgets

fgets은 한줄을 입력받을 때 사용할 수 있다. 개행 문자를 기준으로 입력이 끝난다. 또한 개행문자역시 입력버퍼에 저장된다. 문자열 마지막에 \0 문자가 자동으로 추가된다.

### 입력버퍼의 크기 - 1 길이의 문자열을 입력하는 경우

개행 문자가 신기하게도 사라지는 현상이 발생했다. fgets의 두번째 인자로 전달하는 버퍼의 크기 - 1 만큼 입력을 받고 널 문자를 추가한다는 사실을 잊지말자.

## char * 문자열

```c
char * str = "this is sample text";
```

위와 같이 선언된 문자열은 읽기 전용이다.

## strspn 함수

```c
#include <string.h>

size_t strspn(const char *str1, const char *str2);
```

str1의 맨처음 문자부터 str2 문자열에 포함된 문자만 나타나는 최대 길이

## strtok 함수

```c
#include <string.h>

char* strtok(char* str, const char* delimeters);
```

str 문자열을 토큰들로 분리한다.

## type 명령어

```bash
$ type ipconfig
```

linux의 bash에서 사용하는 명령어 프로그램이 저장된 위치를 알려준다.

## 시그널과 시스템 호출

긴 시간이 소모되는 시스템 호출이 진행되는 과정에서 시그널에 의해 인터럽트가 발생하는 경우 시스템 호출은 실패하고 -1을 반환하게 된다. 인터럽트된 시스템 호출이 시그널 핸들링이 끝난 시점에서 다시 시작하게 하려면 sigaction 구조체의 sa_flags 필드를 `SA_RESTART`로 설정하면 된다.

> 오류 코드

```c
...
main()
{
    act.sa_handler = catchsigchld;
    sigfillset(&(act.sa_mask));         // 처리중에 들어오는 다른 시그널 blocking
    sigaction(SIGCHLD, &act, NULL);

    ...

    while(1) {
        makeprompt();
        fputs(prompt, stdout);
        fgets(cmdline, BUFSIZ, stdin);  // 여기서 시그널 핸들링이 실행되어 시스템 호출이 인터럽트 될 수 있다.
        cmdline[strlen(cmdline) - 1] ='\0';

        ...
    }
}
```

> 수정된 코드

```c
...
main()
{
    act.sa_handler = catchsigchld;
    sigfillset(&(act.sa_mask));         // 처리중에 들어오는 다른 시그널 blocking
    //////////////////////////////
    act.sa_flags = SA_RESTART;  //
    //////////////////////////////
    sigaction(SIGCHLD, &act, NULL);

    ...

    while(1) {
        makeprompt();
        fputs(prompt, stdout);
        fgets(cmdline, BUFSIZ, stdin);  // 여기서 시그널 핸들링이 실행되어 시스템 호출이 인터럽트 될 수 있다.
        cmdline[strlen(cmdline) - 1] ='\0';

        ...
    }
}
```

## getlogin

제어 단말기를 실행시킨 사용자의 이름을 반환한다.

## c언어 2차원 배열 매개변수 사용하기

```c
반환형 함수이름(자료형 매개변수[][가로크기]);

또는

반환형 함수이름(자료형 (*매개변수)[가로크기]);
```

매개변수 이름 옆에 대괄호 2개를 쓰고 두번째 대괄호에 배열의 가로크기(열의 개수)를 넣는다. 또는 괄호로 묶은 포인터 뒤에 대괄호를 붙이고 배열의 가로크기를 넣는다.
