Collection framework represents an architecture to store and manipulate a group of objects. All the classes and interfaces of this framework are present in java.util package.

Some points:

Iterable interface is the root interface for all collection classes, it has one abstract method iterator()
Collection interface extends the Iterable interface
ArrayList
An ArrayList is a re-sizable array, also called a dynamic array. It grows its size to accommodate new elements and shrinks the size when the elements are removed.
ArrayList internally uses an array to store the elements. Just like arrays, It allows you to retrieve the elements by their index.
ArrayList allows duplicate and null values.
ArrayList is an ordered collection. It maintains the insertion order of the elements.
You cannot create an ArrayList of primitive types like int, char etc. You need to use boxed types like Integer, Character, Boolean etc.
ArrayList is not synchronized. If multiple threads try to modify an ArrayList at the same time, then the final outcome will be non-deterministic. You must explicitly synchronize access to an ArrayList if multiple threads are gonna modify it.
Accessing elements
check if an ArrayList is empty using the isEmpty() method.
find the size of an ArrayList using the size() method.
access the element at a particular index in an ArrayList using the get() method.
modify the element at a particular index in an ArrayList using the set() method.
Removing elements
remove the element at a given index in an ArrayList using remove(int index)
remove an element from an ArrayList using remove(Object o)
remove all the elements from an ArrayList that exist in a given collection using removeAll()
remove all the elements matching a given predicate using removeIf()
clear an ArrayList using clear()
Iterating over an ArrayList
Java 8 forEach and lambda expression.
iterator().
iterator() and Java 8 forEachRemaining() method.
listIterator().
Simple for-each loop.
for loop with index.
Searching for elements in an ArrayList
Check if an ArrayList contains a given element | contains()
Find the index of the first occurrence of an element in an ArrayList | indexOf()
Find the index of the last occurrence of an element in an ArrayList | lastIndexOf()
Sorting an ArrayList
Sort an ArrayList using Collections.sort() method.
Sort an ArrayList using ArrayList.sort() method.
Sort an ArrayList of user defined objects with a custom comparator.
HashMap
HashMap class implements the Map interface and it stores data in key, value pairs. HashMap provides constant time performance for its get() and put() operations, assuming the equals and hashcode method has been implemented properly, so that elements can be distributed correctly among the buckets.

Some points to remember:

Keys should be unique in HashMap, if you try to insert the duplicate key, then it will override the corresponding key’s value
HashMap may have one null key and multiple null values
HashMap does not guarantee the insertion order (if you want to maintain the insertion order, use LinkedHashMap class)
HashMap is not synchronized
HashMap uses an inner class Node<K, V> for storing map entries
Hashmap has a default initial capacity of 16, which means it has 16 buckets or bins to store map entries, each bucket is a singly linked list. The default load factor in HashMap is 0.75
Load factor is that threshold value which when crossed will double the hashmap’s capacity i.e. when you add 13th element in hashmap, the capacity will increase from 16 to 32
HashMap Internal working
HashMap works on the principal of hashing.
HashMap in Java uses the hashCode() method to calculate a hash value. Hash value is calculated using the key object. This hash value is used to find the correct bucket where Entry object will be stored.
HashMap uses the equals() method to find the correct key whose value is to be retrieved in case of get() and to find if that key already exists or not in case of put().
With in the internal implementation of HashMap hashing collision means more than one key having the same hash value, in that case Entry objects are stored as a linked-list with in a same bucket.
With in a bucket values are stored as Entry objects which contain both key and value.
In Java 8 hash elements use balanced trees instead of linked lists after a certain threshold is reached while storing values. This improves the worst case performance from O(n) to O(log n).
HashMap class in Java internally uses an array called table of type Node to store the elements which is defined in the HashMap class as-

    /**
 * The table, initialized on first use, and resized as
 * necessary. When allocated, length is always a power of two.
 * (We also tolerate length zero in some operations to allow
 * bootstrapping mechanics that are currently not needed.)
 */
transient Node<K,V>[] table;
Node is defined as a static class with in a Hashmap.

 static class Node<K,V> implements Map.Entry<K,V> {
  final int hash;
  final K key;
  V value;
  Node<K,V> next;

  Node(int hash, K key, V value, Node<K,V> next) {
    this.hash = hash;
    this.key = key;
    this.value = value;
    this.next = next;
  }

  public final K getKey()        { return key; }
  public final V getValue()      { return value; }
  public final String toString() { return key + "=" + value; }

  public final int hashCode() {
    return Objects.hashCode(key) ^ Objects.hashCode(value);
  }

  public final V setValue(V newValue) {
    V oldValue = value;
    value = newValue;
    return oldValue;
  }

  public final boolean equals(Object o) {
    if (o == this)
      return true;
    if (o instanceof Map.Entry) {
      Map.Entry<?,?> e = (Map.Entry<?,?>)o;
      if (Objects.equals(key, e.getKey()) &&
              Objects.equals(value, e.getValue()))
        return true;
    }
    return false;
  }
}
For each element 4 things are stored in the fields-

hash- For storing Hashcode calculated using the key.
key- For holding key of the element.
value- For storing value of the element.
next- To store reference to the next node when a bucket has more than one element and a linkedlist is formed with in a bucket to store elements.
Objects are stored internally in table array of the HashMap class.



How put() method of HashMap works internally
There are 3 steps in the internal implementation of HashMap put() method-

Using hashCode() method, hash value will be calculated. In which bucket particular entry will be stored is ascertained using that hash.
equals() method is used to find if such a key already exists in that bucket, if not found then a new node is created with the map entry and stored within the same bucket. A linked-list is used to store those nodes.
If equals() method returns true, it means that the key already exists in the bucket. In that case, the new value will overwrite the old value for the matched key.
How HashMap get() method works internally
Using the key (passed in the get() method) hash value will be calculated to determine the bucket where that Entry object is stored, in case there are more than one Entry object with in the same bucket (stored as a linked-list) equals() method will be used to find out the correct key. As soon as the matching key is found get() method will return the value object stored in the Entry object.

When null Key is inserted in a HashMap
HashMap in Java also allows null as key, though there can only be one null key in HashMap. While storing the Entry object HashMap implementation checks if the key is null, in case key is null, it is always mapped to bucket 0, as hash is not calculated for null keys.

HashMap implementation changes in Java 8
HashMap implementation in Java provides constant time performance O(1) for get() and put() methods but that is in the ideal case when the Hash function distributes the objects evenly among the buckets.

But the performance may worsen in the case hashCode() used is not proper and there are lots of hash collisions. As we know now that in case of hash collision entry objects are stored as a node in a linked-list and equals() method is used to compare keys. That comparison to find the correct key with in a linked-list is a linear operation so in a worst case scenario the complexity becomes O(n).

