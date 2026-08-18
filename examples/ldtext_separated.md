# LDText Code-generation Few-shot Examples — Separated Connections

These four examples are used for the T-S and NL-S configurations. Each example is represented as a `<Requirement, LDText>` pair. The separated LDText BNF is supplied by the corresponding prompt template and is not repeated inside the examples.

## Example 1

### Requirement
Calculate the sum, difference, product, quotient, and remainder of two INT inputs `A` and `B`. The input `B` is guaranteed to be nonzero.

### LDText
```text
FUNCTION_BLOCK ARITHMETIC
    VAR_INPUT
        A : INT;
        B : INT;
    END_VAR
    VAR_OUTPUT
        Sum : INT;
        Difference : INT;
        Product : INT;
        Quotient : INT;
        Remainder : INT;
    END_VAR

    BLOCKS
        ADD1 : ADD;
        SUB1 : SUB;
        MUL1 : MUL;
        DIV1 : DIV;
        MOD1 : MOD;
    END_BLOCKS

    LADDER ladder001
        FLOWS
            Power_Positive -> ADD1.EN;
            ADD1.ENO -> SUB1.EN;
            SUB1.ENO -> MUL1.EN;
            MUL1.ENO -> DIV1.EN;
            DIV1.ENO -> MOD1.EN;
            MOD1.ENO -> Power_Negative;
        END_FLOWS
        CONNECTIONS
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
        END_CONNECTIONS
    END_LADDER
END_FUNCTION_BLOCK
```

## Example 2

### Requirement
Monitor the REAL input `CurrentTemperature`. When `CurrentTemperature` is greater than 90, set the BOOL outputs `HeaterCutOff` and `TriggerAlarm` to TRUE. Otherwise, both outputs shall be FALSE.

### LDText
```text
FUNCTION_BLOCK TEMPERATURE_PROTECTION
    VAR_INPUT
        CurrentTemperature : REAL;
    END_VAR
    VAR
        OverHeating : BOOL;
    END_VAR
    VAR_OUTPUT
        HeaterCutOff : BOOL;
        TriggerAlarm : BOOL;
    END_VAR

    BLOCKS
        GT1 : GT;
        Contact1 : CONTACT;
        CutOff : COIL;
        Alarm : COIL;
    END_BLOCKS

    LADDER ladder001
        FLOWS
            Power_Positive -> GT1.EN;
            GT1.ENO -> Contact1.EN;
            Contact1.ENO -> CutOff.EN;
            CutOff.ENO -> Alarm.EN;
            Alarm.ENO -> Power_Negative;
        END_FLOWS
        CONNECTIONS
            CurrentTemperature TO GT1.IN1;
            90 TO GT1.IN2;
            GT1.OUT TO OverHeating;
            OverHeating TO Contact1.VAR;
            CutOff.VAR TO HeaterCutOff;
            Alarm.VAR TO TriggerAlarm;
        END_CONNECTIONS
    END_LADDER
END_FUNCTION_BLOCK
```

## Example 3

### Requirement
Count rising edges of the BOOL input `ProductReached`. When the count reaches 20, `START_PACKAGING` shall become TRUE and remain TRUE for 5 seconds. After 5 seconds, `START_PACKAGING` shall become FALSE and `STOP_PACKAGING` shall become TRUE.

### LDText
```text
FUNCTION_BLOCK PACKAGING_CONTROL
    VAR_INPUT
        ProductReached : BOOL;
    END_VAR
    VAR
        Total : DINT;
        Trigger : BOOL;
    END_VAR
    VAR_OUTPUT
        START_PACKAGING : BOOL;
        STOP_PACKAGING : BOOL;
    END_VAR

    BLOCKS
        CTU1 : CTU;
        GE1 : GE;
        TON1 : TON;
        RunEnable : CONTACT;
        NotDone : NEG_CONTACT;
        Start : COIL;
        DoneContact : CONTACT;
        Stop : COIL;
    END_BLOCKS

    LADDER ladder001
        FLOWS
            Power_Positive -> CTU1.EN;
            CTU1.ENO -> GE1.EN;
            GE1.ENO -> TON1.EN;
            TON1.ENO -> Power_Negative;
        END_FLOWS
        CONNECTIONS
            ProductReached TO CTU1.CU;
            20 TO CTU1.PV;
            CTU1.CV TO Total;
            Total TO GE1.IN1;
            20 TO GE1.IN2;
            GE1.OUT TO Trigger;
            Trigger TO TON1.IN;
            T#5s TO TON1.PT;
        END_CONNECTIONS
    END_LADDER

    LADDER ladder002
        FLOWS
            Power_Positive -> RunEnable.EN;
            RunEnable.ENO -> NotDone.EN;
            NotDone.ENO -> Start.EN;
            Start.ENO -> Power_Negative;
        END_FLOWS
        CONNECTIONS
            Trigger TO RunEnable.VAR;
            TON1.Q TO NotDone.VAR;
            Start.VAR TO START_PACKAGING;
        END_CONNECTIONS
    END_LADDER

    LADDER ladder003
        FLOWS
            Power_Positive -> DoneContact.EN;
            DoneContact.ENO -> Stop.EN;
            Stop.ENO -> Power_Negative;
        END_FLOWS
        CONNECTIONS
            TON1.Q TO DoneContact.VAR;
            Stop.VAR TO STOP_PACKAGING;
        END_CONNECTIONS
    END_LADDER
END_FUNCTION_BLOCK
```

## Example 4

### Requirement
Implement a four-state lighting controller driven by rising edges of the BOOL input `SW`: first press, `LAMP0=TRUE` and `LAMP1=FALSE`; second press, `LAMP0=FALSE` and `LAMP1=TRUE`; third press, both outputs TRUE; fourth press, both outputs FALSE. The sequence then repeats.

### LDText
```text
FUNCTION_BLOCK LIGHT_SWITCH
    VAR_INPUT
        SW : BOOL;
    END_VAR
    VAR
        Count : DINT;
        State : DINT;
        S1 : BOOL;
        S2 : BOOL;
        S3 : BOOL;
        LampAOn : BOOL;
        LampBOn : BOOL;
    END_VAR
    VAR_OUTPUT
        LAMP0 : BOOL;
        LAMP1 : BOOL;
    END_VAR

    BLOCKS
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
    END_BLOCKS

    LADDER ladder001
        FLOWS
            Power_Positive -> CTU1.EN;
            CTU1.ENO -> MOD1.EN;
            MOD1.ENO -> EQ1.EN;
            EQ1.ENO -> EQ2.EN;
            EQ2.ENO -> EQ3.EN;
            EQ3.ENO -> OR_A.EN;
            OR_A.ENO -> OR_B.EN;
            OR_B.ENO -> Power_Negative;
        END_FLOWS
        CONNECTIONS
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
        END_CONNECTIONS
    END_LADDER

    LADDER ladder002
        FLOWS
            Power_Positive -> ContactA.EN;
            ContactA.ENO -> LampACoil.EN;
            LampACoil.ENO -> Power_Negative;
        END_FLOWS
        CONNECTIONS
            LampAOn TO ContactA.VAR;
            LampACoil.VAR TO LAMP0;
        END_CONNECTIONS
    END_LADDER

    LADDER ladder003
        FLOWS
            Power_Positive -> ContactB.EN;
            ContactB.ENO -> LampBCoil.EN;
            LampBCoil.ENO -> Power_Negative;
        END_FLOWS
        CONNECTIONS
            LampBOn TO ContactB.VAR;
            LampBCoil.VAR TO LAMP1;
        END_CONNECTIONS
    END_LADDER
END_FUNCTION_BLOCK
```
