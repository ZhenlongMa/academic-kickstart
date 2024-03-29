---
title: Function Call in Fork-Join in SystemVerilog
summary: The issue of parameter passing in function call.

date: ""
publishDate: "2021-04-16T14:33:00+08:00"
lastmod: ""

view: 2

comments: true
profile: true
share: false

featured: false
headless: false
draft: false
private: false

tags:
- technical issue
---
It should be noticed when call a function in fork statement in systemverilog. Take below as an example:
```verilog
module test1;
    initial begin
        for (int j = 0; j < 3; j++)
            begin
                int k;
                k = j;
                fork
                    $write(k);
                join_none
            end
            #0
            $display("end\n");
        end
endmodule
```
If consider this piece of code as usual, the output should be `0, 1, 2`. However, the actual output is `2, 2, 2`. That is because static variable k is allocated memory space **only once**. The child process in fork...join_none executes **not until the execution of parent process finishes**, so all child processes get k=2 when their executions begin.

A solution for this problem is to use `automatic` variable: `automatic int k = j`. Creation of `automatic` type variables **come front of any procedure statements**, and executes **concurrently with parent process**.