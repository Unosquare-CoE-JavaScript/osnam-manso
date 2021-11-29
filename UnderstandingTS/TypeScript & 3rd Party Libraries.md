# Using JavaScript (!)

- lodash

  TypeScript does not understand it and it can't because lodash uses JavaScript not TypeScript.

```jsx
import _ from 'lodash' // get error so:
// set in tsconfig.json:
"noEmitOnError": false,

//or

 npm install --save-dev @types/lodash // translation
```

# No Types Needed: class-transformer

```jsx
npm install class-transformer --save

import {plainToClass} from class-transformer

const loadedProducts = plainToClass(Product, products)
```

# Class Validator

- Decorators for TypeScript

```jsx
npm install class-validator --save
```
