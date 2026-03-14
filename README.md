# SOLID Principles --- 1 Hour Live Lecture Script

**Language Context:** TypeScript\
**Audience:** Beginner--Intermediate Developers\
**Goal:** Understand what SOLID is, why it exists, and how to apply it
**Pre-requistes:** Typescript, OOP

# The difference between Sprint and Marathon

sprint the objective is very clear, reach finish line as quickly as possible.

marathon, yes, you also want to finsh quick but in the back of mind you have to plan what speed you will start with, when to accelerate, its a more systematic approch to winning. It requires more long term thinking.

# 0--5 Minutes --- The Problem With Code that Just works is that its sprinted

It feels good, when you write a code that works, no immediate bugs, no warnings, no errors.
But your code is a part of codebase and codebase is not sprint, its marathon, its long term health deoends on multiple engineers
pushing the code, without a system, this thing collapses.

We act lazy, get things done somehow, but at one point we realise, there is a better way to do this, i can organise this so that it will cause me less trouble, this long term vision is why absent in coding.

why our end goal is always making the code work and forgetting the design and longevity part of it.

## Script

Imagine we are building a software project.

At the beginning everything works fine.

But after a few months: - Code becomes messy - Adding features becomes
risky - Bugs appear everywhere - Multiple developers struggle to
maintain the system

This happens because the system **was not designed properly**.

To solve this, developers follow **design principles**.

One of the most important sets of design principles is:

# SOLID

These principles help us build:

- Maintainable systems
- Scalable software
- Flexible architecture

---

# 5--8 Minutes --- What is SOLID

SOLID is an acronym for **five design principles**.

---

S Single Responsibility Principle
O Open Closed Principle
L Liskov Substitution Principle
I Interface Segregation Principle
D Dependency Inversion Principle

The goal of SOLID is to help developers write **clean architecture
code**. This is about marathon not sprint.

---

# 8--18 Minutes --- Single Responsibility Principle (SRP)

## Definition

A class should have **only one responsibility**.

Or:

A class should have **only one reason to change**.

---

## Bad Example

```ts
class OnlineStore {
  createOrder(item: string, price: number) {
    console.log("Order created:", item);
  }

  deliverOrder(price: number) {
    console.log("Payment processed:", price);
  }

  RefundOrder(item: string) {
    console.log("Saved to DB:", item);
  }
}
```

### Problem

This class does too many things:

- Order logic
- Payment logic
- Database logic
- Email logic

This is called a **God Class**.

---

## Good Example

```ts
class createOrder {}

class deliverOrder {}

class RefundOrder {}
```

Each class now has **one responsibility**.

---

## Real World Analogy

Hospital roles:

Role Responsibility

---

Doctor Diagnose patients
Pharmacist Provide medicine
Receptionist Handle appointments

Each role has **one responsibility**.

That is SRP.

Quick Question:

Does this mean a Class can have only one method.
No, the goal is to have one responsibility, helper functions can help achieve that.

---

# 18--28 Minutes --- Open Closed Principle (OCP)

## Definition

Software entities should be:

- **Open for extension**
- **Closed for modification**

Meaning:

We should be able to **add new features without changing existing
code**.

---

## Bad Example

```ts
class PaymentProcessor {
  pay(type: string, amount: number) {
    if (type === "cash") {
      console.log("Cash payment");
    } else if (type === "card") {
      console.log("Card payment");
    }
  }
}
```

### Problem

Every time we add a new payment type we must modify the class.

---

## Good Example

```ts
interface PaymentMethod {
  pay(amount: number): void;
}

class CashPayment implements PaymentMethod {
  pay(amount: number) {
    console.log("Paid using Cash");
  }
}

class CardPayment implements PaymentMethod {
  pay(amount: number) {
    console.log("Paid using Card");
  }
}
```

Now we can add:

- UPI
- Crypto
- PayPal

Without modifying existing code.

---

# 28--38 Minutes --- Liskov Substitution Principle (LSP)

## Definition

Objects of a subclass should be able to **replace objects of the parent
class** without breaking the program.

---

## Bad Example

```ts
class Account {
  deposit(accountNumber: number, amount: number) {}

  withdraw(accountNumber: number, amount: number) {}

  checkBalence(accountNumber: number) {}
}

class SavingsAccount extends Account {}

class CurrentAccount extends Account {}
class FixedDepositAccount extends Account {}
```

Problem:

Penguin cannot replace Bird.

This violates LSP.

---

## Correct Design

