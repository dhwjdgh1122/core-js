## 재귀와 스택
재귀는 큰 목표 작업 하나를 동일하면서 간단한 작업 여러 개로 나눌 수 있을 떄 유용한 프로그래밍 패넡이다. 목표 작업을 간단한 동작 하나와 목표 작업을 변형한 작업으로 단순화시킬 수 있을 때도 재귀를 사용할 수 있다. 특정 자료구조를 다뤄야 할 때도 재귀가 사용된다.

문제 해결을 하다 보면 함수에서 다른 함수를 호출해야 할 때가 있다. 이때 함수가 ***자기 자신***을 호출할 수도 있는데, 이를 **재귀**라고 부른다.

## 두 가지 사고방식
간단한 예시를 시작으로 재귀에 대해 알아보자. `x`를 `n`제곱해 주는 함수 `pow(x, n)`을 만들어보자.    
`pow(x, n)`는 `x`를 `n`번 곱해주기 때문에 아래 결과를 만족해야 한다.

    pow(2, 2) = 4
    pow(2, 3) = 8
    pow(2, 4) = 16

구현하는 방법 두 가지가 있다.

1. 반복적인 사고를 통한 `for` 루프

    function pow(x, n) {
      let result = 1;

      // 반복문을 돌면서 x를 n번 곱함
      for (let i = 0; i < n; i++) {
        result *= x;
      }

      return result;
    }

    alert( pow(2, 3) ); // 8

2. 재귀적인 사고를 통한 방법: 작업을 단순화하고 자기 자신을 호출함

    function pow(x, n) {
      if (n == 1) {
        return x;
      } else {
        return x * pow(x, n - 1);
      }
    }

    alert( pow(2, 3) ); // 8

재귀를 이용한 예시가 반복문을 사용한 예시와 어떤 부분에서 근본적인 차이가 있는지 살펴보자.    
`pox (x, n)`을 호출하면 아래와 같이 두 갈래로 나뉘어 코드가 실행된다.

                  if n==1  = x
                /
    pow(x, n) =
                \
                  else     = x * pow(x, n - 1)

1. `n == 1`일 때: 모든 절차가 간단해진다. 명확한 결과값을 즉시 도출하므로 이를 ***재귀의 베이스***라고 한다. `pox(x, 1)`는 `x`이다.

2. `n == 1`이 아닐 때, `pox(x, n)`은 `x * pow(x, n - 1)`으로 표현할 수 있다. 수학식으론 `x^n = x * x^(n-1)`로 표현할 수 있다. 이를 재귀 단계(recursive step) 라고 부른다. 여기선 목표 작업 `pow(x, n)`을 간단한 동작(x를 곱하기)과 목표 작업을 변형한 작업`(pow(x, n - 1))`으로 분할하였다. 재귀 단계는 `n`이 `1`이 될 때까지 계속 이어집니다.

즉, `pox`는 `n == 1`이 될 때까지 ***재귀적***으로 자신을 호출한다.

pow (2, 4)를 계산하려면 아래와 같은 재귀 단계가 차례대로 이어진다.

1. `pow(2, 4) = 2 * pow(2, 3)`
2. `pow(2, 3) = 2 * pow(2, 2)`
3. `pow(2, 2) = 2 * pow(2, 1)`
4. `pow(2, 1) = 2`

이렇게 재귀를 이용하면, 함수 호출의 결과가 명확해질 때까지 함수 호출을 더 간단한 함수 호출로 계속 줄일 수 있다.

---
재귀를 사용한 코드는 짧다.    
재귀를 사용한 코드는 반복적 사고에 근거하여 작성한 코드보다 대개 짧다.

