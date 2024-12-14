
```VHDL
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
```



```VHDL
entity FA is
--Define your inputs and outputs
    Port ( 
        A : in STD_LOGIC;
        B : in STD_LOGIC;
        C_in : in STD_LOGIC;
        S : out STD_LOGIC;
        C_out : out STD_LOGIC
    );
end FA;

architecture Behavioral of FA is
--If you are using another design source define it
    component HA
        PORT( 
            A : in STD_LOGIC;
            B : in STD_LOGIC;
            S : out STD_LOGIC;
            C : out STD_LOGIC
        );
    end component;

--Define the signals you need
signal HA0_S,HA0_C,HA1_S,HA1_C: STD_LOGIC;

begin
--Port map to inputs and outputs of another design source items
    HA_0 : HA
    PORT MAP(
        A = > A,
        B = > B,
        C = > HA0_C,
        S = > HA0_S
    );

    HA_1 : HA
    PORT MAP(
        A = > HA0_S,
        B = > C_in,
        C = > HA1_C,
        S = > HA1_S 
    ); 

--Define your outputs
    C_out < = HA0_C OR HA1_C;
    S < = HA1_S;
end Behavioral;

```





### Simulation



```VHDL
library IEEE;
use IEEE.STD_LOGIC_1164.ALL

entity TB_FA is
-- Port ( );
end TB_FA;

architecture Behavioral of TB_FA is
-- Define the component used within the Test Bench (TB_FA)
    component FA 
        port (
            A: in std_logic;      
            B: in std_logic;      
            C_in : in STD_LOGIC;  
            S: out std_logic;     
            C_out : out STD_LOGIC 
        ); 
    end component;

-- Define the signals used in the Test Bench
signal a,b,c_in,s,c_out : STD_LOGIC;

begin
-- Port map with the signals (UUT: Unit Under Test)
    UUT: FA
    PORT MAP (
        A = > a,
        B = > b,
        C_in = > c_in,
        S = > s,
        C_out = > c_out
    );

-- Define the process for the Test Bench
    process
    begin
        A < = '0';
        B < = '0';
        C_in < = '0';
        wait for 100ns;
        A < = '1';
        wait for 100ns;
        A < = '0';
        B < = '1';
        wait for 100ns;
    end process;
end Behavioral;
```


