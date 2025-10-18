***

### مقدمه: چرا کلاس؟

وقتی می‌خواهیم داده‌ها و رفتارهای مرتبط با آنها را در برنامه مدیریت کنیم، نگهداری داده‌ها به صورت پراکنده خیلی زود برنامه را پیچیده و غیرقابل نگهداری می‌کند. کلاس‌ها و Data Classها به ما کمک می‌کنند تا داده‌ها و رفتارها را به صورت منظم و قابل استفاده مجدد سازماندهی کنیم.

***

### 1. مشکل مدل‌سازی بدون کلاس

بدون استفاده از کلاس، اگر بخواهیم اطلاعات چند دانشجو را ذخیره کنیم، مجبوریم متغیرهای پراکنده و جداگانه داشته باشیم:

```kotlin
var student1Name = "Ali"
var student1Age = 20
var student2Name = "Sara"
var student2Age = 22
```

تغییر، افزودن یا مدیریت اطلاعات پراکنده باعث پیچیدگی، کدهای تکراری و در نهایت خطا می‌شود.

***

### 2. استفاده از Data Class برای مدیریت داده‌ها

تعریف یک Data Class برای مدل دانشجو:

```kotlin
data class Student(var name: String, var age: Int)

fun main() {
    var student = Student("Ali", 20)
    while (true) {
        println(
            """
            |Menu:
            |1 - Show student info
            |2 - Change name
            |3 - Change age
            |0 - Exit
        """.trimMargin()
        )
        print("Enter your choice: ")
        when (readln()) {
            "1" -> println(student)
            "2" -> {
                print("Enter new name: ")
                student.name = readln()
            }
            "3" -> {
                print("Enter new age: ")
                student.age = readln().toIntOrNull() ?: student.age
            }
            "0" -> break
            else -> println("Invalid choice")
        }
        println()
    }
}
```

Data Class به طور خودکار توابع مثل `toString()` و `copy()` را دارد و مدیریت داده‌ها را بسیار ساده می‌کند.

***

### 3. کلاس معمولی (non-data class) برای ShoppingCart

نمونه کد کلاس مدیریت سبد خرید:

```kotlin
class ShoppingCart {
    private val items = mutableListOf<String>()

    fun addItem(item: String) {
        items.add(item)
        println("$item added to cart.")
    }

    fun removeItem(item: String) {
        if (items.remove(item)) println("$item removed from cart.")
        else println("$item not found in cart.")
    }

    fun showItems() {
        if (items.isEmpty()) println("Cart is empty.")
        else items.forEach { println("- $it") }
    }

    fun clearCart() {
        items.clear()
        println("Cart cleared.")
    }
}

fun main() {
    val cart = ShoppingCart()

    while (true) {
        println(
            """
            |Menu:
            |1 - Add item
            |2 - Remove item
            |3 - Show items
            |4 - Clear cart
            |0 - Exit
        """.trimMargin()
        )
        print("Enter your choice: ")
        when (readln()) {
            "1" -> {
                print("Enter item to add: ")
                cart.addItem(readln())
            }
            "2" -> {
                print("Enter item to remove: ")
                cart.removeItem(readln())
            }
            "3" -> cart.showItems()
            "4" -> cart.clearCart()
            "0" -> break
            else -> println("Invalid choice")
        }
        println()
    }
}
```

این کلاس علاوه بر نگهداری داده، دارای رفتارهایی مانند افزودن، حذف، و نمایش است که آن را از یک Data Class ساده متمایز می‌کند.

***

### 4. افزودن ارث‌بری به ShoppingCart

یک کلاس فرزند برای افزودن قابلیت‌های جدید مثل تخفیف:

```kotlin
open class ShoppingCart {
    protected val items = mutableListOf<String>()

    fun addItem(item: String) {
        items.add(item)
        println("$item added to cart.")
    }

    fun removeItem(item: String) {
        if (items.remove(item)) println("$item removed from cart.")
        else println("$item not found in cart.")
    }

    fun showItems() {
        if (items.isEmpty()) println("Cart is empty.")
        else items.forEach { println("- $it") }
    }

    fun clearCart() {
        items.clear()
        println("Cart cleared.")
    }
}

class PremiumShoppingCart : ShoppingCart() {
    fun applyDiscount(code: String) {
        println("Discount code $code applied!")
    }
}

fun main() {
    val cart = PremiumShoppingCart()

    while (true) {
        println(
            """
            |Menu:
            |1 - Add item
            |2 - Remove item
            |3 - Show items
            |4 - Clear cart
            |5 - Apply discount
            |0 - Exit
        """.trimMargin()
        )
        print("Enter your choice: ")
        when (readln()) {
            "1" -> {
                print("Enter item: ")
                cart.addItem(readln())
            }
            "2" -> {
                print("Enter item: ")
                cart.removeItem(readln())
            }
            "3" -> cart.showItems()
            "4" -> cart.clearCart()
            "5" -> {
                print("Enter discount code: ")
                cart.applyDiscount(readln())
            }
            "0" -> break
            else -> println("Invalid choice")
        }
        println()
    }
}
```

***

# جمع‌بندی

| بخش           | توضیح                                             | مثال                          |
|---------------|--------------------------------------------------|-------------------------------|
| بدون کلاس     | داده‌ها پراکنده و مدیریت سخت                     | متغیرهای جداگانه دانشجو       |
| Data Class    | مدیریت داده ساده، خودکارسازی توابع مرتبط          | مدل Student                   |
| کلاس معمولی  | اضافه شدن رفتارها و قابلیت‌های مدیریتی            | ShoppingCart                   |
| ارث‌بری      | گسترش کلاس با افزودن قابلیت‌های جدید به صورت فرزند | PremiumShoppingCart ارث‌بری شده |

***

این جزوه به شما کمک می‌کند مفاهیم پایه‌ای برنامه‌نویسی شی‌ءگرا را به صورت کاربردی و گام‌به‌گام یاد بگیرید و در پروژه‌هایتان استفاده کنید.  
