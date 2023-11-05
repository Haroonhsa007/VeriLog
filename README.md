# VeriLog #
# 1-bit full adder.
## Gate-Level Modeling

```
module full_adder_gate_level(input A, input B, input Cin, output Sum, output Cout);
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
  initial $monitor($time, "%b, %b", Sum, Cout)
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
  initial $monitor($time, "%b, %b", Sum, Cout)
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
  initial $monitor($time, "%b, %b", Sum, Cout)
  initial #100 $stop;
endmodule
```
