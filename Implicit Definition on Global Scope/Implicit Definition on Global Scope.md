# Is Assigning on Global Scope Possible?

## 문제

C 전역에서 대입연산이 안 되는 건 다들 알고 있다.

```c
int a[2];
a[0] = 1;
a[1] = 2;
```
전역에 배열 '선언' 후 각 값을 대입하려 하니 에러가 난다.(당연하게도)







그런데 이상한 점이 있다.

```c
int a;
a=1;
```
전역에 변수 '선언' 후 값 대입하려 했더니 에러가 안 난다.(?!)

```c
int a;
a=1;
a=2;
```
근데 전역에 변수 '선언' 후 값을 한번 더 대입하려 하면 에러가 난다.(?!!)





**왜 이런가??????**





## 해결

C 컴파일러가 위 문장들을 선언과 대입이 아니라, 선언과 정의라고 간주하기 때문에 위 현상이 발생한다.



C는 전역에서 선언된 변수가, 전역에서 자료형 없이 '정의'되도 암시적으로 int나 float이라고 생각한다. 즉, 정의시 해당 자료형을 알아서 붙인다는 말이다.



따라서 첫 번째 문장에서
```c
int a[2];
a[0] = 1;
a[1] = 2;
```
컴파일러가 a[0]와 a[1]의 대괄호 안의 숫자를 인덱싱의 의미가 아니라 배열의 크기를 표시해주는 숫자로 처리 한다는 것. 즉 크기 0짜리 배열, 크기 1짜리 배열을 정의하는 문장으로 간주한다는 것이다.



즉 두 번째 줄은 크기 0인 배열을 정의 할 수 없어서 에러가 나는 것이고, 세 번째 줄은 선언한 배열의 크기(2)와 정의한 배열의 크기(1)가 달라서 에러가 나는 것이다. 



따라서
```c
int a[2];
a[2] = {1, 2};
```
이렇게 하면 에러가 안 난다. 또한 위 문장은 정의부에 암시적으로 int나 float을 붙인 것이기 때문에

```c
int a[2];
int a[2] = {1, 2};
```
이렇게 명시적으로 붙여준 것과 동일한 문장이다. 물론 이 문장도 에러가 안 난다.



또한 float까지 생략한 채 암시적으로 쓰는게 허용되므로
```c
float a[2];
a[2] = {1, 2};
```
이것도 에러가 안 난다.



```c
char a[2];
a[2] = {1, 2};
```
int와 float을 제외한 자료형은 이렇게 자료형 생략하고 정의하려고 하면 에러가 난다.

```c
char a[2];
char a[2] = {1, 2};
```
물론 이렇게 명시적으로 자료형을 붙여서 정의하면 에러가 안 난다.


두 번째 문장을 보면
```c
int a;
a=1;
```
위 문장은 마치 전역에서 대입을 하는 모양새로 보이지만, 실제론 선언과 정의이다. 즉 대입이 아니라 선언과 정의라서 에러가 안 나는 것.
```c
int a;
int a=1;
```
당연히 이것도 되고



```c
char a;
char a=1;
```
이것도 되지만



```c
char a;
a=1;
```
char 자료형은 암시적으로 붙여주지 않으므로 이건 에러가 난다.



위 내용들을 생각해보면
```c
int a;
a=1;
a=2;
```
세 번째 문장에서 에러가 나는 이유는 자연스럽다. 위 문장을 명시적으로 풀어쓰면
```c
int a;
int a=1;
int a=2;
```
보이는 바와 같이 재정의이기 때문이다.

## 결론

위 사실들로부터, 굳이 하지 말라는 짓 해가며 기괴하게 코딩하면 안 된다는 결론을 도출할 수 있다.
