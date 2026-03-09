Yes. If `finally` throws an exception, it will **suppress** the one from `try`.

try {  
    throw new RuntimeException("from try");  
} finally {  
    throw new RuntimeException("from finally");  
}

Only **"from finally"** will be thrown.