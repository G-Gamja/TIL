ESM은 es6의 모듈 시스템을 의미한다.

ESM의 간단한 문법은 다음과 같다

```
//file1.js
export default function func1(){}
exprot function func2(){}
export const variable1 = 123;
export let variable2='hello';

//file2.js
import myFunc1,{func2,variable1,variable2} from './file1.js'

//file3.js
import {func2 as myFunc2} from './file1.js'


```
* default 키워드는 한 파일에서 한 번만 사용 가능
    * 이렇게 내보내진 코드는 괄호없이 가져올 수 있고, 이름은 원하는 대로 정할 수 있다.
    * func1(){}이 myFunc1으로 가져온 것이다.
* default없이 가져와진 코드는 괄호와 함께 쓰여야 한다
    * 이름을 그대로 가져와야 하며 원한다면 as 키워드와 함께 이름을 변경할 수 있다.