# Week 3 Lab Report: Servers and Bugs

---

# Student: ***[Ben Stirling](https://github.com/abenstirling)***
Professor: ***[Joe Politz](https://github.com/jpolitz)***
Date: ***10/12/22***

---

# Part 1

---

Below is my code for a basic search-engine:

```java
public String handleRequest(URI url) {

				//Useful landing-page for debugging
        if (url.getPath().equals("/")) {
            String wordsString = "";
            for (int i = 0; i < dictionaryIndex; i++) {
                wordsString += dictionary[i] + ", ";
            }
            return String.format("Numbers in dictionary: %d + Words are: %s", dictionaryIndex, wordsString);
	      
				//Search checks term with all strings in dictionary
				} else if (url.getPath().equals("/search")) {
            String[] query = url.getQuery().split("=");
            if (query[0].equals("s")){
                String searchTerm = query[1];
                String wordsMatching = "";

                for (int i = 0; i < dictionaryIndex; i++) {
                    if (dictionary[i].contains(searchTerm)) {
                        wordsMatching += dictionary[i] + ", ";
                    }
                }
                return String.format("Words matching %s are: %s and that's all.", searchTerm, wordsMatching);
            }
            num += 1;
            return String.format("Number incremented!");
        } 

				//Add adds the term to the dictionary
				else {
            System.out.println("Path: " + url.getPath());
            if (url.getPath().contains("/add")) {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    dictionary[dictionaryIndex] = parameters[1];
                    dictionaryIndex++;
                    return String.format("%s added to the dictionary! It's now %d words", parameters[1], dictionaryIndex);
                }
            }
            return "404 Not Found!";
        }
    }
```

 

### Screenshot 1: Landing Page

---

![Untitled](Week%203%20Lab%20Report%20Servers%20and%20Bugs%202f7e6f082d7645718a8d8abc6af61475/Untitled.png)

Here we are invoking the `url.getPath().equals("/")` path, which indexes and pasts the string array we call `dictionary`. 

Although we don’t manipulate or change any variables here, I found it super useful to check the length and placement for each of the terms. No arguments needed for this page. 

### Screenshot 2: Adding a term

---

![Untitled](Week%203%20Lab%20Report%20Servers%20and%20Bugs%202f7e6f082d7645718a8d8abc6af61475/Untitled%201.png)

Here we are invoking the `url.getPath().equals("/add")` path, which adds the term following `s=`in the url.  I also nicely let the user know what has been added, along with the number of terms in `dictionary`. 

### Screenshot 3: Searching for term(s)

---

![Untitled](Week%203%20Lab%20Report%20Servers%20and%20Bugs%202f7e6f082d7645718a8d8abc6af61475/Untitled%202.png)

Here we are invoking the `url.getPath().equals("/")` path, which searches the term following `s=` in the url. I nicely list out the matches in dictionary using the `contains(searchTerm)` method. 

In short, this “search engine” was a straightforward process that I did not have any major issues implementing. 

---

# Part 2

---

Choose 2 bugs, for each show: 

- code of test
- failing test output
- code fix needed
- connection between the symptom and the bug

### Bug #1: Repeating Lowest Digits

Below is the code for the test I am using for the method `testAverageWithoutLowest()`:

```java
// lowest is -1, so avg(-1,1,3) should be 1
  @Test
  public void testAverageWithoutLowest() {
    double[] input1 = { -1,-1,1, 3.0 };
    assertEquals(1, ArrayExamples.averageWithoutLowest(input1), 0.0001);
  }
```

Below is the image where the test failed:

![Untitled](Week%203%20Lab%20Report%20Servers%20and%20Bugs%202f7e6f082d7645718a8d8abc6af61475/Untitled%203.png)

Below is the fixed code section: 

```java
static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    boolean foundLowest = false;
    for(double num: arr) {
			//added code
      if(foundLowest) {
        sum += num;
      }
      else if (num == lowest && !foundLowest) {
        [foundLowest = true;
      }](https://www.notion.so/CSE-15L-60713fd507c4495cb1fa8c8803bab9a1)      
    }
    return sum / (arr.length - 1);
  }
```

Below is a picture of the successful test: 

![Untitled](Week%203%20Lab%20Report%20Servers%20and%20Bugs%202f7e6f082d7645718a8d8abc6af61475/Untitled%204.png)

From my understanding, it seemed like the initial code was removing the lowest digit and if there were same numbers with that same value it would remove them also. I created a variable named `foundLowest` which allowed the if-loop to just add terms if the lowest had already been found. 

### Bug #2: Who added 1?

Below is the code I am using to test the method `sumEvenIndices`:

```java
@Test 
  public void testSumEvensLength4() {
    int[] input1 = { 12, 13, 7, 2};
    assertEquals(EvensExample.sumEvenIndices(input1), 19);
  }
  @Test 
  public void testSumEvenLength5() {
    int[] input1 = { 12, 13, 7, 2, 33};
    assertEquals(EvensExample.sumEvenIndices(input1), 52);
  }
```

Below is the failure that I got: 

![Untitled](Week%203%20Lab%20Report%20Servers%20and%20Bugs%202f7e6f082d7645718a8d8abc6af61475/Untitled%205.png)

Below is the fixed code section: 

```java
static int sumEvenIndices(int[] nums) {
    int sum = 0;
    for(int i = 0; i < nums.length; i += 2) {
      //changed i+1 to i, step accounted for in loop increment
      sum += nums[i];
    }
    return sum;
  }
```

Below is a screenshot of the successful tests: 

![Untitled](Week%203%20Lab%20Report%20Servers%20and%20Bugs%202f7e6f082d7645718a8d8abc6af61475/Untitled%206.png)

From my understanding, the `i+1` that was initially there would actually count the odd indices. All I had to do was remove the addition, and all of the tests passed and our method works as intended by adding the even indices!

---