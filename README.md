# ReactAutocomplete
awesome responsive react autocomplete component

Preview:
![Alt text](./preview.jpg?raw=true "Preview")

Usage:
```javascript
<Autocomplete
  items={items} //items=[{Title:'anytitle',Value:num},...]
  placeholder={'Your placeholder'}
  disableAutoComplete={true / false}
  width={2 || 3 || 4}
  fill={true / false}
  isRequired={true / false}
  selectedValue = {index number of items}
  resetState = {any state you want}
/>
