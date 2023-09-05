```ts
"no-misused-promises": "error",
    onErrorRetry: (__, ___, _, revalidate, { retryCount }) => {
      if (retryCount >= 6) return;
      if (retryCount === 5) {
        setHasTimedOut(true);
      }
      // Wrong
      setTimeout(() => revalidate({ retryCount }), 5000);
    },

=======================
"no-misused-promises": "error",
        onErrorRetry: (_, __, ___, revalidate, { retryCount }) => {
      if (retryCount >= 6) return;

      if (retryCount === 5) {
        setHasTimedOut(true);
      }
      // Fine
      setTimeout(() => {
        void revalidate({ retryCount });
      }, 5000);
    },

```

참고: https://github.com/typescript-eslint/typescript-eslint/blob/main/packages/eslint-plugin/docs/rules/no-misused-promises.md
