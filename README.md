# VeriLog #
# 1-bit full adder.
## Gate-Level Modeling

```verilog
module full_adder_gate_level (input A, input B, input Cin, output Sum, output Cout);
    wire w1, w2, w3, w4, w5;
    xor gate1 (w1, A, B);
    xor gate2 (Sum, w1, Cin);
    and gate3 (w2, A, B);
    and gate4 (w3, B, Cin);
    and gate5 (w4, A, Cin);
    or  gate6 (Cout, w2, w3, w4);
endmodule
```

### Gate-Level Testbench

```verilog
module testbench_gate_level();
    reg A, B, Cin;
    wire Sum, Cout;

    // Instantiate the full adder
    full_adder_gate_level uut (A, B, Cin, Sum, Cout);

    // Apply test vectors
    initial begin
        A = 0; B = 0; Cin = 0; #10;
        A = 0; B = 0; Cin = 1; #10;
        A = 0; B = 1; Cin = 0; #10;
        A = 0; B = 1; Cin = 1; #10;
        A = 1; B = 0; Cin = 0; #10;
        A = 1; B = 0; Cin = 1; #10;
        A = 1; B = 1; Cin = 0; #10;
        A = 1; B = 1; Cin = 1; #10;
    end
  initial $monitor($time, "%b, %b", Sum, Cout);
  initial #100 $stop;
endmodule
```

## Dataflow Modeling

```verilog
module full_adder_dataflow(input A, input B, input Cin, output Sum, output Cout);
    assign Sum = A ^ B ^ Cin;
    assign Cout = (A & B) | (Cin & (A ^ B));
endmodule
```

### Dataflow Testbench

```verilog
module testbench_dataflow();
    reg A, B, Cin;
    wire Sum, Cout;

    // Instantiate the full adder
    full_adder_dataflow uut (A, B, Cin, Sum, Cout);

    // Apply test vectors
    initial begin
        A = 0; B = 0; Cin = 0; #10;
        A = 0; B = 0; Cin = 1; #10;
        A = 0; B = 1; Cin = 0; #10;
        A = 0; B = 1; Cin = 1; #10;
        A = 1; B = 0; Cin = 0; #10;
        A = 1; B = 0; Cin = 1; #10;
        A = 1; B = 1; Cin = 0; #10;
        A = 1; B = 1; Cin = 1; #10;
    end
  initial $monitor($time, "%b, %b", Sum, Cout);
  initial #100 $stop;
endmodule
```

## Behavioral Modeling

```verilog
module full_adder_behavioral(input A, input B, input Cin, output Sum, output Cout);
    always @(*)
      begin
          Sum = A ^ B ^ Cin;
          Cout = (A & B) | (Cin & (A ^ B));
      end
endmodule
```

### Behavioral Testbench

```verilog
module testbench_behavioral();
    reg A, B, Cin;
    wire Sum, Cout;

    // Instantiate the full adder
    full_adder_behavioral uut (A, B, Cin, Sum, Cout);

    // Apply test vectors
    initial begin
        A = 0; B = 0; Cin = 0; #10;
        A = 0; B = 0; Cin = 1; #10;
        A = 0; B = 1; Cin = 0; #10;
        A = 0; B = 1; Cin = 1; #10;
        A = 1; B = 0; Cin = 0; #10;
        A = 1; B = 0; Cin = 1; #10;
        A = 1; B = 1; Cin = 0; #10;
        A = 1; B = 1; Cin = 1; #10;
    end
  initial $monitor($time, "%b, %b", Sum, Cout);
  initial #100 $stop;
endmodule
```

# 4-bit adder implemented using 1-bit full adder modules:
## Gate-Level Modeling
```verilog
module four_bit_adder_gate_level(
    input [3:0] A,
    input [3:0] B,
    input Cin,
    output [3:0] Sum,
    output Cout
);
    wire [3:0] w1;
    wire w2, w3, w4, w5;

    // Instantiate 1-bit full adders
    full_adder_gate_level fa0 (A[0], B[0], Cin, Sum[0], w2);
    full_adder_gate_level fa1 (A[1], B[1], w2, Sum[1], w3);
    full_adder_gate_level fa2 (A[2], B[2], w3, Sum[2], w4);
    full_adder_gate_level fa3 (A[3], B[3], w4, Sum[3], w5);

    // Calculate final carry out
    or gate1 (Cout, w5, w5, w5, w5);
endmodule
```

