
Nanoprocessor

```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/17/2024 12:17:43 AM
-- Design Name: 
-- Module Name: Nanoprocessor - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;
-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity Nanoprocessor is
 Port ( Clk : in STD_LOGIC;
 Reset : in STD_LOGIC;
 LED_out : out STD_LOGIC_VECTOR (3 downto 0);
 S_7seg : out STD_LOGIC_VECTOR (6 downto 0);
 Anode : out STD_LOGIC_VECTOR (3 downto 0);
 Overflow : out STD_LOGIC;
 Zero_flag : out STD_LOGIC;
 Low : out STD_LOGIC;
 High : out STD_LOGIC;
 Equal : out STD_LOGIC);
end Nanoprocessor;

architecture Behavioral of Nanoprocessor is

component Slow_Clk is
 Port ( Clk_in : in STD_LOGIC;
 Clk_out : out STD_LOGIC);
end component;

component ROM is
 Port ( 
 Clk : in STD_LOGIC;
 Address : in STD_LOGIC_VECTOR (2 downto 0);
 Data : out STD_LOGIC_VECTOR (12 downto 0));
end component;

component Program_Counter is
 Port ( Clk : in STD_LOGIC;
 Reset : in STD_LOGIC;
 D : in STD_LOGIC_VECTOR (2 downto 0);
 Q : out STD_LOGIC_VECTOR (2 downto 0));
end component;

component Ins_Decoder is
 Port (
 Ins : in STD_LOGIC_VECTOR (12 downto 0); 
 Reg_En, MuxA_Sel, MuxB_Sel, Jmp_Addr : out STD_LOGIC_VECTOR(2 downto 0);
 Load_Sel, Sub_Sel, Jmp_on, Com_EN: out STD_LOGIC;
 Im_Val : out STD_LOGIC_VECTOR (3 downto 0) );
end component;

component MUX_2x3 is
    Port ( 
        A : in STD_LOGIC_VECTOR (2 downto 0);   
        B : in STD_LOGIC_VECTOR (2 downto 0);   
        S : in STD_LOGIC;                       
        Z : out STD_LOGIC_VECTOR (2 downto 0)   
    );
end component;
component MUX_2x4 is
    Port ( 
    A : in STD_LOGIC_VECTOR (3 downto 0);   
    B : in STD_LOGIC_VECTOR (3 downto 0);   
    S : in STD_LOGIC;                       
    Z : out STD_LOGIC_VECTOR (3 downto 0)   
);
end component;
 
component MUX_8x4 is
    Port ( 
        R0 : in STD_LOGIC_VECTOR (3 downto 0);   
        R1 : in STD_LOGIC_VECTOR (3 downto 0);   
        R2 : in STD_LOGIC_VECTOR (3 downto 0);  
        R3 : in STD_LOGIC_VECTOR (3 downto 0);   
        R4 : in STD_LOGIC_VECTOR (3 downto 0);   
        R5 : in STD_LOGIC_VECTOR (3 downto 0);   
        R6 : in STD_LOGIC_VECTOR (3 downto 0);   
        R7 : in STD_LOGIC_VECTOR (3 downto 0);   
        S : in STD_LOGIC_VECTOR (2 downto 0);    
        Z : out STD_LOGIC_VECTOR (3 downto 0)    
    );
end component;
 
component Adder_3_bit is
 Port ( A : in STD_LOGIC_VECTOR (2 downto 0);
 B : in STD_LOGIC;
 S : out STD_LOGIC_VECTOR (2 downto 0));
end component;
 
 component Add_sub is
  Port ( A : in STD_LOGIC_VECTOR (3 downto 0);
  B : in STD_LOGIC_VECTOR (3 downto 0);
  Sub_EN : in STD_LOGIC;
  S : out STD_LOGIC_VECTOR (3 downto 0);
  Overflow : out STD_LOGIC;
  Zero_flag : out STD_LOGIC);
 end component;
 
 component Reg_Bank is
  Port ( D : in STD_LOGIC_VECTOR (3 downto 0);
  Clk : in STD_LOGIC;
  Reset : in STD_LOGIC;
  Reg_En : in STD_LOGIC_VECTOR (2 downto 0);
  R0 : out STD_LOGIC_VECTOR (3 downto 0);
  R1 : out STD_LOGIC_VECTOR (3 downto 0);
  R2 : out STD_LOGIC_VECTOR (3 downto 0);
  R3 : out STD_LOGIC_VECTOR (3 downto 0);
  R4 : out STD_LOGIC_VECTOR (3 downto 0);
  R5 : out STD_LOGIC_VECTOR (3 downto 0);
  R6 : out STD_LOGIC_VECTOR (3 downto 0);
  R7 : out STD_LOGIC_VECTOR (3 downto 0));
 end component;
 
 component LUT_16_7 is
  Port ( address : in STD_LOGIC_VECTOR (3 downto 0);
  data : out STD_LOGIC_VECTOR (6 downto 0));
 end component;
 
 component Comparator is
  Port ( A : in STD_LOGIC_VECTOR (3 downto 0);
  B : in STD_LOGIC_VECTOR (3 downto 0);
  Com_EN : in STD_LOGIC;
  Low : out STD_LOGIC;
    High : out STD_LOGIC;
    Equal : out STD_LOGIC);
   end component;
   
   component Jumper is
     Port (
     Jmp_check : in STD_LOGIC_VECTOR (3 downto 0); 
     Jmp_on : in STD_LOGIC;
     Jmp_Flag: out STD_LOGIC );
   end component;
   
   signal Clk_signal, Load_Sel, Sub_En, Com_EN, Jmp_Flag, Jmp_on : STD_LOGIC;
   signal ROM_In, PC_In, Reg_En, MuxA_enable, MuxB_enable, Jmp_Addr, Adder_3bit_out : STD_LOGIC_VECTOR(2 downto 0);
   signal Im_Val, MuxA_out, MuxB_out, AU_out, Data_in_Bus, Jmp_check_out, R0_out, R1_out, R2_out, R3_out, R4_out, R5_out, R6_out, R7_out : STD_LOGIC_VECTOR(3 downto 0);
   signal Ins_Bus : STD_LOGIC_VECTOR (12 downto 0);
   
   begin
   Slow_Clk_0 : Slow_Clk
    port map (
    Clk_in => Clk,
    Clk_out => Clk_signal);
    
   PC_0: Program_Counter
    Port map( 
    Clk => Clk_signal,
    Reset =>Reset,
    D => PC_In,
    Q => ROM_In);
    
   ROM_0 : ROM 
    port map (
    Clk => Clk_signal,
    Address => ROM_In,
    Data => Ins_Bus);
    
   Ins_Decoder_0 : Ins_Decoder
    Port map(
    Ins => Ins_Bus,
    Reg_En => Reg_En,
    MuxA_Sel => MuxA_enable, 
    MuxB_Sel => MuxB_enable, 
    Jmp_Addr =>Jmp_Addr,
    Load_Sel => Load_Sel,
    Sub_Sel => Sub_En,
    Jmp_on => Jmp_on,
    Com_EN => Com_EN,
    Im_Val => Im_val);
    
   Data_select_Mux: MUX_2x4
    Port map( 
    A => AU_out, 
    B => Im_Val,
    S => Load_Sel,
    Z => Data_in_Bus);
    
   Reg_bank0 :Reg_Bank
    Port map ( 
    D => Data_in_Bus,
    Clk => Clk_signal,
    Reset => Reset,
    Reg_En => Reg_En,
    R0 => R0_out,
    R1 => R1_out,
    R2 => R2_out,
    R3 => R3_out,
    R4 => R4_out,
    R5 => R5_out,
    R6 => R6_out,
    R7 => R7_out);
    
   MuxA : MUX_8x4
    Port map ( 
    R0 => R0_out,
    R1 => R1_out,
    R2 => R2_out,
    R3 => R3_out,
    R4 => R4_out,
    R5 => R5_out,
    R6 => R6_out,
    R7 => R7_out,
    s => MuxA_enable,
    z => MuxA_out);
    
   MuxB : MUX_8x4
    Port map ( 
    R0 => R0_out,
    R1 => R1_out,
    R2 => R2_out,
    R3 => R3_out,
    R4 => R4_out,
    R5 => R5_out,
    R6 => R6_out,
    R7 => R7_out,
    s => MuxB_enable,
    z => MuxB_out);
    
   AU : Add_sub
    Port map( 
    A => MuxA_out,
    B => MuxB_out,
    Sub_EN => Sub_En,
    S => AU_out,
    Overflow => Overflow,
    Zero_flag => Zero_flag);
    
   Adder_3bit : Adder_3_bit
    Port map (
    A => ROM_In,
    B => '1',
    S => Adder_3bit_out);
   
    Jumper_0 :Jumper
     Port map( 
     Jmp_check => MuxA_out,
     Jmp_on => Jmp_on,
     Jmp_Flag => Jmp_Flag);
      
   Addr_Sel_Mux :MUX_2x3
    Port map( 
    A => Adder_3bit_out,
    B => Jmp_Addr,
    S => Jmp_Flag,
    Z => PC_In);
      
     LUT_16_7_0 : LUT_16_7
      port map(
      address=>R7_out,
      data=>S_7seg);
    
     COM : Comparator
      Port map( 
      A => MuxA_out,
      B => MuxB_out,
      Com_EN => Com_EN,
      Low => Low,
      High => High,
      Equal => Equal); 
    
     LED_out <= R7_out;
     Anode <= "1110";
     end Behavioral;
```

