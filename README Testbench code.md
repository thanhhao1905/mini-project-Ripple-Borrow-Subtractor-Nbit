```verilog
`timescale 1ps/1ps
module tbRippleBorrowSubtractor;
  parameter N=4;
  reg [N-1:0]a,b;
  reg bin;
  wire [N-1:0]d;
  wire bout;
  integer err =0;
  reg [N:0]exp_d;
  reg exp_b;
  integer j,k,l;
  
  RippleBorrowSubtractor #(N) DUT(a,b,bin,d,bout);
  
  initial begin
    $monitor("time=%0t,a=%b,b=%b,bin=%b,d=%b,bout=%b| exp_d=%b,exp_b=%b",$time ,a,b,bin,d,bout,exp_d,exp_b);
    
    for(j=0;j<2**N;j=j+1)begin
      for(k=0;k<2**N;k=k+1)begin
        for(l=0;l<2;l=l+1)begin
          a=j[N-1:0];
          b=k[N-1:0];
          bin=l;
          exp_d=a-b-bin;
          exp_b=exp_d[N];
          #5
          check(d,bout,exp_d[N-1:0],exp_b);
        end
      end
    end
    
    
    if(err==0)begin
      $display("-----------");
      $display("Test Pass");
      $display("-----------");
    end else begin
      $display("-----------");
      $display("Test False with %d error",err);
      $display("-----------");
    end
    
    $finish;
  end
  
  task check();
    begin
      if(exp_d[N-1:0]!==d||exp_b!==bout)begin
        $display("[CHECK] Error ");
        err=err+1;
      end else begin
        $display("[CHECK] Matching ");
      end
    end
  endtask
  
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end
endmodule
