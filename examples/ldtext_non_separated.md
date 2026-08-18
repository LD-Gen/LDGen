# LDText Code-generation Few-shot Examples — Non-separated Connections

These examples are intended for the T-NS and NL-NS configurations.
Each example is represented as a `<Requirement, LDText>` pair.
The non-separated LDText BNF is supplied by the corresponding prompt template and is not repeated here.

## Example 1

### Requirement

The BOOL output `MotorOutput` shall be TRUE only when both `GuardClosed` and `StartCommand` are TRUE. Otherwise, `MotorOutput` shall be FALSE.

### LDText

```text
FUNCTION_BLOCK MOTOR_CONTROL
    VAR_INPUT
        GuardClosed : BOOL;
        StartCommand : BOOL;
    END_VAR
    VAR_OUTPUT
        MotorOutput : BOOL;
    END_VAR

    BLOCKS
        GuardContact : CONTACT;
        AND1 : AND;
        Motor : COIL;
    END_BLOCKS

    LADDER ladder001
        CONNECTIONS
            Power_Positive -> GuardContact.EN;
            GuardClosed TO GuardContact.VAR;

            GuardContact.ENO -> AND1.IN1;
            StartCommand TO AND1.IN2;

            AND1.OUT -> Motor.EN;
            Motor.VAR TO MotorOutput;
            Motor.ENO -> Power_Negative;
        END_CONNECTIONS
    END_LADDER
END_FUNCTION_BLOCK
```

## Example 2

### Requirement

The BOOL output `HighTemperatureAlarm` shall become TRUE when the REAL input `CurrentTemperature` is greater than the REAL input `HighTemperatureLimit`. Otherwise, `HighTemperatureAlarm` shall be FALSE.

### LDText

```text
FUNCTION_BLOCK TEMPERATURE_ALARM
    VAR_INPUT
        CurrentTemperature : REAL;
        HighTemperatureLimit : REAL;
    END_VAR
    VAR_OUTPUT
        HighTemperatureAlarm : BOOL;
    END_VAR

    BLOCKS
        GT1 : GT;
        Alarm : COIL;
    END_BLOCKS

    LADDER ladder001
        CONNECTIONS
            Power_Positive -> GT1.EN;
            CurrentTemperature TO GT1.IN1;
            HighTemperatureLimit TO GT1.IN2;

            GT1.OUT -> Alarm.EN;
            Alarm.VAR TO HighTemperatureAlarm;
            Alarm.ENO -> Power_Negative;
        END_CONNECTIONS
    END_LADDER
END_FUNCTION_BLOCK
```

## Example 3

### Requirement

When the BOOL input `StartCommand` remains TRUE continuously, start an on-delay timer using the TIME input `DelayTime`. After `DelayTime` has elapsed, set the BOOL output `MotorOutput` to TRUE. If `StartCommand` becomes FALSE, reset the timer and set `MotorOutput` to FALSE.

### LDText

```text
FUNCTION_BLOCK DELAYED_MOTOR_START
    VAR_INPUT
        StartCommand : BOOL;
        DelayTime : TIME;
    END_VAR
    VAR_OUTPUT
        MotorOutput : BOOL;
    END_VAR

    BLOCKS
        StartContact : CONTACT;
        TON1 : TON;
        Motor : COIL;
    END_BLOCKS

    LADDER ladder001
        CONNECTIONS
            Power_Positive -> StartContact.EN;
            StartCommand TO StartContact.VAR;

            StartContact.ENO -> TON1.IN;
            DelayTime TO TON1.PT;

            TON1.Q -> Motor.EN;
            Motor.VAR TO MotorOutput;
            Motor.ENO -> Power_Negative;
        END_CONNECTIONS
    END_LADDER
END_FUNCTION_BLOCK
```

## Example 4

### Requirement

Count rising edges of the BOOL input `ProductDetected`. When the count reaches 10, set the BOOL output `BatchComplete` to TRUE. When the BOOL input `Reset` is TRUE, reset the counter.

### LDText

```text
FUNCTION_BLOCK BATCH_COUNTER
    VAR_INPUT
        ProductDetected : BOOL;
        Reset : BOOL;
    END_VAR
    VAR_OUTPUT
        BatchComplete : BOOL;
    END_VAR

    BLOCKS
        ProductContact : CONTACT;
        CTU1 : CTU;
        Complete : COIL;
    END_BLOCKS

    LADDER ladder001
        CONNECTIONS
            Power_Positive -> ProductContact.EN;
            ProductDetected TO ProductContact.VAR;

            ProductContact.ENO -> CTU1.CU;
            Reset TO CTU1.RESET;
            10 TO CTU1.PV;

            CTU1.Q -> Complete.EN;
            Complete.VAR TO BatchComplete;
            Complete.ENO -> Power_Negative;
        END_CONNECTIONS
    END_LADDER
END_FUNCTION_BLOCK
```
