LIBRARY IEEE;
USE ieee.std_logic_1164.all;
-----------------------------------------------------
ENTITY adicion_negativa IS
  PORT (      swA   : IN  STD_LOGIC_VECTOR(3 DOWNTO 0);--Switchs para definir el numero A
              swB   : IN  STD_LOGIC_VECTOR(3 DOWNTO 0);--Switchs para definir el numero B
				  Cin1  : IN  STD_LOGIC;--Carry inicial de la resta de 4 bits
				  aux   : OUT STD_LOGIC;--Primer auxiliar de 1 bit
              resre : OUT STD_LOGIC_VECTOR(3 DOWNTO 0);--Resultado de la resta de 4 bits
              Cout1 : OUT STD_LOGIC);--Carry final de la resta de 4 bits

END ENTITY adicion_negativa;
------------------------------------------------------
ARCHITECTURE operation OF adicion_negativa IS

SIGNAL C1,C2,C3: STD_LOGIC;--Carrys internos que entran y salen de cada bloque de la operacion

BEGIN
 PROCESS(swA, swB)--Se inicializa el proceso con las variables
	               -- a usar para el IF
						
 BEGIN
 IF (swA > swB) THEN--Se realiza el IF para verificar el cumplimiento de la condicion
                    --Si es mayor el numero A al B, se realiza la resta normal
     
   C1       <= (Cin1 AND (swA(0) XNOR swB(0))) OR ((NOT swA(0))AND swB(0) AND (Cin1 OR(NOT Cin1)));--Se realizan las operaciones con las compuertas 
	                                                                                                --para cada uno de los 4 carrys y bits del resre
																																	--obteniendo la resta entre los numeros
   resre(0) <= (Cin1 AND (swA(0) XNOR swB(0))) OR ((NOT Cin1) AND ( swA(0) XOR swB(0)));

    C2      <= (C1  AND (swA(1) XNOR swB(1))) OR ((NOT swA(1))AND swB(1) AND (C1 OR(NOT C1)));
   resre(1) <= (C1 AND (swA(1) XNOR swB(1))) OR ((NOT C1) AND ( swA(1) XOR swB(1)));
   
    C3      <= (C2  AND (swA(2) XNOR swB(2))) OR ((NOT swA(2))AND swB(2) AND (C2 OR(NOT C2)));
   resre(2) <= (C2 AND (swA(2) XNOR swB(2))) OR ((NOT C2) AND ( swA(2) XOR swB(2)));
   
    Cout1   <= (C3  AND (swA(3) XNOR swB(3))) OR ((NOT swA(3))AND swB(3) AND (C3 OR(NOT C3)));
   resre(3) <= (C3 AND (swA(3) XNOR swB(3))) OR ((NOT C3) AND ( swA(3) XOR swB(3)));
	aux <= '0';--Valor designado al primer auxiliar debido a swA > swB

 ELSE--Si no se cumple la condicion, se realiza la resta invertida para
     --no tener problemas con numeros negativos y al final se le aplica el negativo
	
	C1       <= (Cin1 AND (swB(0) XNOR swA(0))) OR ((NOT swB(0))AND swA(0) AND (Cin1 OR(NOT Cin1)));
   resre(0) <= (Cin1 AND (swB(0) XNOR swA(0))) OR ((NOT Cin1) AND ( swB(0) XOR swA(0)));

    C2      <= (C1  AND (swB(1) XNOR swA(1))) OR ((NOT swB(1))AND swA(1) AND (C1 OR(NOT C1)));
   resre(1) <= (C1 AND (swB(1) XNOR swA(1))) OR ((NOT C1) AND ( swB(1) XOR swA(1)));
   
    C3      <= (C2  AND (swB(2) XNOR swA(2))) OR ((NOT swB(2))AND swA(2) AND (C2 OR(NOT C2)));
   resre(2) <= (C2 AND (swB(2) XNOR swA(2))) OR ((NOT C2) AND ( swB(2) XOR swA(2)));
   
    Cout1   <= (C3  AND (swB(3) XNOR swA(3))) OR ((NOT swB(3))AND swA(3) AND (C3 OR(NOT C3)));
   resre(3) <= (C3 AND (swB(3) XNOR swA(3))) OR ((NOT C3) AND ( swB(3) XOR swA(3)));
	aux <= '1';--Valor designado al primer auxiliar debido a swA < swB
	
	END IF;
	END PROCESS;
    
END ARCHITECTURE operation;