Slow_Clk


```VHDL
---------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
--
-- Create Date: 03/06/2024 02:08:45 PM
-- Design Name: 
-- Module Name: Slow_Clk - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
--
-- Dependencies: 
--
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
--
---------------------------------------------------------------------------------
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;
-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity Slow_Clk is
 Port ( Clk_in : in STD_LOGIC;
 Clk_out : out STD_LOGIC);
end Slow_Clk;
architecture Behavioral of Slow_Clk is
 signal count : integer := 1;
 signal Clk_status : STD_LOGIC := '0';
 
begin
 process (Clk_in) begin 
 if (rising_edge (Clk_in)) then
 count <= Count +1;
 if (count =5) then 
 Clk_status <= not Clk_status;
  Clk_out <= Clk_status;
 count <=1;
 end if;
 end if;
end process;
end Behavioral;
```

Program_Counter


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 04:58:10 PM
-- Design Name: 
-- Module Name: Program_Counter - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity Program_Counter is
 Port ( Clk : in STD_LOGIC;
 Reset : in STD_LOGIC;
 D : in STD_LOGIC_VECTOR (2 downto 0);
 Q : out STD_LOGIC_VECTOR (2 downto 0));
end Program_Counter;

architecture Behavioral of Program_Counter is

signal max_reached : STD_LOGIC := '0'; -- Add this signal

