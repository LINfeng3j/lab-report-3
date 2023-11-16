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
__The main problem with this code is that it ends with /arr.lenth-1, which means it only records the case where there is one lowest number, so I set up an extra loop so that it can record multiple most in sufficient numbers so that when we And changed -1 to -the lowest number.__ <br>
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
75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/government (main)    
$ ls |grep -v Post
About_LSC/
Alcohol_Problems/
Env_Prot_Agen/
Gen_Account_Office/
Media/
```
__-v is to invert the string I input that files and directory have in the working directory__ <br>
__In my observation, grep ignores the type of files and recognizes the entire file or directory as a string to search for it. and -v is to search for a string and only ignore all files and directories with that pattern of string__ <br>

```ruby
75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/911report (main)     
$ ls |grep -v 13
chapter-1.txt
chapter-10.txt
chapter-11.txt
chapter-12.txt
chapter-2.txt
chapter-3.txt
chapter-5.txt
chapter-6.txt
chapter-7.txt
chapter-8.txt
chapter-9.txt
preface.txt
```
__In the second example, I tested using the number in the filename, and that makes it cancel out all text. Files with 13(chapter-13.1 to chapter-13.5) will delete all those with similar patterns here. If I want it to exclude only 13, I may need to use the -w I used later. __ <br>

__2. grep -c__
```ruby
75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/government  (main)     
$ ls |grep -c A
4
```
__-c is the count of how many targets are searched for all strings. My first example searches for A, and in this directory, four directories contain uppercase A. Then it returns four as I thought.__ <br>
```ruby
75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/biomed (main)        
$ ls |grep -c 1468-
5
```
__In the second example, I used 1468- as the string and tried to let it search all text files starting with this number, and it worked successfully and got the correct 5.__ <br>

__3. grep -y__
```ruby
75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/government (main)    
$ ls |grep -y a
About_LSC/
Alcohol_Problems/
Env_Prot_Agen/
Gen_Account_Office/
Media/
Post_Rate_Comm/

75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/government (main)    
$ ls |grep a
Media/
Post_Rate_Comm/
```
__The usage of -y is well demonstrated in this example. It can be searched regardless of the case. This may catch errors in some accidentally misspelled cases and help fix them.__ <br>
```ruby
75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/government/about_LSC (main)
$ ls |grep -y config
CONFIG_STANDARDS.txt

75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/government/about_LSC (main)
$ ls |grep config
```
__This is also a good example. If I forget how many characters I uppercase on a file but still remember its name, I can use -y to find it.__ <br>

__4. grep -w__ <br>
```ruby
75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/government/Media (main)
$ ls |grep BusinessWire
BusinessWire.txt
BusinessWire2.txt

75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/government/Media (main)
$ ls |grep -w BusinessWire
BusinessWire.txt
```
__The use of -w can be seen in this example as to search for a specific target. The output will only be the same string you input; any other strings containing the same string will be canceled.__ <br>
```ruby

75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/government/about_LSC (main)
$ grep "leader" conference_highlights.txt
bring together executive leadership from newly created statewide
LSC-funded programs and the experienced leadership of the more
judges, and bar leaders (including the new executive director of
Participants suggested that statewide program leadership needed
Executive Director can be a visible leader among staff and the
client-centered through client leadership in the state planning
the Pine Tree Legal Assistance site. All client leadership in Maine
embracing cultural diversity, leadership succession planning, and
the general expansion of program leadership, especially as a part
i. Regional leadership conferences - small intense retreat-like
potential leaders from each of
Mentoring/exchange program for newer leaders or leaders
Online leadership clearinghouse with links to non-profit
management and leadership sites and materials, M.I.E.

75442@jfela MINGW64 ~/Documents/GitHub/docsearch/technical/government/about_LSC (main)
$ grep -w  "leader" conference_highlights.txt
Executive Director can be a visible leader among staff and the
```
__This example shows how to use it within text files to search for the word beside the other words that also contain that string. (Like apple and pineapple)__ <br>

# source
__The sources I used are below__ <br>
https://man7.org/linux/man-pages/man1/grep.1.html <br>
__The first source I found contains all options and descriptions for the grep command. I used it to choose options and to guess how they work.__  
https://en.wikibooks.org/wiki/Grep <br>
__This source gives some examples of commands, So I used it as a resource to find some comments to test.__
https://www.gnu.org/savannah-checkouts/gnu/grep/manual/grep.html<br>
__This one is kind of the same as the first one but contains more explanation than the other ones, So it also helps me alot__

__The way I use these URLs is to find grep's various command lines and rough explanations and then test them in my previous lab workspace.__

















