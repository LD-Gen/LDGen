# LD Core Logic Few-shot Examples — Separated Connections

These four examples are used for the T-S and NL-S configurations. Each example is represented as a `<Refined Requirement, Core Logic>` pair using separate `# current connections` and `# data connections` sections.

## Example 1

### Refined Requirement
Calculate the sum, difference, product, quotient, and remainder of two INT inputs `A` and `B`. The input `B` is guaranteed to be nonzero. Write the results to `Sum`, `Difference`, `Product`, `Quotient`, and `Remainder`.

### Core Logic

#### # blocks
```text
ADD1 : ADD;
SUB1 : SUB;
MUL1 : MUL;
DIV1 : DIV;
MOD1 : MOD;
```

#### # current connections
```text
Power_Positive -> ADD1.EN;
ADD1.ENO -> SUB1.EN;
SUB1.ENO -> MUL1.EN;
MUL1.ENO -> DIV1.EN;
DIV1.ENO -> MOD1.EN;
MOD1.ENO -> Power_Negative;
```

#### # data connections
```text
A TO ADD1.IN1;
B TO ADD1.IN2;
ADD1.OUT TO Sum;

A TO SUB1.IN1;
B TO SUB1.IN2;
SUB1.OUT TO Difference;

A TO MUL1.IN1;
B TO MUL1.IN2;
MUL1.OUT TO Product;

A TO DIV1.IN1;
B TO DIV1.IN2;
DIV1.OUT TO Quotient;

A TO MOD1.IN1;
B TO MOD1.IN2;
MOD1.OUT TO Remainder;
```

## Example 2

### Refined Requirement
Monitor the REAL input `CurrentTemperature`. When `CurrentTemperature` is greater than 90, set the BOOL outputs `HeaterCutOff` and `TriggerAlarm` to TRUE. Otherwise, both outputs shall be FALSE.

### Core Logic

#### # blocks
```text
GT1 : GT;
Contact1 : CONTACT;
CutOff : COIL;
Alarm : COIL;
```

#### # current connections
```text
Power_Positive -> GT1.EN;
GT1.ENO -> Contact1.EN;
Contact1.ENO -> CutOff.EN;
CutOff.ENO -> Alarm.EN;
Alarm.ENO -> Power_Negative;
```

#### # data connections
```text
CurrentTemperature TO GT1.IN1;
90 TO GT1.IN2;
GT1.OUT TO OverHeating;
OverHeating TO Contact1.VAR;
CutOff.VAR TO HeaterCutOff;
Alarm.VAR TO TriggerAlarm;
```

## Example 3

### Refined Requirement
Count rising edges of the BOOL input `ProductReached`. When the count reaches 20, `START_PACKAGING` shall become TRUE and remain TRUE for 5 seconds. After 5 seconds, `START_PACKAGING` shall become FALSE and `STOP_PACKAGING` shall become TRUE.

### Core Logic

#### # blocks
```text
CTU1 : CTU;
GE1 : GE;
TON1 : TON;
RunEnable : CONTACT;
NotDone : NEG_CONTACT;
Start : COIL;
DoneContact : CONTACT;
Stop : COIL;
```

#### # current connections
```text
Power_Positive -> CTU1.EN;
CTU1.ENO -> GE1.EN;
GE1.ENO -> TON1.EN;
TON1.ENO -> Power_Negative;

Power_Positive -> RunEnable.EN;
RunEnable.ENO -> NotDone.EN;
NotDone.ENO -> Start.EN;
Start.ENO -> Power_Negative;

Power_Positive -> DoneContact.EN;
DoneContact.ENO -> Stop.EN;
Stop.ENO -> Power_Negative;
```

#### # data connections
```text
ProductReached TO CTU1.CU;
20 TO CTU1.PV;
CTU1.CV TO Total;

Total TO GE1.IN1;
20 TO GE1.IN2;
GE1.OUT TO Trigger;

Trigger TO TON1.IN;
T#5s TO TON1.PT;

Trigger TO RunEnable.VAR;
TON1.Q TO NotDone.VAR;
Start.VAR TO START_PACKAGING;

TON1.Q TO DoneContact.VAR;
Stop.VAR TO STOP_PACKAGING;
```

## Example 4

### Refined Requirement
Implement a four-state lighting controller driven by rising edges of the BOOL input `SW`: first press, `LAMP0=TRUE` and `LAMP1=FALSE`; second press, `LAMP0=FALSE` and `LAMP1=TRUE`; third press, both outputs TRUE; fourth press, both outputs FALSE. The sequence then repeats.

### Core Logic

#### # blocks
```text
CTU1 : CTU;
MOD1 : MOD;
EQ1 : EQ;
EQ2 : EQ;
EQ3 : EQ;
OR_A : OR;
OR_B : OR;
ContactA : CONTACT;
ContactB : CONTACT;
LampACoil : COIL;
LampBCoil : COIL;
```

#### # current connections
```text
Power_Positive -> CTU1.EN;
CTU1.ENO -> MOD1.EN;
MOD1.ENO -> EQ1.EN;
EQ1.ENO -> EQ2.EN;
EQ2.ENO -> EQ3.EN;
EQ3.ENO -> OR_A.EN;
OR_A.ENO -> OR_B.EN;
OR_B.ENO -> Power_Negative;

Power_Positive -> ContactA.EN;
ContactA.ENO -> LampACoil.EN;
LampACoil.ENO -> Power_Negative;

Power_Positive -> ContactB.EN;
ContactB.ENO -> LampBCoil.EN;
LampBCoil.ENO -> Power_Negative;
```

#### # data connections
```text
SW TO CTU1.CU;
2147483647 TO CTU1.PV;
CTU1.CV TO Count;

Count TO MOD1.IN1;
4 TO MOD1.IN2;
MOD1.OUT TO State;

State TO EQ1.IN1;
1 TO EQ1.IN2;
EQ1.OUT TO S1;

State TO EQ2.IN1;
2 TO EQ2.IN2;
EQ2.OUT TO S2;

State TO EQ3.IN1;
3 TO EQ3.IN2;
EQ3.OUT TO S3;

S1 TO OR_A.IN1;
S3 TO OR_A.IN2;
OR_A.OUT TO LampAOn;

S2 TO OR_B.IN1;
S3 TO OR_B.IN2;
OR_B.OUT TO LampBOn;

LampAOn TO ContactA.VAR;
LampACoil.VAR TO LAMP0;

LampBOn TO ContactB.VAR;
LampBCoil.VAR TO LAMP1;
```