### Gate-Level Testbench

```verilog
module testbench_four_bit_adder_gate_level();
    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

    // Instantiate the 4-bit adder
    four_bit_adder_gate_level uut (A, B, Cin, Sum, Cout);

    // Apply test vectors
    initial begin
        A = 4'b0000; B = 4'b0000; Cin = 0; #10;
        A = 4'b0001; B = 4'b0001; Cin = 0; #10;
        A = 4'b0010; B = 4'b0010; Cin = 1; #10;
        A = 4'b0100; B = 4'b0100; Cin = 0; #10;
        A = 4'b1000; B = 4'b1000; Cin = 1; #10;
        A = 4'b1111; B = 4'b1111; Cin = 1; #10;
    end
  initial $monitor($time, "%b, %b", Sum, Cout);
  initial #100 $stop;
endmodule
```

## Dataflow Modeling

```verilog
module four_bit_adder_dataflow(
    input [3:0] A,
    input [3:0] B,
    input Cin,
    output [3:0] Sum,
    output Cout
);
    wire w1;

    // Instantiate 1-bit full adders
    full_adder_dataflow fa0 (A[0], B[0], Cin, Sum[0], w1);
    full_adder_dataflow fa1 (A[1], B[1], w1, Sum[1], w1);
    full_adder_dataflow fa2 (A[2], B[2], w1, Sum[2], w1);
    full_adder_dataflow fa3 (A[3], B[3], w1, Sum[3], Cout);
endmodule
```

### Dataflow Testbench

```verilog
module testbench_four_bit_adder_dataflow();
    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

    // Instantiate the 4-bit adder
    four_bit_adder_dataflow uut (A, B, Cin, Sum, Cout);

    // Apply test vectors
    initial begin
        A = 4'b0000; B = 4'b0000; Cin = 0; #10;
        A = 4'b0001; B = 4'b0001; Cin = 0; #10;
        A = 4'b0010; B = 4'b0010; Cin = 1; #10;
        A = 4'b0100; B = 4'b0100; Cin = 0; #10;
        A = 4'b1000; B = 4'b1000; Cin = 1; #10;
        A = 4'b1111; B = 4'b1111; Cin = 1; #10;
    end
  initial $monitor($time, "%b, %b", Sum, Cout);
  initial #100 $stop;
endmodule
```

## Behavioral Modeling

```verilog
module four_bit_adder_behavioral(
    input [3:0] A,
    input [3:0] B,
    input Cin,
    output [3:0] Sum,
    output Cout
);
    reg [3:0] w1;

    // Instantiate 1-bit full adders
    always @* begin
        full_adder_behavioral fa0 (A[0], B[0], Cin, Sum[0], w1[0]);
        full_adder_behavioral fa1 (A[1], B[1], w1[0], Sum[1], w1[1]);
        full_adder_behavioral fa2 (A[2], B[2], w1[1], Sum[2], w1[2]);
        full_adder_behavioral fa3 (A[3], B[3], w1[2], Sum[3], Cout);
    end
endmodule
```

### Behavioral Testbench

```verilog
module testbench_four_bit_adder_behavioral();
    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

    // Instantiate the 4-bit adder
    four_bit_adder_behavioral uut (A, B, Cin, Sum, Cout);

    // Apply test vectors
    initial begin
        A = 4'b0000; B = 4'b0000; Cin = 0; #10;
        A = 4'b0001; B = 4'b0001; Cin = 0; #10;
        A = 4'b0010; B = 4'b0010; Cin = 1; #10;
        A = 4'b0100; B = 4'b0100; Cin = 0; #10;
        A = 4'b1000; B = 4'b1000; Cin = 1; #10;
        A = 4'b1111; B = 4'b1111; Cin = 1; #10;
    end
  initial $monitor($time, "%b, %b", Sum, Cout);
  initial #100 $stop;
endmodule
```

# 4-to-1 Multiplexer (MUX) Implementation in Verilog

## Gate-Level Modeling

```verilog
module mux_gate_level(input [3:0] D, input [1:0] S, output Y);
    wire w1, w2, w3, w4;

    and gate1 (w1, D[0], ~S[0]);
    and gate2 (w2, D[1], S[0]);
    and gate3 (w3, D[2], ~S[1]);
    and gate4 (w4, D[3], S[1]);
    or  gate5 (Y, w1, w2, w3, w4);
endmodule
```

### Gate-Level Testbench

