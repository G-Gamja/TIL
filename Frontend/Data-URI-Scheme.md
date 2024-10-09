# Data URI Scheme

Data URI Scheme은 데이터를 URI(Uniform Resource Identifier)로 인코딩하여 포함하는 방법입니다. 이를 통해 외부 파일을 참조하지 않고도 데이터를 직접 포함할 수 있습니다. 주로 이미지, CSS, JavaScript 등의 리소스를 HTML이나 CSS 파일에 인라인으로 포함할 때 사용됩니다.

이미지 파일을 예로 들어보겠습니다.
보통 웹 페이지에서 이미지를 포함할 때는 이미지 파일의 경로를 지정합니다:

이 경우 브라우저는 image.png 파일을 서버에서 요청하여 가져옵니다.

```html
<img src="image.png" alt="Example Image" />
```

Data URI Scheme을 사용하면:
이미지 데이터를 Base64로 인코딩하여 URL 형식으로 직접 포함할 수 있습니다:

```html
<img
  src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUA
AAAFCAYAAACNbyblAAAAHElEQVQI12P4
//8/w38GIAXDIBKE0DHxgljNBAAO
9TXL0Y4OHwAAAABJRU5ErkJggg=="
  alt="Example Image"
/>
```

위의 예시에서 src 속성에 포함된 긴 문자열이 바로 인코딩된 이미지 데이터입니다. 이 데이터는 Base64로 인코딩된 PNG 이미지입니다.

### 형식

Data URI는 다음과 같은 형식을 가집니다:

```
data:[<MIME-type>][;charset=<encoding>][;base64],<data>
```

- `data:`: URI 스킴을 나타냅니다.
- `<MIME-type>`: 데이터의 MIME 타입을 지정합니다. 예를 들어, 이미지의 경우 `image/png`, 텍스트의 경우 `text/plain` 등을 사용합니다.
- `charset=<encoding>`: (선택 사항) 문자 인코딩을 지정합니다. 주로 `charset=UTF-8`을 사용합니다.
- `base64`: (선택 사항) 데이터가 Base64로 인코딩되었음을 나타냅니다.
- `<data>`: 실제 데이터입니다. Base64로 인코딩된 데이터일 수도 있고, 일반 텍스트일 수도 있습니다.

### 예시

#### 이미지

다음은 PNG 이미지를 Data URI로 인라인 포함하는 예시입니다:

```html
<img
  src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUA
AAAFCAYAAACNbyblAAAAHElEQVQI12P4
//8/w38GIAXDIBKE0DHxgljNBAAO
9TXL0Y4OHwAAAABJRU5ErkJggg=="
  alt="Red dot"
/>
```

#### CSS

CSS 파일에서 배경 이미지를 Data URI로 포함하는 예시입니다:

```css
body {
  background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUA
  AAAFCAYAAACNbyblAAAAHElEQVQI12P4
  //8/w38GIAXDIBKE0DHxgljNBAAO
  9TXL0Y4OHwAAAABJRU5ErkJggg==");
}
```

### 장점

- **네트워크 요청 감소**: 외부 파일을 요청하지 않으므로 네트워크 요청 수를 줄일 수 있습니다.
- **단일 파일 관리**: 모든 리소스를 하나의 파일에 포함할 수 있어 관리가 용이합니다.

### 단점

- **파일 크기 증가**: 인라인 데이터가 포함되므로 파일 크기가 커질 수 있습니다.
- **캐싱 불가**: 데이터가 인라인으로 포함되므로 브라우저 캐싱을 활용할 수 없습니다.

Data URI Scheme은 주로 작은 리소스를 인라인으로 포함하여 네트워크 요청을 줄이고, 페이지 로딩 속도를 개선하기 위해 사용됩니다. 그러나 큰 리소스를 포함하면 파일 크기가 커져 오히려 성능이 저하될 수 있으므로 주의가 필요합니다.
