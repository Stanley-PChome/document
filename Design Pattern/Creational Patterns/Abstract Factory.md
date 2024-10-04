```
Abstract Factory（抽象工廠模式） 是一種創建型設計模式，允許在不指定具體類別的情況下，創建一組
相關或依賴的物件。此模式主要用於處理多個產品等級結構（product hierarchies），通常涉及到一系列相互關聯的產品。

Abstract Factory 模式的結構與概念
  抽象工廠模式允許客戶端使用工廠方法創建一組「相關」的產品，而不需要了解具體產品的類別。比如，
  針對不同的使用環境或平台（如 Windows 和 Mac），我們可以通過同一抽象工廠來生產不同的 UI 元素（如按鈕、視窗等）。

  抽象工廠模式角色：
    Abstract Factory（抽象工廠）：
      定義了所有產品創建的方法。
    Concrete Factory（具體工廠）：
      實現抽象工廠的方法，負責具體產品物件的生成。
    Abstract Product（抽象產品）：
      定義所有具體產品的共同接口。
    Concrete Product（具體產品）：
      具體產品的實現類別。
    Client（客戶端）：
      通過抽象工廠來取得抽象產品的具體實例，而不需要知道具體工廠和具體產品的類別。

Abstract Factory 的優點與缺點
  優點：
    符合開放封閉原則（OCP）：
      新增產品系列時，不需要更改現有的工廠代碼。
    封裝產品族：
      抽象工廠創建的是一組相關的產品，確保一個工廠生成的產品是相互兼容的。
    減少耦合性：
      客戶端代碼通過抽象接口與具體工廠互動，避免了直接依賴具體產品類別。
  缺點：
    類別數量增多：
      每個產品族都需要創建相應的工廠類別，類別數量可能會迅速增長。
    難以擴展新產品：
      如果需要在同一產品族中新增產品，則必須修改所有工廠接口及其具體實現。

使用情境
  需要創建多個相關的物件時：
      如 UI 工具包中的按鈕、文字框、視窗等。
  希望保證一組物件的一致性時：
      如需要創建不同主題風格的 UI 元素時。
  需要隔離物件的生成邏輯：
      避免直接使用 new 關鍵字創建具體產品，從而提高系統的靈活性。
```

```php
<?php
// 1. 抽象產品：Button 介面
interface Button {
    public function render();
}

// 2. 抽象產品：TextBox 介面
interface TextBox {
    public function render();
}

// 3. 具體產品：深色主題按鈕
class DarkButton implements Button {
    public function render() {
        echo "Render a dark button.\n";
    }
}

// 4. 具體產品：深色主題文字框
class DarkTextBox implements TextBox {
    public function render() {
        echo "Render a dark text box.\n";
    }
}

// 5. 具體產品：淺色主題按鈕
class LightButton implements Button {
    public function render() {
        echo "Render a light button.\n";
    }
}

// 6. 具體產品：淺色主題文字框
class LightTextBox implements TextBox {
    public function render() {
        echo "Render a light text box.\n";
    }
}

// 7. 抽象工廠：UIFactory 介面
interface UIFactory {
    public function createButton(): Button;
    public function createTextBox(): TextBox;
}

// 8. 具體工廠：深色主題工廠
class DarkThemeFactory implements UIFactory {
    public function createButton(): Button {
        return new DarkButton();
    }

    public function createTextBox(): TextBox {
        return new DarkTextBox();
    }
}

// 9. 具體工廠：淺色主題工廠
class LightThemeFactory implements UIFactory {
    public function createButton(): Button {
        return new LightButton();
    }

    public function createTextBox(): TextBox {
        return new LightTextBox();
    }
}

// 10. 客戶端代碼：通過抽象工廠來創建 UI 元素
function renderUIComponents(UIFactory $factory) {
    $button = $factory->createButton();
    $textbox = $factory->createTextBox();

    $button->render();  // Render 按鈕
    $textbox->render(); // Render 文字框
}

// 測試範例
echo "Rendering dark theme components:\n";
renderUIComponents(new DarkThemeFactory());  // 輸出：Render a dark button. Render a dark text box.

echo "\n";

echo "Rendering light theme components:\n";
renderUIComponents(new LightThemeFactory()); // 輸出：Render a light button. Render a light text box.
?>

```
```php

```
