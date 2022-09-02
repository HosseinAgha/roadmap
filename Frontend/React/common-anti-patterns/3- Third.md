# Spec
We want to render a subject tree.
We want to show a skeleton loader while the tree nodes are loading.

# Code
```tsx
// file: TreeChildren

export const TreeChildren: FC<TreeChildrenProps> = (props) => {
  const { treeId, parentId, handleSelect, readOnlyMode, childrenNodes } = props;

  const {
    data: rawTreeNodes,
    loading: loadingTreeNodes,
    error: errorTreeNodes,
  } = useGetTreeNodes({ treeId, filtration: { parentNodeId: parentId } });

  const renderTreeNode = (node: RenderingTreeNode) => {
    return (
      <StyledTreeItem
        key={node._id}
        nodeId={node._id}
        label={<span onClick={(e) => handleSelect(e, node)}>{getTitle(node)}</span>}
      >
        {getChildTree(node, props)}
      </StyledTreeItem>
    );
  };

  // SkeletonLoading shows skeleton loading while loading === true otherwise shows the children
  return (
    <SkeletonLoading
      loading={loadingTreeNodes}
      error={errorTreeNodes}
      skeletonBody={treeNodeSkeletonBody}
    >
      {() => {
        if (readOnlyMode) {
          if (isNil(childrenNodes) || isEmpty(childrenNodes)) {
            return <StyledTreeItem key={`node`} nodeId={`dummyId`} label={'no item found'} />;
          }

          console.log('here');
          return childrenNodes?.map((node) => renderTreeNode(node));
        }

        if (isNil(rawTreeNodes) || isEmpty(rawTreeNodes)) {
          return <StyledTreeItem key={`node`} nodeId={`dummyId`} label={'no item found'} />;
        }

        return rawTreeNodes?.map((section) => renderTreeNode(section));
      }}
    </SkeletonLoading>
  );
};
```

```tsx
// file: SkeletonLoading.tsx
interface ISkeletonLoading {
  loading: boolean;
  error?: ApolloError;
  skeletonBody: ReactElement<any>;
  templateRepeat?: number;
  children: () => ReactNode;
}

export const SkeletonLoading: FC<ISkeletonLoading> = // implementation not important......
```
