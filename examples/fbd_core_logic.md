# FBD Core Logic Generation Example

## Example

### Refined Requirement

Calculate the sum of four analog quantities, `analog1`, `analog2`, `analog3`, and `analog4`. The result shall be written to `SumAnalogQuantities`. All variables are global variables.

### Core Logic

```text
# symbols
ADD1 : ADD;
ADD2 : ADD;
ADD3 : ADD;

# connections
analog1 TO ADD1.IN1;
analog2 TO ADD1.IN2;
ADD1.OUT TO ADD2.IN1;
analog3 TO ADD2.IN2;
ADD2.OUT TO ADD3.IN1;
analog4 TO ADD3.IN2;
ADD3.OUT TO SumAnalogQuantities;
```