`if`대신 조건부 연산자 `?`를 사용하면 `pox (x, n)을 더 간결하고 읽기 쉽게 만들 수도 있다.

function pow(x, n) {
  return (n == 1) ? x : (x * pow(x, n - 1));
}

가장 처음 하는 호출을 포함한 중첩 호출의 최대 개수는 ***재귀 깊이***라고 한다. `pox(x, n)`의 재귀 깊이는 `n`이다.

자바스크립트 엔진은 최대 재귀 깊이를 제한한다. 만개 정도까진 확실히 허용하고, 엔진에 따라 이보다 더 많은 깊이를 허용하는 경우도 있다. 하지만 대다수의 엔진이 십만까지는 다루지 못한다 이런 제한을 완화하려고 엔진 내부에서 자동으로 'tail calls optimization'라는 최적화를 수행하긴 하지만, 모든 곳에 적용되는 것은 아니고 간단한 경우에만 적용된다.

재귀 깊이 제한 때문에 재귀를 실제 적용하는데 제약이 있긴 하지만, 재귀는 여전히 광범위하게 사용되고 있다. 재귀를 사용하면, 간결하고 유지보수가 쉬운 코드를 만들 수 있기 때문이다.

## 실행 컨텍스트와 스택

실제 재귀 호출이 어떻게 동작하는지 알아보자. 이를 위해서 함수의 내부 동작에 대해 살펴보도록 하자.

실행 중인 함수의 실행 절차에 대한 정보는 해당 함수의 ***실행 컨텍스트***에 저장된다.

***실행 컨텍스트***는 함수 실행에 대한 세부 정보를 담고 있는 내부 데이터 구조이다. 제어 흐름의 현재 위치, 변수의 현재 값, `this`의 값(여기선 다루지 않음)등 상세 내부 정보가 실행 컨텍스트에 저장된다.

함수 호출 일 회당 정확히 하나의 실행 컨텍스트가 생성된다.

함수 내부에 중첩 호출이 있을 때는, 아래와 같은 절차가 수행된다.

- 현재 함수의 실행이 일시 중지된다.
- 중지된 함수와 연관된 실행 컨텍스트는 ***실행 컨텍스트 스택***이라는 특별한 자료 구조에 저장된다.
- 중첩 호출이 실행된다.
- 중첩 호출 실행이 끝난 이후 실행 컨텍스트 스택에서 일시 중단한 함수의 실행 컨텍스트를 꺼내오고, 중단한 함수의 실행을 다시 이어간다.

이제 `pox (2, 3)이 호출되면 실행 컨텍스트에서 무슨 일이 일어나는지 살펴보자.

## pox(2, 3)
`pox(2, 3)`을 호출하는 순간, 실행 컨텍스트엔 변수 `x = 2, n = 3`이 저장되고 , 실행 흐름은 함수의 첫 번째 줄에 위치한다.

이를 도식화하면 아래와 같다.

- Context: { x: 2, n: 3, 첫 번째 줄} call: pox(2, 3)

위 코드는 함수 실행이 시작되는 순간을 나타낸 것이다. 지금 상태론 조건 `n == 1`을 만족하지 못하므로 실행 흐름은 `if`의 두 번째 분기로 넘어간다.

    function pow(x, n) {
      if (n == 1) {
        return x;
      } else {
        return x * pow(x, n - 1);
      }
    }

    alert( pow(2, 3) );
변수는 동일하지만, 실행 흐름의 위치가 변경되면서 실행 컨텍스트도 다음과 같이 변경된다.

- Context: { x: 2, n: 3, 다섯 번째 줄} call:pox(2, 3)

