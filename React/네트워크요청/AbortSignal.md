# AbortSignal

관련 문서: https://developer.mozilla.org/ko/docs/Web/API/AbortSignal

예시 코드

```ts
export async function get<T>(URL: string) {
  const response = await fetch(URL, {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
      Cosmostation: `extension/${String(process.env.VERSION)}`,
    },
    signal: AbortSignal.timeout(5000),
  });

  if (!response.ok) {
    throw new FetchError(response);
  }

  const responseJSON = (await response.json()) as unknown as T;

  return responseJSON;
}
```

```ts
export async function get<T>(URL: string) {
  const timeout = 5000; // 5 seconds
  const controller = new AbortController();
  const reason = new DOMException("signal timed out", "TimeoutError");
  const timeoutId = setTimeout(() => controller.abort(reason), timeout);

  const response = await fetch(URL, {
    method: "GET",
    headers: {
      "Content-Type": "application/json",
      Cosmostation: `extension/${String(process.env.VERSION)}`,
    },
  });

  clearTimeout(timeoutId);

  if (!response.ok) {
    throw new FetchError(response);
  }

  const responseJSON = (await response.json()) as unknown as T;

  return responseJSON;
}
// 이 코드에는 fetch에서 에러를 던지면 다음 줄의 claer가 작동을 안해서 setTimeOute이 백그라운드에서 계속 돌아가게 된다.

// 그래서 아래와 같이 수정해야 한다.
export async function get<T>(URL: string) {
  const timeout = 5000; // 5 seconds
  const controller = new AbortController();
  const reason = new DOMException(
    `signal timed out (${timeout}ms)`,
    "TimeoutError"
  );
  const timeoutId = setTimeout(() => controller.abort(reason), timeout);

  try {
    const response = await fetch(URL, {
      method: "GET",
      headers: {
        "Content-Type": "application/json",
        Cosmostation: `extension/${String(process.env.VERSION)}`,
      },
      signal: controller.signal,
    });

    if (!response.ok) {
      throw new FetchError(response);
    }

    const responseJSON = (await response.json()) as unknown as T;

    return responseJSON;
  } catch (error) {
    clearTimeout(timeoutId);
    throw error;
  } finally {
    clearTimeout(timeoutId);
  }
}
```

```ts

```
