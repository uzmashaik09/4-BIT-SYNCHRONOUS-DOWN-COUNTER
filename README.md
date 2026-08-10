# 4-Bit Synchronous Down Counter using Verilog

## 📌 Project Overview

This project implements a **4-bit synchronous down counter** using Verilog HDL.

The counter decrements its binary value by one on every rising edge of the clock when the enable signal is HIGH.

A 4-bit counter has 16 possible states:

```text
0000 to 1111
```

This counter operates in the reverse direction:

```text
15 → 14 → 13 → ... → 1 → 0 → 15
```

---

## 🎯 Objective

The objective of this project is to design and verify a 4-bit synchronous down counter.

The project demonstrates:

* Sequential logic
* Binary counting
* Clocked state transitions
* Down counting
* Enable control
* Reset operation
* Counter rollover
* Testbench verification

---

## 🛠️ Technologies Used

* Verilog HDL
* VS Code
* Icarus Verilog
* ModelSim / QuestaSim
* GTKWave (optional)
* GitHub

---

## 📂 Project Structure

```text
4-Bit-Down-Counter/
│
├── README.md
├── src/
│   └── down_counter_4bit.v
│
├── testbench/
│   └── tb_down_counter_4bit.v
│
└── simulation/
    └── simulation_results.txt
```

---

## 🔢 Inputs and Outputs

| Signal | Width | Description           |
| ------ | ----: | --------------------- |
| CLK    | 1-bit | Clock signal          |
| RESET  | 1-bit | Asynchronous reset    |
| ENABLE | 1-bit | Enables counting      |
| COUNT  | 4-bit | Current counter value |

---

## ⚙️ Working Principle

The counter changes its value on every rising edge of the clock.

When:

```text
RESET = 1
```

the counter is initialized to:

```text
1111
```

which represents decimal 15.

When:

```text
RESET = 0
ENABLE = 1
```

the counter decrements:

```text
COUNT = COUNT - 1
```

---

## 🔢 Counting Sequence

The complete sequence is:

```text
1111
1110
1101
1100
1011
1010
1001
1000
0111
0110
0101
0100
0011
0010
0001
0000
1111
```

In decimal:

```text
15 → 14 → 13 → 12 → 11 → 10 → 9 → 8
→ 7 → 6 → 5 → 4 → 3 → 2 → 1 → 0 → 15
```

---

## 🔄 Counter Rollover

When the counter reaches:

```text
0000
```

the next decrement produces:

```text
1111
```

This occurs because the counter is only 4 bits wide.

---

## 🔒 Enable Operation

When:

```text
ENABLE = 0
```

the counter holds its current value.

For example:

```text
COUNT = 1010
ENABLE = 0
```

After multiple clock cycles:

```text
COUNT = 1010
```

The counter does not change.

---

## 🔌 Reset Operation

When:

```text
RESET = 1
```

the counter immediately becomes:

```text
1111
```

The reset is asynchronous because the design uses:

```verilog
posedge reset
```

---

## 🧪 Testbench

The testbench verifies:

* Reset operation
* Counting from 15 to 0
* Counter rollover
* Enable functionality
* Hold operation

---

## ▶️ Simulation Using Icarus Verilog

Open the terminal inside the project directory.

### Compile

```bash
iverilog -o down_counter_sim src/down_counter_4bit.v testbench/tb_down_counter_4bit.v
```

### Run

```bash
vvp down_counter_sim
```

---

## 📋 Expected Output

```text
================================================
             4-BIT DOWN COUNTER TEST
================================================
 TIME | RESET | ENABLE | COUNT
-----------------------------------------------
  16  |   0   |   1    | 1110
  26  |   0   |   1    | 1101
  36  |   0   |   1    | 1100
  46  |   0   |   1    | 1011
  56  |   0   |   1    | 1010
  66  |   0   |   1    | 1001
  76  |   0   |   1    | 1000
  86  |   0   |   1    | 0111
  96  |   0   |   1    | 0110
 106  |   0   |   1    | 0101
 116  |   0   |   1    | 0100
 126  |   0   |   1    | 0011
 136  |   0   |   1    | 0010
 146  |   0   |   1    | 0001
 156  |   0   |   1    | 0000
 166  |   0   |   1    | 1111

================================================
```