`x * pow (x, n - 1)`을 계산하려면 새로운 인수가 들어가는 `pox`의 서브 호출(subcall), `pox (2, 2)을 만들어야 한다.

## pox(2, 2)
중첩 호출을 하기 위해, 자바스크립트는 ***실행 컨텍스트 스택***에 현재 실행 컨텍스트를 저장한다.

지금 보고 있는 예시에선 실행 컨텍스트 스택에 동일한 함수 `pox`를 호출하였는데, 이는 중요하지 않다. 모든 함수에 대해 아래 프로세스가 똑같이 적용된다.

1. 스택 최상단에 현재 컨텍스트가 '기록'된다.
2. 서브 호출을 위한 새로운 컨텍스트가 만들어진다.
3. 서브 호출이 완료되면, 기존 컨텍스트를 스택에서 꺼내(pop) 실행을 이어간다.

다음은 서브 호출 `pox(2, 2)`이 시작될 때의 실행 컨텍스트 스택이다.

- **Context: { x: 2, n: 2, 첫 번째 줄}** call: pow(2, 2)
- Context: { x: 2, n: 3, 다섯 번째 줄} call pow(2, 3)

굵은 테두리로 표시한 새 실행 컨텍스트는 상단에, 기존 컨텍스트는 하단에 있다.

이전 컨텍스트에 변수 정보, 코드가 일시 중단된 줄에 대한 정보가 저장되어있기 때문에 서브 호출이 끝났을 때 이전 컨텍스트가 문제없이 다시 시작된다.

## pow(2, 1)
동일한 과정이 다시 반복된다. 다섯 번째 줄에서 인수 `x = 2`, `n = 1`과 함께 새로운 서브 호출이 만들어진다.

새로운 실행 컨텍스트가 만들어지고,이전 실행 컨텍스트는 스택 최상단에 올라간다(push)

- **Context: {x: 2, n: 1, 첫 번째 줄}** call: pox(2, 1)
- Context: {x: 2, n: 2, 다섯 번째 줄} call: pox(2, 2)
- Context: {x: 2, n: 3, 다섯 번째 줄} call: pox(2, 3)

기존 컨텍스트 두 개가 밑에, `pox(2, 1)에 상응하는 컨텍스트가 맨 위에 있는 것을 확인할 수 있다.

## 실행 종료

`pow(2, 1)`가 실행될 땐 상황이 달라진다. 이전과는 달리 조건 `n == 1`을 만족시키므로 `if`문의 첫 번째 분기가 실행된다.

    function pow(x, n) {
      if (n == 1) {
        return x;
      } else {
        return x * pow(x, n - 1);
      }
    }

이젠 호출해야 할 중첩 호출이 없다. 따라서 함수는 종료되고 `2`가 반환된다.

함수가 종료되었기 때문에 이에 상응하는 실행 컨텍스트는 쓸모가 없어졌다. 따라서 해당 실행 컨텍스트는 메모리에서 삭제된다. 스택 맨 위엔 이전의 실행 컨텍스가 위치하게 된다.
- **Context: {x: 2, n: 2, 다섯 번째 줄}** call: pow(@, 2)
- Context: {x: 2, n: 3, 다섯 번째 줄} call: pow(2, 3)

`pow(2, 2)`의 실행이 다시 시작된다. 서브 호출 `pow(2, 1)`의 결과를 알고 있으므로, 쉽게 `x * pow(x, n -1)`을 계산해 `4`를 반환한다.    

그리고 다시 이전 컨텍스트가 스택 최상단에 위치하게 된다.

**Context: {x: 2, n: 3, 다섯 번째 줄}** call pow(2, 3)

마지막 실행 컨텍스트까지 처리되면 `pow(2, 3) = 8`이라는 결과가 도출된다.

지금 예시의 재귀 깊이는 **3**이다.

재귀 깊이는 스택에 들어가는 실행 컨텍스트 수의 최댓값과 같다.

실행 컨텍스트는 메모리를 차지하므로 재귀를 사용할 땐 메모리 요구사항에 유의해야 한다. `n`을 늘리면 `n`이 줄어들 때마다 만들어지는 `n`개의 실행 컨텍스트가 저장될 메모리 공간이 필요하기 때문이다.

한편, 반복문 기반 알고리즘을 사용하면 메모리가 절약된다.

    function pow(x, n) {
      let result = 1;

      for (let i = 0; i < n; i++) {
        result *= x;
      }

      return result;
    }
반복을 사용해 만든 함수 `pow`는 컨텍스트를 하나만 사용한다. 이 컨텍스트에서 `i`와 `result`가 변경된다. 실행 컨텍스트가 하나이기 때문에 `n`에 의존적이지 않고, 필요한 메모리가 적다. 사용 메모리 공간도 고정된다.

