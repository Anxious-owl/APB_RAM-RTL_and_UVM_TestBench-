# APB_RAM-RTL_and_UVM_TestBench-
Developed and verified an APB RAM using SystemVerilog/UVM, with protocol assertions, self-checking scoreboard, constrained-random sequences, and 100% functional coverage. Demonstrated robust APB handling and coverage-driven DV methodology.

# &nbsp;			**APB RAM UVM Verification – README**



A complete SystemVerilog UVM verification environment for an APB (AMBA 3) RAM slave, including protocol assertions (SVA), constrained‑random and directed sequences, scoreboard, functional coverage with closure, and simulation scripts. The repository also includes waveform snapshots and coverage reports.



----------------------------------------------------------------------------------------------------------------------------------------------------------



#### Features:



* APB‑spec compliant RAM DUT (Idle → Setup → Access) with zero‑wait default behavior
* Address range checking with PSLVERR for out‑of‑bounds accesses
* UVM agent (driver, sequencer, monitor), scoreboard, environment, and tests
* Constrained‑random and directed sequences: write, read, write→read, error, reset, back‑to‑back transfers
* Protocol SVAs: PSEL→PENABLE ordering, signal stability until PREADY, PENABLE held until PREADY, reset behavior, PSLVERR timing
* Functional coverage: op type, address bins, data ranges, error paths, cross coverage; 100% goal achieved
* Waveform example (added): apb transfer timing showing setup/access cycles



----------------------------------------------------------------------------------------------------------------------------------------------------------



#### Files Description:



* apb\_top.sv – top with APB RAM slave(RTL) and IF
  
* apb\_tb.sv - contains:

&nbsp;	- transaction class(sequence item with constraints)

&nbsp;	- sequences

&nbsp;		write\_data, write\_err, 	read\_data, read\_err, reset\_dut, writeb\_readb, write\_read

&nbsp;	- driver

&nbsp;	- monitor (also contains covergroups and coverpoints)

&nbsp;	- sequencer

&nbsp;	- agent, env, scoreboard

&nbsp;	- Test



* apb\_assertions.sv – Documented assertions that is applied using bind keyword
  
* output/

&nbsp;	- Functional Coverage report (coverage\_report.pdf)

&nbsp;	- UVM output (Tb\_output.pdf)

&nbsp;	- Example Waveform (waveform.png)



----------------------------------------------------------------------------------------------------------------------------------------------------------



#### Typical Results:



* All tests pass with zero UVM\_ERROR/UVM\_FATAL.



* Functional coverage closure at 100% across defined covergroups.



* Waveform shows correct Idle→Setup→Access sequencing, single‑cycle PREADY, and PSLVERR asserted on invalid addresses.



* Example screenshot included: docs/waveform.jpg.



----------------------------------------------------------------------------------------------------------------------------------------------------------