To fix this issue in Java 8 hash elements use balanced trees instead of linked lists after a certain threshold is reached. Which means HashMap starts with storing Entry objects in linked list but after the number of items in a hash becomes larger than a certain threshold, the hash changes from using a linked list to a balanced tree, this improves the worst case performance from O(n) to O(log n).

Interal working of put() and get() methods of HashMap :
put() method internal working: When you call map.put(key,value), the below things happens:

Key’s hashCode() method is called

Hashmap has an internal hash function which takes the key’s hashCode and it calculates the bucket index

If there is no element present at that bucket index, our <key, value> pair along with hash is stored at that bucket

But if there is an element present at the bucket index, then key’s hashCode is used to check whether this key is already present with the same hashCode or not.

If there is key with same hashCode, then equals method is used on the key. If equals method returns true, then the key’s previous value is replaced with the new value otherwise a new entry is appended to the linked list.

get() method internal working: When you call map.get(key), the below things happen:

Key’s hashCode() method is called

Hash function uses this hashCode to calculate the index, just like in put method

Now the key of element stored in bucket is compared with the passed key using equals() method, if both are equals, value is returned otherwise the next element is checked if it exists.

See HashMap’s Javadoc: Default capacity:

/**
* The default initial capacity - MUST be a power of two.
*/
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
Load factor:

/**
* The load factor used when none specified in constructor.
*/
static final float DEFAULT_LOAD_FACTOR = 0.75f;
Node class:

static class Node<K,V> implements Map.Entry<K,V> {
final int hash ;
"final K key ;
    V value ;
    Node<K,V> next ;
    Node(int hash , K key , V value , Node<K,V> next ) {
        this .hash = hash ;
        this .key = key ;
        this .value = value ;
        this .next = next ;
    }
Internal Hash function:

static final int hash(Object key ) {
int h ;
return (key == null ) ? 0 : (h = key .hashCode()) ^ (h >>> 16);
}
Internal Data structure used by HashMap to hold buckets:

transient Node<K,V>[] table ;
HashMap’s default constructor:

/**
* Constructs an empty <tt> HashMap </tt> with the default initial capacity
* (16) and the default load factor (0.75).
  */
public HashMap() {
this .loadFactor = DEFAULT_LOAD_FACTOR ; // all other fields defaulted
}
So, to conclude, Hashmap internally uses an array of Nodes named as table where each Node contains the calculated hash value, the key-value pair and the address to the next node.

HashMap collisions
It is possible that multiple keys will make the hash function generate the same index, this is called a collision. It happens because of poor hashcode method implementation.

One collision handling technique is called Chaining. Since every element in the array is a linked list, the keys which have the same hash function will be appended to the linked list.

Performance improvement in Java 8 : It is possible that due to multiple collisions, the linked list size has become very large, and as we know, searching in a linked list is O(n), it will impact the constant time performance of hashmap’s get() method. So, in Java 8, if the linked list size becomes more than 8, the linked list is converted to a binary search tree which will give a better time complexity of O(log n).
/**
* The bin count threshold for using a tree rather than list for a
* bin. Bins are converted to trees when adding an element to a"
 "* bin with at least this many nodes. The value must be greater
 * than 2 and should be at least 8 to mesh with assumptions in
 * tree removal about conversion back to plain bins upon
 * shrinkage.
 */
static final int TREEIFY_THRESHOLD = 8;
Program showing the default capacity:

import java.lang.reflect.Field;
import java.util.HashMap;

public class TestHashMap {
    public static void main(String[] args) throws Exception {
        HashMap<String, String> map = new HashMap<>();
        map.put("name", "Mike");

		Field tableField = HashMap.class.getDeclaredField("table");
		tableField.setAccessible(true);
		Object[] table = (Object[]) tableField.get(map);
		System.out.print("hashmap capacity: ");
		System.out.print(table == null ? 0 : table.length);
		System.out.println("\nhashmap size:" + map.size());
	}
}
Output:

hashmap capacity: 16
hashmap size: 1
Program showing that hashmap’s capacity gets doubled after load factor’s threshold value breaches :

import java.lang.reflect.Field;
import java.util.HashMap;

class Employee {
	private int age;
	
	public Employee(int age) {
		this.age = age;
	}
}

public class TestHashMap {
	public static void main(String[] args) throws Exception {
		HashMap<Employee, String> map = new HashMap<>();
		
		for(int i=1;i<13;i++) {
			map.put(new Employee(i), "Hello " + i);
		}
		
		Field tableField = HashMap.class.getDeclaredField("table");
		tableField.setAccessible(true);
		Object[] table = (Object[]) tableField.get(map);
		System.out.print("hashmap capacity: ");
		System.out.print(table == null ? 0 : table.length);
		System.out.println("\nhashmap size:" + map.size());
	}
}
Output:

hashmap capacity: 16
hashmap size: 12
Change the for loop condition from i<13 to i<=13, see below:

import java.lang.reflect.Field;
import java.util.HashMap;

class Employee {
	private int age;
	
	public Employee(int age) {
		this.age = age;
	}
}

public class TestHashMap {
	public static void main(String[] args) throws Exception {
		HashMap<Employee, String> map = new HashMap<>();
		
		for(int i=1;i<13;i++) {
			map.put(new Employee(i), "Hello " + i);
		}
		
		Field tableField = HashMap.class.getDeclaredField("table");
		tableField.setAccessible(true);
		Object[] table = (Object[]) tableField.get(map);
		System.out.print("hashmap capacity: ");
		System.out.print(table == null ? 0 : table.length);
		System.out.println("\nhashmap size:" + map.size());
	}
}
Output:

hashmap capacity: 32
hashmap size: 13
equals and hashCode method in HashMap when the key is a custom class
equals and hashCode methods are called when we store and retrieve values from hashmap.

if in your custom class, you are not implementing equals() and hashCode(), then the Object class equals() and hashCode() will be called, and the contract between these 2 methods. It says when 2 objects are equal according to equals() method, then their hashCode must be same, reverse may not be true.

Scenario 1: when custom class does not implement both equals and hashCode methods
public class Employee {
  private String name;
  private int age;
  public Employee(String name, int age) {
    this.name = name;
    this.age = age;
  }
}
Here, Employee class has not given equals() and hashCode() method implementation, so Object’s class equals() and hashCode() methods will be used when we use this Employee class as hashmap’s key, and remember, equals() method of Object class compares the reference.

TestHashMap.java:

import java.util.HashMap;
import java.util.Map;

public class TestHashMap {
  public static void main(String[] args) {

    Map<Employee, Integer> map = new HashMap<>();

    Employee e1 = new Employee("Mike", 15);
    Employee e2 = new Employee("Mike", 15);
    Employee e3 = new Employee("John", 20);
    Employee e4 = e3;

    System.out.println("e1 hashcode: " + e1.hashCode());
    System.out.println("e2 hashcode: " + e2.hashCode());
    System.out.println("e3 hashcode: " + e3.hashCode());
    System.out.println("e4 hashcode: " + e4.hashCode());

    System.out.println("e1 equals e2: " + e1.equals(e2));
    System.out.println("e3 equals e4: " + e3.equals(e4));

    map.put(e1, 100);
    map.put(e2, 200);
    map.put(e3, 300);
    map.put(e4, 400);

    System.out.println(map.get(e1));
    System.out.println(map.get(e2));
    System.out.println(map.get(e3));
    System.out.println(map.get(e4));
    System.out.println("hashmap size: " + map.size());
  }
}
Output:

e1 hashcode: 1324119927
e2 hashcode: 999966131
e3 hashcode: 1989780873
e4 hashcode: 1989780873
e1 equals e2: false
e3 equals e4: true
100
200
400
400
hashmap size: 3
Here, Employee objects e1 and e2 are same but they are both inserted in the HashMap because both are created using new keyword and holding a different reference, and as the Object’s equals() method checks reference, they both are unique. And as for objects e3 and e4, they both are pointing to same reference (e4 = e3), so they are equal according to Object’s equals() method hence the value of e3 which was 300 gets replaced with the value 400 of the same key e4, and finally size of HashMap is 3.

Scenario 2: when only equals() method is implemented by Employee class
public class Employee {

