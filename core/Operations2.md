## 비교 연산자
- `a=b` 와 같이 등호가 하나일 때는 할당을 의미한다.
- `a ! =b` 은 같지 않음을 나타낸다

## 불린형 반환
다른 연산자와 마찬가지로 비교 연산자 역시 값을 반환한다. 반환 값은 불린형이다.

`true`가 반환되면, '긍정', '참', '사실'을 의미한다.
`false`가 반환되면, '부정', '거짓' '사실이 아님'을 의미한다.

반환된 불린값은 다른 여타 값처럼 변수에 할당 할 수 있다.

    let result = 5 > 4; // 비교 결과를 변수에 할당
    alert( result ); // true
변수 `result` 는 4보다5가 크니까  `ture` 값이 할당된 모습

## 문자열 비교
자바스크립트는 '사전'순으로 문자열을 비교한다. '사전편집'순이라고 불리기도 하는 이 기준을 적용하면 사전 뒤쪽의 문자열은 사전 앞쪽의 문자열보다 크다고 판단한다.

문자열을 구성하는 문자 하나하나를 비교해가며 문자열을 비교한다. 아래 예시를 살펴보자

    alert( 'Z' > 'A' ); // true
    alert( 'Glow' > 'Glee' ); // true
    alert( 'Bee' > 'Be' ); // true

문자열 비교 시 적용되는 알고리즘은 다음과 같다.

1. 두 문자열의 첫 글자를 비교합니다.
2. 첫 번째 문자열의 첫 글자가 다른 문자열의 첫 글자보다 크면(작으면), 첫 번째 문자열이 두 번째 문자열보다 크다고(작다고) 결론 내고 비교를 종료합니다.
3. 두 문자열의 첫 글자가 같으면 두 번째 글자를 같은 방식으로 비교합니다.
4. 글자 간 비교가 끝날 때까지 이 과정을 반복한다.
5. 비교가 종료되었고 문자열의 길이도 같다면 두 문자열은 동일하다고 결론 낸다. 비교가 종료되었지만 두 문자열의 길이가 다르면 길이가 긴 문자열이 더 크다고 결론 내린다.

코드의 `'Z' > 'A'`는 알고리즘의 첫 번째 단계에서 비교 결과가 도출된다. 순서가 `Z`가 더 크기 때문에.    
반면, 문자열 `'Glow'`와 `Glee'`는 복수의 문자로 이루어진 문자열이기 때문에, 아래와 같은 순서로 문자열 비교가 이루어진다.

1. `G`는 `G`와 같다.
2. `l`은 `l`와 같다.
3. `o`는 `e`보다 크기 때문에 여기서 비교가 종료되고, `o`가 있는 첫 번째 문자열인 `'Glow'`가 더 크다는 결론이 도출된다.

## 정확히는 사전 순이 아니라 유니코드 순이다.
자바스크립트의 문자열 비교 알고리즘은 사전이나 전화번호부에서 사용되는 정렬 알고리즘과 매우 유사하지만, 완전히 같지는 않다.

차이점 중 하나는 자바스크립트는 대,소문자를 따진다는 것인데. 대문자 `'A'`와 소문자 `'a'`를 비교했을 때 소문자 '`a'`가 더 크다. 자바스크립트 내부에서 사용되는 인코딩 표인 유니코드에선 소문자가 대문자보다 더 큰 인덱스를 갖기 때문이다.

## 다른 형을 가진 값 간의 비교
비교하려는 값의 자료형이 다르면 자바스크립트는 이 값들을 숫자형으로 바꾼다.

    alert( '2' > 1 ); // true, 문자열 '2'가 숫자 2로 변환된 후 비교가 진행됩니다.
    alert( '01' == 1 ); // true, 문자열 '01'이 숫자 1로 변환된 후 비교가 진행됩니다.
불린값의 경우 `true`는 `1`, `false`는 `0`으로 변환된 후 비교가 이뤄진다.

    alert( true == 1 ); // true
    alert( false == 0 ); // true

## 흥미로운 상황
동시에 일어나지 않을 법한 두 상황이 동시에 일어나는 경우도 있다.
- 동등 비교`(==)`시 `true`를 반환함
- 논리 평가 시 값 하나는 `true`, 다른 값 하나는 `false`를 반환함

    let a = 0;
    alert( Boolean(a) ); // false

    let b = "0";
    alert( Boolean(b) ); // true

    alert(a == b); // true!

두 값(a와b)를 비교하면 참이 반환되는데, 값을 논리 평가한 후 비교하면 하나는 참, 하나는 거짓이 반환된다는 점에 고개를 갸우뚱할 수도 있다. 그런데 자바스크립트 관점에선 이런 결과가 아주 자연스러운 현상이다. 동등 비교 연산자 `==`는 (예시에서 문자열 `"0"`을 숫자`0`으로 변환시킨 것처럼) 피연산자를 숫자형으로 바꾸지만, 'Boolean`을 사용한 명시적 변환에는 다른 규칙이 사용되기 때문이다.

