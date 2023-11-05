# Part 1
```ruby
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {

    @Test
    public void testAverage1() {
        double[] input1 = {3.0, 1.0, 4.0, 1.0, 5.0};
        // The lowest value is 1.0 (at index 1), and the average without the lowest is (3.0 + 4.0 + 5.0) / 3 = 4.0.
        assertEquals(4.0, ArrayExamples.averageWithoutLowest(input1), 0.001); // Using delta for double comparison
    }
}
```