```verilog
module testbench_mux_gate_level();
    reg [3:0] D;
    reg [1:0] S;
    wire Y;

    // Instantiate the MUX
    mux_gate_level uut (D, S, Y);

    // Apply test vectors
    initial begin
        D = 4'b0000; S = 2'b00; #10; // Select input 0
        D = 4'b1111; S = 2'b00; #10; // Select input 0
        D = 4'b0000; S = 2'b01; #10; // Select input 1
        D = 4'b1111; S = 2'b01; #10; // Select input 1
        D = 4'b0000; S = 2'b10; #10; // Select input 2
        D = 4'b1111; S = 2'b10; #10; // Select input 2
        D = 4'b0000; S = 2'b11; #10; // Select input 3
        D = 4'b1111; S = 2'b11; #10; // Select input 3
    end
    initial #100 $stop;
endmodule
```

## Dataflow Modeling

```verilog
module mux_dataflow(input [3:0] D, input [1:0] S, output Y);
    assign Y = (S[1] & S[0]) ? D[3] : (S[1] & ~S[0]) ? D[2] : (~S[1] & S[0]) ? D[1] : D[0];
    // if ? (__) : else (__)
    // Cond_exp ? true_exp : false_exp
endmodule
```

### Dataflow Testbench

```verilog
module testbench_mux_dataflow();
    reg [3:0] D;
    reg [1:0] S;
    wire Y;

    // Instantiate the MUX
    mux_dataflow uut (D, S, Y);

    // Apply test vectors
    initial begin
        D = 4'b0000; S = 2'b00; #10; // Select input 0
        D = 4'b1111; S = 2'b00; #10; // Select input 0
        D = 4'b0000; S = 2'b01; #10; // Select input 1
        D = 4'b1111; S = 2'b01; #10; // Select input 1
        D = 4'b0000; S = 2'b10; #10; // Select input 2
        D = 4'b1111; S = 2'b10; #10; // Select input 2
        D = 4'b0000; S = 2'b11; #10; // Select input 3
        D = 4'b1111; S = 2'b11; #10; // Select input 3
    end
    initial #100 $stop;
endmodule
```

## Behavioral Modeling

```verilog
module mux_behavioral(input [3:0] D, input [1:0] S, output Y);
    always @ (*) begin
        case(S)
            2'b00: Y = D[0];
            2'b01: Y = D[1];
            2'b10: Y = D[2];
            2'b11: Y = D[3];
            default: Y = 4'bxxxx; // Handle invalid cases
        endcase
    end
endmodule
```

### Behavioral Testbench

```verilog
module testbench_mux_behavioral();
    reg [3:0] D;
    reg [1:0] S;
    wire Y;

    // Instantiate the MUX
    mux_behavioral uut (D, S, Y);

    // Apply test vectors
    initial begin
        D = 4'b0000; S = 2'b00; #10; // Select input 0
        D = 4'b1111; S = 2'b00; #10; // Select input 0
        D = 4'b0000; S = 2'b01; #10; // Select input 1
        D = 4'b1111; S = 2'b01; #10; // Select input 1
        D = 4'b0000; S = 2'b10; #10; // Select input 2
        D = 4'b1111; S = 2'b10; #10; // Select input 2
        D = 4'b0000; S = 2'b11; #10; // Select input 3
        D = 4'b1111; S = 2'b11; #10; // Select input 3
    end
    initial #100 $stop;
endmodule
```

Certainly! Here's the Markdown (.md) file containing Verilog code for a 2-to-4 decoder implemented using gate-level, dataflow, and behavioral modeling, along with their respective testbenches.

```markdown
# 2-to-4 Decoder Implementation in Verilog

## Gate-Level Modeling

```verilog
module decoder_2to4_gate_level(input [1:0] A, output [3:0] Y);
    wire w0, w1, w2, w3;

    and gate1 (w0, ~A[0], ~A[1]);
    and gate2 (w1, ~A[0], A[1]);
    and gate3 (w2, A[0], ~A[1]);
    and gate4 (w3, A[0], A[1]);

    assign Y = {w3, w2, w1, w0};
endmodule
```

### Gate-Level Testbench

```verilog
module testbench_gate_level_decoder();
    reg [1:0] A;
    wire [3:0] Y;

    // Instantiate the decoder
    decoder_2to4_gate_level uut (A, Y);

    // Apply test vectors
    initial begin
        $dumpfile("gate_level_decoder.vcd");
        $dumpvars(0, testbench_gate_level_decoder);

        A = 2'b00; #10;
        A = 2'b01; #10;
        A = 2'b10; #10;
        A = 2'b11; #10;

        $finish;
    end