  private String name;
  private int age;

  public Employee(String name, int age) {
    this.name = name;
    this.age = age;
  }

  @Override
  public boolean equals(Object obj) {
    if (this == obj)
      return true;
    if (obj == null)
      return false;
    if (getClass() != obj.getClass())
      return false;
    Employee other = (Employee) obj;
    if (age != other.age)
      return false;
    if (name == null) {
      if (other.name != null)
        return false;
    } else if (!name.equals(other.name))
      return false;
    return true;
  }

}
import java.util.HashMap;
import java.util.Map;

public class TestHashMap {
  public static void main(String[] args) {

    Map<Employee, Integer> map = new HashMap<>();

    Employee e1 = new Employee("Mike", 15);
    Employee e2 = new Employee("Mike", 15);
    Employee e3 = new Employee("John", 20);
    Employee e4 = e3;

    System.out.println("e1 hashcode: " + e1.hashCode());
    System.out.println("e2 hashcode: " + e2.hashCode());
    System.out.println("e3 hashcode: " + e3.hashCode());
    System.out.println("e4 hashcode: " + e4.hashCode());

    System.out.println("e1 equals e2: " + e1.equals(e2));
    System.out.println("e3 equals e4: " + e3.equals(e4));

    map.put(e1, 100);
    map.put(e2, 200);
    map.put(e3, 300);
    map.put(e4, 400);

    System.out.println(map.get(e1));
    System.out.println(map.get(e2));
    System.out.println(map.get(e3));
    System.out.println(map.get(e4));
    System.out.println("hashmap size: " + map.size());
  }
}
Let’s see the output:

e1 hashcode: 1324119927
e2 hashcode: 999966131
e3 hashcode: 1989780873
e4 hashcode: 1989780873
e1 equals e2: true
e3 equals e4: true
100
200
400
400
hashmap size: 3
Well, nothing’s changed here. Because even though e1 and e2 are equal according to our newly implemented equals() method, they still have different hashCode as the Object’s class hashCode() is used. So the equals and hashCode contract is not followed and both e1, e2 got inserted in HashMap.

Scenario 3: when only hashCode() method is implemented:
public class Employee {
  private String name;
  private int age;

  public Employee(String name, int age) {
    this.name = name;
    this.age = age;
  }

  @Override
  public int hashCode() {
    final int prime = 31;
    int result = 1;
    result = prime * result + age;
    result = prime * result + ((name == null) ? 0 : name.hashCode());
    return result;
  }

}
Let’s run our TestHashMap class again and see the output:

e1 hashcode: 2399656
e2 hashcode: 2399656
e3 hashcode: 2316120
e4 hashcode: 2316120
e1 equals e2: false
e3 equals e4: true
100
200
400
400
hashmap size: 3

Well, now we have same hashCode for e1 and e2, but Object’s equals method still checks the references and as references are different, both are not equal and are inserted in the hashmap.

Scenario 4: When both equals and hashCode are implemented properly:
public class Employee {
    private String name;
    private int age;

    public Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + age;
        result = prime * result + ((name == null) ? 0 : name.hashCode());
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj)
            return true;
        if (obj == null)
            return false;
        if (getClass() != obj.getClass())
            return false;
        Employee other = (Employee) obj;
        if (age != other.age)
            return false;
        if (name == null) {
            if (other.name != null)
                return false;
        } else if (!name.equals(other.name))
            return false;
        return true;
    }
}
Output:

e1 hashcode: 2399656
e2 hashcode: 2399656
e3 hashcode: 2316120
e4 hashcode: 2316120
e1 equals e2: true
e3 equals e4: true
200
200
400
400
hashmap size: 2
Here, both e1 and e2 are equals as we are comparing the contents of them in our equals() method, so their hashCodes must be same, which they are. So value of e1 which was 100 got replaced by 200, and size of hashmap is 2.

How to make a HashMap synchronized?

Collections.synchronizedMap(map);

Concurrent HashMap
ConcurrentHashMap class provides concurrent access to the map, this class is very similar to HashTable, except that ConcurrentHashMap provides better concurrency than HashTable or even synchronizedMap.

Some points to remember:

ConcurrentHashMap is internally divided into segments, by default size is 16 that means, at max 16 threads can work at a time
Unlike HashTable, the entire map is not locked while reading/writing from the map
In ConcurrentHashMap, concurrent threads can read the value without locking
For adding or updating the map, the lock is obtained on segment level, that means each thread can work on each segment during high concurrency
Concurrency level defines a number, which is an estimated number of threads concurrently accessing the map
ConcurrentHashMap does not allow null keys or null values
put() method acquires lock on the segment
get() method returns the most recently updated value
iterators returned by ConcurrentHashMap are fail-safe and never throw ConcurrentModificationException
ConcurrentHashMap vs Synchronized HashMap
Synchronized HashMap：

Each method is synchronized using an object level lock. So the get and put methods on synchMap acquire a lock.
Locking the entire collection is a performance overhead. While one thread holds on to the lock, no other thread can use the collection.
ConcurrentHashMap was introduced in JDK 5.

