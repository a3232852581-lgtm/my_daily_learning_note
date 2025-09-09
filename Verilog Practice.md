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

generate 块通常用于需要重复实例化模块或根据参数条件生成不同逻辑的场景。  


module top_module(   
    input [3:0] in,  
    output [2:0] out_both,  
    output [3:1] out_any,  
    output [3:0] out_different );  
    integer i;  
    always @(*)begin  
        for(i=0;i<3;i=i+1)begin  
            out_both[i] = in[i]&in[i+1];  
            out_any[i+1] = in[i+1]|in[i];  
            out_different[i] = in[i]^in[i+1];  
        end  
        out_different[3] = in[3]^in[0];  
    end  
    

endmodule  

可以直接写成      
assign out_both = in[2:0]&in[3:1];  
assign out_any = in[2:0]|in[3:1];  
assign out_different = in^{in[0],in[3:1]};  
**用移位可以代替循环的作用**
对于更长的向量也一样

列真值表，求逻辑表达式是一种写模块的方法  
assign out = (sel&b)|((!sel)&a);  
从数据传输描述（数据流描述）
sel?b:a;
逻辑 if else


module top_module( 
    input [1023:0] in,  
    input [7:0] sel,  
    output [3:0] out );  
    always @* begin  
        out = in[sel*4 + 3 -: 4];    
    end  

endmodule  
注意，out = in[sel*4 + 3 -: 4];代表从sel*4 + 3小于它的4位
写in[sel*4 + 3 : sel*4]是错误的
也可以使用拼接符

全加器  
    assign {cout,sum} = a + b + cin;  

module top_module (  
    input [3:0] x,  
    input [3:0] y,   
    output [4:0] sum);  
    assign sum = x + y;  

endmodule  
这里sum比xy高一位，直接相加即可


module top_module (  
    input [7:0] a,  
    input [7:0] b,  
    output [7:0] s,  
    output overflow  
); //  
     assign s = a + b;  
    assign overflow =  a + b -s;  

endmodule
这种写法是错误的！ a + b会截取8位和进行计算  
因该写assign overflow = (a[7] & b[7]) | (a[7] & ~s[7]) | (b[7] & ~s[7]);  

99位全加器  
module top_module(   
    input [99:0] a, b,  
    input cin,  
    output cout,  
    output [99:0] sum );  
    wire [99:0] cout1;  
    generate  
        genvar i ;  
        for(i = 0;i<100;i=i+1)begin:add100  
            if(i == 0)  
                add add1(a[i],b[i],cin,cout1[i],sum[i]);  
            else  
                add add1(a[i],b[i],cout1[i-1],cout1[i],sum[i]);  
         end
    endgenerate  
    assign cout = cout1[99];  

endmodule  

module add(   
    input a, b, cin,  
    output cout, sum );  
    assign {cout,sum} = a + b + cin;   
endmodule  


其实直接写成  
module top_module(   
    input [99:0] a, b,  
    input cin,  
    output cout,  
    output [99:0] sum );  

    assign {cout,sum} = a+b+cin;  

endmodule 就行  

卡诺图 SOP  直接取1的就行
POS  取0的，取完之后  每个值取反，或与互换


module top_module (
    input c,  
    input d,  
    output [3:0] mux_in  
); 
    assign mux_in[0] = (c?1:(d?1:0));  
    assign mux_in[1] = 0;  
    assign mux_in[2] = ~d;  
    assign mux_in[3] = c&d;  

endmodule  
用这种(c?1:(d?1:0))是可以嵌套的，用来达成else if 的目的




Clocked: always @(posedge clk)使用时钟触发    上升沿
注意在使用时钟时不使用=  改为使用<=  

always @(negedge clk)begin  
        q <= reset? 8'h34 : d;  
    end  
下降沿触发

active high asynchronous reset 高电平有效的异步复位
意思就是只要reset = 1就复位，不用等时钟
always @(posedge clk or posedge areset)begin  
        if(areset) q<= 8'h34;  
        else  q <= d;  
    end  
    这样always模块在clk和reset的上升沿都可以触发  

    resetn is a synchronous, active-low reset. 意思是这是低位触发的复位器

    锁存器 与时钟无关 有圆圈：低电平有效

module top_module (  
	input clk,  
	input L ,  
	input r_in,  
	input q_in,  
	output reg Q);  
    wire d;  
    assign d = L?r_in:q_in;  
    always @(posedge clk)begin  
        Q <= d;  
    end  

endmodule  
子模块触发器，仅供参考
module top_module (
	input clk,
	input L,
	input r_in,
	input q_in,
	output reg Q);
    reg q1,q2,q3;
    top top1(q_in,r_in,L,clk,q1);
    top top2(q1,r_in,L,clk,q2);
    assign q3 = Q^q2;
    top top3(q3,r_in,L,clk,Q);

endmodule

module top (
	input d1,
	input d2,
	input L1,
	input clk,
	output reg Q);
    wire d;
    assign d = L1?d2:d1;
    always @(posedge clk)begin
        Q <= d;
    end

endmodule

DFF and gates
module top_module (
    input clk,
    input x,
    output z
); 
    reg mid1,mid2,mid3;
    
    DFFT DFF1(clk,(x^mid1),mid1);
    DFFT DFF2(clk,(x&~mid2),mid2);
    DFFT DFF3(clk,(x|~mid3),mid3);
    assign z = ~(mid1|mid2|mid3);
    

endmodule

module DFFT (
    input clk,
    input x,
    output q1
); 
    always @(posedge clk)begin
        q1 <= x;
    end

endmodule

jk触发器
J	K	Q
0	0	Qold
0	1	0
1	0	1
1	1	~Qold

怎么找上升沿？ 打拍
怎么延迟一个时钟周期？ 一个一位d触发器  

module top_module (  
    input clk,  
    input [7:0] in,  
    output [7:0] pedge  
);  
    reg [7:0] in_1;  
    always @(posedge clk)begin  
        in_1 <= in;  
        pedge <= in&~in_1; //这个周期上升时是pedge取1 ，这里的in_1是上个周期的  
    end  

endmodule  

module top_module (  
    input clk,  
    input [7:0] in,  
    output [7:0] anyedge  
);  
    reg [7:0] in_1;    
    always @(posedge clk)begin   
            in_1 <= in;  
            anyedge <= in^in_1; //上升和下降沿都给   
    end    

endmodule  


对各个位的捕获  捕获下降，不下降时保持，每个位独立
module top_module (  
    input clk,  
    input reset,  
    input [31:0] in,  
    output [31:0] out  
);  
    reg [31:0] in_1;  
    integer i;  
    always @(posedge clk)begin  
        in_1 <= in;  
        for(i=0;i<32;i=i+1)begin  
            if(reset) out[i]<=32'b0;     
            else out[i] <= (in_1[i]&(~in[i]))?in_1[i]:out[i];  
        end  
    end  
endmodule  

上升下降沿都赋值
module top_module (  
    input clk,  
    input d,  
    output q  
);  
    reg q1,q2;  
    always @(posedge clk)begin  
        q1 <=d;  
    end  
    always @(negedge clk)begin  
        q2 <=d;  
    end  
    assign q= clk?q1:q2;  

endmodule  用q1 | q2不行，q1 | q2 的逻辑本质是 “只要任一边沿采样到 1，输出就为 1”  
例如在两个时钟沿中间  假设在一个上升沿 d =0 ，q原本为1，此时只有上升沿的q1成功跟随变为了0，但是下降沿的q2的值仍为1，取|得到的值为1，运算错误  
总结：在使用这种上升下降沿都触发的情况下，要使用clk?来分类是上升还是下降

计数器  
module top_module (  
    input clk,  
    input reset,      // Synchronous active-high reset  
    output [3:0] q);  
    always @(posedge clk)begin  
        if(reset) q <= 4'b0;  
        else q <= q+4'b1;  
    end  
   
