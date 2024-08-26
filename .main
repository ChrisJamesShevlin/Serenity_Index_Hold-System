// Definition of code parameters
DEFPARAM CumulateOrders = False // Cumulating positions deactivated

// Conditions to enter long positions
c1 = (close > DHigh(1)[1])
indicator1 = CALL "Serenity Ratio"[20, 200](close)
c2 = (indicator1 > 20)

IF c1 AND c2 THEN
BUY 1 PERPOINT AT MARKET
ENDIF

// Stops and targets
SET TARGET pPROFIT 5
