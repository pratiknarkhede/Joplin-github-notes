**Solution1**   
<br/>

```java
public static boolean isAnagramUsingHashMap(String s, String t) {
        //first check for length if mismatch surely it is not anagram
        if (s.length() != t.length()) return false;
        HashMap<Character, Integer> sHashMap = new HashMap<>();
        
        //populate hashmap with character frequency of each character for first string
        for (char c : s.toCharArray()) {
            if (sHashMap.containsKey(c)) {
                sHashMap.put(c, sHashMap.get(c) + 1);
            } else {
                sHashMap.put(c, 1);
            }
        }
        System.out.println(sHashMap);
        
        //populate hashmap with character frequency of character for second string
        HashMap<Character, Integer> tHashMap = new HashMap<>();
        for (char c : t.toCharArray()) {
            if (tHashMap.containsKey(c)) {
                tHashMap.put(c, tHashMap.get(c) + 1);
            } else {
                tHashMap.put(c, 1);
            }
        }
        //if both hashmaps are equal then they are anagrams
        System.out.println(tHashMap);
        if (sHashMap.equals(tHashMap)) {
            return true;
        }
        return false;
    }
```

  
<br/><br/>**Solution 2;**  
<br/>

```java
 //optimized solution
    //first populate the hashmap with frequency
    //for second hashmap for each occurence decremenet respective value in hashmap
    //if value become negative then false, if all zeroes then anagram

    public static boolean isAnagramOptimized(String s, String t) {
        if (s.length() != t.length()) return false;
        HashMap<Character, Integer> sHashMap = new HashMap<>();
        for (char c : s.toCharArray()) {
            sHashMap.put(c,sHashMap.getOrDefault(c,0)+1);
        }

        for (char c:t.toCharArray()){
            sHashMap.put(c,sHashMap.getOrDefault(c,0)-1);
            if(sHashMap.get(c)<0) return false;
        }
        return true;
    }
```