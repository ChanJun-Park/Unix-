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