**재귀를 이용해 작성한 코드는 반복문을 사용한 코드로 다시 작성할 수 있다. 반복문을 사용하면 대개 함수 호출의 비용(메모리 사용)이 절약된다.

하지만 코드를 다시 작성해도 큰 개선이 없는 경우가 있다. 조건에 따라 함수가 다른 재귀 서브 호출을 하고 그 결과를 합칠 때가 그렇다. 분기문이 복잡하게 얽혀있을 때도 메모리가 크게 절약되지 않는다. 이런 경우엔 최적화가 필요하지 않을 수 있고 최적화에 드는 노력이 무용지물일 수 있다.

재귀를 사용하면 코드가 짧아지고 코드 이해도가 높아지며 유지보수에도 이점이 있다. 모든 곳에서 메모리 최적화를 신경 써서 코드를 작성해야 하는 것은 아니다. 우리가 필요한 것은 좋은 코드이다. 이런 이유 때문에 재귀를 사용한다!!

## 재귀적 순회
재귀는 재귀적 순회를 구현할 때 사용하면 좋다.

한 회사가 있다고 가정하고. 임직원을 아래와 같이 객체로 표현해 보자

    let company = {
      sales: [{
        name: 'John',
        salary: 1000
      }, {
        name: 'Alice',
        salary: 1600
      }],

      development: {
        sites: [{
          name: 'Peter',
          salary: 2000
        }, {
          name: 'Alex',
          salary: 1800
        }],

        internals: [{
          name: 'Jack',
          salary: 1300
        }]
      }
    };

회사엔 부서가 있다.
- 부서에는 여러 명의 직원이 이를 배열로 표현할 수 있다. `sales`부서의 John과 Alice라는 2명의 직원을 배열 요소로 표현했고,
- 부서는 하위 부서를 가질 수 있다.` development`부서는 `sites`와 `internals`라는 두 개의 하위 부서를 갖는다. 각 하위부서에도 직원이 있다.
- 하위 부서가 커지면 더 작은 단위의 하위 부서(또는 팀)으로 쪼개질 가능성도 있다.    
`sites`부서는 미래에 `siteA`와 `siteB`로 나뉠 수 있다. 이렇게 나눠진 부서가 미래에 더 세분화될 수도 있다. 미래에 벌어질 일까진 나타내지 않았지만, 이러한 가능성도 있다는 걸 염두에 두어야 한다.

이제 모든 임직원들의 급여를 더한 값을 구해야 한다고 가정하자, 어떻게?

구조가 단순하지 않기 때문에 반복문을 사용해선 구하기 쉽지 않아 보인다. 가장 먼저 떠오르는 생각은 `company`를 대상으로 동작하는 `for` 반복문을 만들고 한 단계 아래의 부서에 중첩 반복문을 돌리는 것이다. 그런데 이렇게 하면 `sites` 같은 두 단계 아래의 부서에 속한 임직원의 급여를 뽑아낼 때 또 다른 중첩 반복문이 필요하다. 세 단계 아래의 부서가 미래에 만들어진다고 가정하면 또 다른 중첩 반복문이 필요하다. 얼마만큼의 깊이까지 중첩 반복문을 만들 수 있을까? 객체를 순회하는 중첩 반복문의 깊이가 3~4개가 되는 순간 코드는 정말 지저분해질 것이다.

재귀를 이용한 풀이법을 시도해보자.

앞에 본 바와 같이 임직원 급여 합계를 구할 때는 두 가지 경우로 나누어 생각할 수 있다.

1. 임직원 배열을 가진 '단순한' 부서-간단한 반복문으로 급여 합계를 구할 수 있다.
2. `N`개의 하위 부서가 있는 객체-각 하위 부서에 속한 임직원의 급여 합계를 얻기 위해 `N`번의 재귀 호출을 하고, 최종적으로 모든 하위부서 임직원의 급여를 더한다.

배열을 사용하는 첫 번째 경우는 간단한 경우로, 재귀의 베이스가 된다.

