&nbsp;

&nbsp;

### <span style="color: #f8faff;">Sort a</span> `Map<Integer, String>` <span style="color: #f8faff;">by its</span> **Integer keys.**

&nbsp;

### Key Considerations:

1.  **Maps are not order-aware** (except `LinkedHashMap`, `TreeMap`).
    
2.  **Sorting requires converting entries to a structure that supports ordering** (e.g., `List` or `Stream`).
    
3.  **Choice of comparator** and **result storage** affect correctness and performance.
    

&nbsp;

**Common mistake**:

Forgetting to call `entrySet()` and trying to stream the map directly (e.g., `originalMap.stream()`), which is invalid. Maps aren’t collections!

&nbsp;

&nbsp;