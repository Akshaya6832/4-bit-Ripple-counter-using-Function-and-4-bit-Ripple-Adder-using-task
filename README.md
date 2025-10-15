# 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task

# Aim
To design and simulate a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required
Vivado 2023.1
# Procedure
1. Launch Vivado 2023.1
Open Vivado and create a new project.
2. Design the Verilog Code
Write the Verilog code for the seven-segment display, defining the logic that maps a 4-bit binary input to the corresponding segments (a to g) of the display.
3. Create the Testbench
Write a testbench to simulate the seven-segment display behavior. The testbench should apply various 4-bit input values and monitor the corresponding output.
4. Create the Verilog Files
Create both the design module and the testbench in the Vivado project.
5. Run Simulation
Run the behavioral simulation to verify the output. 
6. Observe the Waveforms
Analyze the output waveforms in the simulation window, and verify that the correct segments light up for each digit.
7. Save and Document Results
Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.

# Verilog Code
# 4 bit Ripple Adder using Task
```
module RCA4B (
    input [3:0] A, B,    
    input Cin,          
    output reg [3:0] Sum,
    output reg Cout     
);
    reg c;               
    integer i;           

    task full_adder;
        input a, b, cin;
        output s, cout;
        begin
            s = a ^ b ^ cin;           
            cout = (a & b) | (b & cin) | (a & cin); 
        end
    endtask

    always @(*) begin
        c = Cin; 
        for (i = 0; i < 4; i = i + 1) begin
            full_adder(A[i], B[i], c, Sum[i], c);
        end
        Cout = c; 
    end
endmodule
```

# Test Bench
```
`timescale 1ns/1ps
module ripple_adder_task_tb;
    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

   
    RCA4B uut (A, B, Cin, Sum, Cout);

    initial begin
        
        $display("Time=%0t | A=%b B=%b Cin=%b | Sum=%b Cout=%b", $time, A, B, Cin, Sum, Cout);
        
        // Test cases
        A = 4'b0000; B = 4'b0000; Cin = 0; #10;
        A = 4'b0101; B = 4'b0011; Cin = 0; #10;
        A = 4'b1111; B = 4'b0001; Cin = 0; #10;
        A = 4'b1010; B = 4'b0101; Cin = 1; #10;
        A = 4'b1111; B = 4'b1111; Cin = 1; #10;

        $finish;
    end
endmodule
```
# Output Waveform

<img width="1919" height="1079" alt="Screenshot 2025-10-15 232626" src="https://github.com/user-attachments/assets/9f53720e-f504-4ea2-9ffb-b336a8ae7a17" />


# 4 bit Ripple counter using Function
```
module RCA4B (
    input clk, 
    input rst, 
    output reg [3:0] Q
);
    function [3:0] count_up;
        input [3:0] data;
        begin
            count_up = data + 1;
        end
    endfunction

    always @(posedge clk or posedge rst)
    begin
        if (rst)
            Q <= 4'b0000;         
        else
            Q <= count_up(Q);   
    end
endmodule
```
# Test Bench
```
`timescale 1ns / 1ps
module ripple_counter_func_tb;
    reg clk, rst;
    wire [3:0] Q;
    RCA4B uut (
        .clk(clk),
        .rst(rst),
        .Q(Q)
    );
    always #5 clk = ~clk;

    initial begin
        clk = 0;
        rst = 1;   
        #10;
        rst = 0;  
        #10 $display("Time=%0t | Q=%b (%0d)", $time, Q, Q);
        #10 $display("Time=%0t | Q=%b (%0d)", $time, Q, Q);
        
        rst = 1;
        #10 $display("Time=%0t | Q=%b (%0d)", $time, Q, Q);
        rst = 0;
        #10 $display("Time=%0t | Q=%b (%0d)", $time, Q, Q);
        #10 $display("Time=%0t | Q=%b (%0d)", $time, Q, Q);
        #10 $display("Time=%0t | Q=%b (%0d)", $time, Q, Q);

        $stop;
    end
endmodule
```
# Output Waveform 

<img width="1919" height="1079" alt="Screenshot 2025-10-15 233409" src="https://github.com/user-attachments/assets/ae726d5c-6c92-4713-b3af-992b8b2c4da9" />


# Conclusion
In this experiment, a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task was successfully designed and simulated using Verilog HDL.
