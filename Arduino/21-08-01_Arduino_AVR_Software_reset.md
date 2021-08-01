# How force reset Arduino Nano (or generic AVR board)

Is it possible to **reset** and Arduino board based on AVR (e.g. Arduino Nano or Arduino UNO) by adding this function:

```arduino
void(* resetFunc) (void) = 0;
```

and call the function:

```arduino
resetFunc();
```

where you need to reset your board.


