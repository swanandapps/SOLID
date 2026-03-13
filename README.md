# SOLID


## Class Outline


1. backstory
- talk about relevant story of chaos, confusion to clarity
- how important it is to have clean, redable, decoupled, scalable and maintainable code?
- we have a principles in place that every good engineer should be aware and consious about while writing code.


2. Introduction to SOLID

What is SOLID?
Why is SOLID?
When to use?

3. SOLID

- DEFINATION OF EACH
- Theoy oexample of each with Drawing
- Bad code
- Optimized code









## 1. Single Responsibility Principle



Copy

// S — Single Responsibility
// Pablo's bag does everything. When shampoo leaks, EVERYTHING pays.
 
// ❌ Before
class PablosBag {
  packClothes()     { console.log("Clothes in."); }
  packElectronics() { console.log("Charger in."); }
  packToiletries()  { console.log("Open shampoo in... 💥"); }
  // shampoo leaks → clothes ruined, electronics ruined, everything ruined
  // one class, every job, one disaster affects all ❌
}
 
// ✅ After — one class, one job
class ClothesBag    { pack() { console.log("Clothes — safe."); } }
class ElectronicsBag { pack() { console.log("Charger — safe."); } }
class ToiletriesBag  { pack() { console.log("Shampoo leaks... only here. ✅"); } }
 
// shampoo leaks? ClothesBag and ElectronicsBag don't even know. ✅
 



## 2. Open for extension closed for Modification


class PablosBag {
  pack(item: string) {
    if      (item === "clothes") { console.log("Clothes in."); }
    else if (item === "jacket")  { console.log("Jacket in."); }
    else if (item === "shoes")   { console.log("Shoes in."); }
    // bought snacks? come back and add another else if ❌
  }
}
 
// ✅ After — new item = new class, PablosBag never touched
class PablosBag {
  pack() { console.log("Clothes in."); }
}
class BagWithJacket extends PablosBag {
  pack() { console.log("Jacket in. ✅"); }
}
class BagWithShoes extends PablosBag {
  pack() { console.log("Shoes in. ✅"); }
}





## 3. Liskov Substitution Principle


class Payment {
  payUPI() { console.log("Scanning QR..."); }
}
class CashPayment extends Payment {
  payUPI() { throw new Error("I only have cash! 💀"); }
  // parent promised payUPI works. child broke that promise. ❌
}
 
// ✅ After
abstract class Payment {
  abstract pay(): void; // just pay — however you can
}
class CashPayment extends Payment {
  pay() { console.log("Cash handed over. 🍛 ✅"); }
}
class UPIPayment extends Payment {
  pay() { console.log("QR scanned. ✅"); }
}
// swap freely — nothing breaks ✅
 





## 4. Interface Seggregation



// I — Interface Segregation
// Don't force a class to implement what it doesn't need.
 
// ❌ Before — one fat interface, everyone must implement everything
interface PabloTasks {
  buyToothpaste(): void;
  buyPen(): void;
  messageManager(): void;
}
 
class ShopVisit implements PabloTasks {
  buyToothpaste() { console.log("Toothpaste bought."); }
  buyPen()        { console.log("Not supported."); }     // forced ❌
  messageManager(){ console.log("Not supported."); }     // forced ❌
}
 
// ✅ After — small focused interfaces
interface Shopper      { buy(): void; }
interface Communicator { message(): void; }
 
class ToothpasteRun implements Shopper {
  buy() { console.log("Toothpaste bought. ✅"); }
}
class ManagerPing implements Communicator {
  message() { console.log("Message sent clearly. ✅"); }
}



 ## 5. Dependency Inversion



 // ❌ Before
class Laptop {
  connect() { console.log("Joining from laptop."); }
}
class Mobile {
  connect() { console.log("Joining from mobile."); }
}

class PabloMeeting {
  private laptop = new Laptop();  // ❌
  private mobile = new Mobile();  // ❌

  join(type: string) {
    if      (type === "laptop") { this.laptop.connect(); }
    else if (type === "mobile") { this.mobile.connect(); }
  }
}

// ✅ After
interface Device {
  connect(): void;
}
class Laptop implements Device {
  connect() { console.log("Joining from laptop. ✅"); }
}
class Mobile implements Device {
  connect() { console.log("Joining from mobile. ✅"); }
}
class Tablet implements Device {
  connect() { console.log("Joining from tablet. ✅"); }
}

class PabloMeeting {
  private device: Device;
  constructor(device: Device) {
    this.device = device;  // injected ✅
  }
  join() { this.device.connect(); }
}

new PabloMeeting(new Laptop()).join();
new PabloMeeting(new Mobile()).join();
new PabloMeeting(new Tablet()).join();  // new device, zero changes ✅
