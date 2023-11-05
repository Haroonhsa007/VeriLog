# VeriLog #
# 1-bit full adder.
## Gate-Level Modeling

```
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

```
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

```
module full_adder_behavioral(input A, input B, input Cin, output Sum, output Cout);
    always @(*)
      begin
          Sum = A ^ B ^ Cin;
          Cout = (A & B) | (Cin & (A ^ B));
      end
endmodule
```

### Behavioral Testbench

```
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
```
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

```
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

```
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

```
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
