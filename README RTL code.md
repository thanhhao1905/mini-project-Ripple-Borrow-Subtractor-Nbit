```verilog
module RippleBorrowSubtractor #(parameter N=4) (input wire [N-1:0] a,b,
                                                input wire bin,
                                                output wire [N-1:0]d,
                                                output wire bout);
  wire [N:0] w ;
  assign w[0]=bin;
  assign bout=w[N];
  genvar i;
  generate 
    for(i=0;i<N;i=i+1) begin
      full_subtractor fs(.a(a[i]),.b(b[i]),.bin(w[i]),.d(d[i]),.bout(w[i+1]));
  end
  endgenerate
 endmodule
  
  
  


module full_subtractor(input wire a,b,bin,
                       output wire d,bout);
  wire [2:0] w;
  
  half_subtractor hs(.a(a),.b(b),.d(w[1]),.bout(w[0]));
  half_subtractor hs1(.a(w[1]),.b(bin),.d(d),.bout(w[2]));
  
                   assign bout = w[0]|w[2];
endmodule


module half_subtractor(input wire a,b,
                       output wire d,bout);
  assign d = a ^ b;
  assign bout= (~a)&b;
endmodule
  
