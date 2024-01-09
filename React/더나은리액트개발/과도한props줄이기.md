https://kyleshevlin.com/quit-your-yapping

```jsx
type Action = {
  onClick: () => void
  text: string
}

/**
 * Bonus tip: I prefer to write keys with adjectives
 * in noun-adjective order. That way, all the like keys
 * are grouped together when I sort them alphabetically
 */
type CardProps = {
  actionLeft?: Action
  actionRight?: Action
  body?: string
  title?: string
}

function Card({ actionLeft, actionRight, body, title }: CardProps) {
  const hasAction = actionLeft || actionRight

  return (
    <div>
      <div>
        {title && <h4>{title}</h4>}
        {body && <div>{body}</div>}
      </div>

      {hasAction && (
        <div style={{ display: 'flex', gap: '1rem' }}>
          {actionLeft && <CardAction action={actionLeft} />}
          {actionRight && <CardAction action={actionRight} />}
        </div>
      )}
    </div>
  )
}

function CardAction({ action }: { action: Action }) {
  return (
    <button
      onClick={action.onClick}
      type="button"
      style={{
        flex: '1 1 auto',
      }}
    >
      {action.text}
    </button>
  )
}
```

여기서 버튼이 하나 더 추가가된다면 굉장히 이상해진다

composition을 활용하자

# 1. Children활용하기

```jsx
function Card({ body, children, title }) {
  return (
    <div>
      <div>
        {title && <h4>{title}</h4>}
        {body && <div>{body}</div>}
      </div>

      {children && <div>{children}</div>}
    </div>
  );
}

function ExampleWithChildren() {
  return (
    <Card body="Here is an example body text." title="Example Title">
      <Flex justify="space-between">
        <button onClick={() => {}} type="button">
          Click me
        </button>

        <button onClick={() => {}} type="button">
          Click me, too
        </button>
      </Flex>
    </Card>
  );
}
```

하지만 이 방식은 children으로 넘겨지는 props가 실제 어디에서 어떻게 동작되고 배치되는지 알 수 없다.

이런건 간단한 텍스트를 넘겨주는게 아니라면 좋지 않다.

# Using Slots !! (Composition Pattern)

그냥 컴포넌트 자체를 props로 넘겨주는 방식

이러면 컴포넌트가 어떻게 동작하는지 알 수 있도록 props키 명을 사용할 수 있다.

```jsx
function Card({ body, footer, header, title }) {
  return (
    <div>
      {header && <div>{header}</div>}

      <div>
        {title && <h4>{title}</h4>}
        {body && <div>{body}</div>}
      </div>

      {footer && <div>{footer}</div>}
    </div>
  );
}

function ExampleWithSlots() {
  const header = <Flex justify="center">EXAMPLE</Flex>;

  const footer = (
    <Flex justify="space-between">
      <button onClick={() => {}} type="button">
        Click me
      </button>

      <button onClick={() => {}} type="button">
        Click me, too
      </button>
    </Flex>
  );

  return (
    <Card
      body="Here is an example body text."
      header={header}
      footer={footer}
      title="Example Title"
    />
  );
}
```
