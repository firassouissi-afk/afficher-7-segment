# 7-Segment Display Driver (Afficheur 7 Segments)

This project implements a **7-segment display decoder** using combinational logic.  
The Boolean equations were derived from truth tables and minimized using Karnaugh maps (K-maps).  
The design is ready for implementation in digital logic (FPGA, CPLD, or discrete gates).

## Files

- `projet.xlsx` – Contains the original truth tables and minimized equations for each segment (a–g).

## Segment Logic Equations

The outputs correspond to the standard 7-segment display inputs:

| Segment | Boolean Equation |
|---------|------------------|
| **A**    | `A = ac + cd! + abd! + b!c!d + a!c!d` |
| **B**    | `B = a!b! + a!c + a!d! + bcd! + b!cd` |
| **C**    | `C = a!b! + a!c! + bcd + bc!d!` |
| **D**    | `D = ab! + ad! + ac + cd! + a!c!d` |
| **E**    | `E = ab! + cd! + ac + a!bc!d` |
| **F**    | `F = b!cd! + a!b!c + bcd + a!bd + bc!d! + ab!c!` |
| **G**    | `G = b + ac + a!d` |

> **Notation**:  
> - `!` = NOT (logical complement)  
> - `+` = OR  
> - Juxtaposition = AND (e.g., `ac` means `a AND c`)

## Inputs / Outputs

- **Inputs**: `a, b, c, d` (4-bit binary coded decimal or arbitrary 4-bit value)
- **Outputs**: `A, B, C, D, E, F, G` (active-high for segment illumination)

## K-Maps

Karnaugh maps for each segment are included in the Excel file (tabs: `Feuil1` to `Feuil5`).  
They confirm the minimized equations above.

## Usage

1. Implement the equations in your preferred hardware description language (VHDL, Verilog) or logic gates.
2. Connect outputs to a common cathode or common anode 7-segment display (invert outputs if common anode).
3. Apply 4-bit inputs to see the corresponding digit/pattern.

## Example (Verilog snippet)

```verilog
module seven_seg (
    input a, b, c, d,
    output A, B, C, D, E, F, G
);
    assign A = (a & c) | (c & ~d) | (a & b & ~d) | (~b & ~c & d) | (~a & ~c & d);
    assign B = (~a & ~b) | (~a & c) | (~a & ~d) | (b & c & ~d) | (~b & c & d);
    assign C = (~a & ~b) | (~a & ~c) | (b & c & d) | (b & ~c & ~d);
    assign D = (a & ~b) | (a & ~d) | (a & c) | (c & ~d) | (~a & ~c & d);
    assign E = (a & ~b) | (c & ~d) | (a & c) | (~a & b & ~c & d);
    assign F = (~b & c & ~d) | (~a & ~b & c) | (b & c & d) | (~a & b & d) | (b & ~c & ~d) | (a & ~b & ~c);
    assign G = b | (a & c) | (~a & d);
endmodule
