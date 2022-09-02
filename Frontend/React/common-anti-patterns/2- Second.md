We want to highlight a field's Wrapper borders in the form if it had errors.  

```tsx
// file: TreeTag.tsx

const TreeTag = (props) => {
  return (
    <Grid container direction="row" spacing={2}>
      {tags.map((tag, idx) => (
        <ValidationWithoutWrapper
          errors={errors}
          valueIndex={idx}
          Component={DraggableWrapper}
          dataValue={tag._id}
          fieldName={field.name}
          isDraggable={!readonly}
        >
          <TagPath tagPath={getTagPath(tag, idx)} />
        </ValidationWithoutWrapper>
      ))}
    </Grid>
  )
}
```

```tsx
// file: ValidationWrapper.tsx
// It renders the field component and if it had errors injects some styles to it.

const errorValidationStyle = ({ hasError, theme }) =>
  hasError &&
  `
    border: 1px solid ${theme.palette.error.main};
    padding:  0 1rem;
    border-radius:  5px;
    `;

interface IValidationWrapper {
  errors?: ValidationResult;
  valueIndex: number;
  style?: (hasError: boolean) => CSSProperties;
}

type IValidationWithoutWrapper<T> = IValidationWrapper & {
  Component: FC<T>; // This is the renderer component 
  children: ReactNode;
} & T;

export const ValidationWithoutWrapper = <
  T extends Record<string, any> = Record<string, any>,
>(
  props: IValidationWithoutWrapper<T>,
): ReactElement => {
  const { errors, valueIndex, Component, children, style, ...restProps } = props;
  const validationResult = getValidationErrors(errors);
  const hasError = validationResult?.errorsIndex.includes(+valueIndex) ?? false;
  const StyledComponent = styled<any>(Component)`
    ${({ theme }) =>
      errorValidationStyle({
        hasError,
        theme,
      })}
  `;
  return (
    <StyledComponent {...restProps} style={style?.(hasError)}>
      {children}
    </StyledComponent>
  );
};
```
