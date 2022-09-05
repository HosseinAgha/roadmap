# Spec

We have a layout component that has a sidebar (thin) and content (fat).  
We want to let users update the styles of the thin and fat components.  
The Scrollbar here is 

# Code

```tsx
import Scrollbar from 'overlayscrollbars-react';

export const ThinFatLayout = ({
  thinElement,
  fatElement,
  className,
}: ThinFatLayoutProps) => {
  const ThinElement = thinElement;
  const FatElement = fatElement;

  return (
    <Wrapper className={className}>
      <Scrollbar className="AK_ThinElement">
        <PartWrapper>
          <ThinElement />
        </PartWrapper>
      </Scrollbar>
      <Scrollbar className="AK_FatElement">
        <PartWrapper>
          <FatElement />
        </PartWrapper>
      </Scrollbar>
    </Wrapper>
  );
};
```