begin
process (clk) begin 
if (rising_edge(Clk)) then
    if Reset = '1' then
        Q <= "000";
        max_reached <= '0'; -- Reset the signal when Reset is '1'
    else
        if max_reached = '0' then -- Only update Q if max_reached is '0'
            Q <= D;
            if D = "111" then -- If D has reached its maximum value
                max_reached <= '1'; -- Set max_reached to '1'
            end if;    
        end if;
    end if;
end if;
end process;
end Behavioral;
```

ROM


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 11:39:46 PM
-- Design Name: 
-- Module Name: ROM - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.numeric_STD.ALL;
-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;
-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity ROM is
 Port ( 
 Clk : in STD_LOGIC;
 Address : in STD_LOGIC_VECTOR (2 downto 0);
 Data : out STD_LOGIC_VECTOR (12 downto 0));
end ROM;
architecture Behavioral of ROM is
type rom_type is array(0 to 7) of std_logic_vector(12 downto 0);
signal ROM : rom_type := (
------------------ Operations -----------------------------
"0010010000001", -- MOVI R1, 1
"0010100000010", -- MOVI R2, 2
"0010110000011", -- MOVI R3, 3 
"0110010100000", --- ADD R1,R2
"0110010110000", --- ADD R1,R3
"0111110010000",--- ADD R7,R1
"0000000000000", -- NULL
"0000000000000" -- NULL
);

signal prev_Address : STD_LOGIC_VECTOR (2 downto 0) := "111"; -- Add this signal to store the previous Address

begin
process (Clk,Address) 
begin
if falling_edge(Clk) then 
    if Address = prev_Address then
        -- If the current Address is the same as the previous one, output the specific instruction
        Data <= "0000000000000"; -- NULL
    else
        -- If it's not, proceed with the normal operation
        Data <= ROM(to_integer(unsigned(Address)));
    end if;

    -- At the end of the process block, update the previous Address signal with the current one
    prev_Address <= Address;
end if;
end process;
end Behavioral;
```