There is no locking at the object level,The locking is at a much finer granularity. For a ConcurrentHashMap, the locks may be at a hashmap bucket level.
The effect of lower level locking is that you can have concurrent readers and writers which is not possible for synchronized collections. This leads to much more scalability.
ConcurrentHashMap does not throw a ConcurrentModificationException if one thread tries to modify it while another is iterating over it.
HashSet class
HashSet is a class in Java that implements the Set Interface and it allows us to have the unique elements only. HashSet class does not maintain the insertion order of elements, if you want to maintain the insertion order, then you can use LinkedHashSet.

Internal implementation of HashSet:
HashSet internally uses HashMap and as we know the keys are unique in hashmap, the value passed in the add() method of HashSet is stored as the key of hashmap, that is how Set maintains the unique elements.

/**
* Constructs a new, empty set; the backing <tt> HashMap </tt> instance has
* default initial capacity (16) and load factor (0.75).
*/
public HashSet() {
        map = new HashMap<>();
}
private transient HashMap<E,Object> map ;
Let’s see add() method’s Javadoc:

public boolean add(E e ) {
        return map .put(e , PRESENT )==null ;
}
// Dummy value to associate with an Object in the backing Map
private static final Object PRESENT = new Object();
So, when we call hashSet.add(element) method then

map.put() is called where key is the element and value is the dummy value (the map.put() method internal working has already been discussed above)
if value is added in the map then put method will return null which will be compared with null, hence returning true from hashSet.add() method indicating the element is added
however if the element is already present in the map, then the value associated with the element will be returned which in turn will be compared with null, returning false from hashSet.add() method
set.contains() method:

public boolean contains(Object o ) {
    return map .containsKey(o );
}
The passed object is given to map.containsKey() method, as the HashSet’s values are stored as the keys of internal map.

NOTE: If you are adding a custom class object inside the HashSet, do follow equals and hashCode contract.

TreeMap
TreeMap class is one of the implementation of Map interface.

Some points to remember:

TreeMap entries are sorted based on the natural ordering of its keys. This means if we are using a custom class as the key, we have to make sure that the custom class is implementing Comparable interface
TreeMap class also provides a constructor which takes a Comparator object, this should be used when we want to do a custom sorting
TreeMap provides guaranteed log(n) time complexity for the methods such as containsKey(), get(), put() and remove()
TreeMap iterator is fail-fast in nature, so any concurrent modification will result in ConcurrentModificationException
TreeMap does not allow null keys but it allows multiple null values
TreeMap is not synchronized, so it is not thread-safe. We can make it thread-safe by using utility method, Collections.synchronizedSortedMap(treeMap)
TreeMap internally uses Red-Black tree based NavigableMap implementation.
Red-Black tree algorithm has the properties:

Color of every node in the tree is either red or black.

Root node must be Black in color.

Red node cannot have a red color neighbor node.

All paths from root node to the null should consist the same number of black nodes

Program 1: Using Wrapper class as key

import java.util.TreeMap;

public class TestTreeMap {
  public static void main(String[] args) {
    TreeMap<Integer, String> map = new TreeMap<>();
    map.put(4, "Mike");
    map.put(1, "John");
    map.put(3, "Jack");
    map.put(2, "Lisa");

    map.forEach((k,v) -> System.out.println(k + ":" + v));
  }
}
Output:

1:John
2:Lisa
3:Jack
4:Mike
Here, Integer class already implements Comparable interface, so the keys are sorted based on the Integer’s natural sorting order (ascending order).

Let’s see, when key is a custom class:

Program 2:
import java.util.TreeMap;

class Employee {
	String name;
	int age;
	Employee(String name, int age) {
		this.name = name;
		this.age = age;
	}
}

public class TestTreeMap {
	public static void main(String[] args) {
		
		TreeMap<Employee, Integer> map = new TreeMap<>();
		
		map.put(new Employee("Mike", 20), 100);
		map.put(new Employee("John", 10), 500);
		map.put(new Employee("Ryan", 15), 200);
		map.put(new Employee("Lisa", 20), 400);
		
		map.forEach((k,v) -> System.out.println(k + ":" + v));		
	}
}
Exception in thread "main" java.lang.ClassCastException: class GrokkingInterview.Grokking.Question109.Program2.Employee cannot be cast to class java.lang.Comparable (GrokkingInterview.Grokking.Question109.Program2.Employee is in unnamed module of loader 'app'; java.lang.Comparable is in module java.base of loader 'bootstrap')
	at java.base/java.util.TreeMap.compare(TreeMap.java:1563)
	at java.base/java.util.TreeMap.addEntryToEmptyMap(TreeMap.java:768)
	at java.base/java.util.TreeMap.put(TreeMap.java:777)
	at java.base/java.util.TreeMap.put(TreeMap.java:534)
	at GrokkingInterview.Grokking.Question109.Program2.TestTreeMap.main(TestTreeMap.java:19)
We get ClassCastException at runtime. Now, let’s implement Comparable interface in Employee class and provide implementation of its compareTo() method:

Program 3:
import java.util.TreeMap;

class Employee implements Comparable<Employee> {
  String name;
  int age;
  Employee(String name, int age) {
    this.name = name;
    this.age = age;
  }
  @Override
  public int compareTo(Employee emp) {
    return this.name.compareTo(emp.name);
  }
  @Override
  public String toString() {
    return "Employee [name=" + name + ", age=" + age + "]";
  }
}

public class TestTreeMap {
  public static void main(String[] args) {

    TreeMap<Employee, Integer> map = new TreeMap<>();

    map.put(new Employee("Mike", 20), 100);
    map.put(new Employee("John", 10), 500);
    map.put(new Employee("Ryan", 15), 200);
    map.put(new Employee("Lisa", 20), 400);

    map.forEach((k,v) -> System.out.println(k + ":" + v));
  }
}
Here, we are sorting based on Employee name,

Output:

Employee [name=John, age=10]:500
Employee [name=Lisa, age=20]:400
Employee [name=Mike, age=20]:100
Employee [name=Ryan, age=15]:200
Let’s look at a program where we pass a Comparator in the TreeMap constructor, and sort the Employee object’s based on age in descending order:

Program 4:
import java.util.Comparator;
import java.util.TreeMap;
import java.util.TreeSet;

class Employee {
	String name;
	int age;
	Employee(String name, int age) {
		this.name = name;
		this.age = age;
	}
	@Override
	public String toString() {
		return "Employee [name=" + name + ", age=" + age + "]";
	}
}

