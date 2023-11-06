# Lab Report 3 - Bugs and Commands

### Part 1

A failure-inducing input for the buggy program, as a JUnit test and any associated code (write it as a code block in Markdown)

``` java
  @Test
  public void testReverseInPlace2() {
    int[] input1 = { 1, 2 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[] { 2, 1 }, input1);
  }
  
```

Associated Code:
``` java
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```

An input that doesn’t induce a failure, as a JUnit test and any associated code (write it as a code block in Markdown)

``` java
  @Test
  public void testReverseInPlace() {
    int[] input1 = { 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[] { 3 }, input1);
  }
```

Associated Code:
``` java
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```


The symptom, as the output of running the tests (provide it as a screenshot of running JUnit with at least the two inputs above)

Running only the good test:
``` java
JUnit version 4.13.2
.
Time: 0.009

OK (1 test)

```
Running the faulty test:
``` java
1) testReverseInPlace2(ArrayTests)
arrays first differed at element [1]; expected:<1> but was:<2>
	at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
	at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
	at org.junit.Assert.internalArrayEquals(Assert.java:534)
	at org.junit.Assert.assertArrayEquals(Assert.java:418)
	at org.junit.Assert.assertArrayEquals(Assert.java:429)
	at ArrayTests.testReverseInPlace2(ArrayTests.java:22)
	... 32 trimmed
Caused by: java.lang.AssertionError: expected:<1> but was:<2>
	at org.junit.Assert.fail(Assert.java:89)
	at org.junit.Assert.failNotEquals(Assert.java:835)
	at org.junit.Assert.assertEquals(Assert.java:120)
	at org.junit.Assert.assertEquals(Assert.java:146)
	at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
	at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
	... 38 more
```
The bug, as the before-and-after code change required to fix it (as two code blocks in Markdown)


Old Code:
``` java
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```

New Code:
``` java
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length/2; i += 1) {
      int temp = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = temp;
    }
  }
```
Originally we would be going through the entire array, but then simply assigning the first value to be the smallest, without "swapping" the values. In the updated code, we go to n/2 floored (in the odd case our middle element need not be swapped), and then swap the elements by using a temp variable.

### Part 2
I chose to look at the `grep` command.

Flag `-v`:
The man page of grep says:

` -v, --invert-match
             Selected lines are those not matching any of the specified
             patterns.
`

```bash
danwyl@danwyl-MBP biomed % grep -v -r "base pair" | wc -l
  490447
```
In this case, we are looking at things that do NOT have base pair. This is helpful because maybe we want to find things that don't have "base pair" in them.

```bash
danwyl@danwyl-MBP biomed % grep -v -l "skin" *.txt |wc -l
     837
```
Similarly in this instance, perhaps we want to file how many disease files do not contain the word skin (thus are unrelated to skin).

Flag `-c`:
The man page of `grep` command:

```
       -c, --count
              Suppress normal output; instead print a count  of  matching
              lines  for  each  input  file.  With the -v, --invert-match
              option (see below), count non-matching lines.
```

```
danwyl@danwyl-MBP biomed % grep -c "base pair" *.txt 
1468-6708-3-1.txt:0
1468-6708-3-10.txt:0
1468-6708-3-3.txt:0
1468-6708-3-4.txt:0
1468-6708-3-7.txt:0
1471-2091-2-10.txt:0
1471-2091-2-11.txt:0
1471-2091-2-12.txt:0
1471-2091-2-13.txt:0
1471-2091-2-16.txt:0
1471-2091-2-5.txt:0
1471-2091-2-7.txt:0
1471-2091-2-9.txt:0
1471-2091-3-13.txt:0
1471-2091-3-14.txt:0
1471-2091-3-15.txt:0
1471-2091-3-16.txt:0
1471-2091-3-17.txt:0
1471-2091-3-18.txt:0
1471-2091-3-22.txt:0
1471-2091-3-23.txt:0
1471-2091-3-30.txt:0
1471-2091-3-31.txt:0
1471-2091-3-4.txt:1
```
Note that I cut off some of the files. Now we can get a count of how many instances of "base pair" lines occur in the file. Maybe a file talks a lot about base pairs and we would like to know.

```
danwyl@danwyl-MBP technical % grep -c "FAA" 911report/chapter-1.txt
96
```
In the instance where we only give one input file, we get the number of lines that contain it, similar as to if we were to pipe it into `wc -l`.

Flag `-h`:
The man page of `grep` command:

```
     -h, --no-filename
             Never print filename headers (i.e., filenames) with output lines.
```

```
danwyl@danwyl-MBP biomed % grep -c -h "base pair" *.txt            
0
0
0
0
0
0
0
0
0
0
0
0
0
0
0
0
0
0
0
0
0
0
0
1
```
This is a very similar case as above, but instead now we just get the numbers of matches in each file directly. This makes it easier if we want to do any counting or mathematical operations, where we do not need to get rid of the file name after grepping.

```
danwyl@danwyl-MBP technical % grep "legal" government/Media/Legal*            
government/Media/Legal-aid_chief.txt:Taking charge of Nebraska's statewide legal-aid law firm fit
government/Media/Legal-aid_chief.txt:legal help.
government/Media/Legal-aid_chief.txt:percent of the state's legal-aid cases, figures it reaches only 15
```
VS

```
danwyl@danwyl-MBP technical % grep -h "legal" government/Media/Legal*
Taking charge of Nebraska's statewide legal-aid law firm fit
legal help.
percent of the state's legal-aid cases, figures it reaches only 15
```

If we are doing grep on a deeper file structure, it can be the case where the file path itself is quite long. Perhaps we don't really care about the file path at all, just what the content holds. Then using `-h` like in the second instance is helpful.


Flag `-i`:
The man page of `grep` command:
```
     -i, --ignore-case
             Perform case insensitive matching.  By default, grep is case sensitive.
```

```
danwyl@danwyl-MBP technical % grep -h "legal" government/Media/Legal* | wc -l
      54
danwyl@danwyl-MBP technical % grep -h -i "legal" government/Media/Legal* | wc -l
     119
```
As we can see in this case, using the `-i` flag is really helpful - there are many instances where we want to be case insensitive, and so using `-i` comes in handy. This is a very basic "fuzzy matching" usage of the flag.

```
danwyl@danwyl-MBP biomed % grep -c "Outcome" *.txt | grep -v 0 | wc -l
      16
danwyl@danwyl-MBP biomed % grep -c -i "Outcome" *.txt | grep -v 0 | wc -l
     114
```

In this instance, we see that "Outcome" (which is not a proper noun) appears with a capital O (likely from being at the beginning of a sentence) 16 times, but when we look at "Outcome" we wee it comes up actually 114 times, which is more likely what we are looking for.