Ins_Decoder


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 08:56:21 PM
-- Design Name: 
-- Module Name: Ins_Decoder - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity Ins_Decoder is
 Port (
 Ins : in STD_LOGIC_VECTOR (12 downto 0);
 Reg_En, MuxA_Sel, MuxB_Sel, Jmp_Addr : out STD_LOGIC_VECTOR(2 downto 0);
 Load_Sel, Sub_Sel, Jmp_on, Com_EN: out STD_LOGIC;
 Im_Val : out STD_LOGIC_VECTOR (3 downto 0) );
end Ins_Decoder;

architecture Behavioral of Ins_Decoder is

signal K : STD_LOGIC_VECTOR (12 downto 0);
signal I : STD_LOGIC_VECTOR (2 downto 0);

begin
K <= Ins;
I <= Ins (12 downto 10);

process (K) 
begin

if I = "001" then -- MOVI R_In, d
 Reg_En <= Ins (9 downto 7);
 Im_Val <= Ins (3 downto 0);
 Load_Sel <= '1';
 Com_En <= '0'; 
 MuxA_Sel <= "000";
 MuxB_Sel <= "000";
 Sub_Sel <= '0';
 Jmp_on <= '0';
-- Jmp_Addr <= "UUU";
 
elsif I = "011" then -- ADD Ra, Rb 
 MuxA_Sel <= Ins (9 downto 7);
 MuxB_Sel <= Ins (6 downto 4);
 Sub_Sel <= '0'; -- adding 
 Load_Sel <= '0';
 Reg_En <= Ins (9 downto 7);
-- Im_Val <= "UUUU";
 Com_En <= '0'; 
 Jmp_on <= '0';
-- Jmp_Addr <= "UUU";

elsif I = "100" then -- SUB Ra, Rb 
 MuxA_Sel <= Ins (9 downto 7);
 MuxB_Sel <= Ins (6 downto 4);
 Sub_Sel <= '1'; -- substract 
 Load_Sel <= '0';
 Reg_En <= Ins (9 downto 7);
-- Im_Val <= "UUUU";
 Com_En <= '0'; 
 Jmp_on <= '0';
-- Jmp_Addr <= "UUU";

elsif I = "010" then -- NEG R_In
 MuxA_Sel <= "000"; -- register 0 has value 0
 MuxB_Sel <= Ins (9 downto 7);
 Sub_Sel <= '1';
 Load_Sel <= '0';
 Reg_En <= Ins (9 downto 7);
-- Im_Val <= "UUUU";
 Com_En <= '0';
 Jmp_on <= '0';
-- Jmp_Addr <= "UUU"; 
 
elsif I = "101" then -- JZR R_In, d 
 Jmp_on <= '1';
 Jmp_Addr <= Ins (2 downto 0);
 Reg_En <= Ins (9 downto 7);
 Load_Sel <= '0';
-- Im_Val <= "UUUU";
 Com_En <= '0';
 MuxA_Sel <= Ins (9 downto 7); 
 MuxB_Sel <= "000";
 Sub_Sel <= '0';
 
elsif I = "110" then -- COM Ra, Rb 
  MuxA_Sel <= Ins (9 downto 7);
  MuxB_Sel <= Ins (6 downto 4);
  Sub_Sel <= '0'; 
  Load_Sel <= '0';
  Reg_En <= "000";
  Im_Val <= "0000";
  Com_EN <= '1'; 
  Jmp_on <= '0';
 -- Jmp_Addr <= "UUU";
 
elsif I = "000" then -- NULL
 Reg_En <= "000";
 Im_Val <= "0000";
 Load_Sel <= '1';
 Com_En <= '0'; 
 MuxA_Sel <= "000";
 MuxB_Sel <= "000";
 Sub_Sel <= '0';
 Jmp_on <= '0';
-- Jmp_Addr <= "UUU";
 
end if;
end process;
end Behavioral;
```

MUX_2x4


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 02:49:56 PM
-- Design Name: 
-- Module Name: MUX_2x4 - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity MUX_2x4 is
    Port ( 
    A : in STD_LOGIC_VECTOR (3 downto 0);   
    B : in STD_LOGIC_VECTOR (3 downto 0);   
    S : in STD_LOGIC;                       
    Z : out STD_LOGIC_VECTOR (3 downto 0)   
);
end MUX_2x4;

architecture Behavioral of MUX_2x4 is

begin
    Z <= A when S = '0' else B;  
    
end Behavioral;

```

