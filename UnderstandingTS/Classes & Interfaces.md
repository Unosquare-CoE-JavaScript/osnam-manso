# What are Classes

<p align="left">
    <img src="https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F411e3f12-05d8-475e-b4d2-fa374426d6cb%2FScreen_Shot_2021-11-02_at_17.23.42.png?id=7e4b214b-c786-4a2b-880a-4f5857e0f239&table=block&spaceId=39c865bd-d151-4ddd-bfd8-f2239f411ed9&width=1910&userId=cc2028a7-e873-4ae8-988c-88e12db2775f&cache=v2"/>
</p>

# Creating a First Class

```jsx
class Department {
  name: string;

  constructor(n: string) {
    this.name = n;
  }
}

const accounting = new Department('Peter');
```

# Compiling to JavaScript

```jsx
'use strict';
class Department {
  constructor(n) {
    this.name = n;
  }
}
const accounting = new Department('Peter');
```

# Constructor Functions & The "this" Keyword

```jsx
class Department {
  name: string;
  employees: string[] = [];

  constructor(n: string) {
    this.name = n;
  }

  describe(this: Department) {
    console.log('Deparment: ' + this.name);
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }
}

const accounting = new Department('Peter');
accounting.addEmployee('Max');
```

# "private" and "public" Access Modifiers

```jsx
class Department {
  name: string;
  private employees: string[] = [];

  constructor(n: string) {
    this.name = n;
  }

  describe(this: Department) {
    console.log('Deparment: ' + this.name);
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }
}

const accounting = new Department('Peter');
accounting.addEmployee('Max');
```

# Shorthand Initialization

```jsx
class Department {
  // private id: string;
  // private name: string;
  private employees: string[] = [];

  constructor(private id: string, public name: string) {
    // this.id = id
    // this.name = n
  }

  describe(this: Department) {
    console.log('Deparment: ' + this.id + this.name);
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }
}

const accounting = new Department('1', 'Peter');
accounting.addEmployee('Max');

```

# "readonly" Properties

```jsx
class Department {
  // private id: string;
  // private name: string;
  private employees: string[] = [];

  constructor(private readonly id: string, public name: string) {
    // this.id = id
    // this.name = n
  }

  describe(this: Department) {
    console.log('Deparment: ' + this.id + this.name);
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }
}

const accounting = new Department('1', 'Peter');
accounting.addEmployee('Max');

```

# Inheritance

```jsx
class Department {
  // private readonly id: string;
  // private name: string;
  private employees: string[] = [];

  constructor(private readonly id: string, public name: string) {
    // this.id = id
    // this.name = n
  }

  describe(this: Department) {
    console.log('Deparment: ' + this.id + this.name);
  }

  addEmployee(employee: string) {
    this.employees.push(employee);
  }
}

class ITDepartment extends Department {
  admins: string[];
  constructor(id: string, admins: string[]) {
    super(id, 'IT');
    this.admins = admins;
  }
}

const accounting = new ITDepartment('id1', ['Max']);

```

# Overriding Properties & The "protected" Modifier

- Private methods/members are accessible only from inside the class.
- Protected methods/members are accessible from inside the class and extending class as well.

```jsx
class Department {
  // private readonly id: string;
  // private name: string;
  protected employees: string[] = [];

  constructor(private readonly id: string, public name: string) {
    // this.id = id
    // this.name = n
  }

  describe(this: Department) {
    console.log('Deparment: ' + this.id + this.name);
  }

  addEmployee(name: string) {
    if (name === 'Max') {
      return;
    }
    this.employees.push(name);
  }
}

class ITDepartment extends Department {
  admins: string[];
  constructor(id: string, admins: string[]) {
    super(id, 'IT');
    this.admins = admins;
  }
}

const accounting = new ITDepartment('id1', ['Max']);
```

# Getters and Setters

```jsx

class AccountingDepartament extends Department {
  private lastReport: string;

  // getters
  get mostRecentReport() {
    if (this.lastReport) return this.lastReport;
    throw new Error('No report found.');
  }
  // setters
  set mostRecentReport(value: string) {
    if (value) {
      throw new Error('Please pass in a valid value.');
    }
    this.addReport(value);
  }

  constructor(id: string, private reports: string[]) {
    super(id, 'Accounting');
    this.lastReport = reports[0];
  }
  addEmployee(name: string) {
    if (name === 'Max') return;
    this.employees.push(name);
  }
  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }
}

const accounting = new AccountingDepartament('d2', []);
console.log(accounting.mostRecentReport);
console.log((accounting.mostRecentReport = 'Year End Report'));


```

# Static Methods & Properties

```jsx
class Department {
  // private readonly id: string;
  // private name: string;
  static fiscalYear = 2020;
  protected employees: string[] = [];

  constructor(private readonly id: string, public name: string) {
    // this.id = id
    // this.name = n
    //console.log(Department.fiscalYear)
  }

  static createEmployee(name: string) {
    return { name: name };
  }

  describe(this: Department) {
    console.log('Deparment: ' + this.id + this.name);
  }

  addEmployee(name: string) {
    if (name === 'Max') {
      return;
    }
    this.employees.push(name);
  }
}

const employee1 = Department.createEmployee('Max');
console.log(employee1, Department.fiscalYear);

```

# Abstract Class

```jsx
abstract class Department {
  static fiscalYear = 2020;
  protected employees: string[] = [];

  constructor(protected readonly id: string, public name: string) {
  }

  static createEmployee(name: string) {
    return { name: name };
  }

  abstract describe(this: Department): void;

  addEmployee(name: string) {
    if (name === 'Max') {
      return;
    }
    this.employees.push(name);
  }
}

class AccountingDepartament extends Department {
  private lastReport: string;

  // getters
  get mostRecentReport() {
    if (this.lastReport) return this.lastReport;
    throw new Error('No report found.');
  }
  // setters
  set mostRecentReport(value: string) {
    if (value) {
      throw new Error('Please pass in a valid value.');
    }
    this.addReport(value);
  }

  constructor(id: string, private reports: string[]) {
    super(id, 'Accounting');
    this.lastReport = reports[0];
  }
  describe() {
    console.log('AccountingDepartment - ID: ' + this.id);
  }

  addEmployee(name: string) {
    if (name === 'Max') return;
    this.employees.push(name);
  }
  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }
}

```

# Singletons & Private Constructors

```jsx
class AccountingDepartament extends Department {
  private lastReport: string;
  private static instance: AccountingDepartament;

  // getters
  get mostRecentReport() {
    if (this.lastReport) return this.lastReport;
    throw new Error('No report found.');
  }
  // setters
  set mostRecentReport(value: string) {
    if (value) {
      throw new Error('Please pass in a valid value.');
    }
    this.addReport(value);
  }

  private constructor(id: string, private reports: string[]) {
    super(id, 'Accounting');
    this.lastReport = reports[0];
  }

  static getInstance() {
    if (AccountingDepartament.instance) {
      return this.instance;
    }
    this.instance = new AccountingDepartament('d2', []);
    return this.instance;
  }

  describe() {
    console.log('AccountingDepartment - ID: ' + this.id);
  }

  addEmployee(name: string) {
    if (name === 'Max') return;
    this.employees.push(name);
  }
  addReport(text: string) {
    this.reports.push(text);
    this.lastReport = text;
  }
}
const accounting = AccountingDepartament.getInstance();
```
