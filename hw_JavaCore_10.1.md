# Понимание JVM
## Код для исследования:
public class JvmComprehension {

    public static void main(String[] args) {
        int i = 1;                      // 1
        Object o = new Object();        // 2
        Integer ii = 2;                 // 3
        printAll(o, i, ii);             // 4
        System.out.println("finished"); // 7
    }

    private static void printAll(Object o, int i, Integer ii) {
        Integer uselessVar = 700;                   // 5
        System.out.println(o.toString() + i + ii);  // 6
    }
}

## ClassLoader
Application ClassLoader (JvmComprehension) ->делегирует-> Platform ClassLoader (java.util) ->делегирует-> Bootstrap ClassLoader ->если не нашел-> Platform ClassLoader ->если не нашел-> Application ClassLoader (загружает JvmComprehension)

## Области памяти

### Stack и Heap

1. Фрейм main():
    1. int i = 1
    2. o -> Object (heap)
    3. Фрейм valueOf()
        1. ii = 2 -> Integer (heap)
        2. Возврат значения и выход
    4. Фрейм printAll()
        1. o -> Object (heap)
        2. int i
        3. ii -> Integer (heap)
        4. Фрейм valueOf()
            1. uselessVar = 700 -> Integer (heap)
            2. Возврат значения и выход из valueOf()
        5. Фрейм  sout()
            1. o -> Object (heap)
            2. Фрейм toString()
                1. Возврат строки и выход из toString()
            3. i
            4. ii
            5. выход из sout()
        6. Выход из printAll()
    5. Фрейм sout()
        1. "finished" -> String (heap) // или тут еще один фрейм valueOf()??
        2. выход из sout()
    6. Выход из main()


### Metaspace
Metaspace будет хранить информацию о JvmComprehension.class, System classes

## Сборщик мусора
Сборщик мусора должен подчищать из heap объекты без ссылок, в том числе после закрытия фреймов.