Reg_Bank


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 11:55:37 PM
-- Design Name: 
-- Module Name: Reg_Bank - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;
-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity Reg_Bank is
 Port ( D : in STD_LOGIC_VECTOR (3 downto 0);
 Clk : in STD_LOGIC;
 Reset : in STD_LOGIC;
 Reg_En : in STD_LOGIC_VECTOR (2 downto 0);
 R0 : out STD_LOGIC_VECTOR (3 downto 0);
 R1 : out STD_LOGIC_VECTOR (3 downto 0);
 R2 : out STD_LOGIC_VECTOR (3 downto 0);
 R3 : out STD_LOGIC_VECTOR (3 downto 0);
 R4 : out STD_LOGIC_VECTOR (3 downto 0);
 R5 : out STD_LOGIC_VECTOR (3 downto 0);
 R6 : out STD_LOGIC_VECTOR (3 downto 0);
 R7 : out STD_LOGIC_VECTOR (3 downto 0));
end Reg_Bank;
architecture Behavioral of Reg_Bank is
component Reg_without_FF is
 Port ( D : in STD_LOGIC_VECTOR (3 downto 0);
 En : in STD_LOGIC;
 Reset : in STD_LOGIC;
 Clk : in STD_LOGIC;
 Q : out STD_LOGIC_VECTOR (3 downto 0));
end component;
component Decoder_3_to_8 is
 Port ( I : in STD_LOGIC_VECTOR (2 downto 0);
 EN : in STD_LOGIC;
 Y : out STD_LOGIC_VECTOR (7 downto 0));
end component;
SIGNAL Dec_out : STD_LOGIC_VECTOR (7 downto 0);
begin
Decoder_3_to_8_0: Decoder_3_to_8
 PORT MAP(
 EN => '1',
 I => Reg_En,
 Y => Dec_out
  );
Reg_0: Reg_without_FF
 PORT MAP(
 D => "0000",
 Q => R0,
 En => '1',
 Clk => Clk,
 Reset => '0'
 );
Reg_1: Reg_without_FF
 PORT MAP(
 D => D,
 Q => R1,
 En => Dec_out(1),
 Clk => Clk,
 Reset => Reset
 );
 
Reg_2: Reg_without_FF
 PORT MAP(
 D => D,
 Q => R2,
 En => Dec_out(2),
 Clk => Clk,
 Reset => Reset
 );
 
Reg_3: Reg_without_FF
 PORT MAP(
 D => D,
 Q => R3,
 En => Dec_out(3),
 Clk => Clk,
 Reset => Reset
  );
Reg_4: Reg_without_FF
 PORT MAP(
 D => D,
 Q => R4,
 En => Dec_out(4),
 Clk => Clk,
 Reset => Reset
 ); 
Reg_5: Reg_without_FF
 PORT MAP(
 D => D,
 Q => R5,
 En => Dec_out(5),
 Clk => Clk,
 Reset => Reset
 );
 
Reg_6: Reg_without_FF
 PORT MAP(
 D => D,
 Q => R6,
 En => Dec_out(6),
 Clk => Clk,
 Reset => Reset
 ); 
 
Reg_7: Reg_without_FF
 PORT MAP(
 D => D,
 Q => R7,
 En => Dec_out(7),
 Clk => Clk,
 Reset => Reset
  ); 
end Behavioral;