객체를 사용하는 두 번째 경우는 재귀 단계가 된다. 복잡한 작업은 작은 작업(하위 부서에 대한 반복문)으로 쪼갤 수 있다. 부서의 깊이에 따라 더 작은 작업으로 쪼갤 수 있는데, 결국 마지막엔 첫 번째 경우가 된다.

코드를 보고 재귀 알고리즘일 이해해보자.

    let company = { // 동일한 객체(간결성을 위해 약간 압축함)
      sales: [{name: 'John', salary: 1000}, {name: 'Alice', salary: 1600 }],
      development: {
        sites: [{name: 'Peter', salary: 2000}, {name: 'Alex', salary: 1800 }],
        internals: [{name: 'Jack', salary: 1300}]
      }
    };

    // 급여 합계를 구해주는 함수
    function sumSalaries(department) {
      if (Array.isArray(department)) { // 첫 번째 경우
        return department.reduce((prev, current) => prev + current.salary, 0); // 배열의 요소를 합함
      } else { // 두 번째 경우
        let sum = 0;
        for (let subdep of Object.values(department)) {
          sum += sumSalaries(subdep); // 재귀 호출로 각 하위 부서 임직원의 급여 총합을 구함
        }
        return sum;
      }
    }

    alert(sumSalaries(company)); // 7700

재귀의 강력함!! 하위 부서의 깊이와 상관없이 원하는 값을 구할 수 있음

객체 `{...}`를 만나면 서브 호출이 만들어지는 반면, 배열 `[...]`을 만나면 더 이상의 서브 호출이 만들어지지 않고 결과가 바로 계산된다.

- 배열과 메서드 챕터에서 학습한 메서드 `arr.reduce`는 배열의 합을 계산해준다.
- `for(val of Object.values (obj))`에서 쓰인 `Object.values`는 프로퍼티의 값이 담긴 배열을 반환한다.

## 재귀적 구조
재귀적으로 정의된 자료구조인 재귀적 자료 구조는 자기 자신의 일부를 복제하는 형태의 자료 구조이다.    
위에서 살펴본 회사 구조 역시 재귀적 자료 구조 형태이다.

회사의 부서 객체는 두 가지 종류로 나뉜다.
- 사람들로 구성된 배열
- 하위 부서로 이루어진 객체

웹 개발자에게 익숙한 HTML XML도 재귀적 자료 구조 형태이다.
HTML 문서에서 html태그는 아래와 같은 항목으로 구성되기 때문이다.
- 일반 텍스트
- HTML 주석
- 이 외의 HTML 태그

다음은 '연결 리스트'라는 재귀적 자료 구조를 살펴보자

## 연결 리스트
객체를 정렬하여 어딘가에 저장하고 싶다고 가정하자    
가장 먼저 떠오르는 자료 구조는 배열이다.

    let arr = [obj1, obj2, obj3];
하지만 배열은 요소 '삭제'와 '삽입'에 들어가는 비용이 많이 든다는 문제가 있다. `arr.unshift(obj)` 연산을 수행하려면 새로운 `obj`를 위한 공간을 만들기 위해 모든 요소의 번호를 다시 매겨야 한다. 배열이 커지면 연산 수행 시간이 더 걸리게 된다. `arr.shift()`를 사용할 때도 마찬가지다.

요소 전체의 번호를 다시 매기지 않아도 되는 조작은 배열 끝에 하는 연산인 `arr.push/pop`뿐이다 앞쪽 요소에 무언가를 할 때 배열은 이처럼 꽤 느리다.

빠르게 삽입 혹은 삭제를 해야 할 때는 배열 대신 **연결 리스트**라 불리는 자료 구조를 사용할 수 있다.

***연결 리스트***의 요소는 객체와 아래 프로퍼티들을 조합해 정의할 수 있다.
- `value`
- `next`: 다음 연결 리스트 요소를 참조하는 프로퍼티, 다음 요소가 없을 땐 `null`이 된다

      let list = {
        value: 1,
        next: {
          value: 2,
          next: {
            value: 3,
            next: {
              value: 4,
              next: null
            }
          }
        }
      };
