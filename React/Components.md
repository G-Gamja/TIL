## 컴포넌트화 하는법


``` typescript
type MainContainerProps = {
  mainText: string;
  onClick?: () => void;
};
export default function MainContainer({ mainText, onClick }: MainContainerProps) {
  return (
    <button type="button" className={styles.buttonStyle} onClick={onClick}>
      <div className={styles.boxButton}>
        <div className={styles.boxText}>{mainText}</div>
        <div className={styles.boxIcon}>
          <RightStroke />
        </div>
      </div>
    </button>
  );
}
```

## 컴포넌트 여러개 반복 렌더링 하는법 

https://goddaehee.tistory.com/303