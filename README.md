# Post-Contenido 2 - Proyecto

## Diagrama UML textual 

classDiagram

    class Product {
        <<abstract>>
        +getName() String
        +getBasePrice() double
        +getCategory() String
        +calculateShipping() double
        +getDescription() String
    }
    class Electronics {
        +calculateShipping() double
        +getDescription() String
    }
    class Clothing {
        +calculateShipping() double
        +getDescription() String
    }
    class Food {
        +calculateShipping() double
        +getDescription() String
    }
    class ProductFactory {
        +createProduct(type, name, price) Product
    }

    Product <|-- Electronics
    Product <|-- Clothing
    Product <|-- Food
    ProductFactory ..> Product : crea

    %% PATRON OBSERVER
    class OrderSubject {
        <<interface>>
        +subscribe(observer) void
        +unsubscribe(observer) void
        +notifyObservers() void
    }
    class OrderObserver {
        <<interface>>
        +update(orderId, oldStatus, newStatus) void
    }
    class EmailNotifier {
        +update(orderId, oldStatus, newStatus) void
    }
    class SMSNotifier {
        +update(orderId, oldStatus, newStatus) void
    }
    class LogNotifier {
        +update(orderId, oldStatus, newStatus) void
    }

    OrderObserver <|.. EmailNotifier
    OrderObserver <|.. SMSNotifier
    OrderObserver <|.. LogNotifier

    %% PATRON STRATEGY
    class PricingStrategy {
        <<interface>>
        +calculateFinalPrice(price) double
        +getDescription() String
    }
    class RegularPricing {
        +calculateFinalPrice(price) double
        +getDescription() String
    }
    class BulkPricing {
        +calculateFinalPrice(price) double
        +getDescription() String
    }
    class MemberPricing {
        +calculateFinalPrice(price) double
        +getDescription() String
    }
    class BlackFridayPricing {
        +calculateFinalPrice(price) double
        +getDescription() String
    }

    PricingStrategy <|.. RegularPricing
    PricingStrategy <|.. BulkPricing
    PricingStrategy <|.. MemberPricing
    PricingStrategy <|.. BlackFridayPricing

    %% CLASE PRINCIPAL
    class OrderService {
        +addProduct(type, name, price) void
        +calculateTotal() double
        +changeStatus(newStatus) void
        +subscribe(observer) void
        +unsubscribe(observer) void
        +notifyObservers() void
    }

    OrderSubject <|.. OrderService
    OrderService o-- OrderObserver
    OrderService --> PricingStrategy
    OrderService ..> ProductFactory

## Justificacion de cada patron elegido
### Patron 'Observer'
Se eligio el patron 'Observer' porque permite establecer una relacion uno-a-muchos entre el objeto principal (Order) y varios observadores, de manera que cuando el estado del pedido cambia, todos los suscriptores son notificados automáticamente. Este patrón desacopla el objeto que genera el evento del objeto que reacciona al cambio, permitiendo agregar o eliminar nuevos tipos de notificaciones sin modificar la lógica del pedido, cumpliendo con el principio de Open/Closed, ya que el sistema puede extenderse con nuevos observadores sin alterar el código existente, y con el principio de Responsabilidad Única, al delegar la notificación a clases especializadas.

### Patron 'Factory Method'
Se eligio el patron 'Factory Method' porque permite crear diferentes tipos de productos (Electronics, Clothing, Food) sin acoplar el codigo cliente a las clases concretas. Asi, el sistema delega la responsabilidad de instanciar los distintos tipos de productos a una fabrica, lo que permite que el cliente trabaje con una abstraccion comun, que es 'Product'. A su vez, la utilizacion de este patron permite que el codigo cumpla con el principio de Open/Closed, ya que permite que el codigo sea extensible sin necesidad de ser modificado.

### Patron 'Strategy'
Se eligio el patron 'Strategy' porque permite encapsular diferentes algoritmos de calculo de precios dentro de clases independientes e intercambiables para que el sistema aplique distintas politicas de descuento sin modificar el codigo. Asi, el calculo del precio final se desacopla de la logica principal del pedido. Se garantiza el principio del Open/Closed al permitir el extender el comportamiento del sistema sin modificar su implementacion base y tambien se garantiza el SRP, ya que cada clase 'Pricing' se encarga de calcular el precio de forma exclusiva.

## Instrucciones de ejecucion
### Requisitos
- Java JDK 17 o superior instalado
- Terminal o consola de comandos

### Compilacion
Desde la carpeta raiz del proyecto, compilar todos los archivos con:
```
javac -d out Main.java factory/ProductFactory.java model/*.java observer/*.java service/OrderService.java strategy/*.java
```

### Ejecucion
Una vez compilado, ejecutar el programa con:
```
java -cp out Main
```

### Resultado esperado
Al ejecutar, se mostrara en consola el flujo completo de dos ordenes: una con precio regular y otra con descuento por volumen, incluyendo las notificaciones de cada cambio de estado via Email, SMS y Log. 
 