public class TestTreeMap {
	public static void main(String[] args) {
		TreeMap<Employee, Integer> map = new TreeMap<>(
				new Comparator<Employee>() {
					@Override
					public int compare(Employee e1, Employee e2) {					
						if(e1.age < e2.age){
							return 1;
						} else if(e1.age > e2.age) {
							return -1;
						}
						return 0;
					}		
				}
			);
		
		map.put(new Employee("Mike", 20), 100);
		map.put(new Employee("John", 10), 500);
		map.put(new Employee("Ryan", 15), 200);
		map.put(new Employee("Lisa", 40), 400);
		
		map.forEach((k,v) -> System.out.println(k + ":" + v));		
	}
}
Output:

Employee [name=Lisa, age=40]:400
Employee [name=Mike, age=20]:100
Employee [name=Ryan, age=15]:200
Employee [name=John, age=10]:500

Here, in Employee class, I have not implemented equals() and hashCode()

TreeMap’s Javadoc:

public class TreeMap<K,V>
        extends AbstractMap<K,V>
        implements NavigableMap<K,V>, Cloneable, java.io.Serializable
{
  /**
   * The comparator used to maintain order in this tree map, or
   * null if it uses the natural ordering of its keys.
   *
   * @serial
   */
  private final Comparator<? super K> comparator ;
  private transient Entry<K,V> root ;
  //No-arg TreeMap constructor:
  public TreeMap() {
    comparator = null ;
  }
}
TreeMap constructor which takes comparator object:

public TreeMap(Comparator<? super K> comparator ) {
            this .comparator = comparator ;
        }
        
       // TreeMap.put() method excerpt:
