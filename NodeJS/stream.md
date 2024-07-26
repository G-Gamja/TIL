### Node.js에서의 Stream

Node.js에서 Stream은 데이터의 흐름을 다루는 방식 중 하나입니다. 데이터를 조각으로 나누어 처리하기 때문에, 큰 데이터를 다룰 때 메모리 효율성과 시간 효율성을 높일 수 있습니다. Stream은 Node.js의 핵심 부분으로, 파일 시스템 작업, 네트워크 통신 등 다양한 곳에서 사용됩니다.

#### Stream의 주요 유형

1. **Readable Streams** - 데이터를 읽을 수 있는 Stream입니다. 예를 들어, 파일 시스템에서 파일을 읽거나 HTTP 요청의 본문을 읽는 경우에 사용됩니다.

2. **Writable Streams** - 데이터를 쓸 수 있는 Stream입니다. 파일에 데이터를 쓰거나, HTTP 응답의 본문을 작성할 때 사용됩니다.

3. **Duplex Streams** - 읽기와 쓰기가 모두 가능한 Stream입니다. TCP 소켓과 같은 네트워크 통신에서 주로 사용됩니다.

4. **Transform Streams** - 데이터를 읽고 변환한 후 쓸 수 있는 Stream입니다. 데이터 압축, 암호화 등의 작업에 사용됩니다.

#### Stream의 주요 이벤트

- `data` - Stream으로부터 데이터를 읽을 때 발생합니다.
- `end` - 더 이상 읽을 데이터가 없을 때 발생합니다.
- `error` - Stream에서 오류가 발생했을 때 발생합니다.
- `finish` - Writable Stream에 모든 데이터가 쓰여졌을 때 발생합니다.

#### Stream 사용 예제

```javascript
const fs = require("fs");
const readStream = fs.createReadStream("./example.txt");
const writeStream = fs.createWriteStream("./exampleCopy.txt");

// 데이터를 읽어서 바로 다른 파일에 쓰기
readStream.on("data", (chunk) => {
  writeStream.write(chunk);
});

// 읽기 완료 시
readStream.on("end", () => {
  console.log("Read Complete");
});

// 오류 처리
readStream.on("error", (err) => {
  console.error("An error occurred", err);
});
```

#### Stream의 장점

- **비동기 처리**: Node.js의 비동기 특성과 결합하여, I/O 작업을 효율적으로 처리할 수 있습니다.
- **메모리 효율성**: 큰 데이터를 한 번에 메모리에 올리지 않고, 조각으로 나누어 처리하기 때문에 메모리 사용을 최적화할 수 있습니다.
- **시간 효율성**: 데이터를 조각으로 나누어 바로 처리할 수 있기 때문에, 전체 데이터를 처리하기 전에 작업을 시작할 수 있어 시간을 절약할 수 있습니다.

Node.js에서 Stream을 사용하면, 대규모 데이터 처리 작업을 더 효율적으로 관리할 수 있습니다.

Stream Piping
pipe 메서드를 사용하면 여러 스트림을 연결하여 데이터를 쉽게 전달할 수 있습니다. 예를 들어, 읽기 스트림에서 쓰기 스트림으로 데이터를 파이핑할 수 있습니다.

```js
const readStream = fs.createReadStream("./example.txt");
const writeStream = fs.createWriteStream("./exampleCopy.txt");

readStream.pipe(writeStream);
```

Backpressure Handling
스트림에서 역압(backpressure)은 소비자가 데이터를 충분히 빨리 처리하지 못할 때 발생합니다. 이를 처리하기 위해 pause()와 resume() 메서드를 사용할 수 있습니다.

```js
const readStream = fs.createReadStream("./example.txt");

readStream.on("data", (chunk) => {
  if (!writeStream.write(chunk)) {
    readStream.pause();
  }
});

writeStream.on("drain", () => {
  readStream.resume();
});
```

Custom Streams
기본 스트림 클래스를 확장하여 사용자 정의 스트림을 만들 수 있습니다. 예를 들어, Transform 스트림을 확장하여 데이터를 대문자로 변환하는 스트림을 만들 수 있습니다.

```js
const { Transform } = require("stream");

class UpperCaseTransform extends Transform {
\_transform(chunk, encoding, callback) {
this.push(chunk.toString().toUpperCase());
callback();
}
}

const upperCaseTransform = new UpperCaseTransform();
process.stdin.pipe(upperCaseTransform).pipe(process.stdout);
```

Stream Methods
스트림 인스턴스에서 사용할 수 있는 중요한 메서드는 다음과 같습니다:

pipe(): 스트림을 다른 스트림에 연결합니다.
unpipe(): 스트림의 연결을 해제합니다.
read(): 데이터를 읽습니다.
write(): 데이터를 씁니다.
end(): 스트림을 종료합니다.
pause(): 스트림을 일시 중지합니다.
resume(): 일시 중지된 스트림을 다시 시작합니다.
이러한 메서드를 사용하면 스트림을 더욱 유연하고 강력하게 사용할 수 있습니다.
