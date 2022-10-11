 ```typescript
 decimals: Joi.string()
      .optional()
      .allow('')// 빈칸이면 인자로 넘겨진 값을 디폴트로 설정함
      .messages({
        'string.base': t('schema.common.string.base'),
        'string.empty': t('schema.common.string.empty'),
      }),

chainId: Joi.string()
      .required()
      // 입력값을 강제로 소문자로 변형한 후 저장함
      .lowercase()
      // .invalid(...officialCosmosLowercaseChainIds)
      .messages({
        'string.base': t('schema.common.string.base'),
        'string.empty': t('schema.common.string.empty'),
        'string.lowecase': t('schema.common.string.invalid'),
      }),

       sendGas: Joi.string()
      .optional()
      .empty('') // 빈 칸이라면 undefined 값을 넘겨준다
      .pattern(/^[0-9]+$/)
      .messages({
        'string.pattern.base': t('schema.common.number.base'),
      }),
```
string.base: 문자 대신 다른게 입력되었을때

any.optional()  
Marks a key as optional which will allow undefined as values. Used to annotate the schema for readability as all keys are optional by default.

Note: this does not allow a null value. To do that, use any.allow(value). Or both!

const schema = Joi.any().optional();

## .label
일반 에러메시지의 context부분을 인자로 오버라이딩함

https://stackoverflow.com/questions/66821398/joi-label-what-does-this-method-of-joi-schema-validation-do

## .default
To specify a default value in case of the empty string use:
```typescript
Joi.string()
    .empty('')
    .default('default value');
```
## empty
empty  
empty matches an empty schema:

let schema = Joi.string().empty('');  
This will consider an empty string to be valid.

And if we have:

let schema = Joi.string().empty();  
then we match undefined .

### 공부해야하는 것들?


  // TODO 인풋 순서 조정 필요
  // TODO 로컬라이징 텍스트 수정 필요
  // TODO joi 좀 더 공부해봐야 할 듯,
  // .label('params')
  // .required(); 원래 있던 스키마의 뒤에 붙어있던 메서드인데 무슨 의미인지 모르겠음
  // 그냥 스키마에 있는 모든 메서드 다 이해하기
  // 1순위 Joi공부해

  // FIXME gasRate부분을 어떻게 인풋을 받아야할지 모르겠음, 그냥 받는거는 타입이 GasRate가 아니고 나눠서 받자니
  // CosAddChainParams에서 gasRate필드에서 타입을 gasRate로 받아서 쪼개서 넣을 수가 없음(인풋이 들어온게 파람타입 필드 참조해서 넣으니깐...gasRate타입으로 넣어야하는거여)

  // FIXME 기존에 존재하는 chain의 chainID를 입력 시 "chainId" contains an invalid value
  // 라는 헬퍼 텍스트가 출력되는데 원래는 위 onsubmit함수에서 스낵바를 띄워야하는데 아마
  // 스키마에서 invalid를 설정해줘서 그런거같음
  // sol: 1.useSchema의 invalid를 삭제 => 모든 인풋들이 invalid한 뒤에 submit함수가 발동되니
  // 인풋들이 검증을 통과한다 -> onsubmit함수가 걸리면서 중복 체크를 하고 스낵바를 띄운다
  // 근데 스키마에서 invalid 조건을 작성하면 인풋 검증 과정에서 부적합판정 뒤 헬퍼 텍스트가 출력되는거임

-------------------------------------
### useSchema  

  // FIXME 현재 옵션값 미 입력시 undefine이 아닌 ''가 입력되어서 newChain할당시에 값이 ''가 들어감
  // -> .empty('')  엠티 필드를 오버라이딩함

  // FIXME 가스 비율 인풋을 아예 비우거나 하나를 입력했을때는 모두 입력하도록 걸고 싶음
  // -> .and() 로 해결한 듯
  // FIXME 1개만 입력시 에러는 나는데 에러 메시지가 안나옴

  // FIXME 가스레이트 값 미입력시 undefine으로 넘어가지 않음...

  // TODO 가스 레이트에도 메시지 달아주기