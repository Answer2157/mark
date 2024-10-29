# 列表（List）、映射（Map）和集合（Set）
### 1）列表（List）
列表是一种有序的可变序列，可以存储多个元素。以下是一些常用的列表方法：
```
import java.util.ArrayList;
import java.util.List;

public class ListExample {
    public static void main(String[] args) {
        List<Integer> myList = new ArrayList<>();

        // 添加元素
        myList.add(1);
        myList.add(2);
        myList.add(3);

        // 获取元素
        int firstElement = myList.get(0);
        System.out.println("First Element: " + firstElement);

        // 修改元素
        myList.set(1, 4);

        // 删除元素
        myList.remove(2);

        // 遍历列表
        for (int element : myList) {
            System.out.println(element);
        }
    }
}

```
### 2）映射（Map）
映射是一种键值对的集合，其中每个键都是唯一的。以下是一些常用的映射方法：
```
import java.util.HashMap;
import java.util.Map;

public class MapExample {
    public static void main(String[] args) {
        Map<String, Integer> myMap = new HashMap<>();

        // 添加键值对
        myMap.put("a", 1);
        myMap.put("b", 2);
        myMap.put("c", 3);

        // 获取值
        int valueA = myMap.get("a");
        System.out.println("Value for key 'a': " + valueA);

        // 判断键是否存在
        boolean containsKey = myMap.containsKey("b");
        System.out.println("Contains key 'b': " + containsKey);

        // 删除键值对
        myMap.remove("c");

        // 遍历映射
        for (Map.Entry<String, Integer> entry : myMap.entrySet()) {
            String key = entry.getKey();
            int value = entry.getValue();
            System.out.println(key + " : " + value);
        }
    }
}

```
### 3）集合（Set）
集合是一种无序且不重复的数据集。以下是一些常用的集合方法：
```
import java.util.HashSet;
import java.util.Set;

public class SetExample {
    public static void main(String[] args) {
        Set<Integer> mySet = new HashSet<>();

        // 添加元素
        mySet.add(1);
        mySet.add(2);
        mySet.add(3);

        // 判断元素是否存在
        boolean containsElement = mySet.contains(2);
        System.out.println("Contains element 2: " + containsElement);

        // 删除元素
        mySet.remove(3);

        // 遍历集合
        for (int element : mySet) {
            System.out.println(element);
        }
    }
}

```