```ts
class NonWithdrwableAccount {
  deposit(accountNumber: number, amount: number) {}

  checkBalence(accountNumber: number) {}
}

class WithdrwableAccount extends NonWithdrwableAccount {
  withdraw(accountNumber: number, amount: number) {}
}

class SavingsAccount extends WithdrwableAccount {}

class CurrentAccount extends WithdrwableAccount {}
class FixedDepositAccount extends NonWithdrwableAccount {}
```

Now substitution works correctly.

---

# 38--48 Minutes --- Interface Segregation Principle (ISP)

## Definition

Clients should not be forced to depend on methods they do not use.

Let's say you are building an app where you need to calculate area and volume of geometric shpaes.

---

## Bad Interface

```ts
interface shape {
  area(): void;
  volume(): void;
}
```

Robot example:

```ts
class Square implements shape {
  area();
}
class Rectangle implements shape {
  area();
}

class Cube implements shape {
  area();
}
```

---

## Good Design

```ts
interface TwoDimensionalShape {
  area(): void;
}

interface ThreeDimensionalShape {
  area(): void;
  volume(): void;
}

class Square implements TwoDimensionalShape {
  area();
}
class Rectangle implements TwoDimensionalShape {
  area();
}

class Cube implements ThreeDimensionalShape {
  area();
  volume();
}
```

Now classes implement only what they need.

---

# 48--57 Minutes --- Dependency Inversion Principle (DIP)

## Definition

High-level modules should not depend on low-level modules.

Both should depend on **abstractions**.

---

## Bad Design

```ts
class EmailService {
  sendEmail(message: string): void {
    console.log("Sending email: " + message);
  }
}

class SMSService {
  sendSMS(message: string): void {
    console.log("Sending SMS: " + message);
  }
}

class NotificationService {
  private emailService: EmailService;
  private smsService: SMSService;

  constructor() {
    this.emailService = new EmailService();
    this.smsService = new SMSService();
  }

  sendNotification(message: string, type: string): void {
    if (type === "email") {
      this.emailService.sendEmail(message);
    } else if (type === "sms") {
      this.smsService.sendSMS(message);
    }
  }
}

const service = new NotificationService();

service.sendNotification("Hello User", "email");
```

Problem:

If database changes, the service must change.

---

## Good Design

```ts
// ===== Interface (Abstraction) =====

interface NotificationSender {
  send(message: string): void;
}

// ===== Email Service =====

class EmailService implements NotificationSender {
  send(message: string): void {
    console.log("Sending Email: " + message);
  }
}

// ===== SMS Service =====

class SMSService implements NotificationSender {
  send(message: string): void {
    console.log("Sending SMS: " + message);
  }
}

// ===== WhatsApp Service (easy to add later) =====

class WhatsAppService implements NotificationSender {
  send(message: string): void {
    console.log("Sending WhatsApp: " + message);
  }
}

// ===== Notification Service =====

class NotificationService {
  private sender: NotificationSender;

  constructor(sender: NotificationSender) {
    this.sender = sender;
  }

  sendNotification(message: string): void {
    this.sender.send(message);
  }
}

// ===== Application =====

// choose notification method
const emailSender = new EmailService();

const notificationService = new NotificationService(emailSender);

notificationService.sendNotification("Hello User");

// change to SMS easily
const smsSender = new SMSService();

const smsNotification = new NotificationService(smsSender);

smsNotification.sendNotification("Hello via SMS");
```

---

# 57--60 Minutes --- Final Recap

Principle Simple Meaning

---

SRP One class → one job
OCP Extend code without modifying
LSP Child classes must behave like parent
ISP Small focused interfaces
DIP Depend on abstractions

---

# Final Questions for Students

1.  Which principle prevents **God Classes**?
2.  Which principle allows adding new features without modifying code?
3.  Why do we use **interfaces in Dependency Inversion**?

---

# One Sentence Summary

SOLID helps us write **clean, maintainable, scalable software**.

# BONUS Material

DIFFERENCE BETWEEN INTERFACE VS Extends ( Rule vs Inheritence
)

In this example, the interface PaymentMethod acts as a contract, requiring every payment class to implement a pay(amount) method, but it does not provide the implementation. Classes like CardPayment implement this interface and define how the payment works. Meanwhile, the PaymentLogger class provides shared functionality through the log() method, and child classes use extends to inherit and reuse this behavior. In short, interfaces define required methods (rules), while extends allows classes to inherit and reuse actual code (shared behavior).

```ts
interface PaymentMethod {
  pay(amount: number): void;
}

class PaymentLogger {
  log(message: string) {
    console.log("LOG:", message);
  }
}

class CardPayment extends PaymentLogger implements PaymentMethod {
  pay(amount: number): void {
    this.log("Processing card payment");
    console.log("Paid:", amount);
  }
}

const payment = new CardPayment();
payment.pay(100);
```
