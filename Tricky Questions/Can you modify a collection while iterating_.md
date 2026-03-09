Yes, but you must use **Iterator’s `remove()`** method.

List&lt;String&gt; list = new ArrayList<>(List.of("A", "B", "C"));  
Iterator&lt;String&gt; it = list.iterator();  
while (it.hasNext()) {  
    if (it.next().equals("B")) {  
        it.remove(); // Safe  
    }  
}

Modifying the list directly inside the loop causes `ConcurrentModificationException`.