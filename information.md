# Travel List Project

## Addim - 1

- Komponentler yaradiriq
  - components/
    - `Logo.jsx`
    - `Form.jsx`
    - `PackingList.jsx`
    - `Stats.jsx`

## Addim - 2

- `Logo.jsx` icini yaziriq
- `Form.jsx` icini yaziriq demo olaraq tam bitmeyib
- `PackingList.jsx` icini yaziriq demo olaraq tam bitmeyib
- `Stats.jsx` icini yaziriq demo olaraq tam bitmeyib

## Addim - 3

- Komponentleri `App.jsx` - e elave edirik

## Addim - 4

- https://css-tricks.com/emoji-as-a-favicon/ saytinan faviconu emoji qoyuruq

## Addim - 5

- `Item.jsx` komponenti yaradiriq
- `PackingList.jsx` de initialItems adinda bir fake data yaradiriq ve bunu `map()` metodu ile butun datalari getirib `Item.jsx` komponentine otururuk `props` olaraq
- `Item.jsx` in icini yazriqi amma tam bitmeyib, `button` nece isleyecyi yazilmayib

## Addim - 6

- `Form.jsx` ici doldurulur `input` `form` `button` `select` `option` lar yazilir
- `useState()` ile `form` dan gelen deyerler saxlalinir ve `handleSubmit()` ile elave edilir `console.log()` (helelik console.log da saxlayiriq)
- `input` da ki deyisikleri `useState()` de saxlamaq ucun `input` ve `select` de `value` ve `onChange` elave edirik. `value` - da deyismis datani saxlayiriq, `oncHange` de ise deyisen datani `set` metodu ile otururuk

## Addim - 7

- `Form.jsx` - deki item elave etmek ucun yaratdigim function ve state i globala danisiyir yani `App.jsx` - e dasiriyir, ve props olaraq `Form.jsx` e ve `PackingList.jsx` e gonderirik

```javascript
const [items, setItems] = useState([]);

function handleAdddItems(item) {
  setItems([...items, item]);
}
```

```javascript
return (
  <div className="app">
    <Logo />
    <Form onAddItems={handleAdddItems} />
    <PackingList items={items} />
    <Stats />
  </div>
);
```

## Addim - 8

- silme funksiyasini yaziriq. `App.jsx` de funksiyani yazip proplarla `Item.jsx` - e otururuk

```javascript
function handleDeleteItem(id) {
  setItems((items) => items.filter((item) => item.id !== id));
}
```

```javascript
<PackingList items={items} onDeleteItem={handleDeleteItem} />
```

```javascript
const Item = ({ item, onDeleteItem }) => {
  return (
    <li>
      <span style={item.packed ? { textDecoration: "line-through" } : {}}>
        {item.quantity} {item.description}
      </span>
      <button onClick={() => onDeleteItem(item.id)}>❌</button>
    </li>
  );
};
```

## Addim - 9

- chekcbox funksiyasini yaziriq. `App.jsx` de funksiyani yazip proplarla `Item.jsx` - e otururuk

```javascript
function handleToggleItem(id) {
  setItems((items) =>
    items.map((item) =>
      item.id === id ? { ...item, packed: !item.packed } : item
    )
  );
}
```

```javascript
<PackingList
  items={items}
  onDeleteItem={handleDeleteItem}
  onToggleItem={handleToggleItem}
/>
```

```javascript
const PackingList = ({ items, onDeleteItem, onToggleItem }) => {
  return (
    <div className="list">
      <ul>
        {items.map((item) => (
          <Item
            item={item}
            key={item.id}
            onDeleteItem={onDeleteItem}
            onToggleItem={onToggleItem}
          />
        ))}
      </ul>
    </div>
  );
};
```

