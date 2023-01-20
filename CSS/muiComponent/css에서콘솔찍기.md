```css
export const StyledChainAccordionSummary = styled((props: AccordionSummaryProps) => (
  <AccordionSummary expandIcon={<BottomArrow24Icon />} {...props} />
))<StyledAccordionSummaryProps>(({ theme, ...props }) => {
  console.log(props);
  return {
    padding: props['data-is-expanded'] ? (props['data-is-length'] ? '1.2rem 0.4rem 0.8rem' : '1.2rem 0.4rem 0.45rem') : '1.2rem 0.4rem',

    '& .MuiAccordionSummary-expandIconWrapper.Mui-expanded': {
      transform: 'rotate(180deg)',
    },

    '& .MuiAccordionSummary-expandIconWrapper': {
      '& > svg > path': {
        stroke: theme.colors.base05,
      },
    },
  };
});
```