## 일치 연산자
동등 연산자 `==`은 `0`과 `false`를 구별하지 못한다.

    alert( 0 == false ); // true
피연산자가 빈 문자열일 때도 같은 문제가 발생한다.

    alert( '' == false ); // true
이런 문제는 동등 연산자 `==`가 형이 다른 피연산자를 비교할 때 피연산자를 숫자형으로 바꾸기 때문에 발생한다. 빈 문자열과 `false`는 숫자형으로 변환하면 0이된다.

그렇다면 `0`과 `false`는 어떻게 구별할 수 있을까?

***일치 연산자 `===`를 사용하면 형 변환 없이 값을 비교할 수 있다.***

일치 연산자는 엄격한(strict) 동등 연산자이다. 자료형의 동등 여부까지 검사하기 때문에 피연산자 `a`와 `b`의 형이 다를 경우 `a === b`는 즉시 `false`를 반환한다.

    alert( 0 === false ); // false, 피연산자의 형이 다르기 때문입니다.
일치 연산자 `===`가 동등 연산자 `==`의 엄격한 버전인 것처럼 '불일치' 연산자 `!==`는 부등 연산자 `!=`의 엄격한 버전이다.

일치 연산자는 동등 연산자보다 한 글자 더 길긴 하지만 비교 결과가 명확하기 때문에 에러가 발생할 확률을 줄여준다.

## null이나 undefined와 비교하기
`null`이나 `undefined`를 다른 값과 비교할 땐 예상치 않은 일들이 발생한다. 일단 몇 가지 규칙을 먼저 살펴본 후, 어떤 예상치 않은 일들이 일어나는지 아래 예시를 통해 살펴보자.

**동등 연산자 `==`를 사용하여 `null`과 `undefined`를 비교**

동등 연산자를 사용해 `null`과 `undefined`를 비교하면 특별한 규칙이 적용돼 `true`가 반환된다. 동등 연산자는 `null`과 `undefined`를 '각별한 커플'처럼 취급한다. 두 값은 자기들끼리는 잘 어울리지만 다른 값들과는 잘 어울리지 못한다.

    alert( null == undefined ); // true

**산술 연산자나 기타 비교 연산자 `<`, `>`, `<=`, `>=`를 사용하여 `null`과 `undefined`를 비교**

`null`과 `undefined`는 숫자형으로 반환된다. `null`은 `0`, `undefined`는 `NaN`으로 변한다.

이제 위에서 살펴본 세 가지 규칙들이 어떤 흥미로운 에지 케이스(edge case)를 만들어내는지 알아보자. 이후, 어떻게 하면 에지 케이스가 만들어내는 함정에 빠지지 않을 수 있을지에 대해 알아보자.

## null vs 0

    alert( null > 0 );  // (1) false
    alert( null == 0 ); // (2) false
    alert( null >= 0 ); // (3) true

위 비교 결과는 논리에 맞지 않아 보인다. (3)에서 `null`은 `0`보다 크거나 같다고 했기 때문에 (1)이나 (2)중 하나는 참이어야 하는데 둘 다 거짓을 반환하고 있다.

이런 결과가 나타나는 이유는 동등 연산자 `==`와 기타 비교 연산자 `<`, `>`, `<=`, `>=`의 동작 방식이 다르기 때문입니다. (1)에서 `null > 0`이 거짓을, (3)에서 `null >= 0`이 참을 반환하는 이유는 (기타 비교 연산자의 동작 원리에 따라) `null`이 숫자형으로 변환돼 0이 되기 때문이다.

그런데 동등 연산자 `==`는 피연산자가 `undefined`나 `null`일 때 형 변환을 하지 않습니다. `undefined`와 `null`을 비교하는 경우에만 `true`를 반환하고, 그 이외의 경우(`null`이나 `undefined`를 다른 값과 비교할 때)는 무조건 `false`를 반환합니다. 이런 이유 때문에 (2)는 거짓을 반환한다.

## 함정 피하기
위와 같은 에지 케이스를 왜 살펴보았을까? 이런 예외적인 경우를 꼭 기억해 놓고 있어야만 할까? 그렇지는 않다. 개발을 하다 보면 자연스레 이런 경우를 만나고 점차 익숙해지기 때문에 지금 당장 암기해야 할 필요는 없다. 하지만 아래와 같은 방법을 사용해 이런 예외 상황을 미리 예방할 수 있다는 점은 알아두자.

## 요약 
- 비교 연산자는 불린값을 반환한다.
- 문자열은 문자 단위로 비교되는데, 이때 비교 기준은 '사전'순이다.
- 서로 다른 타입의 값을 비교할 땐 숫자형으로 형 변환이 이뤄지고 난 후 비교가 진행된다(일치 연산자는 제외).
- `null`과 `undefined`는 동등비교(`==`)시 서로 같지만 다른 값과는 같지 않다.
- `null`이나 `undefined`가 될 확률이 있는 변수가 `>`또는 `<`의 피연산자로 올 때는 주의를 기울자 `null`, `undefined` 여부를 확인하는 코드를 따로 추가하는 습관을 기르자.