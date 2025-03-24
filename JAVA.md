
## Переменные

Ключевое слово `var` позволяет определять переменную (тип переменной, как `auto` в С++)

```java
var x = 10;
System.out.println(x); // 10
```

**Константы** определяются с помощью ключевого слова `final`:

```java
final int LIMIT = 5;
System.out.println(LIMIT);
LIMIT = 57; // -> error, так как LIMIT константа
```

Как правило, константы имеют имена в верхнем регистре.

## Типы данных

Java строготипизированный язык. 

- `boolean`: хранит `true` или `false`
- `byte`: занимает 1 байт, хранит от -128 до 127
- `short`: занимает 2 байта, хранит -32768 до 32767
- `int`: занимает 4 байта
- `long`: занимает 8 байт
- `double`: числа с плавающей точкой, 8 байт
- `float`: числа с плавающей точкой, 4 байта
- `char`: одиночный символ в кодировке UTF-16, **2 байта** (**unsigned по умолчанию**)

Для задания шестнадцатеричного значения после символов **0x** указывается число в шестнадцатеричном формате. Таким же образом восьмеричное значение указывается после символа 0, а двоичное значение - после символов **0b.**

```java
int num111 = 0x6F; // 16-теричная система, число 111`
int num8 = 010; // 8-ричная система, число 8`
int num13 = 0b1101; // 2-ичная система, число 13`
```

Символьные переменные не стоит путать со строковыми, **'a' не идентично "a"**.

## Ввод с консоли

Для получения ввода с консоли в классе `System` определен объект `in.` Однако непосредственно через объект `System.in` не очень удобно работать, поэтому, как правило, используют класс `Scanner`, который, в свою очередь использует `System.in`

Так как класс `Scanner` находится в пакете java.util, то мы вначале его импортируем с помощью инструкции `import java.util.Scanner`.

```java
import java.util.Scanner;

public class Start {  
    public static void main(String[] args) {  
        Scanner in = new Scanner(System.in);  
        int num = in.nextInt();  
        System.out.println(num + 1);  
    }  
}
```

Класс `Scanner` имеет еще ряд методов, которые позволяют захватить ввода с клавы:
- `next()` - считает введенную строку до первого пробела
- `nextLine()` - считывает всю введенную строку
- `nextInt()` - считывает `int`
- `nextDouble()` - считывает `double`
- `nextBoolean()` - считывает `boolean`
- `nextByte()` - считывает `byte`
- `nextFloat()` - считывает `float`
- `nextShort()` - считывает `short`

## Конструкция switch

```java
public static void main(String[] args) {  
    Scanner in = new Scanner(System.in);  
    byte number = in.nextByte();  
    switch (number) {  
        case 1:  
            System.out.println("Number == 1");  
            break;  
        case 2:  
            System.out.println("Number == 2");  
            break;  
        case 8:  
            System.out.println("Number == 8");  
            break;  
        default:  
            System.out.println("Number == unique value");  
    }  
}
```

После ключевого слова `switch` в скобках идет сравниваемое выражение. Значение этого выражения последовательно сравнивается со значениями, помещенными после операторов `сase`. И если совпадение найдено, то будет выполняет соответствующий блок `сase`.

Также можно определить одно действия для нескольких блоков `case` подряд:

```java
public static void main(String[] args) {  
    System.out.println("input number 0 <= x <= 9");  
    Scanner in = new Scanner(System.in);  
    byte number = in.nextByte();  
    switch (number) {  
        case 1:  
        case 3:  
        case 5:  
        case 7:  
        case 9:  
            System.out.println("This number is odd");  
            break;  
        case 2:  
        case 4:  
        case 6:  
        case 8:  
        case 0:  
            System.out.println("This number is even");  
            break;  
        default:  
            System.out.println("Out of range");  
    }  
}
```

## Тернарный оператор

**Тернарный оператор** имеет следующий синтаксис:
`[условие] ? [то, что выполняется при true] : [то, что выполняется при false]`

```java
public static void main(String[] args) {  
    Scanner in = new Scanner(System.in);  
    int x = in.nextInt();  
    int y = in.nextInt();  
    int z = x<y ? (x + y) : (x - y);  
    System.out.println(z);  
}
```

## Методы

### Параметр переменной длины

Метод может принимать параметры переменной длины одного типа.

```java
public class Start {  
    public static void main(String[] args) {  
        System.out.println(sum(2, 3, 4, 5, 11));  
    }  
    static int sum(int ...numbs) {  
        int sum = 0;  
        for (int i : numbs) {  
            sum += i;  
        }  
        return sum;  
    }  
}
```

Троеточие перед названием параметра `int ...nums` указывает на то, что он будет необязательным и будет представлять массив. Мы можем передать в метод `sum` одно число, несколько чисел, а можем вообще не передавать никаких параметров. Причем, если мы хотим передать несколько параметров, то н**еобязательный параметр должен указываться в конце**.

## Обработка исключений

В языке Java предусмотрены специальные средства для обработки подобных ситуаций. Одним из таких средств является конструкция **try...catch...finally**. При возникновении исключения в блоке **try** управление переходит в блок **catch**, который может обработать данное исключение. Если такого блока не найдено, то пользователю отображается сообщение о необработанном исключении, а дальнейшее выполнение программы останавливается. **И чтобы подобной остановки не произошло, и надо использовать блок try..catch.**

