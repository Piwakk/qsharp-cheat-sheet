# Q# cheat sheet

## Variables

```qsharp
let foo = 3.14; // Immutable binding.

mutable bar = 3.14;
set bar = 5.67;
```

### Numbers (`Int`, `Double`)

```qsharp
let power = 3.14^5.67;
```

**Bitwise operators:** `&&&`, `|||`, `~~~`, `^^^`

**Shift operators:**, `<<<`, `>>>`

### Arrays (`Int[]`, `Double[]`)

```qsharp
let arr = [1, 2, 3, 4, 5];

Length(arr); // 5

arr[2...]; // [3, 4, 5]
arr[...2]; // [1, 2, 3]

[1] + [2, 3]; // [1, 2, 3]
```

### Complex numbers (`Complex`)

```qsharp
open Microsoft.Quantum.Math;

let complex = Complex(3.14, 5.67);
let (real, imag) = complex!;
let (real, imag) = (complex::Real, complex::Image);
let abs = AbsComplex(complex);
```

#### Polar form

```qsharp
let polar = ComplexPolar(1.5, PI());
let (mag, arg) = complex!;
let (mag, arg) = (complex::Magnitude, complex::Argument);
let abs = AbsComplexPolar(polar);
```

#### Conversion

```qsharp
let polar = ComplexAsComplexPolar(complex);
let complex = ComplexPolarAsComplex(polar);
```

#### Operations

Replace `C` with `CP` for the polar version.

```qsharp
let sum = PlusC(complexA, complexB);
let product = TimesC(complexA, complexB);
let division = DividedByC(complexNumerator, complexDenominator);
```

### Strings (`String`)

```qsharp
let concatenation = "one " + "string";

Message($"Display {myVar} inside a string.");
```

### Qubits (`Qubit`)

Every qubit is initialized to $\ket{0}$.

```qsharp
use qubit = Qubit();
use qubitRegister = Qubit[4];

// Display the states
open Microsoft.Quantum.Diagnostics;
DumpMachine();
```

#### Operations

```qsharp
X(qubit);
Y(qubit);
Z(qubit);
H(qubit);
S(qubit);
T(qubit);
Rx(PI(), qubit);
CNOT(control, target);

Controlled H([controlA, controlB], target);
```

#### Reset

Every qubit must be reset to $\ket{0}$ at the end of a Q# program.

```qsharp
Reset(qubit);
ResetAll([qubitA, qubitB]);
```

#### Measurement (`Result`)

```
// Measurement along the Z axis
M(qubit);

// Measurement and reset
open Microsoft.Quantum.Measurement;
MResetZ(qubit);
MResetEachZ([qubitA, qubitB])[0];
```

### Custom types

```qsharp
newtype NamedCoordinates = (X: Double, Y: Double);
newtype AnonymousCoordinates = (Double, Double);
```

## Syntax

### Conditions

```qsharp
if x <= 3 and y >= 4 {
    ...
}
elif not (z != 4 or a == 3) {
    ...
}
else {
    ...
}
```

```qsharp
let ternary = x == 1 ? 2 | 3;
```

### Assertions

```qsharp
Fact(x >= 4, "x must be greater than or equal to 4");
```

### Loops

```qsharp
for i in 1..3..10 {
    Message($"{i}"); // 1 4 7 10
}
```

```qsharp
while x != 3 {
    ...
}
```

```qsharp
repeat {
    ...
}
until x == 3
fixup {
    // Optional, executed if x != 3.
};
```

### $U^\dagger V U$

```qsharp
within {
    // U
}
apply {
    // V
}
```

### Operation

```qsharp
operation MyOperation(qubit: Qubit) : Unit {
    X(qubit);
}

qubit => X(qubit); // As a lambda.
```

### Function

A function is purely deterministic, it never calls anything that impacts the
quantum state.

```qsharp
function MyFunction(x: Double) : Double {
    x + 3.14
}

x -> x + 3.14; // As a lambda.
```

### Copy and update

```qsharp
let newArray = [0.0, 1.0, 2.0]
    w/ 0 <- 3.14; // [3.14, 1.0, 2.0]

let newComplex = Complex(1.0, 0.0)
    w/ Real <- 2.0; // Complex(2.0, 0.0)
```

### Alias

```
open Microsoft.Quantum.Math as Math;
let complex = Math.Complex(3.14, 5.67);
```

## `Microsoft.Quantum.Math`

```qsharp
open Microsoft.Quantum.Math;

let squareRoot = Sqrt(4);

let euler = E();
let naturalLog = Log(2.0);
let base10Log = Log10(10.0);

// Trigonometry
PI();
Cos(PI() / 4.0);
Sin(PI() / 2.0);
Tan(PI());
ArcCos(1.0);
ArcSin(0.0);
ArcTan2(y, x);
```
