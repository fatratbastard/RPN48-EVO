# RPN48 for the TI-84 Evo — User Manual

RPN48 is a Reverse Polish Notation (RPN) calculator for the TI-84 Evo, written in TI-BASIC and modeled on the HP-48SX. You put numbers on a stack, then apply operations to them. There is no `=` key and no parentheses: the stack holds intermediate results for you.

This manual covers version 0.1 (file `RPN48.8xp2`): arithmetic, trigonometry, geometry, stack manipulation, storage registers, and display modes.

---

## Contents

1. [Installing and running](#1-installing-and-running)
2. [How RPN works](#2-how-rpn-works)
3. [The screen](#3-the-screen)
4. [Entering numbers](#4-entering-numbers)
5. [Keyboard reference](#5-keyboard-reference)
6. [Soft-key menus](#6-soft-key-menus)
7. [Function reference with examples](#7-function-reference-with-examples)
   - [7.1 Arithmetic keys](#71-arithmetic-keys)
   - [7.2 STACK menu](#72-stack-menu)
   - [7.3 MATH menu](#73-math-menu)
   - [7.4 TRIG menu](#74-trig-menu)
   - [7.5 GEOM menu](#75-geom-menu)
   - [7.6 MODE menu](#76-mode-menu)
8. [Storage registers](#8-storage-registers)
9. [UNDO and LAST ARG](#9-undo-and-last-arg)
10. [Error messages](#10-error-messages)
11. [What RPN48 changes on your calculator](#11-what-rpn48-changes-on-your-calculator)
12. [Limits and differences from the HP-48](#12-limits-and-differences-from-the-hp-48)
13. [Troubleshooting](#13-troubleshooting)

---

## 1. Installing and running

1. Open TI Connect Evo in your browser at <https://connectevo.ti.com> and connect the calculator with its USB-C cable.
2. Choose **Send to calculator** and select `RPN48.8xp2`.
3. On the calculator, open the **TI-Basic** folder and run **RPN48**.

To quit, press **2nd** then **MODE** (QUIT), or choose **QUIT** from the MODE menu. Your stack, registers and settings are saved and come back the next time you run the program.

Pressing **ON** also stops the program, with a BREAK error. Your stack is still kept, but QUIT is the clean way out.

---

## 2. How RPN works

On an ordinary calculator you type `3 + 4 =`. In RPN you enter both numbers first and then the operation:

```
3  ENTER  4  +        →  7
```

`ENTER` pushes 3 onto the stack. Typing 4 starts a new number, and `+` pushes it automatically, then adds the top two values and leaves the answer.

Results stay on the stack, so longer calculations need no parentheses. To compute (3 + 4) × 5:

```
3  ENTER  4  +  5  ×    →  35
```

To compute (2 + 3) × (7 − 1):

```
2  ENTER  3  +          →  5
7  ENTER  1  −          →  5, 6   (level 2 = 5, level 1 = 6)
×                       →  30
```

### Stack levels and argument order

The stack is numbered from the bottom up. **Level 1** is the most recent value; level 2 is the one before it, and so on.

When a function takes two arguments, the one entered **first** is on level 2 and the one entered **second** is on level 1. For subtraction, division and powers, think of it as "level 2 *operation* level 1":

```
10  ENTER  4  −         →  6      (10 − 4)
2   ENTER  10 ^         →  1024   (2^10)
```

Functions that take three or four arguments follow the same rule: enter them in the order they are listed in this manual.

---

## 3. The screen

RPN48 uses the 26-column × 10-line home screen:

```
DEG STD          GEOM  1/4      ← status line
7:                              ← stack levels 7 … 1
6:
5:
4:
3:                       5
2:             1.414213562
1:             .3333333333
12.5ᴇ⁻3_                        ← command line (number being typed)
HYPT LEG  HERN LAWC LAWS        ← soft-key labels for Y= … GRAPH
```

| Line | Shows |
|---|---|
| 1 (status) | Angle mode (`DEG`/`RAD`), display mode (`STD`, `FIX n`, `SCI n`, `ENG n`), `2ND` when 2nd is active, `STO?`/`RCL?` when waiting for a register digit, and on the right the current menu and page (e.g. `GEOM 1/4`). Error and information messages replace this line until the next keypress. |
| 2–8 | Stack levels 7 down to 1. Numbers are right-aligned. Only the bottom 7 levels are shown, but the stack can hold many more. |
| 9 | The command line: the number you are typing, with a `_` cursor. |
| 10 | Labels for the five soft keys (Y=, WINDOW, ZOOM, TRACE, GRAPH), left to right. |

Numbers are shown the way the calculator shows them: a leading zero is dropped (`.5`), negatives use the raised minus (`⁻3`), and exponents use `ᴇ` (`1.23ᴇ4`).

---

## 4. Entering numbers

| Key | While typing a number | With no number being typed |
|---|---|---|
| **0–9** | Adds a digit | Starts a new number |
| **.** | Adds a decimal point (ignored if there is already one, or after EEX) | Starts `0.` |
| **(−)** | Changes the sign of the number, or of the exponent if you have pressed EEX | **NEG**: negates level 1 |
| **,** (comma) | **EEX**: starts the exponent | Starts `1ᴇ` |
| **ENTER** | Pushes the number onto the stack | **DUP**: copies level 1 |
| **DEL** | Backspace | **DROP**: removes level 1 |
| **CLEAR** | Cancels the number being typed | **CLEAR**: empties the stack (UP undoes it) |

Any operation key also pushes the number you are typing first, so you rarely need ENTER before an operation.

> **Type the digits first, then press (−).** `5 (−)` gives −5. Pressing (−) first, with nothing typed, would negate whatever is already on level 1.

**Examples**

| Keys | Command line | Value pushed |
|---|---|---|
| `2 . 5 , 3 (−)` | `2.5ᴇ⁻3` | 0.0025 |
| `, 6 ENTER` | `1ᴇ6` | 1000000 |
| `4 7 (−)` | `⁻47` | −47 |
| `. 5 ENTER` | `0.5` | .5 |
| `1 2 3 DEL` | `12` | (still typing) |

Limits: the command line holds up to 23 characters, and the exponent can have at most 2 digits.

---

## 5. Keyboard reference

### Direct keys

| Key | Action |
|---|---|
| **Y=, WINDOW, ZOOM, TRACE, GRAPH** | Soft keys 1–5: run the command labeled above them on the bottom line |
| **◄ / ►** (left/right arrows) | Previous / next menu page (cycles through all 19 pages) |
| **▼** (down arrow) | SWAP levels 1 and 2 |
| **▲** (up arrow) | UNDO the last operation |
| **MATH** | Jump to the next menu: STACK → MATH → TRIG → GEOM → MODE → STACK |
| **MODE** | Jump to the MODE menu |
| **+ − × ÷** | Add, subtract, multiply, divide |
| **^** | y^x (level 2 raised to level 1) |
| **x²** | Square |
| **n/d** (fraction key) | 1/x |
| **SIN COS TAN** | Sine, cosine, tangent (in the current angle mode) |
| **LOG** | Base-10 logarithm |
| **LN** | Natural logarithm |
| **(** | ROT |
| **)** | OVER |
| **◄►** (toggle key, above ENTER) | SWAP |
| **STO→** | Store level 1 in a register (then press a digit 0–9) |
| **ENTER, DEL, CLEAR, (−), comma, .** | See [Entering numbers](#4-entering-numbers) |

### 2nd functions

Press **2nd**, then the key. `2ND` appears on the status line while 2nd is active.

| 2nd + key | Action |
|---|---|
| **x²** | √x (square root) |
| **^** | x-th root of y |
| **SIN / COS / TAN** | ASIN / ACOS / ATAN |
| **LOG** | 10^x |
| **LN** | e^x |
| **STO→** | RCL: recall a register (then press a digit 0–9) |
| **ENTER** | LAST ARG: restore the arguments of the last command |
| **DEL** | CLEAR the stack |
| **MODE** | QUIT (saves everything) |
| **,** (comma) | EEX (same as without 2nd) |

π and e are not on keys; use **PI** (TRIG menu) and **E** (MATH menu).

---

## 6. Soft-key menus

There are 5 menus with 19 pages in total. Each page has up to five commands, run by the five top-row keys.

| Menu | Page | Y= | WINDOW | ZOOM | TRACE | GRAPH |
|---|---|---|---|---|---|---|
| STACK | 1/4 | DUP | SWAP | DROP | OVER | ROT |
| STACK | 2/4 | PICK | ROLL | RLLD | DPTH | DRPN |
| STACK | 3/4 | DUPN | DUP2 | DRP2 | CLR | LAST |
| STACK | 4/4 | STO | RCL | UNDO | — | — |
| MATH | 1/5 | ABS | NEG | 1/X | SQRT | XRT |
| MATH | 2/5 | IP | FP | FLOR | CEIL | RND |
| MATH | 3/5 | MOD | MIN | MAX | SIGN | N! |
| MATH | 4/5 | PCT | PCH | PCTT | COMB | PERM |
| MATH | 5/5 | 10^X | E^X | CUBE | CBRT | E |
| TRIG | 1/4 | ASIN | ACOS | ATAN | PI | Y^X |
| TRIG | 2/4 | D>R | R>D | >POL | >REC | HYPT |
| TRIG | 3/4 | SINH | COSH | TANH | ASNH | ACSH |
| TRIG | 4/4 | ATNH | >H.M | H.M> | — | — |
| GEOM | 1/4 | HYPT | LEG | HERN | LAWC | LAWS |
| GEOM | 2/4 | CIRA | CIRC | SPHV | SPHA | ARC |
| GEOM | 3/4 | CYLV | CYLA | CONV | CONA | SECT |
| GEOM | 4/4 | DIST | SLOP | MID | ANGL | POLY |
| MODE | 1/2 | DEG | RAD | STD | FIX | SCI |
| MODE | 2/2 | ENG | KEYS | HELP | QUIT | — |

**Example:** to take the hypotenuse of 5 and 12, press `5 ENTER 12`, then **MATH** until the status line shows `GEOM 1/4`, then **Y=** (HYPT). Result: `13`.

---

## 7. Function reference with examples

Notation used below:

- **Stack** shows the arguments in the order you enter them, then `→` and the result. `x` is level 1 and `y` is level 2.
- **Example** shows the keys to press from an empty stack. `[NAME]` means the soft key with that label.
- Examples assume the default **DEG** and **STD** modes unless stated.
- **Results** are exactly what RPN48 displays. The calculator shows negatives with a raised minus (`⁻3`); this manual writes `−3`.

### 7.1 Arithmetic keys

| Function | Key | Stack | Example | Result |
|---|---|---|---|---|
| Add | **+** | y x → y+x | `3 ENTER 4 +` | 7 |
| Subtract | **−** | y x → y−x | `10 ENTER 4 −` | 6 |
| Multiply | **×** | y x → y×x | `6 ENTER 7 ×` | 42 |
| Divide | **÷** | y x → y÷x | `22 ENTER 7 ÷` | 3.142857143 |
| Power | **^** | y x → y^x | `2 ENTER 10 ^` | 1024 |
| Square | **x²** | x → x² | `5 x²` | 25 |
| Square root | **2nd x²** | x → √x | `2 2nd x²` | 1.414213562 |
| Reciprocal | **n/d** | x → 1/x | `4 n/d` | .25 |
| x-th root of y | **2nd ^** | y x → y^(1/x) | `27 ENTER 3 2nd ^` | 3 |
| | | | `8 (−) ENTER 3 2nd ^` | −2 |
| Sine | **SIN** | x → sin x | `30 SIN` | .5 |
| Cosine | **COS** | x → cos x | `60 COS` | .5 |
| Tangent | **TAN** | x → tan x | `45 TAN` | 1 |
| Arcsine | **2nd SIN** | x → asin x | `.5 2nd SIN` | 30 |
| Arccosine | **2nd COS** | x → acos x | `.5 2nd COS` | 60 |
| Arctangent | **2nd TAN** | x → atan x | `1 2nd TAN` | 45 |
| Log base 10 | **LOG** | x → log x | `1000 LOG` | 3 |
| 10 to the x | **2nd LOG** | x → 10^x | `3 2nd LOG` | 1000 |
| Natural log | **LN** | x → ln x | `10 LN` | 2.302585093 |
| e to the x | **2nd LN** | x → e^x | `1 2nd LN` | 2.718281828 |
| Negate | **(−)** (nothing typed) | x → −x | `7 ENTER (−)` | −7 |

Notes:

- Trig functions use the current angle mode. In DEG mode, exact multiples of 90° give exact results (`180 SIN` is 0), and `90 TAN` gives **INFINITE RESULT**.
- A negative number raised to a non-integer power, or an even root of a negative number, gives **BAD ARGUMENT VALUE**.
- `0 ENTER 0 ^` gives 1.

### 7.2 STACK menu

| Command | Stack | What it does | Example | Result (bottom = level 1) |
|---|---|---|---|---|
| **DUP** | x → x x | Copies level 1. ENTER does the same when nothing is being typed. | `5 [DUP]` | 5, 5 |
| **SWAP** | y x → x y | Exchanges levels 1 and 2. Also on ▼ and the ◄► toggle key. | `1 ENTER 2 [SWAP]` | 2, 1 |
| **DROP** | x → | Removes level 1. Also DEL when nothing is being typed. | `1 ENTER 2 [DROP]` | 1 |
| **OVER** | y x → y x y | Copies level 2 onto level 1. Also on the **)** key. | `1 ENTER 2 [OVER]` | 1, 2, 1 |
| **ROT** | z y x → y x z | Moves level 3 to level 1. Also on the **(** key. | `1 ENTER 2 ENTER 3 [ROT]` | 2, 3, 1 |
| **PICK** | … n → … copy | Copies level n (counted after removing n) to level 1. | `10 ENTER 20 ENTER 30 ENTER 3 [PICK]` | 10, 20, 30, 10 |
| **ROLL** | … n → … | Moves level n to level 1; the levels below it shift down. | `10 ENTER 20 ENTER 30 ENTER 40 ENTER 3 [ROLL]` | 10, 30, 40, 20 |
| **RLLD** (roll down) | … n → … | Moves level 1 up to level n; the reverse of ROLL. | `10 ENTER 20 ENTER 30 ENTER 40 ENTER 3 [RLLD]` | 10, 40, 20, 30 |
| **DPTH** (depth) | → n | Pushes the number of items on the stack. | `7 ENTER 8 ENTER 9 [DPTH]` | 7, 8, 9, 3 |
| **DRPN** (drop n) | … n → | Removes n more levels. | `1 ENTER 2 ENTER 3 ENTER 4 ENTER 2 [DRPN]` | 1, 2 |
| **DUPN** (dup n) | … n → … | Copies the bottom n levels. | `1 ENTER 2 ENTER 3 ENTER 2 [DUPN]` | 1, 2, 3, 2, 3 |
| **DUP2** | y x → y x y x | Copies levels 1 and 2. | `1 ENTER 2 [DUP2]` | 1, 2, 1, 2 |
| **DRP2** (drop 2) | y x → | Removes levels 1 and 2. | `1 ENTER 2 ENTER 3 [DRP2]` | 1 |
| **CLR** | … → | Empties the stack. Also CLEAR and 2nd DEL. Shows `CLEARED (UP=UNDO)`. | `1 ENTER 2 [CLR]` | (empty) |
| **LAST** | → args | LAST ARG: puts back the arguments of the previous command. Also 2nd ENTER. | `3 ENTER 4 + [LAST]` | 7, 3, 4 |
| **STO** | x → | Same as the STO→ key; see [Storage registers](#8-storage-registers). | `9 [STO] 2` | (empty; R2 = 9) |
| **RCL** | → value | Same as 2nd STO→. | `[RCL] 2` | 9 |
| **UNDO** | | Restores the stack from before the last command. Also ▲. | `3 ENTER 4 + [UNDO]` | 3, 4 |

For PICK, ROLL, RLLD, DRPN and DUPN, n must be a whole number, and there must be at least n items below it. Otherwise you get **BAD ARGUMENT VALUE** or **TOO FEW ARGUMENTS**.

### 7.3 MATH menu

| Command | Stack | What it does | Example | Result |
|---|---|---|---|---|
| **ABS** | x → \|x\| | Absolute value | `7.5 (−) [ABS]` | 7.5 |
| **NEG** | x → −x | Negate | `5 [NEG]` | −5 |
| **1/X** | x → 1/x | Reciprocal (same as the n/d key) | `8 [1/X]` | .125 |
| **SQRT** | x → √x | Square root (same as 2nd x²) | `9 [SQRT]` | 3 |
| **XRT** | y x → y^(1/x) | x-th root of y (same as 2nd ^) | `16 ENTER 4 [XRT]` | 2 |
| **IP** | x → integer part | Drops the fraction, keeping the sign | `3.7 (−) [IP]` | −3 |
| **FP** | x → fractional part | Keeps the sign | `3.7 (−) [FP]` | −.7 |
| **FLOR** (floor) | x → ⌊x⌋ | Largest integer ≤ x | `3.7 (−) [FLOR]` | −4 |
| **CEIL** (ceiling) | x → ⌈x⌉ | Smallest integer ≥ x | `3.7 (−) [CEIL]` | −3 |
| **RND** | x → rounded x | Rounds the value to the current FIX/SCI/ENG digits (no change in STD) | In FIX 2: `2 ENTER 3 ÷ [RND]` | 0.67 (stored as .67, not .666…) |
| **MOD** | y x → y mod x | Remainder, with the sign of x (HP style). x = 0 returns y. | `17 ENTER 5 [MOD]` | 2 |
| | | | `7 (−) ENTER 3 [MOD]` | 2 |
| **MIN** | y x → smaller | | `3 ENTER 8 [MIN]` | 3 |
| **MAX** | y x → larger | | `3 ENTER 8 [MAX]` | 8 |
| **SIGN** | x → −1, 0 or 1 | | `4 (−) [SIGN]` | −1 |
| **N!** | n → n! | Factorial of a whole number from 0 to 69 | `6 [N!]` | 720 |
| **PCT** | y x → y·x/100 | x percent of y | `80 ENTER 15 [PCT]` | 12 |
| **PCH** | y x → 100(x−y)/y | Percent change from y to x | `50 ENTER 65 [PCH]` | 30 |
| **PCTT** | y x → 100x/y | x as a percent of the total y | `200 ENTER 50 [PCTT]` | 25 |
| **COMB** | n r → nCr | Combinations | `10 ENTER 3 [COMB]` | 120 |
| **PERM** | n r → nPr | Permutations | `10 ENTER 3 [PERM]` | 720 |
| **10^X** | x → 10^x | Same as 2nd LOG | `3 [10^X]` | 1000 |
| **E^X** | x → e^x | Same as 2nd LN | `2 [E^X]` | 7.389056099 |
| **CUBE** | x → x³ | | `3 [CUBE]` | 27 |
| **CBRT** | x → ∛x | Works for negative numbers | `27 (−) [CBRT]` | −3 |
| **E** | → e | Pushes Euler's number | `[E]` | 2.718281828 |

### 7.4 TRIG menu

All angle inputs and outputs use the current angle mode (DEG or RAD), except D>R and R>D, which always convert.

| Command | Stack | What it does | Example | Result |
|---|---|---|---|---|
| **ASIN** | x → asin x | Arcsine (same as 2nd SIN) | `.5 [ASIN]` | 30 |
| **ACOS** | x → acos x | Arccosine | `.5 [ACOS]` | 60 |
| **ATAN** | x → atan x | Arctangent | `1 [ATAN]` | 45 |
| **PI** | → π | Pushes π | `[PI]` | 3.141592654 |
| **Y^X** | y x → y^x | Power (same as ^) | `2 ENTER 10 [Y^X]` | 1024 |
| **D>R** | deg → rad | Degrees to radians | `180 [D>R]` | 3.141592654 |
| **R>D** | rad → deg | Radians to degrees | `[PI] [R>D]` | 180 |
| **>POL** | x y → r θ | Rectangular to polar. r goes to level 2, θ to level 1. | `3 ENTER 4 [>POL]` | 5, 53.13010235 |
| **>REC** | r θ → x y | Polar to rectangular. x goes to level 2, y to level 1. | `2 ENTER 30 [>REC]` | 1.732050808, 1 |
| **HYPT** | a b → √(a²+b²) | Hypotenuse (also in GEOM) | `5 ENTER 12 [HYPT]` | 13 |
| **SINH** | x → sinh x | Hyperbolic sine | `1 [SINH]` | 1.175201194 |
| **COSH** | x → cosh x | Hyperbolic cosine | `1 [COSH]` | 1.543080635 |
| **TANH** | x → tanh x | Hyperbolic tangent | `1 [TANH]` | .761594156 |
| **ASNH** | x → asinh x | Inverse hyperbolic sine | `1 [ASNH]` | .881373587 |
| **ACSH** | x → acosh x | Inverse hyperbolic cosine (x ≥ 1) | `2 [ACSH]` | 1.316957897 |
| **ATNH** | x → atanh x | Inverse hyperbolic tangent (−1 < x < 1) | `.5 [ATNH]` | .5493061443 |
| **>H.M** | decimal → H.MMSS | Decimal hours (or degrees) to hours.minutesseconds | `2.5125 [>H.M]` | 2.3045 (2 h 30 m 45 s) |
| **H.M>** | H.MMSS → decimal | The reverse | `2.3045 [H.M>]` | 2.5125 |

**RAD mode examples:** `1 COS` gives .5403023059; `[PI] 6 ÷ SIN` gives .5.

### 7.5 GEOM menu

Angles use the current angle mode. Enter arguments in the order shown.

#### Triangles

| Command | Stack | What it computes | Example | Result |
|---|---|---|---|---|
| **HYPT** | a b → c | Hypotenuse √(a² + b²) | `5 ENTER 12 [HYPT]` | 13 |
| **LEG** | c a → b | Missing leg √(c² − a²), from hypotenuse c and leg a | `13 ENTER 5 [LEG]` | 12 |
| **HERN** | a b c → area | Area from three sides (Heron's formula) | `7 ENTER 8 ENTER 9 [HERN]` | 26.83281573 |
| **LAWC** | a b γ → c | Law of cosines: side c from sides a, b and the angle γ between them | `5 ENTER 7 ENTER 60 [LAWC]` | 6.244997998 |
| **LAWS** | a α β → b | Law of sines: side b (opposite angle β), from side a and its opposite angle α | `10 ENTER 30 ENTER 45 [LAWS]` | 14.14213562 |

HERN gives **BAD ARGUMENT VALUE** if the sides can't form a triangle (for example 1, 1, 5).

#### Circles, arcs and spheres

| Command | Stack | What it computes | Example | Result |
|---|---|---|---|---|
| **CIRA** | r → area | Circle area πr² | `3 [CIRA]` | 28.27433388 |
| **CIRC** | r → length | Circumference 2πr | `3 [CIRC]` | 18.84955592 |
| **SPHV** | r → volume | Sphere volume 4/3·πr³ | `3 [SPHV]` | 113.0973355 |
| **SPHA** | r → area | Sphere surface area 4πr² | `3 [SPHA]` | 113.0973355 |
| **ARC** | r θ → length | Arc length (θ in the current angle mode) | `10 ENTER 45 [ARC]` | 7.853981634 |
| **SECT** | r θ → area | Sector area | `10 ENTER 45 [SECT]` | 39.26990817 |

(SPHV and SPHA are equal for r = 3; that's a coincidence of the example, not a bug.)

#### Cylinders and cones

| Command | Stack | What it computes | Example | Result |
|---|---|---|---|---|
| **CYLV** | r h → volume | Cylinder volume πr²h | `3 ENTER 5 [CYLV]` | 141.3716694 |
| **CYLA** | r h → area | Cylinder total surface 2πr(r + h) | `3 ENTER 5 [CYLA]` | 150.7964474 |
| **CONV** | r h → volume | Cone volume πr²h/3 | `3 ENTER 4 [CONV]` | 37.69911184 |
| **CONA** | r h → area | Cone total surface πr(r + slant height) | `3 ENTER 4 [CONA]` | 75.39822369 |

#### Coordinates and polygons

| Command | Stack | What it computes | Example | Result |
|---|---|---|---|---|
| **DIST** | x1 y1 x2 y2 → d | Distance between two points | `1 ENTER 2 ENTER 4 ENTER 6 [DIST]` | 5 |
| **SLOP** | x1 y1 x2 y2 → m | Slope of the line through two points | `1 ENTER 2 ENTER 4 ENTER 8 [SLOP]` | 2 |
| **MID** | x1 y1 x2 y2 → mx my | Midpoint. x goes to level 2, y to level 1. | `1 ENTER 2 ENTER 5 ENTER 8 [MID]` | 3, 5 |
| **ANGL** | x1 y1 x2 y2 → θ | Direction of the line from point 1 to point 2 | `0 ENTER 0 ENTER 1 ENTER 3 [SQRT] [ANGL]` | 60 |
| **POLY** | n s → area | Area of a regular n-sided polygon with side s | `6 ENTER 2 [POLY]` | 10.39230485 |

SLOP of a vertical line gives **INFINITE RESULT**. POLY needs a whole number n ≥ 3.

### 7.6 MODE menu

| Command | Stack | What it does | Example | Display |
|---|---|---|---|---|
| **DEG** | | Angles in degrees (default) | `[DEG] 30 SIN` | .5 |
| **RAD** | | Angles in radians | `[RAD] 1 SIN` | .8414709848 |
| **STD** | | Standard display, up to 10 digits (default) | `2 ENTER 3 ÷ [STD]` | .6666666667 |
| **FIX** | n → | Fixed decimals, n = 0–9 | `2 ENTER 3 ÷ 4 [FIX]` | 0.6667 |
| **SCI** | n → | Scientific notation with n decimals | `12345 ENTER 2 [SCI]` | 1.23ᴇ4 |
| **ENG** | n → | Engineering notation (exponent a multiple of 3) with n decimals | `12345 ENTER 2 [ENG]` | 12.35ᴇ3 |
| **KEYS** | | Key-code viewer: shows the code of each key you press. Press CLEAR twice to exit. | `[KEYS] 1` | shows 92 |
| **HELP** | | Two help screens; press any key to move on. | `[HELP]` | |
| **QUIT** | | Saves and exits (same as 2nd MODE) | `[QUIT]` | |

FIX, SCI and ENG take the number of digits from level 1, HP-style. For example, `4 [FIX]` means "FIX 4". They only change how numbers are **displayed**; the full value is kept unless you use RND.

The status line shows the current settings, for example `DEG FIX 4` or `RAD ENG 2`.

---

## 8. Storage registers

There are 10 registers, R0–R9, which keep their values between runs.

| Action | Keys | Effect |
|---|---|---|
| Store | **STO→**, then a digit | Moves level 1 into that register (it is removed from the stack). The status line shows `STORED IN Rn`. |
| Recall | **2nd STO→**, then a digit | Pushes a copy of the register onto the stack. |
| Cancel | Any non-digit key after STO→ | Cancels; CLEAR cancels without doing anything else. |

While waiting for the digit, the status line shows `STO?` or `RCL?`.

**Example: store a value, then reuse it**

```
4  STO→ 1              →  (empty)     "STORED IN R1"
2nd STO→ 1             →  4
3  2nd STO→ 1          →  4, 3, 4
```

**Example: area of a circle with r = 2.5, saved for later**

```
2.5  [CIRA]  STO→ 7    →  R7 = 19.63495408
```

---

## 9. UNDO and LAST ARG

**UNDO** (▲, or UNDO in STACK 4/4) restores the stack to how it was before the last command. Pressing it again redoes the command. It also undoes CLEAR, DROP, STO and number entry.

```
3 ENTER 4 +            →  7
▲                      →  3, 4
▲                      →  7
```

**LAST ARG** (2nd ENTER, or LAST in STACK 3/4) pushes back the values the last command used. This is handy for reusing inputs:

```
3 ENTER 4 +            →  7
2nd ENTER              →  7, 3, 4
```

If no command has run yet, LAST shows **NO LAST ARGUMENTS**.

---

## 10. Error messages

Errors appear on the status line and leave the stack unchanged. Fix the input and try again. The next keypress clears the message.

| Message | Cause | Example |
|---|---|---|
| `TOO FEW ARGUMENTS` | Not enough values on the stack | `+` on an empty stack |
| `BAD ARGUMENT VALUE` | Input outside the function's domain | `4 (−) 2nd x²` (√ of a negative), `2 2nd SIN`, `1 ENTER 1 ENTER 5 [HERN]`, `12 [FIX]` |
| `INFINITE RESULT` | Division by zero or an infinite result | `1 ENTER 0 ÷`, `0 LOG`, `90 TAN` in DEG, vertical SLOP |
| `OVERFLOW` | Result would be 1ᴇ100 or larger | `1 , 99 ENTER 1 , 99 ×` (1ᴇ99 × 1ᴇ99) |
| `NO LAST ARGUMENTS` | LAST used before any command | |
| `KEY n UNUSED` | The key has no function in RPN48 | Pressing PRGM shows its key code |

Two messages are informational, not errors: `CLEARED (UP=UNDO)` after clearing the stack, and `STORED IN Rn` after STO.

---

## 11. What RPN48 changes on your calculator

RPN48 is a TI-BASIC program, so it shares variables and settings with the rest of the calculator:

- **Variables A–Z, θ and Str1–Str6** are used as working storage and will be overwritten. Don't keep anything you need in them.
- **Angle and number-format modes**: RPN48 sets the calculator's own Degree/Radian and Float/Fix/Sci/Eng modes to match your RPN48 settings, and leaves them that way after you quit. It also sets Real mode.
- **Saved lists**: RPN48 keeps its data in these lists. Deleting them resets RPN48 to an empty stack and default settings.

| List | Holds |
|---|---|
| ʟRPNS | The stack |
| ʟRPNU | The UNDO copy of the stack |
| ʟRPNL | LAST ARG values |
| ʟRPNR | Registers R0–R9 |
| ʟRPNM | Settings: angle mode, display mode, digits, menu page |

While running, it also creates ʟRPNK, ʟRPNJ, ʟRPNA, ʟRPNP and ʟRPNO (key map and menu tables). These are deleted when you QUIT; if you exit with ON, you can delete them yourself.

---

## 12. Limits and differences from the HP-48

| Topic | RPN48 |
|---|---|
| Speed | TI-BASIC is slow. Expect a short pause after each command while the screen redraws. Typing digits only redraws the command line, so entry stays quick. |
| Numbers | Real numbers only, up to about ±9.99ᴇ99, with 14 digits of internal precision. No complex numbers, units, vectors, matrices or symbolic algebra yet. |
| Stack display | Shows 7 levels; the stack itself can be much deeper. |
| Angle modes | DEG and RAD only (the TI-84 has no GRAD mode). |
| Display digits | FIX/SCI/ENG accept 0–9 digits (the HP allows 11). In ENG mode, n is the number of decimals in the mantissa, which differs slightly from the HP. |
| Registers | Numbered R0–R9 instead of named variables. STO removes the value from the stack, as on the HP-48. |
| Menus | Text labels of up to 4 characters, without the HP's inverse-video boxes or the `■` mark on active modes; the status line shows active modes instead. |
| Programmability | No user programs, the interactive stack (▲ browser) or EVAL. |

---

## 13. Troubleshooting

**The screen layout wraps or looks shifted.** RPN48 assumes a 26-column × 10-line home screen. If the Evo's home screen is a different size, the layout needs adjusting.

**An error screen appears as soon as the stack has a number on it.** RPN48 displays numbers with `toString(`. If the Evo doesn't support that command, this is where it would fail.

**A key does the wrong thing, or shows `KEY n UNUSED`.** Use **MODE → KEYS** to see the code that key sends. The key assignments are near the top of the program, as lines like `85→ʟRPNK(85)` (the first number is the command, the second the key code; `ʟRPNJ` holds the 2nd-key assignments). Report the codes and the mapping can be corrected.

**I left with ON and now the program behaves strangely.** Run RPN48 again; it rebuilds its tables on start. If problems persist, delete the lists ʟRPNS, ʟRPNU, ʟRPNL, ʟRPNR and ʟRPNM to reset it completely (this erases the saved stack and registers).
