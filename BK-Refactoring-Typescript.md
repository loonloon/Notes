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

---

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

---

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

---

#### Nested Conditionals ####

```
//Bad
let result = null;

if (!order.wasCancelled()) {
    if (order.isPaid()) {
        result = order.sendToShipping();
    } else {
        if (order.isFraudulent()) {
            result = order.sendToFraudDept();
        } else {
            result = order.tryAgainLater();
        }
    }
}

return result;

//Good
//Guard Clauses
if (order.wasCancelled()) {
    return;
}

if (order.wasPaid()) {
    return order.sendToShipping();
}

if (order.isFraudulent()) {
    return order.sendToFraudDept();
}

return order.tryAgainLater();

```

---

#### Gate Classes ####

```
//Bad
//Example 1
if (!order.wasCancelled()) {
    // A bunch of code that does stuff.
    if (!order.isFraudlent()) {
        // Some more code.
    }
}

//Example 2
const accountIsVerified = await accountRepo.accountIsVerified(userAccount);
const canPlaceOrder = await orderRepo.userCanPlaceOrder(user);

if (accountIsVerified) {
    if (canPlaceOrder) {
        await orderRepo.placeOrder(order);
    }
}

//Good
class AccountIsVerifiedGate {
    private _accountRepo: AccountRepo;

    constructor(accountRepo: AccountRepo) {
        this._accountRepo = accountRepo;
    }

    async invoke(account: UserAccount) {
        const isVerified = await this._accountRepo.accountIsVerified(account);
        
        //This pattern is useful in Web APIs, for example. When a gate class fails and throws a
        //special type of exception, a middleware will send back a specific HTTP status, such as
        //401 or 403, automatically for you
        if (!isVerified) {
            throw "Gate exception";
        }
    }
}

await accountIsVerifiedGate.invoke(userAccount);
await userCanPlaceOrderGate.invoke(user);
await orderRepo.placeOrder(order);

```

---

#### Primitive Overuse ####
```
//Bad
//Example 1
//That logic is not sharable and therefore will be duplicated all over the place.

//In more complex scenarios, it's hard to see what the 
//underlying business concept represents (which leads to code that's hard to understand).

//If there is an underlying business concept, it's implicit, not explicit.

const email: string = user.email;

if(email !== null && email !== "") {
// Do something with the email.
}

const firstname = user.firstname || "";
const lastname = user.lastname || "";
const fullName: string = firstname + " " + lastname;

//Example 2
const domain: string = email.replace(/.*@/, '');
const userName = email.replace(domain, '');
const sendInternal: boolean = domain === 'internal-company.com';

if (sendInternal) {
  if (userName === 'info') {
    mailer.sendToCustomerServiceTeam(email, message);
  } else {
    mailer.mailToInternalServer(email, message);
  }
} else {
  throw 'Cannot email externally.';
}

//Initial refactor
const domain: string = email.replace(/.*@/, '');
const userName = email.replace(domain, '');
const sendExternal: boolean = domain !== 'internal-company.com';

if (sendExternal) {
  throw 'Cannot email externally.';
}

if (userName === 'info') {
  mailer.sendToCustomerServiceTeam(email, message);
  return;
}

mailer.mailToInternalServer(email, message);

//Good
//Value objects is that they cannot be modified once instantiated

//Example 1
class EmailAddress {
    private _value: string;
    private _domain: string;
    private _userName: string

    constructor(value: string) {
        if (this.isExternal(value)) {
            throw "Cannot email externally.";
        }

        // Other code when this object is valid.
        this._value = value;
        this._domain: string = value.replace(/.*@/, "");
        this._userName = value.replace(domain, "");
    }

    isExternal(): boolean {
        return this._domain !== "internal-company.com";
    }

    isInfoUser(): boolean {
        Value Objects | 45
        return this._userName === "info";
    }

    value() {
        return this._value;
    }
}

const emailAddress: EmailAddress = new EmailAddress(email);

if (emailAddress.isExternal()) {
    throw "Cannot email externally.";
}
if (emailAddress.isInfoUser()) {
    mailer.sendToCustomerServiceTeam(emailAddress.value(), message);
    return;
}

mailer.mailToInternalServer(emailAddress.value(), message);

```

---

