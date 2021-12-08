# Singlenton

```jsx
class Singleton {
    constructor(){
       const instance = this.constructor.instance
       if (instance){
           return instance
       }
       this.constructor.instance = this
    }
 }

class MyDbConn{
    protected static instance: MyDbConn | null = null
    private id: number = 0

    constructor(){
        this.id = Math.random()
    }

    public getID: number {
        return this.id
    }

    public static getInstance(): MyDBCoon{
        if(!MyDBConn.instance){
            MyDbConn.instance = new MyDbConn()
        }
        return MyDbConn.instance
    }
}
```

# Monostate

```jsx
class ChiefExecutiveOfficer {
  getName(value) {
    ChiefExecutiveOfficer._name = value
  }

  get age() {
    return ChiefExecutiveOfficer._age
  }
  set age(value) {
    ChiefExecutiveOfficer._age = value
  }
}

ChiefExecutiveOfficer._name = undefined
ChiefExecutiveOfficer._age = undefined

let ceo = new ChiefExecutiveOfficer()
ceo.name = 'Adam Smith'
ceo.age = 55
```

# Summary

- A constructor can choose what to return; we can keep returning same instance.
- Monostate: many instances, shared data.
- Directly depending on the Singleton is a bad idea; introduce a dependency instead.
