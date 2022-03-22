# chall_06_pwn_writeup
write to stack, jump to stack, execute the stack


Recon the binary:
  
  Checksec: no canary and executable stack
  file: 64 bit dynamically linked
  
  We know we can execute shellcode from the stack and stack smash. 
  
  Now lets recon the binary w/ radare 2.
  
  ![image](https://user-images.githubusercontent.com/79220528/159389568-6bb8fc8e-a240-48cf-9897-c3d9ac8bd9f0.png)

  We see and main, vuln function but no win().
  
  The main function prints something, takes in 0x50 bytes to Var_50, which is situated at the stack pointer. Vuln is then     called, with another gets, writing to var_60, then a call to rax. 
  
  After stepping throught the debugger, the leaked address is determined to be the stack pointer of main. 
  
Here's a thought: 
  payload 1 - 64 bit shellcode 
  payload 2 - junk * 0x60 + leakedStackPointer
  
![image](https://user-images.githubusercontent.com/79220528/159390031-e9db550e-ecb2-41d1-a0e2-07cd573db137.png)
