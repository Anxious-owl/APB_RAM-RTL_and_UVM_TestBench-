module apb_assertions(
	input logic pclk,
	input logic presetn,
	input logic [31:0] paddr,
	input logic pwrite,
	input logic [31:0] pwdata,
	input logic penable,
	input logic psel,
	input logic [31:0] prdata,
	input logic pslverr,
	input logic pready);
	
	//----------------------------------------------
    // 1) Handshake Consistency — PSEL -> PENABLE
    //----------------------------------------------
    // APB Spec: On a new transaction start (PSEL rises),
    // the first cycle is Setup phase with PENABLE low,
    // then next cycle is Access phase with PENABLE high.
	psel_penable : assert property(@(posedge pclk) $rose(psel) |-> !penable ##1 $rose(penable));


	//----------------------------------------------
    // 2) PSEL held until PREADY
    //----------------------------------------------
    // APB Spec: Once PSEL is asserted, it must remain asserted
    // through all wait states until transfer completes (PREADY high).
	psel_pready : assert property(@(posedge pclk) $rose(psel) |-> psel s_until pready);


	//----------------------------------------------
    // 3) PENABLE held until PREADY
    //----------------------------------------------
    // APB Spec: PENABLE must remain asserted during the Access phase
    // until the slave asserts PREADY to indicate transfer completion.
	penable_pready : assert property(@(posedge pclk) $rose(pready) |-> pready s_until pready);


	//----------------------------------------------
    // 4) Address & Write Data Stability
    //----------------------------------------------
    // APB Spec: During an active transfer, PADDR and PWDATA must stay
    // stable from Setup phase through Access phase until transfer ends.
    // This check starts on PSEL rising and enforces stability while PSEL is high.
	paddr_pwdata_stable : assert property(@(posedge pclk) $rose(psel) |=> ($stable(paddr) && $stable(pwdata)) throughout psel[*1:$]);


	//----------------------------------------------
    // 5) PREADY should be high for only 1 clock
    //----------------------------------------------
    // If the design spec says the slave responds in exactly 1 cycle (no wait states),
    // then PREADY must drop in the following cycle after it rises.
	pready_1clk : assert property(@(posedge pclk) $rose(pready) |=> $fell(pready));


	//----------------------------------------------
    // 6) PSLVERR timing relative to PREADY
    //----------------------------------------------
    // APB Spec: PSLVERR is only valid in the last cycle of transfer
    // (when PREADY is high). It should clear in the next cycle.
	pslverr_pready_check : assert property(@(posedge pclk) $rose(pslverr) |-> pready ##1 (!pslverr && !pready));


	//----------------------------------------------
    // 7) Reset behavior
    //----------------------------------------------
    // During an active-low reset (PRESETn = 0), no handshake or transfer
    // should be occurring. PSEL and PREADY must stay low throughout reset.
	reset_check : assert property(@(posedge pclk) $fell(presetn) |=> (!psel && !pready) throughout !presetn[*1:$]);

endmodule