endmodule
```

## Dataflow Modeling

```verilog
module decoder_2to4_dataflow(input [1:0] A, output [3:0] Y);
    assign Y[0] = ~A[0] & ~A[1];
    assign Y[1] = ~A[0] & A[1];
    assign Y[2] = A[0] & ~A[1];
    assign Y[3] = A[0] & A[1];
endmodule
```

### Dataflow Testbench

```verilog
module testbench_dataflow_decoder();
    reg [1:0] A;
    wire [3:0] Y;

    // Instantiate the decoder
    decoder_2to4_dataflow uut (A, Y);

    // Apply test vectors
    initial begin
        $dumpfile("dataflow_decoder.vcd");
        $dumpvars(0, testbench_dataflow_decoder);

        A = 2'b00; #10;
        A = 2'b01; #10;
        A = 2'b10; #10;
        A = 2'b11; #10;

        $finish;
    end
endmodule
```

## Behavioral Modeling

```verilog
module decoder_2to4_behavioral(input [1:0] A, output [3:0] Y);
    always @* begin
        Y[0] = ~A[0] & ~A[1];
        Y[1] = ~A[0] & A[1];
        Y[2] = A[0] & ~A[1];
        Y[3] = A[0] & A[1];
    end
endmodule
```

### Behavioral Testbench

```verilog
module testbench_behavioral_decoder();
    reg [1:0] A;
    wire [3:0] Y;

    // Instantiate the decoder
    decoder_2to4_behavioral uut (A, Y);

    // Apply test vectors
    initial begin
        $dumpfile("behavioral_decoder.vcd");
        $dumpvars(0, testbench_behavioral_decoder);

        A = 2'b00; #10;
        A = 2'b01; #10;
        A = 2'b10; #10;
        A = 2'b11; #10;

        $finish;
    end
endmodule
```

Certainly! Below is the Verilog code for a parity generator implemented using dataflow modeling:

```verilog
module parity_generator_dataflow(input [7:0] data, output P);
    assign P = ^data;
endmodule
```

In this code, we define a module named `parity_generator_dataflow` with an 8-bit input `data` and a single-bit output `P`. The `^` operator performs a bitwise XOR operation on all the bits in `data`, generating the parity bit `P`.

Here's a simple testbench to verify the functionality of the parity generator:

```verilog
module testbench_parity_generator();
    reg [7:0] data;
    wire P;

    // Instantiate the parity generator
    parity_generator_dataflow uut (data, P);

    // Apply test vectors
    initial begin
        $dumpfile("parity_generator.vcd");
        $dumpvars(0, testbench_parity_generator);

        data = 8'b00000000; #10; // Even parity, P = 0
        data = 8'b10101010; #10; // Even parity, P = 0
        data = 8'b01010101; #10; // Even parity, P = 0
        data = 8'b11111111; #10; // Even parity, P = 0

        data = 8'b00000001; #10; // Odd parity, P = 1
        data = 8'b10101011; #10; // Odd parity, P = 1
        data = 8'b01010100; #10; // Odd parity, P = 1
        data = 8'b11111110; #10; // Odd parity, P = 1

        $finish;
    end