public V put(K key , V value ) {
        Entry<K,V> t = root ;
        if (t == null ) {
          compare(key , key ); // type (and possibly null) check
          root = new Entry<>(key , value , null );
          size = 1;
          modCount ++;
          return null ;
        }
        
        int cmp ;
        Entry<K,V> parent ;
// split comparator and comparable paths
        Comparator<? super K> cpr = comparator ;
        if (cpr != null ) {
          do {
            parent = t ;
            cmp = cpr .compare(key , t .key );
            if (cmp < 0)
                t = t .left ;
            else if (cmp > 0)
              t = t .right ;
            else
                return t .setValue(value );
          } while (t != null );
        }
TreeSet
TreeSet class is one of the implementation of Set interface

Some points to remember:

TreeSet class contains unique elements just like HashSet
TreeSet class does not allow null elements
TreeSet class is not synchronized
TreeSet class internally uses TreeMap, i.e. the value added in TreeSet is internally stored in the key of TreeMap
TreeSet elements are ordered using their natural ordering or by a Comparator which can be provided at the set creation time
TreeSet provides guaranteed log(n) time cost for the basic operations (add, remove and contains)
TreeSet iterator is fail-fast in nature
TreeSet Javadoc:

public class TreeSet<E> extends AbstractSet<E>
        implements NavigableSet<E>, Cloneable, java.io.Serializable
        public TreeSet() {
          this (new TreeMap<E,Object>());
        }
        public TreeSet(Comparator<? super E> comparator ) {
          this (new TreeMap<>(comparator ));
        }
fail-safe and fail-fast iterators
Iterators in Java are used to iterate over the Collection objects.

Fail-fast iterators : immediately throw ConcurrentModificationException, if the collection is modified while iterating over it. Iterator of ArrayList and HashMap are fail-fast iterators. All the collections internally maintain some sort of array to store the elements, Fail-fast iterators fetch the elements from this array. Whenever, we modify the collection, an internal field called modCount is updated. This modCount is used by Fail-safe iterators to know whether the collection is structurally modified or not. Every time when the Iterator’s next() method is called, it checks the modCount. If it finds that modCount has been
updated after the Iterator has been created, it throws ConcurrentModificationException.

Program 1:

import java.util.ArrayList;
import java.util.Iterator;

public class FailFastIteratorTest {
	public static void main(String[] args) {
		
		ArrayList<Integer> list = new ArrayList<>();
		list.add(1);
		list.add(2);
		list.add(3);
		
		Iterator<Integer> itr = list.iterator();
		while(itr.hasNext()) {
			System.out.println(itr.next());
			list.add(4);
		}
	}
}
Output:

1
Exception in thread "main" java.util.ConcurrentModificationException
at java.base/java.util.ArrayList$Itr.checkForComodification(ArrayList.java:1013)
at java.base/java.util.ArrayList$Itr.next(ArrayList.java:967)
at GrokkingInterview.Grokking.Question111.Failfastiterator.Program1.FailFastIteratorTest.main(FailFastIteratorTest.java:16)
But they don’t throw the exception, if the collection is modified by Iterator’s remove() method.

Program 2:

import java.util.ArrayList;
import java.util.Iterator;

public class FailFastIteratorTest {
	public static void main(String[] args) {
		
		ArrayList<Integer> list = new ArrayList<>();
		list.add(1);
		list.add(2);
		list.add(3);
		System.out.println("List: " + list);
		Iterator<Integer> itr = list.iterator();
		while(itr.hasNext()) {
			System.out.println(itr.next());
			itr.remove();
		}
		System.out.println("List: " + list);
	}
}
Output:

List: [1, 2, 3]
1
2
3
List: []

Javadoc:

arrayList.iterator() method:

public Iterator<E> iterator(){
        return new Itr();
}
    
Itr is a private nested class in ArrayList:

private class Itr implements Iterator<E> {
  int cursor ;       // index of next element to return
  int lastRet = -1; // index of last element returned; -1 if no such
  int expectedModCount = modCount ;
  Itr() {}
  public boolean hasNext() {
    return cursor != size ; 
  }
Itr.next() method:

@SuppressWarnings ("unchecked" )
public E next() {
        checkForComodification();
        int i = cursor ;
        if (i >= size )
            throw new NoSuchElementException();
        Object[] elementData = ArrayList.this .elementData ;
        if (i >= elementData .length )
            throw new ConcurrentModificationException();
        cursor = i + 1;
        return (E) elementData [lastRet = i ];
        }
        
See the first statement is a call to checkForComodification():

final void checkForComodification() {
    if (modCount != expectedModCount )
        throw new ConcurrentModificationException();
}
On the other hand,

Fail-safe iterators
does not throw ConcurrentModificationException , because they operate on the clone of the collection, not the actual collection. This also means that any modification done on the actual collection goes unnoticed by these iterators. The last statement is not always true though, sometimes it can happen that the iterator may reflect modifications to the collection after the iterator is created. But there is no guarantee of it. CopyOnWriteArrayList, ConcurrentHashMap are the examples of fail-safe iterators.

Program 1: ConcurrentHashMap example

import java.util.Iterator;
import java.util.TreeMap;
import java.util.concurrent.CopyOnWriteArrayList;

public class FailSafeIteratorTest {
	public static void main(String[] args) {
		CopyOnWriteArrayList<Integer> list = new CopyOnWriteArrayList<>();
		list.add(1);
		list.add(2);
		list.add(3);
		
		Iterator<Integer> itr = list.iterator();
		while(itr.hasNext()) {
			System.out.println(itr.next());
			list.add(4);
		}
		System.out.println("List: " + list);
	}
}
Output:

1 : Mike
2 : John
3 : Lisa
4 : Ryan
Map: {1=Mike, 2=John, 3=Lisa, 4=Ryan}
Here, iterator is reflecting the element which was added during the iteration operation.

Program 2: CopyOnWriteArrayList example

import java.util.Iterator;
import java.util.TreeMap;
import java.util.concurrent.CopyOnWriteArrayList;

public class FailSafeIteratorTest {
	public static void main(String[] args) {
		CopyOnWriteArrayList<Integer> list = new CopyOnWriteArrayList<>();
		list.add(1);
		list.add(2);
		list.add(3);
		
		Iterator<Integer> itr = list.iterator();
		while(itr.hasNext()) {
			System.out.println(itr.next());
			list.add(4);
		}
		System.out.println("List: " + list);
	}
}
Output:

1
2
3
List: [1, 2, 3, 4, 4, 4]
For more information:

Java ArrayList Tutorial with Examples
Guide to the Java ArrayList
How HashMap Works Internally in Java
Java 8 Collectors toMap with Examples
interview-notes/Java/collection/collection-framework.md at main · sunilsoni/interview-notes · GitHub


# NOTESONSQL# Database


## SQL

Joins
---------
The SQL Joins clause is used to combine records from two or more tables in a database. A JOIN is a means for combining fields from two tables by using values common to each.

- [Pictorial Reference](https://commons.wikimedia.org/wiki/File:SQL_Joins.svg)
- **Inner join**: Rows common in both T1 and T2
- **Left outer join**: All rows in T1
- **Right outer join**: All rows in T2
- **Full join**: All rows in T1 and T2
- **Cross join**: All rows in T1 * all rows in T2

Inner join
---------
The INNER JOIN creates a new result table by combining column values of two tables (table1 and table2) based upon the join-predicate. The query compares each row of table1 with each row of table2 to find all pairs of rows which satisfy the join-predicate. When the join-predicate is satisfied, column values for each matched pair of rows of A and B are combined into a result row.

Syntax - 
```sql
SELECT table1.column1, table2.column2...
FROM table1
INNER JOIN table2
ON table1.common_field = table2.common_field;
```

```sql
SQL> SELECT  ID, NAME, AMOUNT, DATE
FROM CUSTOMERS
INNER JOIN ORDERS
ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
```



Left outer join
---------
The SQL LEFT JOIN returns all rows from the left table, even if there are no matches in the right table. This means that if the ON clause matches 0 (zero) records in the right table; the join will still return a row in the result, but with NULL in each column from the right table.

This means that a left join returns all the values from the left table, plus matched values from the right table or NULL in case of no matching join predicate.

Syntax- 
```sql
SELECT table1.column1, table2.column2...
FROM table1
LEFT JOIN table2
ON table1.common_field = table2.common_field;
```

```sql
SQL> SELECT  ID, NAME, AMOUNT, DATE
FROM CUSTOMERS
LEFT JOIN ORDERS
ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
```


Right outer join
---------
The SQL RIGHT JOIN returns all rows from the right table, even if there are no matches in the left table. This means that if the ON clause matches 0 (zero) records in the left table; the join will still return a row in the result, but with NULL in each column from the left table.

This means that a right join returns all the values from the right table, plus matched values from the left table or NULL in case of no matching join predicate.

Syntax- 
```sql
SELECT table1.column1, table2.column2...
FROM table1
RIGHT JOIN table2
ON table1.common_field = table2.common_field;
```

```sql
SQL> SELECT  ID, NAME, AMOUNT, DATE
FROM CUSTOMERS
RIGHT JOIN ORDERS
ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
```
Full join
---------
The SQL FULL JOIN combines the results of both left and right outer joins.

The joined table will contain all records from both the tables and fill in NULLs for missing matches on either side.

Syntax −
```sql
SELECT table1.column1, table2.column2...
FROM table1
FULL JOIN table2
ON table1.common_field = table2.common_field;
```

```sql
SQL> SELECT  ID, NAME, AMOUNT, DATE
FROM CUSTOMERS
FULL JOIN ORDERS
ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID;
```


If your Database does not support FULL JOIN (MySQL does not support FULL JOIN), then you can use UNION ALL clause to combine these two JOINS as shown below.
```sql
SQL> SELECT  ID, NAME, AMOUNT, DATE
FROM CUSTOMERS
LEFT JOIN ORDERS
ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID
UNION ALL
SELECT  ID, NAME, AMOUNT, DATE
FROM CUSTOMERS
RIGHT JOIN ORDERS
ON CUSTOMERS.ID = ORDERS.CUSTOMER_ID

```

Cross join
---------
The CARTESIAN JOIN or CROSS JOIN returns the Cartesian product of the sets of records from two or more joined tables. Thus, it equates to an inner join where the join-condition always evaluates to either True or where the join-condition is absent from the statement.

Syntax  −

```sql
SELECT table1.column1, table2.column2...
FROM  table1, table2 [, table3 ]
```

```sql
SQL> SELECT  ID, NAME, AMOUNT, DATE
FROM CUSTOMERS, ORDERS;
```

Self Join
---------
The SQL SELF JOIN is used to join a table to itself as if the table were two tables; temporarily renaming at least one table in the SQL statement.

Syntax−

```sql
SELECT a.column_name, b.column_name...
FROM table1 a, table1 b
WHERE a.common_field = b.common_field;
```

```sql
SQL> SELECT  a.ID, b.NAME, a.SALARY
FROM CUSTOMERS a, CUSTOMERS b
WHERE a.SALARY < b.SALARY;
```

Keys
---------

- **Primary key**: Uniquely identify the row. Cannot be null.
- **Candidate key**: Can be chosen as primary key
- **Composite key**: Combination of multiple columns
- **Foreign key**: Column referencing other table's primary key

Indexes
---------

- Improves speed of data retrieval.
- Primary and foreign keys are indexed by default.
- **Non-clustered index**: Rows are unordered (stored in heap). While index is stored separately as sorted column.
- **Clustered index**: The rows themselves are ordered (thus there can be only 1 clustered index per table).
- **Composite index**: Index on multiple columns (c1,c2,c3). Order of columns should match the where clauses for maximum efficiency.
- **Cardinality**: Uniqueness of rows (eg: PassportID vs Gender column values).
- **Bitmap-index**: Used when cardinality is very low (eg: Gender column).
- **Data structures used**: Bit arrays, hashmaps or B+Trees (most common).


SQL - Indexes
---------
Indexes are special lookup tables that the database search engine can use to speed up data retrieval. Simply put, an index is a pointer to data in a table. An index in a database is very similar to an index in the back of a book.

For example, if you want to reference all pages in a book that discusses a certain topic, you first refer to the index, which lists all the topics alphabetically and are then referred to one or more specific page numbers.

An index helps to speed up SELECT queries and WHERE clauses, but it slows down data input, with the UPDATE and the INSERT statements. Indexes can be created or dropped with no effect on the data.

Creating an index involves the CREATE INDEX statement, which allows you to name the index, to specify the table and which column or columns to index, and to indicate whether the index is in an ascending or descending order.

Indexes can also be unique, like the UNIQUE constraint, in that the index prevents duplicate entries in the column or combination of columns on which there is an index.

The CREATE INDEX Command
---------

The basic syntax of a CREATE INDEX is as follows.
```sql
CREATE INDEX index_name ON table_name;
```

Single-Column Indexes

A single-column index is created based on only one table column. The basic syntax is as follows.

```sql
CREATE INDEX index_name
ON table_name (column_name);
```

Unique Indexes
---------
Unique indexes are used not only for performance, but also for data integrity. A unique index does not allow any duplicate values to be inserted into the table. The basic syntax is as follows.

```sql
CREATE UNIQUE INDEX index_name
on table_name (column_name);
```

Composite Indexes
---------
A composite index is an index on two or more columns of a table. Its basic syntax is as follows.
```sql
CREATE INDEX index_name
on table_name (column1, column2);
```

Whether to create a single-column index or a composite index, take into consideration the column(s) that you may use very frequently in a query's WHERE clause as filter conditions.

Should there be only one column used, a single-column index should be the choice. Should there be two or more columns that are frequently used in the WHERE clause as filters, the composite index would be the best choice.

Implicit Indexes
---------
Implicit indexes are indexes that are automatically created by the database server when an object is created. Indexes are automatically created for primary key constraints and unique constraints.

The DROP INDEX Command
---------

An index can be dropped using SQL DROP command. Care should be taken when dropping an index because the performance may either slow down or improve.

The basic syntax is as follows −
```sql
DROP INDEX index_name;
```

When should indexes be avoided?
---------

Although indexes are intended to enhance a database's performance, there are times when they should be avoided.

The following guidelines indicate when the use of an index should be reconsidered.

- Indexes should not be used on small tables.
- Tables that have frequent, large batch updates or insert operations.
- Indexes should not be used on columns that contain a high number of NULL values.
- Columns that are frequently manipulated should not be indexed.


`Ref:` https://www.tutorialspoint.com/sql/sql-indexes.htm


Queries
---------

- **Union**: Merges content of 2 structurally compatible tables
- **Group By**: Used to aggregate (avg, sum, count) values. Aggregate column should be same as group-by column.
- **Having**: Adding where clauses on top of group-by.

Frequently asked queries
---------

- Find all departments with sales more than 1000

```sql
SELECT department, SUM(sales) AS "Total sales"
FROM order_details	
GROUP BY department
HAVING SUM(sales) > 1000;
```

- Print all employee ids with their manager ids

```sql
SELECT e1.emp_id, e1.emp_mgr_id 
FROM employee e1 LEFT JOIN employee e2 
   ON e1.emp_mgr_id = e2.emp_id
```

- Print all manager names with count of directs

```sql
SELECT e2.ename, count(e1.ename) 
FROM employee_s e1 LEFT OUTER JOIN employee_s e2 
  ON e1.manager_id = e2.eid 
group by e2.ename;
```

- Print 10th highest salary

```sql
SELECT Salary FROM
(  SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC LIMIT 10 ) 
AS Emp ORDER BY Salary LIMIT 1;
```

SQL
-------------
SQL stands for Structured Query Language. SQL is a standard language for storing, manipulating, and retrieving data in relational database systems.

NoSQL
-------------
NoSQL or `non-SQL` is a non-relational database that does not require a fixed schema and is easy to scale.

NoSQL does not necessarily mean that a database does not support SQL. Instead, it means that the database is not an RDBMS.

While traditional RDBMS rely on SQL syntax to store and query data, on the other hand, NoSQL database systems use other technologies and programming languages to store structured, unstructured or semi-structured data.


SQL vs NoSQL
-------------

|Comparing       | SQL	                                |NoSQL   |
|------------------|--------------------------|-------------------------------------------|
|Query Language 	    |  Structured query language (SQL)                 | No declarative query language                |
| Database type      |         Table          |     Key-value, document, wide-column, and graph            |
|  Schema     |    Predefined               |    Dynamic             |
| Data model      |   Relational                |  Non-relational               |
|  Popular database examples     | MySQL, PostgreSQL, Oracle, and MS-SQL                  |  MongoDB, Apache HBase, Amazon DynamoDB, Redis, Couchbase, Cassandra, and Elasticsearch               |
| Ability to scale      |  Vertical                 |   Horizontal              |
| ACID vs BASE      |    ACID               |     BASE            |
|  Advantages     |   Cross-platform support, secure and free                | Easy to use, high performance, and flexible tool                |
|  Disadvantages     |  Complex to maintain and inefficient if processing big data, complex relational database systems are difficult to export into other systems, not good for handling various data types                 | Data is less structured, NoSQL databases are not as reliable (no ACID support), NoSQL databases are newer and may offer less features than their SQL counterparts                |
| Use cases      |  ACID support, complex queries, no changes or growth                 | Real-time data, volumes of data with no structure, agile business, cloud computing                |

Database Types
-------------

Database types depend on the way the data is stored.
 `SQL` has a table-based database. Table database stores data into tables with fixed rows and columns.

`NoSQL` has 4 types of databases:
1. Key-value database – Stores every data element as an attribute name or key together with its value.
2. Document database – Stores data in JSON, BSON, or XML documents.
3. Wide-column database – Stores and groups data into columns instead of rows.
4. Graph database – Optimized to capture and search the connections between data elements.

**Schema**
-------------

A database schema is a structure that defines how a database is constructed. It defines how the data is organized and how the relations among data are associated. There are two types of schemas:

- Predefined
- Dynamic

SQL needs a predefined schema for unstructured data. You need to predefine data structure in the form of tables before you start to use SQL to manipulate data.

However, a NoSQL database does not require a predefined schema. NoSQL uses a dynamic schema for unstructured data. A dynamic schema allows storing data before applying schema. Schema completely depends on how you want to store data.


**Data Model**
-------------

The data model shows the logical structure of the database. It organizes elements of data and standardizes how they relate to each other. There are two types of data models:

- Relational
- Nonrelational

We can observe differences between these data models by looking at the multiple entities. Consider an order from a restaurant as an example and two entities: Order and Delivery Address.

SQL uses a `relational data model`. SQL relational model uses many-to-many relationship. In many-to-many relationship, a single Order row can relate to several Delivery Address rows. Similarly, each Delivery address row can relate to several Order rows.

NoSQL uses a `nonrelational data model` that does not use relationships. NoSQL databases denormalize data by duplicating Delivery address in each Order row that contains that delivery address. Therefore, data is stored multiple times. This enables easy storage and data retrieval and increases the speed of the query.


**Ability to Scale**
-------------
Database scalability is the ability to hold increasing amounts of data without sacrificing performance. There are two types of scalability:

- Vertical
- Horizontal

SQL databases are vertically scalable. In vertical scaling, data resides on a single node, and the only way to scale up is by adding more hardware resources, such as CPU and RAM, to one existing machine. This makes vertical scaling more costly. An additional downside of vertical scaling is that it runs on one machine so if the server goes down, your application will go down too.

NoSQL databases are horizontally scalable. In horizontal scaling, each node contains only part of the data which allows you to add more machines to the existing group of distributed systems. This makes horizontal scaling cheaper and quicker.

**ACID vs BASE**
-------------
SQL databases use the ACID consistency model. ACID stands for:

- Atomic – All operations in a transaction succeed or every operation is rolled back. Partial success is not allowed.
- Consistent – Each transaction moves the database from one valid state to another. Transaction can’t leave the database in an inconsistent state.
- Isolated – Transactions can’t interfere with each other.
- Durable – The results of applying a transaction are permanent, even in the presence of failures.

The main feature of the ACID model is consistency. When you complete a transaction, its data is consistent and stable.

NoSQL databases use the BASE consistency model. BASE stands for:

- Basically Available – All users can perform a query. The database spreads data across several systems so in case that a failure happens to a segment of data, the database will not experience a complete outage.
- Soft State – Database state can change over time.
- Eventual Consistency – If the system is functioning and we wait long enough, the database will eventually become consistent.

The advantage of the BASE consistency model is that transactions are committed faster. Databases that use the BASE model prefer availability over consistency of replicated data.

**Use Cases**
-------------

Not every database fits every business needs. Let’s take a closer look at use cases for both types of databases.

Reasons to use an SQL database:
-------------

- When you need ACID support – With ACID support you get data consistency and 100% data integrity.
- When you are working with complex queries and reports – SQL is a better fit for complex query environments when compared to NoSQL.
- When you don’t anticipate a lot of changes or growth – If your business is not growing exponentially, there is no
  reason to use a system designed to support an increase in data volume.

Reasons to use a NoSQL database:
-------------

- When you need real-time data – NoSQL does not require schemas, so it makes the information process quicker.
- When you store volumes of data with no structure – NoSQL supports all data types.
- When you run an agile business – NoSQL does not require the preparation process, so it reduces downtime.
- When you want to make most out of cloud computing and storage – For a cloud solution to be scalable, the data must be
  easy to share across multiple servers.

## UNION vs UNION ALL

UNION removes duplicate records (where all columns in the results are the same), UNION ALL does not.

There is a performance hit when using UNION instead of UNION ALL, since the database server must do additional work to
remove the duplicate rows, but usually you do not want the duplicates (especially when developing reports).

To identify duplicates, records must be comparable types as well as compatible types. This will depend on the SQL
system. For example the system may truncate all long text fields to make short text fields for comparison (MS Jet), or
may refuse to compare binary fields (ORACLE)

UNION Example:

```sql
SELECT 'foo' AS bar
UNION
SELECT 'foo' AS bar
```

Result:
```log
+-----+
| bar |
+-----+
| foo |
| foo |
+-----+
2 rows in set (0.00 sec)
```

UNION ALL example:

```sql
SELECT 'foo' AS bar
UNION ALL
SELECT 'foo' AS bar
```

Result:
```log
+-----+
| bar |
+-----+
| foo |
| foo |
+-----+
2 rows in set (0.00 sec)
```

Reference: 
https://stackoverflow.com/questions/49925/what-is-the-difference-between-union-and-union-all
https://www.javatpoint.com/mysql-union-vs-union-all


## TRUNCATE vs DELETE, vs  DROP

|TRUNCATE       | DELETE	                                |DROP   |
|------------------|--------------------------|-------------------------------------------|
|  The TRUNCATE TABLE the command deletes the data inside a table, but not the table itself. TRUNCATE deletes all the rows of a table at once. It only logs once in the transaction log.| The DELETE query deletes all records from a table of a database without deleting the table schemas like columns, indexes, etc.  | The DROP TABLE statement is used to drop an existing table in a database. DROP TABLEquery removes the table definition and all data, indexes, triggers, constraints, and permissions for that table.      |
|  TRUNCATE TABLE table_name;| DELETE FROM table_name WHERE condition;|  DROP TABLE table_name;     |



## TRUNCATE
- TRUNCATE is a Data Definition Language(DDL) command.
- TRUNCATE is executed using a table lock and the whole table is locked to remove all records.
- TRUNCATE removes all rows from a table at once.
- WHERE clause cannot be used with TRUNCATE.
- It is faster performance-wise than DELETE as it only logs once in the transaction log.
- TRUNCATE cannot be used with indexed views.
- To use Truncate on a table, ALTER permission is required on the table.

## DELETE
- DELETE is a Data Manipulation Language(DML) command.
- DELETE is executed using a row lock mechanism, each row in the table is locked for deletion.
- WHERE clause can be used with DELETE query to filter out records and then delete them.
- DELETE maintains an entry in the transaction log for each row deleted. Hence it is slower than TRUNCATE.
- DELETE permission is required on the table to delete the records.
- The DELETE can be used with indexed views.


## DROP
- The DROP command removes a table from the database.
- All the tables’ rows, indexes, and privileges will also be removed.
- No Data Manipulation Language(DML) triggers will be fired.
- DROP is a Data Definition Language(DDL).
- DELETE operations can be rolled back (undone), while DROP and TRUNCATE operations cannot be rolled back.


Reference: https://medium.com/javarevisited/know-the-differences-between-truncate-delete-and-drop-4ee70bb736fb



For more information:

- [Cheat sheet](http://files.zeroturnaround.com/pdf/zt_sql_cheat_sheet.pdf)
- [Clustered vs Non-clustered indexes](https://docs.microsoft.com/en-us/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described)
- [DB Index Wiki](https://en.wikipedia.org/wiki/Database_index)
- [SQL vs NoSQL: What's the Difference?](https://phoenixnap.com/kb/sql-vs-nosql)
- [SQL vs NoSQL: when to use?](https://www.imaginarycloud.com/blog/sql-vs-nosql/)








