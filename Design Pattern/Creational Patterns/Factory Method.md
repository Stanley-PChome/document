```
Factory Method（工廠方法模式） 是一種創建型設計模式，定義了一個用來創建物件的接口，但由子類別決定
要實例化的類別是哪一個。這樣做可以在不改變客戶端程式碼的情況下來引入新的產品類型，從而實現對擴展開放、對修改封閉（OCP 原則）。

Factory Method 模式結構與概念
  Factory Method 模式包含以下角色：
    Product：抽象產品，定義了產品物件的接口或抽象類別。
    Concrete Product：具體產品類別，實現了抽象產品接口。
    Creator（工廠類）：定義了工廠方法（通常為 createProduct()），來返回 Product 類別的實例，但並不具體定義哪個類別。
    Concrete Creator（具體工廠類別）：實現 Creator 並定義 createProduct() 方法來返回具體產品。

Factory Method 的優點與缺點
  優點：
    遵循開放封閉原則（OCP）：
      當需要引入新產品時，只需創建一個新的 Concrete Product 類別和對應的 Concrete Factory 類別即可，而無需更改現有代碼。
    更高的靈活性：
      可以在運行時指定具體產品，而不是在編譯時綁定。
    簡化客戶端程式碼：
      客戶端不需要知道具體的產品類別，只需知道如何調用工廠方法即可。
  缺點：
    增加類別數量：
      每次引入新的產品類別都需要創建相應的工廠類別，導致類別數量增多。
    抽象與具體類別的依賴增加：
      所有的具體工廠都需要實現抽象工廠中的工廠方法，這導致系統的複雜度提升。

Factory Method 模式的應用情境
  當需要創建多種類型的物件，且這些物件有共同的接口或抽象父類。
  當物件的創建涉及複雜邏輯時（如需要根據不同的參數創建不同類型的物件）。
  當物件的創建應該由子類別來確定，而不是基礎類別。
  在架構中需要隱藏具體產品的實例化過程時。

延伸：將 Factory Method 與其他模式結合
  Factory Method 通常與以下設計模式搭配使用：
    Abstract Factory（抽象工廠模式）：抽象工廠模式可以視為多個 Factory Method 的組合，允許創建一系列相關的產品。
    Singleton（單例模式）：某些情況下，工廠類別本身可以被設計成 Singleton，確保只有一個工廠實例。
```

```php
<?php
// 1. 定義抽象產品接口
interface Vehicle {
    public function drive();
}

// 2. 定義具體產品：Car 類別
class Car implements Vehicle {
    public function drive() {
        echo "Driving a car...\n";
    }
}

// 3. 定義具體產品：Bicycle 類別
class Bicycle implements Vehicle {
    public function drive() {
        echo "Riding a bicycle...\n";
    }
}

// 4. 定義抽象工廠 Creator 類別
abstract class VehicleFactory {
    // 抽象方法：定義工廠方法來創建產品，但不指定具體產品
    abstract public function createVehicle(): Vehicle;

    // 具體方法：通過工廠方法來取得具體產品並進行操作
    public function deliverVehicle() {
        $vehicle = $this->createVehicle();
        $vehicle->drive();
    }
}

// 5. 定義具體工廠：CarFactory 類別
class CarFactory extends VehicleFactory {
    // 工廠方法：返回具體產品 Car 的實例
    public function createVehicle(): Vehicle {
        return new Car();
    }
}

// 6. 定義具體工廠：BicycleFactory 類別
class BicycleFactory extends VehicleFactory {
    // 工廠方法：返回具體產品 Bicycle 的實例
    public function createVehicle(): Vehicle {
        return new Bicycle();
    }
}

// 7. 使用 Factory Method 模式來創建產品
function clientCode(VehicleFactory $factory) {
    // 使用具體工廠來創建並操作產品
    $factory->deliverVehicle();
}

// 測試範例
echo "Client: Testing CarFactory...\n";
clientCode(new CarFactory());  // 輸出：Driving a car...

echo "\n";

echo "Client: Testing BicycleFactory...\n";
clientCode(new BicycleFactory());  // 輸出：Riding a bicycle...
?>

```

