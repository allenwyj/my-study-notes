# TypeScript

# Basic TypeScript

## <> symbol

```tsx
// X's type is an React Component
// Only has props.children
const X: React.FC = () ...
// X's type is an React Component with Props
// props' type is Props
const X: React.FC<Props> = (props)...
```

# TS in React

## Type Declaration with React Component

```tsx
const myComponent: React.FC = () => {
	return ()
};

//or 
const myComponent: React.FunctionComponent = () => {
	return ()
};
```

## Type Declaration with Props in React Component

```tsx
type Props = { selected: string[] };
const myComponent: React.FunctionComponent<Props> = (props) => {
	return ()
};
```

## Type Declaration with Hook

```tsx
const [tags, setTags] = useState<string[]>([]);
```

## Type Declaration 强制限定type

```tsx
const [selected, setSelected] = useState({
    **tags: [] as string[],**
    note: '',
    **category: '-' as Category,**
    amount: 0
  });
```