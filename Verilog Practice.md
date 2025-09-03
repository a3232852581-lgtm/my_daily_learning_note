参考网址   https://hdlbits.01xz.net/wiki/Main_Page

主要用于之后做一些verilog练习后把错误点记录在此处
![导入图片](images/.png)
2025/9/3  
开始今日练习
Q1 We want to assign 1 to the output one.  
`module top_module( output one );`  
`	assign one = 1'b1;`  
`endmodule`  

注意：1'b1代表了2进制形式的1，最好不要省略'b1

assign 用于声明一个“连续赋值”  
它表示等号（=）右侧表达式的结果会持续地、主动地驱动左侧的线网（wire）。只要右侧表达式的值发生变化，左侧的值就会立即更新。
你可以把它想象成用一根导线直接将右侧的信号源连接到左侧的信号线上。
在 assign 语句中，这个等号表示“驱动”或“连接”，而不是软件编程中的“赋值”操作
我们来解析 1'b0：

1： 表示这个数字的位宽（Bit-width） 是 1 位。也就是说，这个常量只有 1 个二进制位。

b： 表示进制（Base），这里是二进制（Binary）。其他常见的进制有：

d 或省略：十进制（Decimal）

h：十六进制（Hexadecimal）

o：八进制（Octal）

0： 表示在这个指定位宽和进制下的数值。这里就是在二进制下的 0。

所以，1'b0 的含义就是：一个 1 位宽的二进制数，其值为 0。

**Wire**  
Verilog里的线是定向的，将右边的值驱动到左边
是连续的，不是一次性的  

**Four Wire**   
注意，当你使用=代表wire时，最前面要有assign不然会报错  

**Notgate**  
非操作 可以使用 按位取反~  和 逻辑取反!  
用“ - ”不行  
逻辑取反是 将真值翻转：非零 变 0，零 变 1，返回一个单比特的布尔值：1‘b0 或 1’b1  

**Andgate**  
与   bitwise-AND (&) and logical-AND (&&)    

**Norgate** 
bitwise-OR (|) and logical-OR (||) operators
做或非的时候要注意加括号      assign out = ~(a|b);    
**XNorgate**   
The bitwise-XOR operator is ^. There is no logical-XOR operator.  
    assign out = a^~b  

**Declaring wires**   
声明一个wire 用于连接


再写一个多个门的时候，您可以选择使用一个赋值语句来驱动每条输出线，或者您可以选择声明（多）条线作为中间信号，其中每条内部线由一个与门的输出驱动。  

**Vector** 
向量 例：wire [99:0] my_vector; 

wire [2:0] a, c;   // Two vectors
assign a = 3'b101;  // a = 101
assign b = a;       // b =   1  implicitly-created wire
assign c = b;       // c = 001  <-- bug
my_module i1 (d,e); // d and e are implicitly one-bit wide if not declared.
                    // This could be a bug if the port was intended to be a vector.

b被隐性赋值，导致 b只是1位的1，c只有c[0]被赋值了

`default_nettype none
添加这行后，任何未声明的标识符都会导致编译错误，从而迫使你立刻发现并修复问题。

reg [7:0] mem [255:0]; 使用这样的方法后，若要调用其中一个值，要经过以下步骤

wire [7:0] data_out; // 一个8位的线网用来接收数据
assign data_out = mem[5]; // 读取地址5的内容