```

Decoder_3_to_8


```VHDL
------------------------------------------------------------------------------
----
-- Company: 
-- Engineer: 
--
-- Create Date: 02/20/2024 02:53:12 PM
-- Design Name: 
-- Module Name: Decoder_3_to_8 - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
--
-- Dependencies: 
--
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
--
------------------------------------------------------------------------------
----
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;
-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity Decoder_3_to_8 is
Port ( I : in STD_LOGIC_VECTOR (2 downto 0);
EN : in STD_LOGIC;
Y : OUT STD_LOGIC_VECTOR (7 downto 0));
end Decoder_3_to_8;
architecture Behavioral of Decoder_3_to_8 is
component Decoder_2_to_4
port(
I: in STD_LOGIC_VECTOR;
EN: in STD_LOGIC;
Y: out STD_LOGIC_VECTOR );
end component;
signal I0,I1 : STD_LOGIC_VECTOR (1 downto 0);
signal Y0,Y1 : STD_LOGIC_VECTOR (3 downto 0);
signal en0,en1, I2 : STD_LOGIC;
begin
Decoder_2_to_4_0 : Decoder_2_to_4
port map(
I => I0,
EN => en0,
Y => Y0 );
Decoder_2_to_4_1 : Decoder_2_to_4
port map(
I => I1,
EN => en1,
Y => Y1 );
en0 <= NOT(I(2)) AND EN;
en1 <= I(2) AND EN;
I0 <= I(1 downto 0);
I1 <= I(1 downto 0);
I2 <= I(2);
Y(3 downto 0) <= Y0;
Y(7 downto 4) <= Y1;
end Behavioral;
```

Decoder_2_to_4


```VHDL
------------------------------------------------------------------------------
----
-- Company: 
-- Engineer: 
--
-- Create Date: 02/20/2024 02:01:05 PM
-- Design Name: 
-- Module Name: Decoder_2_to_4 - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
--
-- Dependencies: 
--
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
--
------------------------------------------------------------------------------
----
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;
-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity Decoder_2_to_4 is
Port ( I : in STD_LOGIC_VECTOR (1 downto 0);
EN : in STD_LOGIC;
Y : out STD_LOGIC_VECTOR (3 downto 0));
end Decoder_2_to_4;
architecture Behavioral of Decoder_2_to_4 is
begin
Y(0) <= EN AND NOT(I(0)) AND NOT(I(1));
Y(1) <= EN AND I(0) AND NOT(I(1));
Y(2) <= EN AND NOT(I(0)) AND I(1);
Y(3) <= EN AND I(0) AND I(1);
end Behavioral;
```

Reg_without_FF


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 11:47:48 PM
-- Design Name: 
-- Module Name: Reg_without_FF - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;
-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity Reg_without_FF is
 Port ( D : in STD_LOGIC_VECTOR (3 downto 0);
 En : in STD_LOGIC;
 Reset : in STD_LOGIC;
 Clk : in STD_LOGIC;
 Q : out STD_LOGIC_VECTOR (3 downto 0));
end Reg_without_FF;
architecture Behavioral of Reg_without_FF is
begin
process (Clk) 
begin 
 if (falling_edge(Clk)) then 
 if Reset = '1' then
 Q <= "0000";
 elsif (En = '1') then
 Q <= D;
 end if;
 end if;
end process;
end Behavioral;

```

MUX_8x4


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 02:59:42 PM
-- Design Name: 
-- Module Name: MUX_8x4 - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity MUX_8x4 is
    Port ( 
        R0 : in STD_LOGIC_VECTOR (3 downto 0);   
        R1 : in STD_LOGIC_VECTOR (3 downto 0);   
        R2 : in STD_LOGIC_VECTOR (3 downto 0);  
        R3 : in STD_LOGIC_VECTOR (3 downto 0);   
        R4 : in STD_LOGIC_VECTOR (3 downto 0);   
        R5 : in STD_LOGIC_VECTOR (3 downto 0);   
        R6 : in STD_LOGIC_VECTOR (3 downto 0);   
        R7 : in STD_LOGIC_VECTOR (3 downto 0);   
        S : in STD_LOGIC_VECTOR (2 downto 0);    
        Z : out STD_LOGIC_VECTOR (3 downto 0)    
    );
end MUX_8x4;

architecture Behavioral of MUX_8x4 is
begin
    Z <= R0 when S = "000" else
         R1 when S = "001" else
         R2 when S = "010" else
         R3 when S = "011" else
         R4 when S = "100" else
         R5 when S = "101" else
         R6 when S = "110" else
         R7;  
