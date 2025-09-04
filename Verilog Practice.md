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

记住8bit 为一个字节


![导入图片](images/20250903-1.png)
为什么按位取或第一段一直是7？ 因为此时b的值为7，即111 
同理逻辑或也会一直是1
outnot 是b为高a为低 b为高时去反为000 a取反为111（7）110（6） 101（5）。。。
第二段第一个数： b取反111 a取反111  111111=3F 之后同理


**Gates4**
    assign out_and = in[0]&in[1]&in[2]&in[3];  
    assign out_or = in[0]|in[1]|in[2]|in[3];  
    assign out_xor = in[0]^in[1]^in[2]^in[3];  

可以写成单目形式 assign out_and = &in;   缩位运算
把一长串数据挤压（Reduce）成一个单一的比特值  

4'ha 和 4'd10 在数值上是等价的
{4'ha, 4'd10} => 8'b10101010  在使用这种连接方法时，必须确定用于连接的每个向量的长度  

assign out[15:0] = {in[7:0], in[15:8]};     
assign out = {in[7:0], in[15:8]};   
这两行的不同在于 例如如果out是32位 前一行只会对低16位赋值，后一行会不止赋值低16还会将高16赋值0
如果out是8位 前一行直接报错，后一行截断，不报错

assign z = {e[0],f[4:0],2'b11}; z为8位，11必须标出位数，不然报错


module top_module (
    input [4:0] a, b, c, d, e, f,
    output [7:0] w, x, y, z );//
    assign w = {a[4:0],b[4:2]};
    assign x = {b[1:0],c[4:0],d[4]};
    assign y = {d[3:0],e[4:1]};
    assign z = {e[0],f[4:0],2'b11};
    // assign { ... } = { ... };

endmodule

这种写法太麻烦！其实只需要  
assign {w,x,y,z} = { a, b, c, d, e, f, 2'b11};就可以一起处理  
系统会自动分配结果给w，x，y，z  

**Reversing a longer vector**

assign {out[0],out[1],out[2],out[3],out[4],out[5],out[6],out[7]} = in;
可以直接翻转，不用打8行
或者 assign out = {in[0]  ....... 也可以

连接的另一种方法：{num{vector}}

注意：in[7]是属于变量而非常量  
24{in[7]}用于拼接时在外面要再多打一个括号，就像下面
{3'd5, {2{3'd6}}}


**module**  
By position mod_a instance1 ( wa, wb, wc );   直接连接了 wa，wb和wc
By name mod_a instance2 ( .out(wc), .in1(wa), .in2(wb) );  
mod_a 模块名，它从哪里来？ 它是在另一个 Verilog 文件中已经定义好的一个模块  
instance2   这是你为这个具体的实例起的名字，也称为实例名（Instance name）  
.<port_name>(<signal_name>) 语法详解：

.：一个点，表示开始连接一个端口。

<port_name>：mod_a 模块蓝图中定义的端口名称（如 out, in1, in2）。这必须是完全一致的名字。

(<signal_name>)：当前模块中存在的实际信号名称，你要把 mod_a 的端口连接到这些信号上。这些信号可以是 input, output, wire, reg 等。  

wire [7:0]q0,[7:0]q1,[7:0]q2; 这样声明是有问题的，只能wire [7:0] q0, q1, q2;，如果长度不一样必须要分开写

 always @(*) begin
        case (sel)
        2'b00: q = d; // 00时输出d
        2'b01: q = q0; // 01时输出q0
        2'b10: q = q1; // 全零时输出q1
        default: q = q2;     // 其他情况输出q2
    endcase
end

四位选择器，使用了always +  case方法分类  
(*)  代表组合逻辑电路

= 是 阻塞赋值（Blocking Assignment）
一位全加器  
assign {cout,sum} = a+b+cin;
<= 是 非阻塞赋值（Non-Blocking Assignment）  

 黄金法则：
   - 在描述组合逻辑的always块中使用阻塞赋值（=）。
   - 在描述时序逻辑的always块中使用非阻塞赋值（<=）。
例：
always @(posedge clk) begin
    reg1 <= in1;  // 这些赋值同时发生
    reg2 <= reg1; // 使用reg1的旧值，不是上面刚赋的值
    reg3 <= reg2; // 使用reg2的旧值
end

verilog 可以用资源换速度   异或的 verilog符号： `^`

减法器合并  The net result is a circuit that can do two operations: (a + b + 0) and (a + ~b + 1).  用一个信号切换b的正反即可   （补码 = 反码+1）


![导入图片](images/2025090401.png)
在上升沿时always @(posedge clk)begin才会发生变化，组合电路变成1的那个时刻clk不是上升沿，所以比组合电路满了一个上升沿周期

优先编码器  从小的位开始和1’b1比较即可

注：always里使用if里也要有begin end

注意 ： 使用if时需要else 给他赋一个其他情况的固定值防止乱飞

8'bzzzzzzz1: pos = 3'd0; 在casez 里可以表示最后一位为1的所有数
**case/casez/casex 语句的核心逻辑 ——“匹配即跳出**
只要第一个赋值成功了就不会再进行后面的赋值


4个数取最小  
我的写法：
module top_module (
    input [7:0] a, b, c, d,
    output [7:0] min);//
    reg [7:0]ab,cd;
    always @(*) begin
        ab = (a>b)? b:a;
        cd = (c>d)? d:c;
        min = (ab>cd)?cd:ab;
    end
    // assign intermediate_result1 = compare? true: false;

endmodule

up写法  assign 其实差不多

奇校验  偶校验

for循环  
module top_module( 
    input [99:0] in,
    output reg [99:0] out  // 将输出声明为 reg 类型
);
    
    // 在 always 块外部声明循环变量
    integer i;  // 或者使用 reg [6:0] i;（7位可以表示0-127）
    
    always @(*) begin
        for (i = 0; i < 100; i = i + 1) begin
            out[i] = in[99-i];  // 将输入向量反转
        end
    end

endmodule

注意 使用1时尽量用1'b1

**使用计数时，切记一定要初始化！！！**
module top_module( 
    input [254:0] in,
    output reg[7:0] out );
    integer i;
    always @(*) begin
        out = 8'b0;
        for(i=0;i<255;i=i+1) begin
            if(in[i] == 1'b1)begin
                out = out+1'b1;
            end
        end
    end

endmodule

其实直接写out = out+in[i] 就行，因为只有在1的时候才会加1 我这里的if多余了

100位全加器   
我的写法  
module top_module( 
    input [99:0] a, b,
    input cin,
    output [99:0] cout,
    output [99:0] sum );
    integer i;
    reg [100:0]cin1;
    always @(*)begin
        cin1[0] = cin;
        for(i = 0;i<100;i=i+1)begin
            {cin1[i+1],sum[i]} = cin1[i] + a[i] + b[i];
            cout[i] = cin1[i+1];
        end
    end

endmodule
使用generate的写法
module top_module( 
    input [99:0] a, b,
    input cin,
    output [99:0] cout,
    output [99:0] sum );
    generate
        genvar i;
        for(i = 0;i<100;i=i+1)begin:adder
            if(i==0)begin
                fulladder fulladder1(a[i],b[i],cin,cout[i],sum[i]);
            end
            else begin
                fulladder fulladder2(a[i],b[i],cout[i-1],cout[i],sum[i]);
            end
        end
    endgenerate

endmodule

module fulladder(
    input a,b,cin,
    output cout,sum
);
    assign {cout,sum} = a + b + cin;
endmodule  

其实就是定义了一个全加器然后循环了100遍，第一遍的输出比较独立就单独拉了出来

genvar是一个标量，不能 给位宽