```javascript
const Item = ({ item, onDeleteItem, onToggleItem }) => {
  return (
    <li>
      <input
        type="checkbox"
        value={item.packed}
        onChange={() => onToggleItem(item.id)}
      />
      <span style={item.packed ? { textDecoration: "line-through" } : {}}>
        {item.quantity} {item.description}
      </span>
      <button onClick={() => onDeleteItem(item.id)}>❌</button>
    </li>
  );
};
```

## Addim - 10

- `Stats.jsx` tamamlandi, butun statistikalar hesablanmasi yazildi

```javascript
const Stats = ({ items }) => {
  const numItems = items.length;
  const numPacked = items.filter((item) => item.packed).length;
  const percentage = Math.round((numPacked / numItems) * 100) || 0;

  if (!items.length) {
    return (
      <p className="stats">
        <em>Start adding some items to your packing list 🚀</em>
      </p>
    );
  }

  return (
    <footer className="stats">
      <em>
        {percentage === 100
          ? "You got everything! Ready to go ✈️"
          : `💼 You have ${numItems} items on your list, and you already packed ${numPacked} (${percentage}%)`}
      </em>
    </footer>
  );
};

export default Stats;
```

## Addim - 11

- `PackingList.jsx` de sort etmeyi yaziriq

```javascript
import { useState } from "react";
import Item from "./Item";

const PackingList = ({ items, onDeleteItem, onToggleItem }) => {
  const [sortBy, setSortBy] = useState("input");

  let sortedItems;

  if (sortBy === "input") sortedItems = items;

  if (sortBy === "description")
    sortedItems = items
      .slice()
      .sort((a, b) => a.description.localeCompare(b.description));

  if (sortBy === "packed")
    sortedItems = items
      .slice()
      .sort((a, b) => Number(a.packed) - Number(b.packed));

  return (
    <div className="list">
      <ul>
        {sortedItems.map((item) => (
          <Item
            item={item}
            key={item.id}
            onDeleteItem={onDeleteItem}
            onToggleItem={onToggleItem}
          />
        ))}
      </ul>

      <div className="actions">
        <select value={sortBy} onChange={(e) => setSortBy(e.target.value)}>
          <option value="input">Sort by input order</option>
          <option value="description">Sort by descrription</option>
          <option value="packed">Sort by packed status</option>
        </select>

        <button>Clear list</button>
      </div>
    </div>
  );
};

export default PackingList;
```

## Addim - 12

- `App.jsx` de clear list funksiyasini yazip propla `PackingList.jsx` de oturub, clear list metodunu clear list `button`na onClick olarak elave edirik

```javascript
function handleClearList() {
  const confirmClear = window.confirm(
    "Are you sure you want to delete all items?"
  );

  if (confirmClear) {
    setItems([]);
  }
}
```

```javascript
<PackingList
  items={items}
  onDeleteItem={handleDeleteItem}
  onToggleItem={handleToggleItem}
  onClearList={handleClearList}
/>
```

```javascript
import { useState } from "react";
import Item from "./Item";

const PackingList = ({ items, onDeleteItem, onToggleItem, onClearList }) => {
  const [sortBy, setSortBy] = useState("input");

  let sortedItems;

  if (sortBy === "input") sortedItems = items;

  if (sortBy === "description")
    sortedItems = items
      .slice()
      .sort((a, b) => a.description.localeCompare(b.description));

  if (sortBy === "packed")
    sortedItems = items
      .slice()
      .sort((a, b) => Number(a.packed) - Number(b.packed));

  return (
    <div className="list">
      <ul>
        {sortedItems.map((item) => (
          <Item
            item={item}
            key={item.id}
            onDeleteItem={onDeleteItem}
            onToggleItem={onToggleItem}
          />
        ))}
      </ul>

      <div className="actions">
        <select value={sortBy} onChange={(e) => setSortBy(e.target.value)}>
          <option value="input">Sort by input order</option>
          <option value="description">Sort by descrription</option>
          <option value="packed">Sort by packed status</option>
        </select>

        <button onClick={onClearList}>Clear list</button>
      </div>
    </div>
  );
};

export default PackingList;
```
