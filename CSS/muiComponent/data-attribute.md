```css
type ExpandedButtonProps = {
  'data-is-expanded'?: number;
};

export const ExpandedButton = styled('button')<ExpandedButtonProps>(({ theme, ...props }) => ({
  border: 0,
  paddingBottom: '1rem',

  backgroundColor: 'transparent',
  width: '100%',

  display: 'flex',
  justifyContent: 'center',
  alignItems: 'center',

  cursor: 'pointer',

  '& > svg': {
    transform: props['data-is-expanded'] ? 'rotate(180deg)' : 'none',

    '& > path': {
      fill: theme.colors.base05,
    },
  },

  '&:hover': {
    '& > svg': {
      '& > path': {
        fill: theme.colors.base06,
      },
    },
  },
}));
```