end Behavioral;
```

Add_sub


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 01:12:16 PM
-- Design Name: 
-- Module Name: Add_sub - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity Add_sub is
 Port ( A : in STD_LOGIC_VECTOR (3 downto 0);
 B : in STD_LOGIC_VECTOR (3 downto 0);
 Sub_EN : in STD_LOGIC;
 S : out STD_LOGIC_VECTOR (3 downto 0);
 Overflow : out STD_LOGIC;
 Zero_flag : out STD_LOGIC);
end Add_sub;

architecture Behavioral of Add_sub is
component FA is
 Port ( A : in STD_LOGIC;
 B : in STD_LOGIC;
 C_in : in STD_LOGIC;
 S : out STD_LOGIC;
 C_out : out STD_LOGIC);
end component;

SIGNAL FA0_C, FA1_C, FA2_C, FA3_C: STD_LOGIC;
SIGNAL XOR_OUT,S_out : STD_LOGIC_VECTOR (3 downto 0);

begin

XOR_OUT(0) <= B(0) XOR Sub_EN; 
XOR_OUT(1) <= B(1) XOR Sub_EN; 
XOR_OUT(2) <= B(2) XOR Sub_EN; 
XOR_OUT(3) <= B(3) XOR Sub_EN; 

FA_0 : FA
 port map (
 A => A(0),
 B => XOR_OUT(0),
 C_in => Sub_EN,
 S => S_out(0),
 C_Out => FA0_C);
 
FA_1 : FA
 port map (
 A => A(1),
 B => XOR_OUT(1),
 C_in => FA0_C,
 S => S_out(1),
 C_Out => FA1_C);
 
FA_2 : FA
 port map (
 A => A(2),
 B => XOR_OUT(2),
 C_in => FA1_C,
 S => S_out(2),
 C_Out => FA2_C);
 
FA_3 : FA
 port map (
 A =>A(3),
 B => XOR_OUT(3),
 C_in => FA2_C,
 S => S_out(3),
 C_Out => FA3_C);
 
S(0)<=S_out(0);
S(1)<=S_out(1);
S(2)<=S_out(2);
S(3)<=S_out(3);

Zero_flag <= NOT(S_out(0) OR S_out(1) OR S_out(2) OR S_out(3)) AND ((A(0) OR B(0)) OR (A(1) OR B(1)) OR (A(2) OR B(2)) OR (A(3) OR B(3)));
Overflow<= FA3_C XOR FA2_C;

end Behavioral;
```

FA


```VHDL
------------------------------------------------------------------------------
----
-- Company: 
-- Engineer: 
--
-- Create Date: 02/13/2024 03:20:23 PM
-- Design Name: 
-- Module Name: FA - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
--
-- Dependencies: 
--
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
--
------------------------------------------------------------------------------
----
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;
-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity FA is
Port ( A : in STD_LOGIC;
B : in STD_LOGIC;
C_in : in STD_LOGIC;
S : out STD_LOGIC;
C_out : out STD_LOGIC);
end FA;
architecture Behavioral of FA is
component HA
PORT( 
A : in STD_LOGIC;
B : in STD_LOGIC;
S : out STD_LOGIC;
C : out STD_LOGIC
);
end component;
signal HA0_S,HA0_C,HA1_S,HA1_C: STD_LOGIC;
begin
HA_0 : HA
PORT MAP(
A=>A,
B=>B,
C=>HA0_C,
S=>HA0_S
);
HA_1 : HA
PORT MAP(
A=>HA0_S,
B=>C_in,
C=>HA1_C,
S=>HA1_S 
); 
C_out<= HA0_C OR HA1_C;
S <= HA1_S;
end Behavioral;
```

HA


```VHDL
------------------------------------------------------------------------------
----
-- Company: 
-- Engineer: 
--
-- Create Date: 02/13/2024 03:06:31 PM
-- Design Name: 
-- Module Name: HA - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
--
-- Dependencies: 
--
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
--
------------------------------------------------------------------------------
----
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;
-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity HA is
Port ( A : in STD_LOGIC;
B : in STD_LOGIC;
S : out STD_LOGIC;
C : out STD_LOGIC);
end HA;
architecture Behavioral of HA is
begin
S <= A xor B; 
C <= A and B;
end Behavioral;

```

Adder_3_bit


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 01:54:27 PM
-- Design Name: 
-- Module Name: Adder_3_bit - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;
-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity Adder_3_bit is
 Port ( A : in STD_LOGIC_VECTOR (2 downto 0);
 B : in STD_LOGIC;
 S : out STD_LOGIC_VECTOR (2 downto 0));
end Adder_3_bit;

architecture Behavioral of Adder_3_bit is
component HA is
 Port ( A : in STD_LOGIC;
 B : in STD_LOGIC;
 S : out STD_LOGIC;
 C : out STD_LOGIC);
end component;

SIGNAL HA0_C, HA1_C, HA2_C: STD_LOGIC;

