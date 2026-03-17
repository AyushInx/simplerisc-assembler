# 🔩 SimpleRISC Assembler

A web-based assembler for the **SimpleRISC 32-bit RISC architecture**, built with Python and Streamlit. Write assembly code in the browser and instantly get the 32-bit machine code output with full field-level breakdown.

---

## 🚀 Features

- **Live Assembly** — Paste or write SimpleRISC assembly and click **Assemble** to get binary machine code instantly
- **Full Field Breakdown** — View each instruction decoded into its fields: `opcode[31:27]`, `I[26]`, `rd[25:22]`, `rs1[21:18]`, `imm/rs2[17:0]`
- **Three Output Tabs** — Full table, field breakdown, and raw text output
- **Summary Metrics** — Instruction count, total bytes, immediate-type ops, register-type ops
- **Downloadable Output** — Export machine code as a `.txt` file
- **Built-in Examples** — 7 ready-to-run example programs (ALU, loops, memory, branches, function calls, shifts, modifiers)
- **ISA Reference** — In-app expandable table of all 27 mnemonics with opcodes and syntax
- **Instruction Format Legend** — Built-in guide for the 32-bit encoding layout
- **Dark themed UI** — Custom styled with Space Mono & Unbounded fonts

---

## 🛠 Installation

### Prerequisites

- Python 3.8+
- pip

### Install dependencies

```bash
pip install streamlit pandas
```

### Run the app

```bash
streamlit run simplerisc_assembler.py
```

Then open your browser at `http://localhost:8501`.

---

## 📐 SimpleRISC ISA Overview

### Instruction Encoding (32-bit)

```
[31:27]  opcode   — 5 bits
[26]     I-bit    — 1 = immediate operand,  0 = register operand
[25:22]  rd       — destination register (4 bits)
[21:18]  rs1      — source register 1    (4 bits)
[17:0]   imm/rs2  — 18-bit immediate  OR  rs2 register (4 bits)
```

### Immediate Modifier Bits `imm[17:16]`

| Bits | Suffix | Meaning |
|------|--------|---------|
| `00` | *(none)* | Signed, sign-extended (default) |
| `01` | `u`  | Unsigned, zero-extended |
| `10` | `h`  | Left shift by 16 |

### Branch Encoding

```
[31:27] opcode | [26:0] 27-bit PC-relative offset
offset = (target_address - current_pc) / 4
```

### Registers

| Name | Alias | Purpose |
|------|-------|---------|
| `r0`–`r13` | — | General purpose |
| `r14` | `sp` | Stack pointer |
| `r15` | `ra` | Return address |

---

## 📋 Supported Instructions (27 Mnemonics)

| Category | Mnemonics |
|----------|-----------|
| **ALU** | `add`, `addu`, `addh`, `sub`, `subu`, `subh`, `mul`, `div`, `mod` |
| **Compare** | `cmp` |
| **Logical** | `and`, `or`, `not` |
| **Move** | `mov`, `movu`, `movh` |
| **Shift** | `lsl`, `lsr`, `asr` |
| **NOP** | `nop` |
| **Memory** | `ld`, `st` |
| **Branch** | `beq`, `bgt`, `b`, `call`, `ret` |

---

## ✍️ Assembly Syntax

### Comments
```asm
// This is a comment
```

### Labels
```asm
loop: add r3, r3, r1   // Label followed by instruction
```

### Immediate Formats
```asm
mov r1, 42        // decimal
mov r1, 0xFF      // hexadecimal
mov r1, 0b1010    // binary
mov r1, #FF       // hex with # prefix
mov r1, -5        // negative
```

### Memory Access
```asm
ld  rd, offset[base_reg]    // load
st  rd, offset[base_reg]    // store
```

---

## 🧪 Example Programs

### Basic ALU
```asm
mov r1, 5
mov r2, 10
add r3, r1, r2   // r3 = 15
sub r4, r2, r1   // r4 = 5
mul r5, r1, r2   // r5 = 50
nop
```

### Loop (sum 1..5)
```asm
mov r1, 1
mov r2, 5
mov r3, 0
loop: add r3, r3, r1
      add r1, r1, 1
      cmp r1, r2
      bgt end
      b loop
end: nop
```

### Function Call
```asm
mov r1, 7
mov r2, 3
call add_func
b end
add_func: add r3, r1, r2
          ret
end: nop
```

### Load/Store
```asm
mov r1, 0xAB
st  r1, 0[r0]    // mem[0] = r1
ld  r2, 0[r0]    // r2 = mem[0]
```

---

## 📁 File Structure

```
simplerisc_assembler.py   # Main Streamlit app (single file)
```

---

## 🔧 How It Works

1. **First pass** — Scans all lines, collects label addresses
2. **Second pass** — Encodes each instruction into a 32-bit binary word
3. **Output** — Displays PC (hex), binary encoding (field-spaced), and the original instruction

---

## 📄 License

MIT License — free to use, modify, and distribute.
