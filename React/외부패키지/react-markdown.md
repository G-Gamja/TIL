# 리액트 마크다운 (react-markdown)

## 커스텀 레시피

```tsx
import Markdown from "react-markdown";

import { Container, ScrollContainer } from "./styled";

type MarkdownTextProps = {
  children?: string;
};

// eslint-disable-next-line jsx-a11y/anchor-has-content, @typescript-eslint/no-explicit-any
const renderLink = (props: any) => (
  <a {...props} target="_blank" rel="noopener noreferrer" />
);

export default function MarkdownText({ children }: MarkdownTextProps) {
  return (
    <ScrollContainer>
      <Container>
        <Markdown
          components={{
            a: renderLink,
          }}
        >
          {children}
        </Markdown>
      </Container>
    </ScrollContainer>
  );
}
```

## 스타일

```tsx
import { styled } from "@mui/material/styles";

export const ScrollContainer = styled("div")(({ theme }) => ({
  "*::-webkit-scrollbar": {
    width: "0.1rem",
    height: "0.1rem",
    backgroundColor: "transparent",
  },
  "*::-webkit-scrollbar-thumb": {
    backgroundColor: theme.colors.base04,
  },
  "*::-webkit-scrollbar-corner": {
    backgroundColor: "transparent",
  },
}));

export const Container = styled("div")(({ theme }) => ({
  height: "7rem",

  fontSize: theme.typography.h9.fontSize,
  fontFamily: theme.typography.h9.fontFamily,

  "& a": {
    textDecoration: "none",
    color: theme.colors.accentBlue,
    "&:hover": {
      opacity: 0.8,
    },
  },

  overflow: "auto",
}));
```
