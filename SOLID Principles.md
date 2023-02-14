# SOLID Principles

- [x]  The Single Responsibility Principle
- [x]  The Open / Close Principle
- [x]  The Liskov Substitution Principle
- [x]  The Interface Segmented Principle
- [x]  The Dependency Inversion Principle

# Advantages of following these Principles

Help us to write better code:

- Avoid Duplicate code
- Easy to maintain
- Easy to understand
- Flexible software
- Reduces Complexity

## Single Responsibility Principle

A class should do one thing and therefore it should have only one reason to change.

 Book Class

```python
class Book:
    """Book class"""

    def __init__(self, name, authorName, year, price, isbn):
        self.name = name
        self.authorName = authorName
        self.year = year
        self.price = price
        self.isbn = isbn

class Invoice:
    """Invoice Class"""

    def __init__(self, book: Book, quantity: int, discountRate: float, taxRate: float):
        self.book = self.book
        self.quantity = quantity
        self.discountRate = discountRate
        self.taxRate = taxRate
        self.total = self.calculateTotal()

    def calculateTotal(self) -> float:
        price = (self.book.price - self.book.price * self.discountRate) * self.quantity
        priceWithTaxes = price * (1 + self.taxRate)
        return priceWithTaxes

    def printInvoice(self):
        print(self.quantity + "x " + self.book.name + " " + self.book.price + "₹")
        print("Discount Rate: " + self.discountRate)
        print("Tax Rate: " + self.taxRate)
        print("Total " + self.total)

    def saveToFile(self, filename: str):
        pass
```

According to our principle a class should have only one reason to change but look at the above code it need to be changed when any of the three method:

- calculateTotal
- printInvoice
- saveToFile

Changes which made our above Invoice class inconsistent with the Single Responsibility Principle.

Modifying the above code to follow the Principle.

 

```python
class Book:
    """Book class"""

    def __init__(self, name, authorName, year, price, isbn):
        self.name = name
        self.authorName = authorName
        self.year = year
        self.price = price
        self.isbn = isbn

class Invoice:
    """Invoice Class"""

    def __init__(self, book: Book, quantity: int, discountRate: float, taxRate: float):
        self.book = self.book
        self.quantity = quantity
        self.discountRate = discountRate
        self.taxRate = taxRate
        self.total = self.calculateTotal()

    def calculateTotal(self) -> float:
        price = (self.book.price - self.book.price * self.discountRate) * self.quantity
        priceWithTaxes = price * (1 + self.taxRate)
        return priceWithTaxes

class InvoicePrinter:
    def __init__(self, invoice: Invoice):
        self.invoice = invoice

    def printInvoice(self):
        print(
            self.invoice.quantity
            + "x "
            + self.invoice.book.name
            + " "
            + self.invoice.book.price
            + "₹"
        )
        print("Discount Rate: " + self.invoice.discountRate)
        print("Tax Rate: " + self.invoice.taxRate)
        print("Total " + self.invoice.total)

class InvoicePersistence:
    def __init__(self, invoice: Invoice):
        self.invoice = invoice

    def saveToFile(self, filename: str):
        pass
```

## O - Open/Closed Principle

Class should be open for extension but closed to modification.

Modification means changing the code of an existing class, and extension means adding new functionality.

This means we should write interface classes to fit all current and upcoming possible requirements.

This Principle ensures we are not creating bugs by mistake in the running production code.

```python
class InvoicePersistence:
    def __init__(self, invoice: Invoice):
        self.invoice = invoice

    @abstractmethod
    def save(self, filename: str):
        pass

class DatabasePersistence(InvoicePersistence):
    def save(self):
        pass
        # Need to be implemented here
        # save to DB

class FilePersistence(InvoicePersistence):
    def save(self):
        pass
        # Need to be implemented here
        # save to file
```

## L - Liskov Substitution Principle

If class B is subtype of Class A, then we should be able to replace object of A with B without breaking the behaviour of the program.

Subclass should extend the capability of parent class not narrow it down.

```python
class Rectangle:
    def __init__(self, width: int, height: int):
        self.width = width
        self.height = height

    def getWidth(self) -> int:
        return self.width

    def setWidth(self, width: int):
        self.width = width

    def getHeight(self) -> int:
        return self.height

    def setHeight(self, height: int):
        self.height = height

    def getArea(self):
        return self.height * self.width

class Square(Rectangle):
    def __init__(self, size: int):
        self.width = size
        self.height = size

    def setWidth(self, width: int):
        super().setWidth(width)
        super().setHeight(width)

    def setHeight(self, height: int):
        super().setHeight(height)
        super().setHeight(height)

class Test:
    def getAreaTest(self, r: Rectangle) -> int:
        width = r.getWidth()
        r.setHeight(10)
        print("Expected Area of " + width * 10 + ", got " + r.getArea())

    def test(self):
        rc = Rectangle(2, 3)
        self.getAreaTest(rc)

        sq = Square(5)
        self.getAreaTest(sq)
```

Above Code fails the square test case hence does not follows Liskov Substitution Principle.

## I - Interface Segregation Principle

Segregation means keeping things separated, and the Interface Segregation Principle is about separating the interfaces.

The Principle states that many client-specific interfaces are better than one general-purpose interface. Client should not be forced to implement a function they do not need.

```python
class ParkingLot:
    @abstractmethod
    def parkCar(self):
        pass

    @abstractmethod
    def unparkCar(self):
        pass

    @abstractmethod
    def getCapacity(self):
        pass

    @abstractmethod
    def calculateFee(self, car: Car) -> float:
        pass

    @abstractmethod
    def doPayment(self, car: Car):
        pass

class FreeParking(ParkingLot):
    def parkCar(self):
        pass
    def unparkCar(self):
        pass
    def getCapacity(self):
        pass
    def calculateFee(self, car: Car) -> float:
        return 0
    def doPayment(self, car: Car):
        raise  RuntimeError("Parking lot is free")
```

Need at-least two interface

- FreeParkingLot
- PaidParkingLot

## D - Dependency Inversion Principle

The Dependency Inversion Principle states that our classes should depend upon interfaces or abstract classes instead of concrete classes and functions.

Example: Our PersistenceManager class depends on InvoicePersistence instead of the class that implement that interface.
