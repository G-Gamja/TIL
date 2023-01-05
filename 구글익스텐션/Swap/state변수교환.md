
`inputDisplayAmount`, `outputDisplayAmount`의 값이 서로 setter로 바뀌어야하는 상황에서 useEffect로 후자 상태변수에 곱셈이 가해지면서   불필요한 곱셈이 swap시에 지속적으로 반영됨

=> 디바운스 변수를 활용하여 `입력시의`상태변수 , `입력 후의` 상태변수를 분리하여 사용하도록 함
```tsx  
  const [inputDisplayAmount, setInputAmout] = useState<string>('');

  const [outputDisplayAmount, setOutputAmout] = useState<string>('0');

  // NOTE debounce
  const [debouncedInputDisplayAmount] = useDebounce(inputDisplayAmount, 400);
  useEffect(() => {
    if (debouncedInputDisplayAmount && swapPercentage) {
      setOutputAmout(times(debouncedInputDisplayAmount, swapPercentage, 6));
    } else {
      setOutputAmout('0');
    }
  }, [debouncedInputDisplayAmount, swapPercentage]);

  const swapCoin = useCallback(() => {
    setInputAmout(outputDisplayAmount);
    setOutputAmout(inputDisplayAmount);
  }, [inputDisplayAmount, outputDisplayAmount]);
```
위 코드가 정상작동하지 않는데 조건문의 차이인지 디버깅할 필요성이 있어보임 최종적으로 useEffect가 작동하여 다시 곱셈이 가해짐
디바운스 버젼
```ts
  const [inputDisplayAmount, setInputAmout] = useState<string>('');
  const [outputDisplayAmount, setOutputAmout] = useState<string>('0');

  // NOTE debounce
  const [debouncedInputDisplayAmount] = useDebounce(inputDisplayAmount, 400);
  useEffect(() => {
    if (debouncedInputDisplayAmount && swapPercentage) {
      setOutputAmout(times(debouncedInputDisplayAmount, swapPercentage, 6));
    } else {
      setOutputAmout('0');
    }
  }, [debouncedInputDisplayAmount, swapPercentage]);

  const swapCoin = useDebouncedCallback(() => {
    if (debouncedInputDisplayAmount) {
      setInputCoinBaseDenom(outputCoin.chain.baseDenom);
      setoutputCoinBaseDenom(inputCoin.chain.baseDenom);
      if (inputDisplayAmount) {
        setInputAmout(outputDisplayAmount);
        setOutputAmout(debouncedInputDisplayAmount);
      }
    }
  }, 300);
```