```php
<?php

namespace RefactoringGuru\FactoryMethod\RealWorld;

/**
 * The Creator declares a factory method that can be used as a substitution for
 * the direct constructor calls of products, for instance:
 *
 * - Before: $p = new FacebookConnector();
 * - After: $p = $this->getSocialNetwork;
 *
 * This allows changing the type of the product being created by
 * SocialNetworkPoster's subclasses.
 */
abstract class SocialNetworkPoster
{
    /**
     * The actual factory method. Note that it returns the abstract connector.
     * This lets subclasses return any concrete connectors without breaking the
     * superclass' contract.
     */
    abstract public function getSocialNetwork(): SocialNetworkConnector;

    /**
     * When the factory method is used inside the Creator's business logic, the
     * subclasses may alter the logic indirectly by returning different types of
     * the connector from the factory method.
     */
    public function post($content): void
    {
        // Call the factory method to create a Product object...
        $network = $this->getSocialNetwork();
        // ...then use it as you will.
        $network->logIn();
        $network->createPost($content);
        $network->logout();
    }
}

/**
 * This Concrete Creator supports Facebook. Remember that this class also
 * inherits the 'post' method from the parent class. Concrete Creators are the
 * classes that the Client actually uses.
 */
class FacebookPoster extends SocialNetworkPoster
{
    private $login, $password;

    public function __construct(string $login, string $password)
    {
        $this->login = $login;
        $this->password = $password;
    }

    public function getSocialNetwork(): SocialNetworkConnector
    {
        return new FacebookConnector($this->login, $this->password);
    }
}

/**
 * This Concrete Creator supports LinkedIn.
 */
class LinkedInPoster extends SocialNetworkPoster
{
    private $email, $password;

    public function __construct(string $email, string $password)
    {
        $this->email = $email;
        $this->password = $password;
    }

    public function getSocialNetwork(): SocialNetworkConnector
    {
        return new LinkedInConnector($this->email, $this->password);
    }
}

/**
 * The Product interface declares behaviors of various types of products.
 */
interface SocialNetworkConnector
{
    public function logIn(): void;

    public function logOut(): void;

    public function createPost($content): void;
}

/**
 * This Concrete Product implements the Facebook API.
 */
class FacebookConnector implements SocialNetworkConnector
{
    private $login, $password;

    public function __construct(string $login, string $password)
    {
        $this->login = $login;
        $this->password = $password;
    }

    public function logIn(): void
    {
        echo "Send HTTP API request to log in user $this->login with " .
            "password $this->password\n";
    }

    public function logOut(): void
    {
        echo "Send HTTP API request to log out user $this->login\n";
    }

    public function createPost($content): void
    {
        echo "Send HTTP API requests to create a post in Facebook timeline.\n";
    }
}

/**
 * This Concrete Product implements the LinkedIn API.
 */
class LinkedInConnector implements SocialNetworkConnector
{
    private $email, $password;

    public function __construct(string $email, string $password)
    {
        $this->email = $email;
        $this->password = $password;
    }

    public function logIn(): void
    {
        echo "Send HTTP API request to log in user $this->email with " .
            "password $this->password\n";
    }

    public function logOut(): void
    {
        echo "Send HTTP API request to log out user $this->email\n";
    }

    public function createPost($content): void
    {
        echo "Send HTTP API requests to create a post in LinkedIn timeline.\n";
    }
}

/**
 * The client code can work with any subclass of SocialNetworkPoster since it
 * doesn't depend on concrete classes.
 */
function clientCode(SocialNetworkPoster $creator)
{
    // ...
    $creator->post("Hello world!");
    $creator->post("I had a large hamburger this morning!");
    // ...
}

/**
 * During the initialization phase, the app can decide which social network it
 * wants to work with, create an object of the proper subclass, and pass it to
 * the client code.
 */
echo "Testing ConcreteCreator1:\n";
clientCode(new FacebookPoster("john_smith", "******"));
echo "\n\n";

echo "Testing ConcreteCreator2:\n";
clientCode(new LinkedInPoster("john_smith@example.com", "******"));
```