---

## 📚 Concepts Demonstrated

* Synchronous counter
* Sequential logic
* Binary down counting
* Clock signals
* Reset
* Enable
* State transitions
* Counter rollover
* Non-blocking assignments
* Testbench development
* Functional verification

---

## 🔄 Up Counter vs Down Counter

| Feature     | Up Counter  | Down Counter |
| ----------- | ----------- | ------------ |
| Direction   | Increasing  | Decreasing   |
| Start value | 0000        | 1111         |
| Next state  | COUNT + 1   | COUNT - 1    |
| Rollover    | 1111 → 0000 | 0000 → 1111  |

---

## 🚀 Applications

Down counters are used in:

* Countdown timers
* Digital clocks
* Frequency division
* Event timing
* Control systems
* Digital measurement systems
* Processor systems

---

## 🚀 Future Improvements

This project can be extended to:

* 4-bit up/down counter
* 8-bit down counter
* Modulo-N down counter
* Programmable counter
* Ring counter
* Johnson counter
* FPGA implementation

---

## 👩‍💻 Author

**Harshu**

B.Tech - Electronics and Communication Engineering

---

## 📄 License

This project is created for educational and academic purposes.
```verilog
`timescale 1ns/1ps

module down_counter_4bit (
    input  wire       clk,
    input  wire       reset,
    input  wire       enable,
    output reg  [3:0] count
);

    always @(posedge clk or posedge reset) begin

        if (reset)
            count <= 4'b1111;

        else if (enable)
            count <= count - 4'b0001;

    end

endmodule
```
```verilog
`timescale 1ns/1ps

module tb_down_counter_4bit;

    reg clk;
    reg reset;
    reg enable;

    wire [3:0] count;

    down_counter_4bit DUT (
        .clk(clk),
        .reset(reset),
        .enable(enable),
        .count(count)
    );

    // Clock generation
    always #5 clk = ~clk;

    initial begin

        clk    = 1'b0;
        reset  = 1'b1;
        enable = 1'b0;

        $display("================================================");
        $display("             4-BIT DOWN COUNTER TEST");
        $display("================================================");
        $display(" TIME | RESET | ENABLE | COUNT");
        $display("-----------------------------------------------");

        // Reset
        #10;

        reset = 1'b0;
        enable = 1'b1;

        // Count from 15 down to 0
        repeat (16) begin

            @(posedge clk);
            #1;

            $display(
                " %0t |   %b   |   %b    | %b",
                $time,
                reset,
                enable,
                count
            );

        end

        // Test hold operation
        enable = 1'b0;

        repeat (3) begin

            @(posedge clk);
            #1;

            $display(
                " %0t |   %b   |   %b    | %b  <- HOLD",
                $time,
                reset,
                enable,
                count
            );

        end

        $display("================================================");

        $finish;

    end

endmodule
```
# 4-BIT SYNCHRONOUS DOWN COUNTER SIMULATION RESULTS

RESET = 1
COUNT = 1111

After reset release:

COUNT = 1110
COUNT = 1101
COUNT = 1100
COUNT = 1011
COUNT = 1010
COUNT = 1001
COUNT = 1000
COUNT = 0111
COUNT = 0110
COUNT = 0101
COUNT = 0100
COUNT = 0011
COUNT = 0010
COUNT = 0001
COUNT = 0000
COUNT = 1111

ENABLE = 0

COUNT = 1111  <- HOLD
COUNT = 1111  <- HOLD
COUNT = 1111  <- HOLD

================================================
Simulation completed successfully.
==================================
