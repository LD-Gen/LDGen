# FBDText Code Generation Example

## Example

### Requirement

Write a function to add four analog quantities. All variables are global variables.

### FBDText

```text
VAR_GLOBAL
    analog1 : REAL;
    analog2 : REAL;
    analog3 : REAL;
    analog4 : REAL;
END_VAR

FUNCTION SumAnalogQuantities : REAL
    SYMBOLS
        ADD1 : ADD;
        ADD2 : ADD;
        ADD3 : ADD;
    END_SYMBOLS

    CONNECTIONS
        analog1 TO ADD1.IN1;
        analog2 TO ADD1.IN2;
        ADD1.OUT TO ADD2.IN1;
        analog3 TO ADD2.IN2;
        ADD2.OUT TO ADD3.IN1;
        analog4 TO ADD3.IN2;
        ADD3.OUT TO SumAnalogQuantities;
    END_CONNECTIONS
END_FUNCTION
```
