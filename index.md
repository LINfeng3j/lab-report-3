# Part 1
1.A failure-inducing input for the buggy program, as a JUnit test and any associated code
```ruby
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {

    @Test
    public void testAverage1() {
        double[] input1 = {3.0, 1.0, 4.0, 1.0, 5.0};
        // The lowest value is 1.0 (at index 1), and the average without the lowest is (3.0 + 4.0 + 5.0) / 3 = 4.0.
        assertEquals(4.0, ArrayExamples.averageWithoutLowest(input1), 0.001); 
    }
}
```
2.An input that doesn’t induce a failure, as a JUnit test and any associated code (write it as a code block in Markdown)
```ruby
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {

    @Test
    public void testAverage2() {
        double[] input2 = {2.0, 3.0, 4.0, 6.0, 8.0};
        // The lowest value is 2.0 (at index 0), and the average without the lowest is (3.0 + 4.0 + 6.0 + 8.0) / 4 = 5.25.
        assertEquals(5.25, ArrayExamples.averageWithoutLowest(input2), 0.001); 
    }
}
```
3.The symptom, as the output of running the tests<br>
__created test.sh to fast test file__ <br>
![Image](3.01.png)  <br>
__The symptom for testAverage1__ <br>
![Image](3.02.png)  <br>
__The symptom for testAverage2__ <br>
![Image](3.03.png)] <br>
4.The bug, as the before-and-after code change required to fix it <br>
__Original code__ <br>
```ruby
static double averageWithoutLowest(double[] arr) {
    if (arr.length < 2) {
        return 0.0;
    }
    double lowest = arr[0];
    for (double num : arr) {
        if (num < lowest) {
            lowest = num;
        }
    }
    double sum = 0;
    for (double num : arr) {
        if (num != lowest) {
            sum += num;
        }
    }
    return sum / (arr.length - 1);
}
```
__This code cannot recognize the occurrence of multiple lowest numbers.__ <br>
__To fix that, I choose to make a change in the loop and set a value that removes each one to the lowest number to make it work.__ <br>
```ruby
static double averageWithoutLowest(double[] arr) {
    if (arr.length < 2) return 0.0;

    double lowest = arr[0];
    int countLowest = 0;
    double sum = 0;

    for (double num : arr) {
        if (num < lowest) lowest = num;
    }

    for (double num : arr) {
        if (num == lowest) countLowest++;
        else sum += num;
    }

    return sum / (arr.length - countLowest);
}
```
# Part 2 <br>
__grep is command i choosed__ <br>
__1.grep -v__ <br>
```ruby
75442@jfela MINGW64 ~/Documents/GitHub/lab3 (main)
$ ls |grep -v Array
FileExample.class
FileExample.java
lib/
LinkedList.class
LinkedListExample.java
ListExamples.class
ListExamples.java
Node.class
StringChecker.class
test.sh
```
__-v is to invert the string I input that files and directory have in the working directory__ <br>
__In my observation, grep ignores the type of files and recognizes the entire file or directory as a string to search for it.__ <br>

```ruby
75442@jfela MINGW64 ~/Documents/GitHub/lab3 (main)
$ ls |grep -v lib/
ArrayExamples.class
ArrayExamples.java
ArrayTests.class
ArrayTests.java
FileExample.class
FileExample.java
LinkedList.class
LinkedListExample.java
ListExamples.class
ListExamples.java
Node.class
StringChecker.class
test.sh
```
__In the second example, I tested using the directory as the target of grep -v, and the results were exactly as expected. It will be the opposite of the normal grep result and display all information about /lib.__ <br>

__2. grep -c__
```ruby
75442@jfela MINGW64 ~/Documents/GitHub/lab3 (main)
$ ls |grep -c lib/
1
```
__-c is the count of how many targets are searched for all strings. My first example makes it search the only directory of this working directory. Then it returns one as I thought.__ <br>
```ruby
75442@jfela MINGW64 ~/Documents/GitHub/lab3 (main)
$ ls |grep -c .class
7

75442@jfela MINGW64 ~/Documents/GitHub/lab3 (main)
$ ls |grep -c .class
8
```
__In the second example, I used .class as the string and tried to let it search all .class files, and then it worked successfully and got the correct 7. But if I modify the name of one of the files to LinkedListExample.class.java, but it is a Java file, I find that grep will still pull it out. So I may have to pay attention to this in the future.__ <br>