#### Descriptive Booleans ####
```
//Bad
//Example 1

const user: User = await userIsAuthenticated(username, password);
const isAuthenticated: boolean = user !== null;

if (isAuthenticated) {
    if (user.isActive) {
        redirectToUserDashboard();
    } else {
        returnErrorOnLoginPage("User is not active.");
    }
} else {
    returnErrorOnLoginPage("Credentials are not valid.");
}

//Example 2
if (!user.isAuthenticated()) {
    returnErrorOnLoginPage("Credentials are not valid.");
}

if (!user.isActive()) {
    returnErrorOnLoginPage("User is not active.");
}

redirectToUserDashboard();

//Good
enum AuthenticationResult {
    InvalidCredentials,
    UserIsNotActive,
    HasExistingSession,
    IsLockedOut,
    IsFirstLogin,
    Successful
}

const strategies: any = [];
strategies[AuthenticationResult.InvalidCredentials] = () => returnErrorOnLoginPage("Credentials are not valid.");
strategies[AuthenticationResult.UserIsNotActive] = () => returnErrorOnLoginPage("User is not active.");
strategies[AuthenticationResult.HasExistingSession] = () => redirectToHomePage();
strategies[AuthenticationResult.IsLockedOut] = () => redirectToUserLockedOutPage();
strategies[AuthenticationResult.IsFirstLogin] = () => redirectToWelcomePage();
strategies[AuthenticationResult.Successful] = () => redirectToUserDashboard();

strategies[result]();

```

---

#### Lengthy Method Signatures ####
```
//Bad
//Example 1
public getUsers(
    includeInactive: boolean = false,
    filterText: string = null,
    orderByName: boolean = false,
    forHireDate: Date = null) : User[] {
}

//Good
private getUsers(
    includeInactive: boolean = false,
    filterText: string = null,
    orderByName: boolean = false,
    forHireDate: Date = null) : User[] {
}

public getActiveUsers() : User[] {
    return getUsers(false);
}

public getInactiveUsers() : User[] {
    return getUsers(true);
}

public getActiveUsersByName(filter: string) : User[] {
    return getUsers(false, filter);
}

public getActiveUsersOrderedAndFilteredByName(filter: string) : User[] {
    return getUsers(false, filter, true);
}

public getActiveUsersForHireDate(hireDate: Date) : User[] {
    return getUsers(false, null, false, hireDate);
}

```

---

#### Methods That Never End ####
```
//Bad
public async processOrder(orderId: string): Promise<ValidationMessage> {
    const user = await this.getUserFromSession();
    const userId = user.id;
    const userRole = user.role;
    const userAllowed: boolean = await this.userCanModifyOrder(userId, userRole);

    if (!userAllowed) {
        return new ValidationMessage("Permission denied.");
    }

    let saveAttempts = 1;
    while (saveAttempts > 3) {
        const order = await this.getOrderById(orderId);

        if (order.isActive()) {
            const status: OrderStatus = order.getStatus();

            if (status == OrderStatus.Pending) {
                // do some more stuff
            } else if (status == OrderStatus.Shipped) {
                // do some more stuff
            } else if (status == OrderStatus.Cancelled) {
                // do some stuff
            } else if (status == OrderStatus.Returned) {
                // do more stuff
            }
        } else {
            const archiveOnDate: Date = await this.getOrderArchiveOnDate(orderId);

            if (order.getOrderedOnDate() > archiveOnDate) {
                // Do some stuff
            }
        }

        const result: OrderUpdateResult = await this.tryUpdateOrder(order);

        if (result == OrderUpdateResult.Successful) {
            return new ValidationMessage("Order updated successfully.");
        } else if (result == OrderUpdateResult.OrderVersionOutOfSync) {
            return new ValidationMessage("Order is outdated. Try again.");
        } else if (result == OrderUpdateResult.NetworkError) {
            saveAttempts++;
            // The while loop will try to process the order again.
        }
    }
}

//Good
const userAllowed: boolean = await this.sessionUserCanModifyOrder();

if (!userAllowed) {
    return new ValidationMessage("User not allowed.");
}

const threeTimes = 3;
return this.retry(threeTimes, () => this.processOrder(orderId));

public async processOrder(orderId: string): Promise<void> {
    const order = await this.getOrderById(this.orderId);

    if (order.isActive()) {
        this.processActiveOrder(order);
    } else {
        this.tryArchivingOrder(order);
    }

    return await tryUpdateOrder(order);
}

enum OrderStatus {
    Pending = 0,
    Shipped = 1,
    Cancelled = 2,
    Returned = 3
}

const strategies: Function[] = [];
strategies[OrderStatus.Pending] = this.processPendingOrder;
strategies[OrderStatus.Shipped] = this.processShippedOrder;
strategies[OrderStatus.Cancelled] = this.processCancelledOrder;
strategies[OrderStatus.Returned] = this.processReturnedOrder;

// Execute the appropriate strategy.
strategies[order.getStatus()]();
```

--- 

#### Dumping Grounds ####
What you'll also find in many code bases are very generic classes such as User, Customer, and Order. Is that bad? Well, yes.

Let me ask you a question: Is User used in many different unrelated places in your application? For example, is your User class used in the billing part of your code, the user profile parts, the shipping parts, and so on? Most systems do something like that.

