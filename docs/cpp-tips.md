Macro Variant Parameters:

```c++
#ifdef DEBUG
  #define debug_printf(str, ...)     do { printf(str, __VA_ARGS__); } while (0)
#else
  #define debug_printf(str, ...)
#endif
```

