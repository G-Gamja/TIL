https://channel.io/ko/blog/react-dnd-tips-tricks


https://codesandbox.io/s/react-dnd-example-12-kkqpklk3q7?file=/src/example.js

https://medium.com/nmc-techblog/easy-drag-and-drop-in-react-22778b30ba37
```ts
import * as React from 'react';
import { DndProvider, useDrag, useDrop } from 'react-dnd';
import HTML5Backend from 'react-dnd-html5-backend';

interface Props {
  text: string;
}

const Container: React.FC<Props> = ({ text }) => {
  const [{ isDragging }, drag] = useDrag({
    item: { type: 'MyDraggable' },
    collect: (monitor) => ({
      isDragging: !!monitor.isDragging(),
    }),
  });

  return (
    <div ref={drag}>
      {isDragging ? 'dragging' : text}
    </div>
  );
};

const DropTarget: React.FC = () => {
  const [{ isOver }, drop] = useDrop({
    accept: 'MyDraggable',
    collect: (monitor) => ({
      isOver: !!monitor.isOver(),
    }),
  });
  return (
    <div ref={drop}>
     {isOver ? 'Dropped!' : 'Drop Here!' }
    </div>
  );
};

const App: React.FC = () => {
  return (
  <DndProvider backend={HTML5Backend}>
    <Container text="Drag Me!" />
    <DropTarget/>
  </DndProvider>
     );
};

export default App;

```

이 코드가 왜 안먹히지?
```jsx
      <DndProvider backend={HTML5Backend}>
        <ListContainer ref={drop}>
            {indexedAccounts.map((account) => (
              <AccountItem
                draggableItem={account}
                moveItem={moveItem}
                findItem={findItem}
                accountType={account.type}
                isActive={selectedAccount?.id === account.id && isOpenPopover}
                key={account.id}
                itemIndex={account.index}
                onClick={(event) => {
                  setSelectedAccount(account);
                  setPopoverAnchorEl(event.currentTarget);
                }}
              >
                {accountName[account.id]}
              </AccountItem>
            ))}
        </ListContainer>
      </DndProvider>
```