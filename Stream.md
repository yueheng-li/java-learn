# java Stream学习
 * [Java 8 Stream Tutorial](https://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/)
```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;
import java.util.function.Supplier;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class Java8Stream {
    public static void main(String[] args) {
        // 初始化方式
        // Stream
        Stream<String> stream = Stream.of("A","B","C","D","E");

        // Arrays
        Stream<String> stream1 = Arrays.stream(new String[]{"A","B","C","D","E"});

        // Collection
        List<String> list = Arrays.asList("A","B","C","D","E");
        Stream<String> stream2 = list.stream();

        // CharSequence
        String str = "ABCDE";
        IntStream stream3 = str.chars();

        // Stream循环输出
        //forEachStream();

        //Stream（转换后）循环输出
        //forEachStream2();

        convertToUpperList();
    }

    // 1
    public static void forEachStream() {
        // 循环(1) 函数式
        Stream<String> streamf = Arrays.stream(new String[]{"A","B","C","D","E"});
        streamf.forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        });


        // 循环(2) lambda(流的一次性,如果对于同一个流对象多次操作，（stream has already been operated upon or closed）)
        Stream<String> stream1 = Arrays.stream(new String[]{"A","B","C","D","E"});
        stream1.forEach((name) -> System.out.println(name));

        // stream supplier创建两个流
        Supplier<Stream<String>> streamSupplier = () -> Arrays.stream(new String[]{"A","B","C","D","E"});
        streamSupplier.get().forEach(name -> System.out.println("name : " + name));
        System.out.println(streamSupplier.get().count());

        // 循环(3)
        streamSupplier.get().forEach(System.out::println);

    }

    // 2(stream整理后的内容保存)
    public static void forEachStream2() {
        Supplier<Stream<String>> streamSupplier = () -> Arrays.stream(new String[]{"A","B","C","D","E"});
        // （1）
        final List<String> result = new ArrayList<String>();
        streamSupplier.get().forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
                String tmp = s.toLowerCase();
                result.add(tmp);
            }
        });
        result.stream().forEach(System.out::println);

        // （2）lambda
        result.clear();
        streamSupplier.get().forEach(name -> {
            String tmp = name.toLowerCase() + "@";
            result.add(tmp);
        });
        result.stream().forEach(System.out::println);
    }

    // 3 转换成小写并且List输出
    public static void convertToUpperList() {
        Supplier<Stream<String>> streamSupplier = () -> Arrays.stream(new String[]{"A","B","C","D","E"});
        List result = streamSupplier.get().map(name -> name.toLowerCase()).collect(Collectors.toList());
        result.stream().forEach(System.out::println);

    }
}
```
