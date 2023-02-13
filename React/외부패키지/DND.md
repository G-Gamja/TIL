https://channel.io/ko/blog/react-dnd-tips-tricks


https://codesandbox.io/s/react-dnd-example-12-kkqpklk3q7?file=/src/example.js
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