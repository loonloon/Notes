#### Null Checks Everywhere! ####

* Null Object Pattern

```
//Bad
const legalCases: LegalCase[] = await fetchCasesFromAPI();

for (const legalCase of legalCases) {
    if (legalCase.documents != null) {
        uploadDocuments(legalCase.documents);
    }
}

//Take 1
const fetchCasesFromAPI = async function () {
    const legalCases: LegalCase[] = await $http.get('legal-cases/');

    for (const legalCase of legalCases) {
        // Null Object Pattern
        legalCase.documents = legalCase.documents || [];
    }

    return legalCases;
}

//Take 2
class EmptyArray<T> {
    static create<T>() {
        return new Array<T>()
    }
}

// Use it like this:
const myEmptyArray: string[] = EmptyArray.create<string>();

```

* What About Objects?

```
class NullBoss implements IBoss {
    fight(player: Player) {
        // Player always wins.
    }

    isDead() {
        return true;
    }
}
```

#### Special Case Pattern ####

Since we've exposed all our orders via an interface, there's no need to figure out what
the status of the class is! Each individual class will just know what it needs to do

```
//Bad
enum OrderStatus {
    Pending, FraudulentAccount, PaymentRejected
}

class Order {
    public status: OrderStatus;

    constructor() {
    }

    public placeOrder() {
        // Do some stuff...
    }
}

if (order.status === OrderStatus.Pending) {
    order.placeOrder();
} else if (order.status === OrderStatus.PaymentRejected) {
    // Something else...
}

// And on and on...

//Good
class PendingOrder implements IOrder {
    constructor() {
    }

    public placeOrder() {
        // API call, etc.
    }
}
class PaymentRejectedOrder implements IOrder {
    constructor() {
    }

    public placeOrder() {
        // Try to pay again. Etc.
    }
}

class OrderOnFraudulentAccount implements IOrder {
    constructor() {
    }

    public placeOrder() {
        // Notify the fraud department. The actual order placement will be
        // decided and placed by the fraud department.
    }
}

const ordersCollection: IOrder[] = await getOrders();

for (const order of ordersCollection) {
    order.placeOrder();
}

```

#### Wordy Conditionals ####

```
//Bad
if (user.role === "admin"
    && user.isActive
    && user.permissions.some(p => p === "edit")) {
    // Do stuff
}

//Good

//Take 1
//Is it possible that somewhere else in our application we will need to check whether:
//A user is an admin?
//A user is active?
//A user can edit certain resources?
//The answer is... probably... yes.

const isAdmin: boolean = user.role === "admin";
const userIsActive: boolean = user.active;
const userCanEdit: boolean = user.permissions.some(p => p === "edit");
const activeAdminCanEdit: boolean = isAdmin && userIsActive && userCanEdit;

if (activeAdminCanEdit) {
    // Do stuff.
}

//Take 2
//Why don't we take the logic from those conditionals and extract them as methods on
//our user class? Since these methods are performing logic that's about the user, it makes
//sense to give our user object ownership of these behaviors/states

const activeAdminCanEdit: boolean = user.isAdmin() && user.isActive() && user.canEdit();

if (activeAdminCanEdit) {
    // Do stuff.
}

//What we are left with is a User class that is filled with tons of these kinds of methods
class User {
    isAdmin(): boolean { /* Code */ }
    isActive(): boolean { /* Code */ }
    canEdit(): boolean { /* Code */ }
    isActiveAdmin(): boolean { /* Code */ }
    isActiveAdminThatCanEdit(): boolean { /* Code */ }
    // And dozens of more methods...
}

//Take 3
//Let's take the isActiveAdmin method, for example. What if we extracted this as an
//entirely new class? What would that look like?

const activeAdminCanEdit: boolean = new UserIsActiveAdmin(user).invoke() && user.canEdit();

if (activeAdminCanEdit) {
    // Do stuff.
}

class UserIsActiveAdmin {
    private _user: User;
    constructor(user: User) {
        this._user = user;
    }

    public invoke(): boolean {
        return this._user.isAdmin() && this._user.isActive();
    }
}

//Take 4
//Piping Our Logic

interface IPipeableCondition {
    check(): boolean;
}

class UserIsActiveAdmin implements IPipeableCondition {
    private _user: User;

    constructor(user: User) {
        this._user = user;
    }

    public check(): boolean {
        return this._user.isAdmin()
            && this._user.isActive();
    }
}

const conditions: IPipeableCondition[] = [
    new UserIsActiveAdmin(user),
    new UserCanEdit(user),
    new UserIsNotBlacklisted(user),
    new UserLivesInAvailableLocation(user)
];

const valid = conditions.every(p => p.check());

if (valid) {
    // Do stuff.
}

//Bonus Refactor
class ConditionsPipe {
    private _conditions: IPipeableCondition[];

    constructor(conditions: IPipeableCondition[]) {
        this._conditions = conditions;
    }

    check(): boolean {
        return this._conditions.every(p => p.check());
    }
}

const pipe = new ConditionsPipe([
    new UserIsActiveAdmin(user),
    new UserCanEdit(user),
    new UserIsNotBlacklisted(user),
    new UserLivesInAvailableLocation(user)
]);

if (pipe.check()) {
    // Do stuff.
}

```