begin
HA0 : HA PORT MAP (A=>A(0), B=>'1', S=>S(0), C=>HA0_C);
HA1 : HA PORT MAP (A=>A(1), B=>HA0_C, S=>S(1), C=>HA1_C);
HA2 : HA PORT MAP (A=>A(2), B=>HA1_C, S=>S(2), C=>HA2_C);
end Behavioral;

```

Jumper


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/18/2024 10:35:08 PM
-- Design Name: 
-- Module Name: Jumper - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity Jumper is
  Port (
  Jmp_check : in STD_LOGIC_VECTOR (3 downto 0); 
  Jmp_on : in STD_LOGIC;
  Jmp_Flag: out STD_LOGIC );
end Jumper;

architecture Behavioral of Jumper is

begin
process(Jmp_on, Jmp_check) 
begin
if Jmp_on = '1' then
    if Jmp_check = "0000" then
        Jmp_Flag <= '1';
    else
         Jmp_Flag <= '0';
    end if;
else
    Jmp_Flag <= '0';
end if;
end process;
end Behavioral;
```

MUX_2x3


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 02:38:03 PM
-- Design Name: 
-- Module Name: MUX_2x3 - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiaing
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity MUX_2x3 is
    Port ( 
        A : in STD_LOGIC_VECTOR (2 downto 0);   
        B : in STD_LOGIC_VECTOR (2 downto 0);   
        S : in STD_LOGIC;                       
        Z : out STD_LOGIC_VECTOR (2 downto 0)   
    );
end MUX_2x3;

architecture Behavioral of MUX_2x3 is

begin
    Z <= A when S = '0' else B;  
    
end Behavioral;
```

LUT_16_7


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 03/18/2024 10:00:33 AM
-- Design Name: 
-- Module Name: LUT_16_7 - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use ieee.numeric_std.all;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;

entity LUT_16_7 is
    Port ( address : in STD_LOGIC_VECTOR (3 downto 0);
           data : out STD_LOGIC_VECTOR (6 downto 0));
end LUT_16_7;

architecture Behavioral of LUT_16_7 is

type rom_type is array (0 to 15) of std_logic_vector(6 downto 0);
signal sevenSegment_ROM : rom_type := (
"1000000", -- 0
"1111001", -- 1
"0100100", -- 2
"0110000", -- 3
"0011001", -- 4
"0010010", -- 5
"0000010", -- 6
"1111000", -- 7
"0000000", -- 8
"0010000", -- 9
"0001000", -- a
"0000011", -- b
"1000110", -- c
"0100001", -- d
"0000110", -- e
"0001110" -- f
);

begin

data <= sevenSegment_ROM(to_integer(unsigned(address)));

end Behavioral;

```

Comparator


```VHDL
----------------------------------------------------------------------------------
-- Company: 
-- Engineer: 
-- 
-- Create Date: 04/16/2024 08:14:33 PM
-- Design Name: 
-- Module Name: Comparator - Behavioral
-- Project Name: 
-- Target Devices: 
-- Tool Versions: 
-- Description: 
-- 
-- Dependencies: 
-- 
-- Revision:
-- Revision 0.01 - File Created
-- Additional Comments:
-- 
----------------------------------------------------------------------------------


library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

-- Uncomment the following library declaration if using
-- arithmetic functions with Signed or Unsigned values
--use IEEE.NUMERIC_STD.ALL;

-- Uncomment the following library declaration if instantiating
-- any Xilinx leaf cells in this code.
--library UNISIM;
--use UNISIM.VComponents.all;
entity Comparator is
 Port ( A : in STD_LOGIC_VECTOR (3 downto 0);
 B : in STD_LOGIC_VECTOR (3 downto 0);
 Com_EN : in STD_LOGIC;
 Low : out STD_LOGIC;
 High : out STD_LOGIC;
 Equal : out STD_LOGIC);
end Comparator;

architecture Behavioral of Comparator is
begin
    process(A, B, Com_EN)
    begin
        if Com_EN = '1' then
            if A = B then
                Equal <= '1';
                Low <= '0';
                High <= '0';
            elsif A > B then  
                Low <= '0';
                High <= '1';
                Equal <= '0';
            else
                Low <= '1';
                High <= '0';
                Equal <= '0';
            end if;
        else
            Low <= '0';
            High <= '0';
            Equal <= '0';
        end if;
    end process;
end Behavioral;


```

