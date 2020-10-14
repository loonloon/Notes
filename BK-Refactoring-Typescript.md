#### Null Checks Everywhere! ####

* Null Object Pattern

```
//Before
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
//Before
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

//After
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

