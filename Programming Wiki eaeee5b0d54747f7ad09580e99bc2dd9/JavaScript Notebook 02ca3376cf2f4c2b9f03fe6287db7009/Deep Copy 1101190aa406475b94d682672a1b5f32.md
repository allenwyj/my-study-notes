# Deep Copy

```tsx
// deep clone tags array
const tagsClone = JSON.parse(JSON.stringify(tags));
// delete one item at index, and add a new obj
tagsClone.splice(index, 1, { id: id, name: obj.name });
```