endmodule
```

In this testbench, we apply various test vectors to the `data` input and check the output `P` to verify if the parity generator is functioning correctly. The expected outputs are commented next to each test vector.

Certainly! Here are the implementations of an 8-to-1 MUX and a 3-to-8 Decoder using dataflow modeling along with their respective testbenches:

## 8-to-1 MUX in Dataflow Modeling

```verilog
module mux_8to1_dataflow(input [7:0] D, input [2:0] S, output Y);
    assign Y = (S == 3'b000) ? D[0] :
              (S == 3'b001) ? D[1] :
              (S == 3'b010) ? D[2] :
              (S == 3'b011) ? D[3] :
              (S == 3'b100) ? D[4] :
              (S == 3'b101) ? D[5] :
              (S == 3'b110) ? D[6] :
                              D[7];
endmodule
```

### 8-to-1 MUX Testbench

```verilog
module testbench_mux_8to1_dataflow();
    reg [7:0] D;
    reg [2:0] S;
    wire Y;

    // Instantiate the 8-to-1 MUX
    mux_8to1_dataflow uut (D, S, Y);

    // Apply test vectors
    initial begin
        $dumpfile("mux_8to1_dataflow.vcd");
        $dumpvars(0, testbench_mux_8to1_dataflow);

        D = 8'b00000000; S = 3'b000; #10; // Selecting input 0, Y = 0
        D = 8'b11111111; S = 3'b001; #10; // Selecting input 1, Y = 1
        D = 8'b10101010; S = 3'b010; #10; // Selecting input 2, Y = 0
        D = 8'b11001100; S = 3'b011; #10; // Selecting input 3, Y = 0
        D = 8'b01010101; S = 3'b100; #10; // Selecting input 4, Y = 0
        D = 8'b00110011; S = 3'b101; #10; // Selecting input 5, Y = 1
        D = 8'b00001111; S = 3'b110; #10; // Selecting input 6, Y = 1
        D = 8'b11110000; S = 3'b111; #10; // Selecting input 7, Y = 0

        $finish;
    end
endmodule
```

## 3-to-8 Decoder in Dataflow Modeling

```verilog
module decoder_3to8_dataflow(input [2:0] A, output [7:0] Y);
    assign Y[0] = (A == 3'b000) ? 1'b1 : 1'b0;
    assign Y[1] = (A == 3'b001) ? 1'b1 : 1'b0;
    assign Y[2] = (A == 3'b010) ? 1'b1 : 1'b0;
    assign Y[3] = (A == 3'b011) ? 1'b1 : 1'b0;
    assign Y[4] = (A == 3'b100) ? 1'b1 : 1'b0;
    assign Y[5] = (A == 3'b101) ? 1'b1 : 1'b0;
    assign Y[6] = (A == 3'b110) ? 1'b1 : 1'b0;
    assign Y[7] = (A == 3'b111) ? 1'b1 : 1'b0;
endmodule
```

### 3-to-8 Decoder Testbench

```verilog
module testbench_decoder_3to8_dataflow();
    reg [2:0] A;
    wire [7:0] Y;

    // Instantiate the 3-to-8 Decoder
    decoder_3to8_dataflow uut (A, Y);

    // Apply test vectors
    initial begin
        $dumpfile("decoder_3to8_dataflow.vcd");
        $dumpvars(0, testbench_decoder_3to8_dataflow);

        A = 3'b000; #10; // Y = 00000001
        A = 3'b001; #10; // Y = 00000010
        A = 3'b010; #10; // Y = 00000100
        A = 3'b011; #10; // Y = 00001000
        A = 3'b100; #10; // Y = 00010000
        A = 3'b101; #10; // Y = 00100000
        A = 3'b110; #10; // Y = 01000000
        A = 3'b111; #10; // Y = 10000000

        $finish;
    end
endmodule
```

Sure! Below are implementations of different types of 4-bit counters (Up-counter, Down-counter, Up/Down-counter, and Johnson counter) using behavioral modeling, along with their respective testbenches.

## 4-Bit Up-Counter

```verilog
module up_counter(input clk, input rst, output reg [3:0] count);
    always @(posedge clk or posedge rst) begin
        if (rst)
            count <= 4'b0000;
        else
            count <= count + 1;
    end
endmodule
```

### Up-Counter Testbench

```verilog
module testbench_up_counter();
    reg clk, rst;
    wire [3:0] count;

    // Instantiate the up-counter
    up_counter uut (clk, rst, count);

    // Clock generation
    always #5 clk = ~clk;

    // Apply test vectors
    initial begin
        $dumpfile("up_counter.vcd");
        $dumpvars(0, testbench_up_counter);

        // Reset
        rst = 1;
        #10 rst = 0;

        // Count for 20 clock cycles
        repeat(20) #10;

        $finish;
    end
endmodule
```

## 4-Bit Down-Counter

```verilog
module down_counter(input clk, input rst, output reg [3:0] count);
    always @(posedge clk or posedge rst) begin
        if (rst)
            count <= 4'b1111;
        else
            count <= count - 1;
    end
endmodule
```

### Down-Counter Testbench

```verilog
module testbench_down_counter();
    reg clk, rst;
    wire [3:0] count;

    // Instantiate the down-counter
    down_counter uut (clk, rst, count);

    // Clock generation
    always #5 clk = ~clk;

    // Apply test vectors
    initial begin
        $dumpfile("down_counter.vcd");
        $dumpvars(0, testbench_down_counter);

        // Reset
        rst = 1;
        #10 rst = 0;

        // Count for 20 clock cycles
        repeat(20) #10;

        $finish;
    end
endmodule
```

## 4-Bit Up/Down-Counter

```verilog
module up_down_counter(input clk, input up, input down, input rst, output reg [3:0] count);
    always @(posedge clk or posedge rst) begin
        if (rst)
            count <= 4'b0000;
        else if (up)
            count <= count + 1;
        else if (down)
            count <= count - 1;
    end
endmodule
```

### Up/Down-Counter Testbench

```verilog
module testbench_up_down_counter();
    reg clk, up, down, rst;
    wire [3:0] count;

    // Instantiate the up/down-counter
    up_down_counter uut (clk, up, down, rst, count);

    // Clock generation
    always #5 clk = ~clk;

    // Apply test vectors
    initial begin
        $dumpfile("up_down_counter.vcd");
        $dumpvars(0, testbench_up_down_counter);

        // Reset
        rst = 1;
        #10 rst = 0;

        // Count up for 10 clock cycles
        up = 1;
        repeat(10) #10;
        up = 0;

        // Count down for 10 clock cycles
        down = 1;
        repeat(10) #10;
        down = 0;

        $finish;
    end
endmodule
```

## 4-Bit Johnson Counter

```verilog
module johnson_counter(input clk, input rst, output reg [3:0] count);
    always @(posedge clk or posedge rst) begin
        if (rst)
            count <= 4'b0001;
        else begin
            count <= {count[2:0], count[3]};
        end
    end
endmodule
```

### Johnson Counter Testbench

```verilog
module testbench_johnson_counter();
    reg clk, rst;
    wire [3:0] count;

    // Instantiate the Johnson counter
    johnson_counter uut (clk, rst, count);

    // Clock generation
    always #5 clk = ~clk;

    // Apply test vectors
    initial begin
        $dumpfile("johnson_counter.vcd");
        $dumpvars(0, testbench_johnson_counter);

        // Reset
        rst = 1;
        #10 rst = 0;

        // Count for 16 clock cycles
        repeat(16) #10;

        $finish;
    end
endmodule
```

You can save each module in separate files (e.g., `up_counter.v`, `down_counter.v`, `up_down_counter.v`, `johnson_counter.v`) 

Certainly! I'll provide you with the Verilog code for each of the components you've requested, along with their respective testbenches.

### 1. 4-bit Binary Counter (Behavioral Modeling) with Testbench

```verilog
module binary_counter_4bit_behavioral(input clk, input rst, output reg [3:0] count);
    always @(posedge clk or posedge rst) begin
        if (rst) begin
            count <= 4'b0000;
        end else begin
            count <= count + 1;
        end
    end
endmodule
```

Testbench for 4-bit Binary Counter:

```verilog
module testbench_binary_counter_4bit_behavioral();
    reg clk, rst;
    reg [3:0] count;

    // Instantiate the binary counter
    binary_counter_4bit_behavioral uut (clk, rst, count);

    // Clock generation
    always #5 clk = ~clk;

    // Apply test vectors
    initial begin
        $dumpfile("binary_counter_4bit_behavioral.vcd");
        $dumpvars(0, testbench_binary_counter_4bit_behavioral);

        // Initialize signals
        clk = 0; rst = 0;

        // Reset and check initial value
        rst = 1; #10; rst = 0; #10;
        if (count !== 4'b0000) $fatal("Initial value incorrect");

        // Count up to 15
        rst = 0; #10; // Release reset
        if (count !== 4'b0001) $fatal("Value after 1st clock cycle incorrect");
        #10; if (count !== 4'b0010) $fatal("Value after 2nd clock cycle incorrect");
        #10; if (count !== 4'b0011) $fatal("Value after 3rd clock cycle incorrect");
        #10; if (count !== 4'b0100) $fatal("Value after 4th clock cycle incorrect");
        #10; if (count !== 4'b0101) $fatal("Value after 5th clock cycle incorrect");
        #10; if (count !== 4'b0110) $fatal("Value after 6th clock cycle incorrect");
        #10; if (count !== 4'b0111) $fatal("Value after 7th clock cycle incorrect");
        #10; if (count !== 4'b1000) $fatal("Value after 8th clock cycle incorrect");
        #10; if (count !== 4'b1001) $fatal("Value after 9th clock cycle incorrect");
        #10; if (count !== 4'b1010) $fatal("Value after 10th clock cycle incorrect");
        #10; if (count !== 4'b1011) $fatal("Value after 11th clock cycle incorrect");
        #10; if (count !== 4'b1100) $fatal("Value after 12th clock cycle incorrect");
        #10; if (count !== 4'b1101) $fatal("Value after 13th clock cycle incorrect");
        #10; if (count !== 4'b1110) $fatal("Value after 14th clock cycle incorrect");
        #10; if (count !== 4'b1111) $fatal("Value after 15th clock cycle incorrect");

        // Finish simulation
        $finish;
    end
endmodule
```

### 2. 3 to 8 Decoder (Behavioral Modeling) with Testbench

```verilog
module decoder_3to8_behavioral(input [2:0] A, output [7:0] Y);
    assign Y[0] = (A == 3'b000) ? 1'b1 : 1'b0;
    assign Y[1] = (A == 3'b001) ? 1'b1 : 1'b0;
    assign Y[2] = (A == 3'b010) ? 1'b1 : 1'b0;
    assign Y[3] = (A == 3'b011) ? 1'b1 : 1'b0;
    assign Y[4] = (A == 3'b100) ? 1'b1 : 1'b0;
    assign Y[5] = (A == 3'b101) ? 1'b1 : 1'b0;
    assign Y[6] = (A == 3'b110) ? 1'b1 : 1'b0;
    assign Y[7] = (A == 3'b111) ? 1'b1 : 1'b0;
endmodule
```

Testbench for 3 to 8 Decoder:

```verilog
module testbench_decoder_3to8_behavioral();
    reg [2:0] A;
    wire [7:0] Y;

    // Instantiate the 3 to 8 Decoder
    decoder_3to8_behavioral uut (A, Y);

    // Apply test vectors
    initial begin
        $dumpfile("decoder_3to8_behavioral.vcd");
        $dumpvars(0, testbench_decoder_3to8_behavioral);

        A = 3'b000; #10; // Y = 00000001
        A = 3'b001; #10; // Y = 00000010
        A = 3'b010; #10; // Y = 00000100
        A = 3'b011; #10; // Y = 00001000
        A = 3'b100; #10; // Y = 00010000
        A = 3'b101; #10; // Y = 00100000
        A = 3'b110; #10; // Y = 01000000
        A = 3'b111; #10; // Y = 10000000

        $finish;
    end
endmodule
```

### 3. 4-bit Up Down Counter (Behavioral Modeling) with Testbench

```verilog
module up_down_counter_4bit_behavioral(input clk, input rst, input up, output reg [3:0] count);
    always @(posedge clk or posedge rst) begin
        if (rst) begin
            count <= 4'b0000;
        end else if (up) begin
            count <= count + 1;
        end else begin
            count <= count - 1;
        end
    end
endmodule
```

Testbench for 4-bit Up Down Counter:

```verilog
module testbench_up_down_counter_4bit_behavioral();
    reg clk, rst, up;
    wire [3:0] count;

    // Instantiate the up down counter
    up_down_counter_4bit_behavioral uut (clk, rst, up, count);

    // Clock generation
    always #5 clk = ~clk;

    // Apply test vectors
    initial begin
        $dumpfile("up_down_counter_4bit_behavioral.vcd");
        $dumpvars(0, testbench_up_down_counter_4bit_behavioral);

        // Initialize signals
        clk = 0; rst = 0; up = 0;

        //

 Reset and check initial value
        rst = 1; #10; rst = 0; #10;
        if (count !== 4'b0000) $fatal("Initial value incorrect");

        // Count up to 15
        up = 1; #10; // Set direction to up
        if (count !== 4'b0001) $fatal("Value after 1st clock cycle incorrect");
        #10; if (count !== 4'b0010) $fatal("Value after 2nd clock cycle incorrect");
        #10; if (count !== 4'b0011) $fatal("Value after 3rd clock cycle incorrect");

        // Count down to 0
        up = 0; #10; // Set direction to down
        if (count !== 4'b0010) $fatal("Value after 4th clock cycle incorrect");
        #10; if (count !== 4'b0001) $fatal("Value after 5th clock cycle incorrect");
        #10; if (count !== 4'b0000) $fatal("Value after 6th clock cycle incorrect");

        // Finish simulation
        $finish;
    end
endmodule
```

### 4. 4-bit Ring Counter (Behavioral Modeling) with Testbench

```verilog
module ring_counter_4bit_behavioral(input clk, input rst, output reg [3:0] count);
    always @(posedge clk or posedge rst) begin
        if (rst) begin
            count <= 4'b0001;
        end else begin
            count <= {count[2:0], count[3]};
        end
    end
endmodule
```

Testbench for 4-bit Ring Counter:

```verilog
module testbench_ring_counter_4bit_behavioral();
    reg clk, rst;
    wire [3:0] count;

    // Instantiate the ring counter
    ring_counter_4bit_behavioral uut (clk, rst, count);

    // Clock generation
    always #5 clk = ~clk;

    // Apply test vectors
    initial begin
        $dumpfile("ring_counter_4bit_behavioral.vcd");
        $dumpvars(0, testbench_ring_counter_4bit_behavioral);

        // Initialize signals
        clk = 0; rst = 0;

        // Reset and check initial value
        rst = 1; #10; rst = 0; #10;
        if (count !== 4'b0001) $fatal("Initial value incorrect");

        // Count for a few cycles
        #10; if (count !== 4'b0010) $fatal("Value after 1st clock cycle incorrect");
        #10; if (count !== 4'b0100) $fatal("Value after 2nd clock cycle incorrect");
        #10; if (count !== 4'b1000) $fatal("Value after 3rd clock cycle incorrect");
        #10; if (count !== 4'b0001) $fatal("Value after 4th clock cycle incorrect");
        #10; if (count !== 4'b0010) $fatal("Value after 5th clock cycle incorrect");

        // Finish simulation
        $finish;
    end
endmodule
```

### 5. 8-bit ALU (Behavioral Modeling) with Testbench

```verilog
module alu_8bit_behavioral(input [7:0] A, input [7:0] B, input [2:0] opcode,
                           output reg [7:0] result);
    always @* begin
        case(opcode)
            3'b000: result = A + B; // Add operation
            3'b001: result = A - B; // Subtract operation
            3'b010: result = A & B; // Bit-wise AND operation
            3'b011: result = A | B; // Bit-wise OR operation
            default: result = 8'b00000000; // Default case
        endcase
    end
endmodule
```

Testbench for 8-bit ALU:

```verilog
module testbench_alu_8bit_behavioral();
    reg [7:0] A, B;
    reg [2:0] opcode;
    wire [7:0] result;

    // Instantiate the 8-bit ALU
    alu_8bit_behavioral uut (A, B, opcode, result);

    // Apply test vectors
    initial begin
        $dumpfile("alu_8bit_behavioral.vcd");
        $dumpvars(0, testbench_alu_8bit_behavioral);

        // Test addition
        A = 8'b01010101; B = 8'b10101010; opcode = 3'b000; #10;
        if (result !== 8'b00000000) $fatal("Addition operation incorrect");

        // Test subtraction
        A = 8'b01010101; B = 8'b10101010; opcode = 3'b001; #10;
        if (result !== 8'b11111111) $fatal("Subtraction operation incorrect");

        // Test bit-wise AND
        A = 8'b01010101; B = 8'b10101010; opcode = 3'b010; #10;
        if (result !== 8'b00000000) $fatal("Bit-wise AND operation incorrect");

        // Test bit-wise OR
        A = 8'b01010101; B = 8'b10101010; opcode = 3'b011; #10;
        if (result !== 8'b11111111) $fatal("Bit-wise OR operation incorrect");

        // Finish simulation
        $finish;
    end
endmodule
```

### 6. BCD to Seven Segment Decoder (Behavioral Modeling)

```verilog
module bcd_to_seven_segment_behavioral(input [3:0] BCD, output reg [6:0] seg_display);
    always @* begin
        case(BCD)
            4'b0000: seg_display = 7'b1000000; // 0
            4'b0001: seg_display = 7'b1111001; // 1
            4'b0010: seg_display = 7'b0100100; // 2
            4'b0011: seg_display = 7'b0110000; // 3
            4'b0100: seg_display = 7'b0011001; // 4
            4'b0101: seg_display = 7'b0010010; // 5
            4'b0110: seg_display = 7'b0000010; // 6
            4'b0111: seg_display = 7'b1111000; // 7
            4'b1000: seg_display = 7'b0000000; // 8
            4'b1001: seg_display = 7'b0010000; // 9
            default: seg_display = 7'b1111111; // Default case (All segments off)
        endcase
    end
endmodule
```