What will end up happening is that, because these classes are so generic, they'll become dumping grounds for code that we don't know where it belongs.

Instead of taking the time to think about the business need for this new code we've written, often, we feel that it's easier to put it into our generic classes. It's all sharable, right? And we're all about code reuse, right?

##### Coupling #####
So... what if I changed the User class to conform to some billing logic? What are the chances that I've also broken the shipping feature by changing this class? I don't know, but it's higher than 0%

We want to be able to change the shipping feature, for example, and not have to test our entire application again. But if we're sharing our User class everywhere, to have confidence that we didn't break stuff, we need to re-test everything

##### Warning Sign #####
Here's a basic technique that might help you get started on analyzing your classes. Take your class name and answer these questions:
1. Is there a subject (user, client, order, and so on)?
2. Is there a context for that subject (shipping, orders, dashboard, and so on)?
3. Is there even perhaps an action being performed on the subject (as we'll see in more detail soon)?

```
class User {
    firstName: string;
    lastName: string;
    id: number;
    jwtToken: string;
    homeAddress: string;
    creditCardNo: string;

    getFullName(): string {
        return this.firstName + " " + this.lastName;
    }

    decodeJwtToken(): string {
        return decode(this.jwtToken);
    }
}
```

##### You Have Mail #####
You've been tasked with adding a new business requirement. We need users to be able to pay for their products using PayPal.

This User class is already used in multiple places, such as the user profile, shipping, and payment features.

All we need to do is add the user's PayPal email address to the user. Right?

##### Breaking It Up #####
If we start changing this User class so that it works with the payment feature, then we risk affecting the user profile or the shipping feature (since they use this class too).

* What should we do?

The best thing to do here is to create a different User class that's used within each specific context.

Out of this should come classes such as UserForAuthentication, UserProfileUser, ShippingUser, and PaymentUser.

Are those models/classes going to contain similar pieces of data that all of them will need? Sure.

Will they also have pieces of data that are only used in one context? Sure. For example, the user's id is needed everywhere. But the user's home address is only ever needed for shipping. Why, then, does the payment feature need access to that data? It doesn't.

```
class UserProfileUser {
    firstName: string;
    lastName: string;
    id: number;
    homeAddress: string;

    getFullName(): string {
        return this.firstName + " " + this.lastName;
    }
}

class ShippingUser {
    id: number;
    homeAddress: string;
}

class UserForAuthentication {
    id: number;
    jwtToken: string;

    decodeJwtToken(): string {
        return decode(this.jwtToken);
    }
}

class PaymentUser {
    id: number;
    creditCardNo: string;
}
```

##### Keep Separate Things Separate #####
Sometimes, it's better to duplicate code and/or data if they are within different contexts. Again, we want to avoid coupling our features and classes together.

Let me ask you a question: Is it probable that the behavior for the home address within the user's profile will be different than the behavior for it in the shipping feature?
The answer: yes.

So, we are talking about two different things. It's the same raw data but not the same business function or concept. Shipping needs the home address so that it knows where to send products. The user profile needs the home address so the user can update its values from a UI.

Not the same thing.

Also, consider that it might also make sense to add an address to the PaymentUser class. But should this context share the same address as shipping? Well, is it possible that
your shipping address wouldn't be the same address you want to bill to? Sure! This happens all the time!

Using the SRP, we see that these two concepts/responsibilities should be kept separate.

##### CQRS #####
CQRS stands for Command Query Responsibility Segregation. It's a pattern that can be
helpful when dealing with more complex business logic.

Usually, when we create classes, they are used in both write and read scenarios. So, for example, a PaymentUser class will have methods for modifying its data or applying business rules, and also methods or properties that are used in scenarios where we are displaying data to a user.
```
class PaymentUser {
    id: number;
    firstName: string;
    lastName: string;
    creditCardNo: string;

    changeCreditCard(newNumber: string) {
        if (this.validCreditCard(newNumber)) {
            this.creditCardNo = newNumber;
        } else {
            throw 'Invalid credit card number.';

        }
    }

    displayName() {
        return this.lastName + ", " + this.firstName;
    }
}


```

Notice that the changeCreditCard method is a write method. It changes the state of our object and applies a certain business rule (that only a valid credit card can be used/
stored).

However, the displayName method is used to show data on a UI. Here's my question: Why are both of these different concerns in the same place?

These kinds of classes can quickly become confusing. We also tend to start loading them up with tons of display logic or tons of business rules. This makes them hard to understand and reason about.

So, instead, why don't we separate our model/class into two?

```
class PaymentUserForDisplay {
    id: number;
    firstName: string;
    lastName: string;
    creditCardNo: string;

    displayName() {
        return this.lastName + ", " + this.firstName;
    }
}

class PaymentUserForWrite {
    id: number;
    creditCardNo: string;

    changeCreditCard(newNumber: string) {
        if (this.validCreditCard(newNumber)) {
            this.creditCardNo = newNumber;
        } else {
            throw 'Invalid credit card number.';
        }
    }
}
```

Well, we'll take each business scenario and create a class to model that exact scenario! Each of these classes will either be read-only or write-only
* Commands: These are scenarios that change our system but don't return data to the user to display.
* Queries: Scenarios where we display data to a user so that they can decide on something.

```
class UpdateUserCreditCardInfoCommand {
    id: number;
    creditCardNo: string;

    handle(newNumber: string) {
        if (this.validCreditCard(newNumber)) {
            this.creditCardNo = newNumber;
        } else {
            throw 'Invalid credit card number.';
        }
    }
}

class UserProfileView {
    id: number;
    firstName: string;
    lastName: string;
    creditCardNo: string;
}

class UserProfileQuery {
    async handle(forUserId: number): UserProfileView {
        const view: UserProfileView = await this.fetch(forUserId);
        return view;
    }
}
```

--- 

#### Messy Object Creation ####

```
//Bad
const plane = new Airplane();
plane.type = PlaneType.Passenger;
plane.engine = new PassengerPlaneEngine();
plane.hasFirstClass = true;
plane.hasBathroom = false;
plane.numberOfSeats = 100;

//Good
//Example 1 factory function
const createPassengerPlane = (numberOfSeats: number): Airplane => {
    const plane = new Airplane();
    plane.type = PlaneType.Passenger;
    plane.engine = new PassengerPlaneEngine();
    plane.hasFirstClass = true;
    plane.hasBathroom = false;
    plane.numberOfSeats = numberOfSeats;
    return plane;
}

//Example 2
class AirplaneFactory {
    public static createPassengerPlane = (numberOfSeats: number): Airplane => {
        const plane = new Airplane();
        plane.type = PlaneType.Passenger;

        plane.engine = new PassengerPlaneEngine();
        plane.hasFirstClass = true;
        plane.hasBathroom = false;
        plane.numberOfSeats = numberOfSeats;
        return plane;
    }
}
```

In the previous section, we created a factory function. Given these new requirements, our function might look like this now:

```
//Bad
//Example 1
const createPlane =
    (
        type: PlaneType,
        engine: IPlaneEngine,
        hasFirstClass: boolean,
        hasBathroom: boolean,
        numberOfSeats: number
    ): Airplane => {
        const plane = new Airplane();
  
        plane.type = type;
        plane.engine = engine;
        plane.hasFirstClass = hasFirstClass;
        plane.hasBathroom = hasBathroom;
        plane.numberOfSeats = numberOfSeats;
        return plane;
    }
    
//When used, it might look like this:
const plane = createPlane(PlaneType.Passenger, new PassengerPlaneEngine(), true, true, 100);

//Example 2
class PlaneCreationOptions {
    type: PlaneType;
    engine: IPlaneEngine;
    hasFirstClass: boolean;
    hasBathroom: boolean;
    numberOfSeats: number;
}

//The only param now is the data object we created.
const createPlane = (options: PlaneCreationOptions): Airplane => {
    const plane = new Airplane();
    plane.type = options.type;
    plane.engine = options.engine;
    plane.hasFirstClass = options.hasFirstClass;
    plane.hasBathroom = options.hasBathroom;
    plane.numberOfSeats = options.numberOfSeats;
    return plane;
}

//It would look something like this in use:
const options = new PlaneCreationOptions();
options.type = PlaneType.Passenger;
options.engine = new PassengerPlaneEngine();
options.hasFirstClass = true;
options.hasBathroom = true;
options.numberOfSeats = 100;

const plane = createPlane(options);

//Good
const fullyLoadedPassengerOptions = () => {
    const options = new PlaneCreationOptions();
    options.type = PlaneType.Passenger;
    options.engine = new PassengerPlaneEngine();
    options.hasFirstClass = true;
    options.hasBathroom = true;
    options.numberOfSeats = 100;
    return options;
}

const bareBonesPassengerOptions = () => {
    const options = new PlaneCreationOptions();
    options.type = PlaneType.Passenger;
    options.engine = new PassengerPlaneEngine();
    options.hasFirstClass = false;
    options.hasBathroom = false;
    options.numberOfSeats = 10;
    return options;
}

class PlaneCreationOptions {
    //All the public properties...
    withSeats(numberOfSeats: number) {
        this.numberOfSeats = numberOfSeats;
        return this;
    }

    //Etc.
}

const plane1 = createPlane(fullyLoadedPassengerOptions());
const plane2 = createPlane(bareBonesPassengerOptions().withSeats(50));
```