endmodule  


20250908
1-12计数  
module top_module (  
    input clk,  
    input reset,  
    input enable,  
    output [3:0] Q,  
    output c_enable,  
    output c_load,  
    output [3:0] c_d  
);   
    assign c_enable = enable;  
    assign c_load = (Q == 4'd12 && enable)|| reset ? 1'd1 : 1'd0;  
    assign c_d = 4'd1; //初始值  


    count4 the_counter (clk, c_enable, c_load, c_d ,Q);  

endmodule  

注意assign只能赋值一次  
计数器BCD不是最高位为1就进位，而是要到9才进位
用1000计数：  

module top_module (  
    input clk,  
    input reset,  
    output OneHertz,  
    output [2:0] c_enable  
); //  
    wire [11:0]Q;  
    assign c_enable[0] = 1'b1;  
    assign c_enable[1] = (Q[3:0]==4'h9);  
    assign c_enable[2] = (Q[7:0]==8'h99);  
    assign OneHertz = (Q[11:0]==12'h999);  

    bcdcount counter0 (clk, reset, c_enable[0],Q[3:0]);  
    bcdcount counter1 (clk, reset, c_enable[1],Q[7:4]);  
    bcdcount counter2 (clk, reset, c_enable[2],Q[11:8]);  

endmodule  
注意 使用BCD码计数器要用h，用d就错了

module top_module (
    input clk,
    input reset,
    output OneHertz,
    output [2:0] c_enable
); //
    wire [3:0]Q1,Q2,Q3;
    assign c_enable[0] = 1'b1;
    assign c_enable[1] = Q1==4'd9;
    assign c_enable[2] = (Q2==4'd9)&&Q1==4'd9;
    assign OneHertz = (Q3 == 4'd9);

    bcdcount counter0 (clk, reset, c_enable[0],Q1);
    bcdcount counter1 (clk, reset, c_enable[1],Q2);
    bcdcount counter2 (clk, reset, c_enable[2],Q3);
endmodule
错在Q3 == 4'd9只是900不是1000  别忘记：不是最高位有数就进位  
改成OneHertz = (Q3 == 4'd9)&(Q2==4'd9)&(Q1==4'd9);即可  

4位BCD
module top_module (
    input clk,
    input reset,   // Synchronous active-high reset
    output [3:1] ena,
    output [15:0] q);
    
    assign ena[1] = (q[3:0] == 4'H9 );
    assign ena[2] = (q[7:0] == 8'H99 );
    assign ena[3] = (q[11:0] == 12'H999 ); //这里可以直接用一行大括号写，看个人习惯
    
    Count10 cout1(clk,reset,1'b1,q[3:0]);
    Count10 cout2(clk,reset,ena[1],q[7:4]);
    Count10 cout3(clk,reset,ena[2],q[11:8]);
    Count10 cout4(clk,reset,ena[3],q[15:12]);

endmodule

module Count10 (
    input clk,
    input reset,
    input enable,
    output reg [3:0] q); 
    
    always @(posedge clk)begin  
        if(reset) begin                
            q <= 4'b0;
        end
        else if(enable) begin          
            if(q == 4'd9) 
                q <= 4'b0;           
            else 
                q <= q + 4'b1;         
        end
        else begin
            q <= q;    // 为防止报错一定要加                  
        end
    end  

endmodule

时钟    
module top_module(  
    input clk,  
    input reset,  
    input ena,  
    output pm,  
    output [7:0] hh,  
    output [7:0] mm,  
    output [7:0] ss);   
    wire [1:0]ss_ena,mm_ena;  
    wire hh_ena;  
    wire ss_reset,mm_reset;  
    
    assign ss_ena = {(ss[3:0] == 4'H9)&&ena,ena};  
    assign ss_reset = reset||(ss[7:0] == 8'H59 && ena);//ss_ena[1]一定要加，防止振荡  
    assign mm_ena = {(mm[3:0] == 4'H9)&&(ss[7:0] == 8'H59)&&ena,(ss[7:0] ==   8'H59)&&ena};  //分钟高位的进位不止要低位为1还需要秒位为59
    assign mm_reset = reset||((mm[7:0] == 8'H59)&&(ss[7:0] == 8'H59) && ena);  
    assign hh_ena = (mm[7:0] == 8'H59)&&(ss[7:0] == 8'H59)&&ena;  
    
    always @(posedge clk) begin  
        if (reset)begin  
            hh <= 8'H12;  // 复位为12  
            pm <= 1'b0;  
        end  
        else if (hh_ena) begin  
            if (hh == 8'H12)begin  
                hh <= 8'H01;  // 12→1  
            end  
            else if (hh == 8'H11)begin  
                hh <= 8'H12;  // 11→12  
                pm <= ~pm;  
            end  
            else  
                hh <= (hh[3:0] == 4'd9) ? {hh[7:4]+1'b1, 4'd0} : hh + 1'b1;  
        end  
    end  
    
    Count10 ss_cout1(clk,ss_reset,ss_ena[0],ss[3:0]);  
    Count10 ss_cout2(clk,ss_reset,ss_ena[1],ss[7:4]);  
    Count10 mm_cout1(clk,mm_reset,mm_ena[0],mm[3:0]);  
    Count10 mm_cout2(clk,mm_reset,mm_ena[1],mm[7:4]);  

endmodule  

module Count10 (   
    input clk,  
    input reset,  
    input enable,  
    output reg [3:0] q);   
    
    always @(posedge clk)begin    
        if(reset) begin                  
            q <= 4'b0;  
        end  
        else if(enable) begin            
            if(q == 4'd9)   
                q <= 4'b0;             
            else   
                q <= q + 4'b1;           
        end  
        else begin  
            q <= q;    // 为防止报错一定要加                    
        end  
    end    

endmodule   
参考，主要是他写的使能信号，比我写的要好很多，不容易出错   
他的时钟用的是24进制，分成了两半  
![导入图片](images/2025090801.png)  




向右移位器  
module top_module(  
    input clk,  
    input areset,  // async active-high reset to zero  
    input load,  
    input ena,  
    input [3:0] data,  
    output reg [3:0] q);   
       
    always @(posedge clk or posedge areset)begin  
        if(areset)  
            q <= 4'd0;  
        else if(load)  
            q <= data;  
        else if(ena)begin  
            q[3] <= 1'b0;  
            q[2] <= q[3];  
            q[1] <= q[2];  
            q[0] <= q[1];  
        end  
            
    end  
    

endmodule  



旋转寄存器  
module top_module(
    input clk,
    input load,
    input [1:0] ena,
    input [99:0] data,
    output reg [99:0] q); 
    integer i;
    reg q1;
    always @(posedge clk)begin
        if(load) q <= data;
        else if(ena == 2'b01)begin
            q <= {q[0],q[99:1]}; //注意，这样移位会方便很多！记住
        end
        else if(ena == 2'b10)begin
            q <= {q[98:0],q[99]};
        end
        else q <= q;
    end
        

endmodule

如果写成else if(ena == 2'b01)begin  
            q1 <= q[0];  
            for(i=0;i<99;i=i+1)begin
                q[i] <= q[i+1];  
            end  
            q[99] <= q1;  
        end  
会出现一个问题，q[99]实际上接收到的是上个周期的q1  ，如果真的要这么写，只能用组合逻辑  





shift18  

module top_module(
    input clk,
    input load,
    input ena,
    input [1:0] amount,
    input [63:0] data,
    output reg [63:0] q); 
    always @(posedge clk)begin
        if(load)begin
            q<= data;
        end
        else if (ena) begin
            case (amount)
                2'b00: q <= {q[62:0], 1'b0};         // 左移1位
                2'b01: q <= {q[55:0], 8'd0};         // 左移8位
                2'b10: q <= {q[63],q[63:1]};        // 算术右移1位
                2'b11: q <= {{8{q[63]}},q[63:8]};     // 算术右移8位  注意！8{q[63]}外面要再加一层{}不然会报错
            endcase
        end
        else  q <= q;
    end

endmodule  



伽罗瓦线性反馈移位寄存器  
module top_module(  
    input clk,  
    input reset,      // Active-high synchronous reset to 5'h1
    output [4:0] q  
);   
    always @(posedge clk)begin  
        if(reset) q<=5'h1;  
        else begin  
            q[4] <= 1'b0^q[0];  
            q[3] <= q[4];  
            q[2] <= q[3]^q[0];  
            q[1] <= q[2];   
            q[0] <= q[1];  //可以用大括号写在一行内
        end  
    end  
        

endmodule  


3位LFSR    
module top_module (  
	input [2:0] SW,      // R  
	input [1:0] KEY,     // L and clk 
	output [2:0] LEDR);  // Q  
    always @(posedge KEY[0])begin  
        LEDR <= {(KEY[1]?SW[2]:(LEDR[1]^LEDR[2])),(KEY[1]?SW[1]:LEDR[0]),(KEY[1]?  SW[0]:LEDR[2])};  
    end  


endmodule  

32位LFSR  
module top_module(
    input clk,
    input reset,    // Active-high synchronous reset to 32'h1
    output [31:0] q
); 
    integer i;
    
    always @(posedge clk)begin
        if(reset) q <= 32'd1;
        else begin
            for(i=0;i<32;i=i+1)begin
                if(i == 5'd0) q[0] <= q[1]^q[0];
                else if(i == 5'd1) q[1] <= q[2]^q[0];
                else if(i == 5'd21) q[21] <= q[22]^q[0];
                else if(i == 5'd31) q[31] <= 1'b0^q[0];
                else q[i] <= q[i+1]; //如果要用一行的方法，就把1，2，22，32单独用wire表示一下先计算出来，再在大括号里移位
            end
        end           
    end
endmodule


移位器  
module top_module (
    input clk,
    input resetn,   // synchronous reset
    input in,
    output out);
    reg q1,q2,q3;
    always @(posedge clk)begin
        if(!resetn)begin
            out <= 1'b0;
            q1 <= 1'b0;
            q2 <= 1'b0;
            q3 <= 1'b0;  //可以直接写{out,q1,q2,q3} <= 4'b0;
        end
        else begin
            q1 <= in;
            q2 <= q1;
            q3 <= q2;
            out <= q3;//{out,q3,q2,q1} <= {q3,q2,q1,in};
        end
    end

endmodule

n位移位器 
module top_module (  
    input [3:0] SW,  
    input [3:0] KEY,  
    output [3:0] LEDR  
);   
    MUXDFF mux1( KEY[0],LEDR[1],SW[0],KEY[1],KEY[2],LEDR[0]);  
    MUXDFF mux2( KEY[0],LEDR[2],SW[1],KEY[1],KEY[2],LEDR[1]);  
    MUXDFF mux3( KEY[0],LEDR[3],SW[2],KEY[1],KEY[2],LEDR[2]);  
    MUXDFF mux4( KEY[0],KEY[3],SW[3],KEY[1],KEY[2],LEDR[3]);  

endmodule  

module MUXDFF  (    
    input clk,  
    input w, R, E, L,  
    output Q  
);  
    always @(posedge clk)begin  
        Q <= L?R:(E?w:Q);  
    end  

endmodule  


移位+选择  
module top_module (  
    input clk, 
    input enable,  
    input S,  
    input A, B, C,  
    output Z );  
    reg [7:0]M;  
    wire [2:0]select;  
    always @(posedge clk)begin  
        if(enable) M <= {M[6:0],S};  
    end  
    assign select = {A,B,C};  
    assign Z = M[select];  
  
endmodule  



rule 90  
module top_module(  
    input clk,  
    input load,  
    input [511:0] data,  
    output [511:0] q );   
    integer i;  
    always @(posedge clk)begin  
        if(load) q <= data;  
        else begin  
            q[511] <= q[510];  
            q[0] <= q[1];  
            for(i = 1;i<511;i=i+1)begin  
                q[i] <= q[i-1]^q[i+1];  
            end  
        end //可以直接用 q <= {1'b0,q[511:1]}^{q[510:0],1'b0};
    end  
    

endmodule  




rule 110   

module top_module(  
    input clk,  
    input load,  
    input [511:0] data,  
    output [511:0] q  
);   
    always @(posedge clk)begin    
        if(load) q <= data;    
        else begin    
            q <= ~q&(q<<1)|q&~(q<<1)|(q<<1)&~(q>>1);//因为左右移位自动补的数就是0所以可以这么做  
        end  
    end    

endmodule 





Conwaylife  扩展矩阵
 
module top_module(  
    input clk,  
    input load,  
    input [255:0] data,  
    output [255:0] q );   
    wire [323:0]ex;  
    integer i,j,z;  
    reg [3:0]count;  
    always @(*)begin  
        ex[17:0] = {q[240],q[255:240],q[255]};  
        ex[323:306] = {q[0],q[15:0],q[15]};  
        for(i=1;i<17;i=i+1)begin  
            ex[18*i +: 18] = {q[16*(i-1)],q[16*i-16 +:16],q[16*i-1]};                     
        end  
    end  
    always @(posedge clk)begin  
        if(load) q <= data;  
        else begin  
            for(j=0;j<16;j=j+1)begin  
                for(z=0;z<16;z=z+1)begin  
                    count = ex[18*j+z+1-1] + ex[18*j+z+1] + ex[18*j+z+1+1] + ex[18*(j+1)+z+1-1]+ ex[18*(j+1)+z+1+1]  //要放外面算的！会有时序问题
                    + ex[18*(j+2)+z+1-1]+ ex[18*(j+2)+z+1]+ ex[18*(j+2)+z+1+1];  
                    case(count)  
                        4'd2: q[16*j+z] <= q[16*j+z];  
                        4'd3: q[16*j+z] <= 1'b1; 
                        default: q[16*j+z] <= 1'b0;  
                    endcase  
                    
                end  
            end  
        end  
    end  
endmodule  
其实应该把clk里的计算拉出来单独写一个，因为这样写的话ex会用前一时刻的q计算，但是仿真过了。。。。就先不改了先放在这，实际是有问题的  




有限状态机  
 Moore state    
 module top_module(  
    input clk,  
    input areset,    // Asynchronous reset to state B  
    input in,  
    output out);//    

    parameter A=0, B=1;   
    reg state, next_state;  

    always @(*) begin    // This is a combinational always block  
        case(state) // State transition logic  
            A: next_state = in?A:B;  
            B: next_state = in?B:A;  
            default: next_state = B;  
        endcase  
    end  

    always @(posedge clk, posedge areset) begin    // This is a sequential always   block  
        // State flip-flops with asynchronous reset  
        if(areset)  
            state <= B;  
        else  
            state <= next_state;  
    end  
            

    // Output logic  
    assign out = (state == B);  

endmodule  


另一种写法（狗屎，别用）
// Note the Verilog-1995 module declaration syntax here:
module top_module(clk, reset, in, out);
    input clk;
    input reset;    // Synchronous reset to state B
    input in;
    output out;//  
    reg out;

    // Fill in state name declarations
    parameter A=0, B=1; 

    reg present_state, next_state;

    always @(posedge clk) begin
        if (reset) begin  
            present_state <= B;
            out<=1'b1;
        end else begin
            case (present_state)
                A:next_state=in?A:B;
                B:next_state=in?B:A;
                default :next_state=B;
            endcase

            // State flip-flops
            present_state = next_state;   

            case (present_state)
               A: out= 1'b0;
               B: out= 1'b1;// Fill in output logic
               default:out=1'b1;
            endcase
        end
    end

endmodule  在clk里面必须用阻塞赋值，不然会出错
另一种方法是不用next_state，第一个case里直接靠in更新out


JK触发
module top_module(  
    input clk,  
    input areset,    // Asynchronous reset to OFF  
    input j,  
    input k, 
    output out); //    

    parameter OFF=0, ON=1;   
    reg state, next_state;  
 
    always @(*) begin   
        case(state)     
            OFF: next_state = j?ON:OFF;    
            ON: next_state = k?OFF:ON;    
            default: next_state = OFF;    
        endcase    
    end   

    always @(posedge clk, posedge areset) begin  
        if(areset)     
            state <= OFF;    
        else    
            state <= next_state;    
    end  
    assign out = (state == ON);  

endmodule  


独热编码  每个状态单独占一位  
module top_module(  
    input in,  
    input [3:0] state,  
    output [3:0] next_state,  
    output out); //  

    parameter A=0, B=1, C=2, D=3;  

    // State transition logic: Derive an equation for each state flip-flop.  
    assign next_state[A] = ~in&state[A]|~in&state[C];  
    assign next_state[B] = in&state[A]|in&state[D]|in&state[B];  
    assign next_state[C] = ~in&state[B]|~in&state[D];  
    assign next_state[D] = in&state[C];  

    // Output logic:   
    assign out = state[D];  

endmodule  


状态机练习  
module top_module(  
    input clk,  
    input in,  
    input areset,  
    output out); //  
    reg [1:0]state,next_state;  
    parameter A=0,B=1,C=2,D=3;   
    
    always @(*)begin  
        case(state)  
            A: next_state=in?B:A;  
            B: next_state=in?B:C;  
            C: next_state=in?D:A;  
            D: next_state=in?B:C;  
        endcase  
    end  
    
    always @(posedge clk or posedge areset)begin  
        if(areset) state <= A;  
        else state <= next_state;  
    end  
    assign out = (state == D );  

endmodule  


module top_module (  
    input clk,  
    input reset,  
    input [3:1] s,  // s[3]最高，s[1]最低  
    output reg fr3,  
    output reg fr2,  
    output reg fr1,  
    output reg dfr  
);  
    reg [3:0]state,next_state;    
    parameter L1=4'b0001;    
    parameter B21=4'b0010;     
    parameter B32=4'b0100;     
    parameter A3=4'b1000;     
    reg [2:0]fr;  
    assign {fr3,fr2,fr1} = fr;  
    
    always @(posedge clk )begin    
        if(reset) state <= L1;    
        else state <= next_state;    
    end      
    
    always @(*)begin    
        case(s)    
            3'b000: next_state=L1;    
            3'b001: next_state=B21;    
            3'b011: next_state=B32;    
            3'b111: next_state=A3;    
        endcase    
    end    
     
    always @(posedge clk )begin    
        if(reset) fr <= 3'b111;    
        else   
            case(next_state)  
                L1 : fr <= 3'b111;  
                B21 : fr <= 3'b011;  
                B32 : fr <= 3'b001;  
                A3 : fr <= 3'b000;  
                default: fr <= 3'b111;  
            endcase  
    end    
    
     always @(posedge clk )begin    
         if(reset)   
             dfr <= 1'b1;  //其实就这里一开始写错了。。  
         else   
             if(next_state<state)    
                 dfr <= 1'b1;  
             else if(next_state>state)   
                 dfr <= 1'b0;  
             else   
                 dfr <= dfr;   
         
    end    
 
endmodule  






Lemmings1   
module top_module(  
    input clk,  
    input areset,    // Freshly brainwashed Lemmings walk left.  
    input bump_left,  
    input bump_right,  
    output walk_left,  
    output walk_right); //    

    parameter LEFT=0, RIGHT=1;  
    reg state, next_state;  

    always @(*) begin  
        case(state)       
            LEFT: next_state = bump_left?RIGHT:LEFT;      
            RIGHT: next_state = bump_right?LEFT:RIGHT;         
        endcase     
 
    end  

    always @(posedge clk, posedge areset) begin  
        if(areset)       
            state <= LEFT;      
        else      
            state <= next_state;   

    end  

     assign walk_left = (state == LEFT);  
     assign walk_right = (state == RIGHT);  

endmodule  


小鼠游戏2    
module top_module(  
    input clk,  
    input areset,      // Freshly brainwashed Lemmings walk left.  
    input bump_left,  
    input bump_right,  
    input ground,  
    output walk_left,  
    output walk_right,  
    output aaah );   
    
    parameter LEFT=0, RIGHT=1,FALL_L=2,FALL_R=3;  
    reg [1:0]state, next_state;   
    reg ground1;  

    always @(*) begin    
        case(state)         
            LEFT: next_state = ground?(bump_left?RIGHT:LEFT):FALL_L;        
            RIGHT: next_state = ground?(bump_right?LEFT:RIGHT):FALL_R; 
            FALL_L: next_state = ground?LEFT:FALL_L;  
            FALL_R: next_state = ground?RIGHT:FALL_R;  
        endcase       
 
    end    

    always @(posedge clk, posedge areset) begin    
        if(areset)         
            state <= LEFT;        
        else begin      
            state <= next_state;   
            aaah  <= ~ground;  
        end  

    end    

     assign walk_left = (state == LEFT);    
     assign walk_right = (state == RIGHT);  


endmodule  



小鼠游戏3  
module top_module(  
    input clk,  
    input areset,    // Freshly brainwashed Lemmings walk left.  
    input bump_left,  
    input bump_right,  
    input ground,  
    input dig,  
    output walk_left,  
    output walk_right,  
    output aaah,  
    output digging );   
    parameter LEFT=6'b000001,   RIGHT=6'b000010,FALL_L=6'b000100,FALL_R=6'b001000,DIG_L=6'b010000,DIG_R=6'b100000;    
    reg [31:0]state, next_state;     

    always @(*) begin      
        case(state)           
            LEFT: next_state = ground?(dig?DIG_L:(bump_left?RIGHT:LEFT)):FALL_L;          
            RIGHT: next_state = ground?(dig?DIG_R:(bump_right?LEFT:RIGHT)):FALL_R;   
            FALL_L: next_state = ground?LEFT:FALL_L;    
            FALL_R: next_state = ground?RIGHT:FALL_R;    
            DIG_L: next_state = ground?DIG_L:FALL_L;     
            DIG_R: next_state = ground?DIG_L:FALL_R;    
            default: next_state = LEFT;  
        endcase         
 
    end      

    always @(posedge clk, posedge areset) begin      
        if(areset)           
            state <= LEFT;          
        else begin        
            state <= next_state;     
        end     

    end      
    

    assign walk_left = (state == LEFT);      
    assign walk_right = (state == RIGHT);    
    assign aaah = (state == FALL_L|state ==FALL_R);   
    assign digging = (state == DIG_L|state ==DIG_R);   
endmodule  





小鼠游戏4  
module top_module(   
    input clk,  
    input areset,    // Freshly brainwashed Lemmings walk left.  
    input bump_left,  
    input bump_right,  
    input ground,  
    input dig,  
    output walk_left,   
    output walk_right,  
    output aaah,  
    output digging );   
    parameter LEFT=7'b0000001,   RIGHT=7'b0000010,FALL_L=7'b0000100,FALL_R=7'b0001000,DIG_L=7'b0010000,DIG_R=7'b0100000,SPLAT=7'b1000000;    
    reg [31:0]state, next_state;     
    reg [31:0]fall_time;  
    parameter fall_flag=32'd20;  

    
    always @(posedge clk, posedge areset) begin      
        if(areset)  
            fall_time <= 32'd0;  
        else if(state ==FALL_L|state ==FALL_R)  
            fall_time <= fall_time + 32'd1;  

        else   
            fall_time <= 32'd0;  

    end   
    always @(*) begin   
        
        case(state)           
            LEFT: next_state = ground?(dig?DIG_L:(bump_left?RIGHT:LEFT)):FALL_L;          
            RIGHT: next_state = ground?(dig?DIG_R:(bump_right?LEFT:RIGHT)):FALL_R;   
            FALL_L:   
                if(ground)    
                    if(fall_time < fall_flag)   
                        next_state = LEFT;  
                    else  
                        next_state = SPLAT;  
                else  
                    next_state = state;  
            FALL_R:   
                if(ground)  
                    if(fall_time < fall_flag)  
                        next_state = RIGHT;  
                    else  
                        next_state = SPLAT;  
                else  
                    next_state = state;   
            DIG_L: next_state = ground?DIG_L:FALL_L;    
            DIG_R: next_state = ground?DIG_R:FALL_R;   
            SPLAT: next_state = state;  
            default: next_state = LEFT;  
        endcase         
 
    end      

    always @(posedge clk, posedge areset) begin      
        if(areset)  
            state <= LEFT;   
        else   
            state <= next_state;      

    end      
    

    assign walk_left = (state!=SPLAT)&(state == LEFT);     
    assign walk_right = (state!=SPLAT)&(state == RIGHT);    
    assign aaah = (state!=SPLAT)&(state == FALL_L|state ==FALL_R);   
    assign digging = (state!=SPLAT)&(state == DIG_L|state ==DIG_R);   



endmodule  

独热  
module top_module(
    input in,
    input [9:0] state,
    output [9:0] next_state,
    output out1,
    output out2);
    
    assign next_state[0] = ((|state[4:0])|(|state[9:7]))&~in;
    assign next_state[1] = ((|state[9:8])|state[0])&in;
    assign next_state[2] = state[1]&in;
    assign next_state[3] = state[2]&in;
    assign next_state[4] = state[3]&in;
    assign next_state[5] = state[4]&in;
    assign next_state[6] = state[5]&in;
    assign next_state[7] = (state[6]|state[7])&in;
    assign next_state[8] = state[5]&~in;
    assign next_state[9] = state[6]&~in;
    
    assign out1 = |state[9:8];
    assign out2 = state[7]|state[9];
    
endmodule  直接判断那些状态会到达未来状态  




PS2   
module top_module(  
    input clk,   
    input [7:0] in,  
    input reset,    // Synchronous reset  
    output done); //  
    
    parameter BYTE1 = 0,BYTE2 = 1,BYTE3 = 2,DONE = 3;  
    reg [32:0]state,next_state;  
    
    always @(*)begin  
        case(state)  
            BYTE1: next_state=in[3]?BYTE2:BYTE1;  
            BYTE2: next_state=BYTE3; 
            BYTE3: next_state=DONE;  
            DONE:  next_state=in[3]?BYTE2:BYTE1;  
        endcase  
    end  
    
    always @(posedge clk)begin 
        if(reset)  
            state <= BYTE1;  
        else  
            state <= next_state;  
    end  
    
    assign done = (state==DONE);  
endmodule  


加个输出  
module top_module(  
    input clk,  
    input [7:0] in,  
    input reset,    // Synchronous reset  
    output [23:0] out_bytes,  
    output done); //  

    parameter BYTE1 = 0,BYTE2 = 1,BYTE3 = 2,DONE = 3;    
    reg [32:0]state,next_state;    
    
    always @(*)begin    
        case(state)    
            BYTE1: next_state=in[3]?BYTE2:BYTE1;    
            BYTE2: next_state=BYTE3;   
            BYTE3: next_state=DONE;    
            DONE:  next_state=in[3]?BYTE2:BYTE1;    
        endcase    
    end    
    
    always @(posedge clk)begin   
        if(reset)    
            state <= BYTE1;    
        else    
            state <= next_state;    
    end    
    
    assign done = (state==DONE);    
     
     always @(posedge clk)begin   
        if(reset)   
            out_bytes <= 24'b0;    
        else    
            out_bytes <= {out_bytes[15:0],in};    
    end    

endmodule  


串行校验  
module top_module(  
    input clk,  
    input in,  
    input reset,    // Synchronous reset  
    output done  
);   
    parameter   
    IDLE = 4'b0000,  
    START = 4'b0001,    
    STOP = 4'b0010,  
    WAIT = 4'b0100;  
    
    reg [3:0]state,next_state;  
    integer data_cnt;  
    
     always @(posedge clk)begin  
        if(reset)  
            state <= IDLE;  
        else  
            state <= next_state;  
    end  
    
    always @(posedge clk)begin  
        if(reset)  
            data_cnt <= 0;  
        else if(state == START)  
            data_cnt <= data_cnt + 1;  
        else if(state == WAIT)   
            data_cnt <= data_cnt;  
        else  
            data_cnt <= 0;  
    end  
    
    always @(*)begin  
        case(state)  
            IDLE:  
                if(!in)  
                    next_state = START;  
                else  
                    next_state = state;  
            START:  
                if(data_cnt == 8)  
                    if(in)  
                        next_state = STOP;  
                    else  
                        next_state = WAIT;  
                else  
                    next_state = state;  
            STOP:  
                if(!in)  
                    next_state = START;  
                else  
                    next_state = IDLE;  
            WAIT:  
                if(in)  
                    next_state = IDLE;  
                else  
                    next_state = state;  
            default: next_state = IDLE;  
        endcase   
    end  
    
    assign done = (state == STOP);  

endmodule  


加数据输出： always @(posedge clk)begin  
        if(reset)  
            out_byte <= 8'b0;  
        else if(state == START&&data_cnt < 8)  
            out_byte[data_cnt] <= in;  
        else  
            out_byte <= out_byte;  
    end  


奇偶校验
module top_module(
    input clk,
    input in,
    input reset,    // Synchronous reset
    output [7:0] out_byte,
    output done
); //

    parameter   
    IDLE = 4'b0000,  
    START = 4'b0001,    
    STOP = 4'b0010,  
    WAIT = 4'b0100;  
    
    reg [3:0]state,next_state;  
    integer data_cnt;  
    wire odd;
    
     always @(posedge clk)begin  
        if(reset)  
            state <= IDLE;  
        else  
            state <= next_state;  
    end  
    
    always @(posedge clk)begin  
        if(reset)  
            data_cnt <= 0;  
        else if(state == START)  
            data_cnt <= data_cnt + 1;  
        else if(state == WAIT)   
            data_cnt <= data_cnt;  
        else  
            data_cnt <= 0;  
    end  
    
    always @(*)begin  
        case(state)  
            IDLE:  
                if(!in)  
                    next_state = START;  
                else  
                    next_state = state;  
            START:  
                if(data_cnt == 9)  
                    if(in)  
                        next_state = STOP;  
                    else  
                        next_state = WAIT;  
                else  
                    next_state = state;  
            STOP:  
                if(!in)  
                    next_state = START;  
                else  
                    next_state = IDLE;  
            WAIT:  
                if(in)  
                    next_state = IDLE;  
                else  
                    next_state = state;  
            default: next_state = IDLE;  
        endcase   
    end  
    
    assign done = ((state == STOP)&&(odd == 1'b0)); 
    
    always @(posedge clk)begin  
        if(reset)  
            out_byte <= 8'b0;  
        else if(state == START&&data_cnt < 8)  
            out_byte[data_cnt] <= in;  
        else  
            out_byte <= out_byte;  
    end  
    
    parity parity1(clk,(state != START),in,odd);
endmodule


自检报错  
module top_module(
    input clk,
    input reset,    // Synchronous reset
    input in,
    output disc,
    output flag,
    output err);
    parameter IDLE = 4'b0000,
    ONE = 4'b0001,
    TWO = 4'b0010,
    THREE = 4'b0011,
    FOUR = 4'b0100,
    FIVE = 4'b0101,
    SIX = 4'b0110,
    DISC = 4'b0111,
    FLAG = 4'b1000,
    ERROR = 4'b1001;
    reg [3:0]state,next_state;  
    always @(posedge clk)begin  
        if(reset)  
            state <= IDLE;  
        else  
            state <= next_state;  
    end  
    
       always @(*)begin  
        case(state)  
            IDLE:  next_state = in ? ONE:IDLE;
            ONE:  next_state = in ? TWO:IDLE;
            TWO:  next_state = in ? THREE:IDLE;
            THREE:  next_state = in ? FOUR:IDLE;
            FOUR:  next_state = in ? FIVE:IDLE;
            FIVE:  next_state = in ? SIX:DISC;
            SIX:  next_state = in ? ERROR:FLAG;
            DISC:  next_state = in ? ONE:IDLE;
            FLAG:  next_state = in ? ONE:IDLE;
            ERROR:  next_state = in ? ERROR:IDLE;
                
        endcase   
    end  
    assign disc =( state == DISC);
    assign flag =( state == FLAG);
    assign err =( state == ERROR);
endmodule


Q8  
mealy型  
module top_module (
    input clk,
    input aresetn,    // Asynchronous active-low reset
    input x,
    output z ); 
    parameter Zero=4'b0000,
    s1=4'b0001,
    s01=4'b0010,
    s101=4'b0100;
    reg [3:0]state,next_state;
    always @(*)begin
        case(state)
            Zero : next_state= x ? s1 : state;
            s1 : next_state= x ? s1 : s01;
            s01 : next_state= x ? s101 : Zero;
            s101 : next_state= x ? s1 : s01;
        endcase
    end
    
    always @(posedge clk or negedge aresetn)begin
        if(!aresetn)
            state <= Zero;
        else
            state <= next_state;
    end
    assign z = (next_state == s101); //用next_state就是即时输出，用state就慢一个周期

endmodule



二进制补码  
补码性质：找到最低位的1并作为状态AB的分界点，进入状态B就只用取反  

module top_module (
    input clk,
    input areset,
    input x,
    output z
); 
    parameter A=0,B=1,C=2,D=3;
    reg [1:0]state,next_state;
    
    always @(posedge clk or posedge areset)begin
        if(areset) 
            state <= A;
        else
            state <= next_state;
    end
    
    always @(*)begin
        case(state)
            A: next_state= x ? B : A;
            B: next_state= x ? D : C;
            C: next_state= x ? D : C;
            D: next_state= x ? D : C;
            default: next_state = A;
        endcase
    end
        
    always@(*)begin
        case(state)
            A: z = 1'b0;
            B: z = 1'b1;
            C: z = 1'b1;
            D: z = 1'b0;
            default: z = 1'b0;
        endcase
    end

endmodule


q3fsm
module top_module (
    input clk,
    input reset,   // Synchronous reset
    input s,
    input w,
    output z
);
    parameter A=0,B=1;
    reg state,next_state;
    reg [1:0]count,countw;
    
        always @(posedge clk)begin
        if(reset) 
            state <= A;
        else
            state <= next_state;
    end
    
    always @(*)begin
        case(state)
            A: next_state= s ? B : A;
            B: next_state= B;
            default: next_state = A;
        endcase
    end
        
    always@(posedge clk)begin
        if(reset)
            countw <= 2'd0;
        else if(state == B)
            countw <= {countw[0],w};
        else
            countw <= 2'd0;  
    end
    
     always@(posedge clk)begin
        if(reset)
            count <= 2'd0;
        else if(state == B)
            if(count == 2'd2)
                count <= 2'd0;
            else
                count <= count + 2'd1;
        else
            count <= 2'd0;  
    end
    
    always@(posedge clk)begin
        if(reset)
            z <= 1'b0;
        else if((state==B)&&(count==2'd2)&&({countw,w} == 3'b110) |({countw,w} == 3'b101) | ({countw,w} == 3'b011))
            z <= 1'b1;
        else
            z <= 1'b0;
    end

endmodule


q3b  
module top_module (
    input clk,
    input reset,   // Synchronous reset
    input x,
    output z
);
    parameter s000 = 3'b000,s001=3'b001,s010=3'b010,s011=3'b011,s100=3'b100;
    reg [2:0]state,next_state;
    always @(*)begin
        case(state)
            s000 : next_state = x ? s001 : s000;
            s001 : next_state = x ? s100 : s001;
            s010: next_state = x ? s001 : s010;
            s011: next_state = x ? s010 : s001;
            s100 : next_state = x ? s100 : s011;
        endcase
    end
    
    always @(posedge clk)begin
        if(reset) 
            state <= s000;
        else
            state <= next_state;
    end
    
    assign z = (state==s011|state==s100);

endmodule  

q3C
module top_module (
    input clk,
    input [2:0] y,
    input x,
    output Y0,
    output z
);
    parameter s000 = 3'b000,s001=3'b001,s010=3'b010,s011=3'b011,s100=3'b100;
    reg [2:0]next_state;
    always @(*)begin
        case(y)
            s000 : next_state = x ? s001 : s000;
            s001 : next_state = x ? s100 : s001;
            s010: next_state = x ? s001 : s010;
            s011: next_state = x ? s010 : s001;
            s100 : next_state = x ? s100 : s011;
            default: next_state = s000;
        endcase
    end
   
    
    assign z = (y==s011||y==s100);
    assign Y0 = next_state[0];

endmodule




Q6B

module top_module (
    input [3:1] y,
    input w,
    output Y2);
    
    parameter A=3'b000,B=3'b001,C=3'b010,D=3'b011,E=3'b100,F=3'b101;
    reg [3:1]Y;
    
    always @(*)begin
        case(y)
            A: Y = w ? A : B;
            B: Y = w ? D : C;
            C: Y = w ? D : E;
            D: Y = w ? A : F;
            E: Y = w ? D : E;
            F: Y = w ? D : C;
            default: Y <= A;
        endcase   
    end
    assign Y2 = Y[2];

endmodule




Q6C
module top_module (
    input [6:1] y,
    input w,
    output Y2,
    output Y4);
    parameter A=6'b000001,B=6'b000010,C=6'b000100,D=6'b001000,E=6'b010000,F=6'b100000;
    reg [6:1]Y;
    
    always @(*)begin
        case(y)
            A: Y = w ? A : B;
            B: Y = w ? D : C;
            C: Y = w ? D : E;
            D: Y = w ? A : F;
            E: Y = w ? D : E;
            F: Y = w ? D : C;
            default: Y <= A;
        endcase   
    end
    assign Y2 = y[1]&~w;
    assign Y4 = y[2]&w | y[3]&w | y[5]&w | y[6]&w;
endmodule


Q6 FSM  
module top_module (
    input clk,
    input reset,     // synchronous reset
    input w,
    output z);
    parameter A=6'b000001,B=6'b000010,C=6'b000100,D=6'b001000,E=6'b010000,F=6'b100000;
    reg [6:1]state,next_state;
    
    
    always @(*)begin
        case(state)
            A: next_state = w ? A : B;
            B: next_state = w ? D : C;
            C: next_state = w ? D : E;
            D: next_state = w ? A : F;
            E: next_state = w ? D : E;
            F: next_state = w ? D : C;
            default: next_state <= A;
        endcase   
    end
    
    always @(posedge clk)begin
        if(reset) 
            state <= A;
        else
            state <= next_state;
    end
    
    assign z = (state==E)|(state==F);

endmodule  

Q2a
module top_module (
    input clk,
    input reset,   // Synchronous active-high reset
    input w,
    output z
);
    parameter A=6'b000001,B=6'b000010,C=6'b000100,D=6'b001000,E=6'b010000,F=6'b100000;
    reg [6:1]state,next_state;
    
    
    always @(*)begin
        case(state)
            A: next_state = ~w ? A : B;
            B: next_state = ~w ? D : C;
            C: next_state = ~w ? D : E;
            D: next_state = ~w ? A : F;
            E: next_state = ~w ? D : E;
            F: next_state = ~w ? D : C;
            default: next_state <= A;
        endcase   
    end
    
    always @(posedge clk)begin
        if(reset) 
            state <= A;
        else
            state <= next_state;
    end
    
    assign z = (state==E)|(state==F);

endmodule  


q2b
module top_module (
    input [5:0] y,
    input w,
    output Y1,
    output Y3
);
    assign Y1 = y[0]&w;
    assign Y3 = y[1]&~w|y[2]&~w|y[4]&~w|y[5]&~w;

endmodule



q2A FSM
module top_module (
    input clk,
    input resetn,    // active-low synchronous reset
    input [3:1] r,   // request
    output [3:1] g   // grant
); 
    reg [3:1]state,next_state;
    parameter A=3'b000,B=3'b001,C=3'b010,D=3'b100;
    always @(*)begin
        case(state)
            A:  if(r[1])begin
                    next_state = B;
                end
                else if(r[2])begin
                    next_state = C;
                end
                else if(r[3])begin
                    next_state = D;
                end
                else
                    next_state = A;
            B: next_state = r[1] ? B : A ;
            C: next_state = r[2] ? C : A ;
            D: next_state = r[3] ? D : A ;
        endcase
    end
    
    always @(posedge clk)begin
        if(!resetn) 
            state <= A;
        else
            state <= next_state;
    end
    
    assign g = state;

endmodule  





q2b
module top_module (
    input clk,
    input resetn,    // active-low synchronous reset
    input x,
    input y,
    output f,
    output g
); 
    parameter IDLE = 4'd0,S1 = 4'd1,S2 = 4'd2,S3 = 4'd3,S4 = 4'd4,S5 = 4'd5,G_HIGH = 4'd6,G_LOW = 4'd7,F_HIGH=4'd8;
    reg [3:0]state,next_state;
    
    always @(posedge clk)begin
        if(!resetn) 
            state <= IDLE;
        else
            state <= next_state;
    end
    
    always @(*)begin
        case(state)
            IDLE:  next_state = F_HIGH;
            F_HIGH:  next_state = S1;
            S1: next_state = x ? S2 : S1 ;
            S2: next_state = x ? S2 : S3 ;
            S3: next_state = x ? S4 : S1 ;
            S4: next_state = y ? G_HIGH : S5 ;
            S5: next_state = y ? G_HIGH : G_LOW ;
            G_HIGH: next_state =G_HIGH ;
            G_LOW: next_state =G_LOW ;
            default: next_state = IDLE;
        endcase
    end
    
    assign f = ( state == F_HIGH);
    assign g = ( state == S4|state == S5|state == G_HIGH);

endmodule



count1000
module top_module (
    input clk,
    input reset,
    output [9:0] q);

    always @(posedge clk)begin
        if(reset) 
            q <= 10'd0;
        else if(q == 10'd999)
            q <= 10'd0;
        else
            q <= q + 10'd1;
    end

endmodule


移位或计数器
module top_module (
    input clk,
    input shift_ena,
    input count_ena,
    input data,
    output [3:0] q);
    always @(posedge clk)
        case({shift_ena,count_ena})
            2'b10: q <= {q[2:0],data};
            2'b01: q <= q-4'd1;
            default q<=q;
        endcase
                

endmodule



大电路第二部分

module top_module (
    input clk,
    input reset,      // Synchronous reset
    input data,
    output start_shifting);
    parameter s=0,s1=1,s11=2,s110=3,s1101=4;
    reg [2:0]state,next_state;
    always @(*)
        case(state)
            s: next_state = data ? s1 : s;
            s1:next_state = data ? s11 : s;
            s11:next_state = data ? s11 : s110;
            s110:next_state = data ? s1101 : s;
            s1101:next_state = s1101;
        endcase
    
    always @(posedge clk)
        if(reset) 
            state <= s;
        else
            state <= next_state;
    
    assign start_shifting = (state == s1101);
            

endmodule




大电路第三部分之四周期使能
module top_module (
    input clk,
    input reset,      // Synchronous reset
    output shift_ena);
    parameter s=0,s1=1,s11=2,s110=3,s1101=4;
    reg [2:0]state,next_state;
    always @(*)
        case(state)
            s: next_state = reset ? s : s1;
            s1:next_state = reset ? s : s11;
            s11:next_state = reset ? s : s110;
            s110:next_state = reset ? s : s1101;
            s1101:next_state = s1101;
            default: next_state = s;
        endcase
    
    always @(posedge clk)
        if(reset) 
            state <= s;
        else
            state <= next_state;
    
    assign shift_ena = ~(state == s1101);

endmodule


大电路4  
module top_module (
    input clk,
    input reset,      // Synchronous reset
    input data,
    output shift_ena,
    output counting,
    input done_counting,
    output done,
    input ack );
    
    parameter s=0,s1=1,s11=2,s110=3,B0=4,B1=5,B2=6,B3=7,Count=8,Wait=9;
    reg [3:0]state,next_state;
    always @(*)
        case(state)
            s: next_state = data ? s1 : s;
            s1:next_state = data ? s11 : s;
            s11:next_state = data ? s11 : s110;
            s110:next_state = data ? B0 : s;
            B0: next_state = reset ? B0 : B1;
            B1:next_state = reset ? B0 : B2;
            B2:next_state = reset ? B0 : B3;
            B3:next_state = reset ? B0 : Count;
            Count:next_state = done_counting ? Wait : Count;
            Wait:next_state = ack ? s : Wait;
            default: next_state = s;
        endcase
    
    always @(posedge clk)
        if(reset) 
            state <= s;
        else
            state <= next_state;
    
    assign shift_ena = (state == B0)|(state == B1)|(state == B2)|(state == B3);
    assign counting = (state == Count);
    assign done = (state == Wait);

endmodule


大电路全部  

module top_module (
    input clk,
    input reset,      // Synchronous reset
    input data,
    output [3:0] count,
    output counting,
    output done,
    input ack );
    parameter s=0,s1=1,s11=2,s110=3,B0=4,B1=5,B2=6,B3=7,Count=8,Wait=9;
    reg [3:0]state,next_state;
    always @(*)
        case(state)
            s: next_state = data ? s1 : s;
            s1:next_state = data ? s11 : s;
            s11:next_state = data ? s11 : s110;
            s110:next_state = data ? B0 : s;
            B0: next_state = reset ? B0 : B1;
            B1:next_state = reset ? B0 : B2;
            B2:next_state = reset ? B0 : B3;
            B3:next_state = reset ? B0 : Count;
            Count:next_state = done_counting ? Wait : Count;
            Wait:next_state = ack ? s : Wait;
            default: next_state = s;
        endcase
    
    always @(posedge clk)
        if(reset) 
            state <= s;
        else
            state <= next_state;
    wire shift_ena;
    assign shift_ena = (state == B0)|(state == B1)|(state == B2)|(state == B3);
    assign counting = (state == Count);
    assign done = (state == Wait);
    
    wire done_counting = ((count == 4'd0)&&(cnt==10'd0));
    
    always @(posedge clk)
        if(reset) 
            count <= 4'd0;
        else if(shift_ena)
            count <= {count[2:0],data};
        else if(counting)
            count <= (cnt == 10'd0)?count-4'd1:count;
        else
            count <= count;
    reg [9:0]cnt;
    always @(posedge clk)
        if(reset) 
            cnt <= 10'd999;
        else if(counting)
            cnt <= (cnt == 10'd0)?10'd999:cnt-10'd1;
        else
            cnt <= 10'd999;

endmodule




mux2
module top_module (
    input sel,
    input [7:0] a,
    input [7:0] b,
    output [7:0]out  );

    assign out = sel?a:b;

endmodule


与非门 

module top_module (input a, input b, input c, output out);//
    wire out1;
    andgate inst1 ( .out(out1) ,.a(a), .b(b), .c(c),.d(1'b1),.e(1'b1));
    assign out = ~out1;

endmodule



2选一改4选一  

module top_module (
    input [1:0] sel,
    input [7:0] a,
    input [7:0] b,
    input [7:0] c,
    input [7:0] d,
    output [7:0] out  ); //

    wire [7:0]mux0, mux1;
    mux2 mux_0 ( sel[0],    a,    b, mux0 );
    mux2 mux_1 ( &sel[0],    c,    d, mux1 );
    mux2 mux_2 ( sel[1], mux0, mux1,  out );

endmodule

选择器
module top_module (
    input [7:0] code,
    output reg [3:0] out,
    output reg valid=1 );

    always @(*)begin
         valid = 1;
        case (code)
            8'h45: out = 0;
            8'h16: out = 1;
            8'h1e: out = 2;
            8'h26: out = 3;
            8'h25: out = 4;
            8'h2e: out = 5;
            8'h36: out = 6;
            8'h3d: out = 7;
            8'h3e: out = 8;
            8'h46: out = 9;
            default: begin valid = 0;
                           out = 0; //注意两个值都要清零
            end
        endcase
    end

endmodule


tb1  按时序图设计一些信号
module top_module ( output reg A, output reg B );//

    // generate input patterns here
    initial begin
        A = 0;
        B = 0;
        #10 A = 1;
        #5 B = 1; //10+5
        #5 A = 0; //10+5+5
        #20 B = 0;

    end

endmodule





tb  
module top_module();
    reg [1:0]in;
    wire out;
    andgate and1(in,out);
    
    initial begin
        in = 2'b00;
        #10 in= 2'b01;
        #10 in= 2'b10;
        #10 in= 2'b11;
    end

endmodule





tb2  
module top_module();
    reg clk,in;
    reg [2:0]s;
    reg out;
    
    q7 uut1(clk,in,s,out);
    
    initial begin
        in = 0;
        s = 3'd2;
        #10 s = 3'd6;
        #10 s = 3'd2;
            in = 1'b1;
        #10 s = 3'd7;
            in = 1'b0;
        #10 s = 3'd0;
            in = 1'b1;
        #30 in = 1'b0;
        
    end
    
    initial clk = 0;
    always
        #5 clk = ~clk;
    

endmodule




tb/tff 测试用例
module top_module ();
    reg clk;
    reg reset;
    reg t;
    
    wire q;
    
    tff uut(clk,reset,t,q);
    
    always @(posedge clk)
        if(reset)
            t <= 1'b0;
        else
            t <= 1'b1;
    
    initial begin
        reset = 1'b0;
        #10
        reset = 1'b1;
        #10
        reset = 1'b0;
    end
    
    //clk
    initial clk = 0;
    always #5 clk = ~clk;

endmodule





timer
module top_module(
	input clk, 
	input load, 
	input [9:0] data, 
	output tc
);

    reg [9:0] counter;
    
    //data input
    always@(posedge clk)begin
        if(load)begin
            counter <= data;
        end
        else if(counter)begin
            counter <= counter - 1;
        end
        else begin
            counter <= 0;
        end
    end
    assign tc = !counter;

endmodule

条件跳转
module top_module(
    input clk,
    input areset,
    input train_valid,
    input train_taken,
    output reg [1:0] state
);
 
    parameter SNT   =2'b00; 
    parameter WNT   =2'b01; 
    parameter WT    =2'b10; 
    parameter ST    =2'b11; 
    
    reg [1:0] next_state;
    
    wire SNT2SNT,SNT2WNT;
    wire WNT2SNT,WNT2WT,WNT2WNT;
    wire WT2WNT,WT2ST,WT2WT;
    wire ST2WT,ST2ST;
    
    //状态跳转
    always @(posedge clk, posedge areset) begin
        if(areset)begin
            state <= WNT;
        end
        else begin
            state <= next_state;
        end
    end
    
    //状态跳转控制
    always @(*) begin
        case(state)
            SNT : begin
                if (SNT2SNT)begin
                    next_state = SNT;
                end
                else if(SNT2WNT)begin
                    next_state = WNT;
                end
            end
            WNT : begin
                if (WNT2SNT)begin
                    next_state = SNT;
                end
                else if(WNT2WT)begin
                    next_state = WT;
                end
                else if(WNT2WNT)begin
                    next_state = WNT;
                end
            end
            WT : begin
                if (WT2WNT)begin
                    next_state = WNT;
                end
                else if(WT2ST)begin
                    next_state = ST;
                end
                else if(WT2WT)begin
                    next_state = WT;
                end
            end
            ST : begin
                if (ST2WT)begin
                    next_state = WT;
                end
                else if(ST2ST)begin
                    next_state = ST;
                end
            end
            default next_state = WNT;
        endcase
    end
    
    //状态跳转控制条件
    assign SNT2SNT  = train_valid && !train_taken || !train_valid;
    assign SNT2WNT  = train_valid && train_taken;
    assign WNT2SNT  = train_valid && !train_taken;
    assign WNT2WT   = train_valid && train_taken;
    assign WNT2WNT  = !train_valid;
    assign WT2WNT   = train_valid && !train_taken;
    assign WT2ST    = train_valid && train_taken;
    assign WT2WT    = !train_valid;
    assign ST2WT    = train_valid && !train_taken;
    assign ST2ST    = train_valid && train_taken || !train_valid;
 
endmodule

分支方向预测器
module top_module(
    input clk,
    input areset,
 
    input predict_valid,
    input predict_taken,
    output [31:0] predict_history,
 
    input train_mispredicted,
    input train_taken,
    input [31:0] train_history
);
    
    always @(posedge clk, posedge areset) begin
        if(areset)begin
            predict_history <= 32'd0;
        end
        else if(train_mispredicted )begin
            predict_history <= {train_history[30:0],train_taken};
        end
        else if(predict_valid)begin
            predict_history <= {predict_history[30:0],predict_taken};
        end
        else begin
            predict_history <= predict_history;
        end
    end
endmodule

分支方向预测器

module top_module(
    input clk,
    input areset,
 
    input  predict_valid,
    input  [6:0] predict_pc,
    output predict_taken,
    output [6:0] predict_history,
 
    input train_valid,
    input train_taken,
    input train_mispredicted,
    input [6:0] train_history,
    input [6:0] train_pc
);
    
    reg [1:0] PHT[127:0];
    integer i;
    always@(posedge clk,posedge areset) begin
        if(areset) begin
            predict_history <= 7'd0;
            for(i=0;i<=127;i++) PHT[i] <= 2'b01;
        end
        else begin
            if(train_mispredicted&&train_valid) begin
                predict_history <= {train_history[5:0],train_taken};            
            end
            else if(predict_valid) begin
                predict_history <= {predict_history[5:0],predict_taken};
            end
            if(train_valid) begin
                if(train_taken) begin //11
                    PHT[train_pc^train_history] <= (PHT[train_pc^train_history] == 2'b11)? PHT[train_pc^train_history]:PHT[train_pc^train_history] + 2'd1;
                end
                else begin //10               
                    PHT[train_pc^train_history] <= (PHT[train_pc^train_history] == 2'b00)? PHT[train_pc^train_history]:PHT[train_pc^train_history] - 2'd1;
                end            
            end           
        end
    end
    assign predict_taken = PHT[predict_pc^predict_history][1];
 
endmodule


刷完了！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！
