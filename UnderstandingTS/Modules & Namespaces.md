# Working with Namespaces

```jsx
//file-1.ts
namespace App {

    export interface Draggable {
        dragStartHandler(event: DragEvent): void
        dragEndHandler(event: DragEvent): void
    }
    export interface DragTarget {
        dragOverHandler(event: DragEvent): void
        dropHandler(event: DragEvent): void
        dragLeaveHandler(event: DragEvent): void
    }
}

// file-2.ts
 <reference path="file-1.ts">
namespace App {
    // all code here
}
```