아래처럼 코드를 작성해도 동일한 연결 리스트가 된다.

    let list = { value: 1 };
    list.next = { value: 2 };
    list.next.next = { value: 3 };
    list.next.next.next = { value: 4 };
    list.next.next.next.next = null;
이렇게 연결 리스트를 만들면 객체가 여러개 있꼬, 각 객체엔 `value`와 이웃 객체를 가리키는 프로퍼티인 `next`가 있는게 명확히 보인다. 체인의 시작 객체는 변수 `list`에 저장되어 있다. 우리는 `list`의 `next` 프로퍼티를 이용해 이어지는 객체 어디든 도달할 수 있다.

연결 리스트를 사용하면 전체 리스트를 여러 부분으로 쉽게 나눌 수 있고, 다시 합치는 것도 가능하다.

    let secondList = list.next.next;
    list.next.next = null;

합치기:

    list.next.next = secondList;

그리고 쉽게 요소를 추가하거나 삭제할 수 있다.    
리스트의 처음 객체를 바꾸면 리스트 맨 앞에 새로운 값을 추가 가능

    let list = { value: 1 };
    list.next = { value: 2 };
    list.next.next = { value: 3 };
    list.next.next.next = { value: 4 };

    // list에 새로운 value를 추가합니다.
    list = { value: "new item", next: list };

중간 요소를 제거하려면 이전 요소의 `next`를 변경하면 된다.

    list.next = list.next.next;

`list.next`가 `1`이 아닌 `2`를 `value`로 갖는 객체를 가리키게 만들었다. 이제 `value` `1`은 체인에서 제외된다. 이 객체는 다른 곳에 따로 저장하지 않으면 자동으로 메모리에서 제거된다.

지금까지 살펴본 바와 같이 연결 리스트는 배열과는 달리 대량으로 요소 번호를 재할당하지 않으므로 요소를 쉽게 재배열할 수 있다는 장점이 있다.

물론 연결 리스트가 항상 배열보다 우월하지는 않다.

연결 리스트의 가장 큰 단점은 번호(인덱스)만 사용해 요소에 접근할 수 없다는 점이다. 배열을 사용하면 `arr[n]`처럼 번호 `n`만으로도 원하는 요소에 바로 접근할 수 있다. 그러나 연결 리스트에선 `N`번째 값을 얻기 위해 첫 번째 항목부터 시작해 `N`번 `next`로 이동해야 한다.

그런데 중간에 요소를 삽입하거나 삭제하는 연산이 항상 필요한 것은 아니다. 이럴 땐 순서가 있는 자료형 중에 큐(queue)나 데크(deque)를 사용할 수 있다. 데크를 사용하면 양 끝에서 삽입과 삭제를 빠르게 수행할 수 있다.

위에서 구현한 연결 리스트는 아래와 같은 기능을 더해 개선할 수 있음.
- 이전 요소를 참조하는 프로퍼티 `prev`를 추가해 이전 요소로 쉽게 이동하게 할 수 있다.
- 리스트의 마지막 요소를 참조하는 변수 `tail`룰 추가할 수 있다. 다만 이때 주의할 점은 리스트 마지막에 요소를 추가하거나 삭제할 때 `tail`도 갱신해 줘야 한다.

## 요약

- 재귀 - 함수 내부에서 자기 자신을 호출하는 것을 나타내는 프로그래밍 용어이다.    
함수가 자신을 호출하는 단계를 재귀 단계라고 부른다. basic라고도 불리는 재귀의 베이스는 작업을 아주 간단하게 만들어서 함수가 더 이상은 서브 호출을 만들지 않게 해주는 인수이다.
- 재귀적으로 정의된 자료 구조는 자기 자신을 이용해 자료 구조를 정의한다.    
재귀적으로 정의된 자료 구조에 속하는 연결 리스트 혹은 null을 참조하는 객체로 이루어진 데이터 구조를 사용해 정의된다.