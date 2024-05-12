# FAQs

1. **How to check whether a value is a number i.e real number?**

   - `typeof value === 'number' && isFinite(value)`
   - To check if a value is **number** or **string representation of number**: `!isNaN(value) && isFinite(value)`
     - `isNaN('123')` is **false** and so is `isNaN(123)`

2. **How to parse a number?**
   - `Number('12.3m')` returns `NaN`
   - `parseFloat('12.3n')` returns `12.3`
   - `parseInt('12.3n')` returns `12`
