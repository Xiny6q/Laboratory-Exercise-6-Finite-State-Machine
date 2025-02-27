Download link :https://programming.engineering/product/laboratory-exercise-6-finite-state-machines-2/


# Laboratory-Exercise-6-Finite-State-Machine
Laboratory Exercise 6 Finite State Machine
The purpose of this lab is to:

learn how to write Finite State Machines (FSMs) in Verilog

learn how to use an FSM to control the sequencing of logical operations.

Work ow

For each part of the lab you should begin by writing and testing Verilog code and compiling it with Quartus. You should be prepared to show schematics, Verilog, and simulations to your TA, if requested. You must simulate your circuit with ModelSim using reasonable test vectors written in the format used in Lab 2 for the simulation les.

For Parts I and II, you are given a lot of code in the form of templates. This provides you with some good examples to use as models for your own code, but they also allow you to focus on the key elements relevant to building the required control sequences without having to write the supporting infrastructure. The amount of code you need to write in these parts is relatively small. Most likely, more of your time will be spent understanding all of the code presented to you and how to modify it. Learning to read other code is also a good thing to learn as you will do that a lot in industry. A good way to start is to reverse engineer a schematic from the code.

Test Plans: Getting More from Your Simulations

As the complexity of the circuits you build increases, the importance of simulation also increases. Thinking about how you are going to test your circuit is an integral part of doing a design. Without thinking about testing as you do your design, it is possible that you will build a circuit that you cannot properly test, or it will be extremely di cult to test. While the circuits you build for these lab exercises are still basic and straightforward, the goal at this time is for you to start thinking more about how you will do your testing.

Here are the steps you should do. Be prepared to discuss your Test Plans with your TA if requested.


Write out a Test Plan. The Test Plan lists everything you want to test to show your circuit is working, how you are going to do the test, and the correct result to expect. For example, if you are going to test a 4-bit adder, your test plan might look like:

Test carry in = 0 Add 0000 + 1111 with carry in of 0. Expected output = 1111, carry out = 0;

Test carry in = 1 Add 0000 + 1111 with carry in of 1. Expected output = 0000, carry out = 1;

And so on Other tests . . .

As you execute each test, you can document the behaviour of a test by annotating a screen capture of the waveforms to show the inputs and the outputs.

If something is not working, indicate the inputs, but then show in the simulation where it is not doing what you expect. This is important if you want to discuss the issue with a TA, or your manager if you are working.

The goal of doing the above is to make you think more about the purpose of simulation and how to get the most out of doing simulation.

Part I

In this part you will implement a basic nite state machine (FSM) in Verilog. All FSMs you write in Verilog should follow this structure or you can get into lots of trouble.

We wish to implement a FSM that recognizes two speci c sequences of applied input symbols, namely four consecutive 1s or the sequence 1101. There is an input w and an output z. Whenever w = 1 for four consecutive clock pulses, or when the sequence 1101 appears on w across four consecutive clock pulses, the value of z has to be 1; otherwise, z = 0. Overlapping sequences are allowed, so that if w = 1 for ve consecutive clock pulses the output z will be equal to 1 after the fourth and fth pulses. Figure 1 illustrates the required relationship between w and z for an example input sequence. A state diagram for this FSM is shown in Figure 2.

Figure 3 shows a partial Verilog le for the required state machine. It is the template code in part1_template.v that you will need to complete for this part. Study and understand this code as it provides a model for how to clearly describe a nite state machine that will both simulate and synthesize properly.

Note that the top-level module of the template has the following signature description:

module part1(Clock, Resetn, w, z, CurState);


!”#$%&

‘&

(&

Figure 1: Required timing for the output z.

This circuit uses an active-low synchronous reset called Resetn. The input of the sequence detector is w and the output is z. The current state of the FSM is output with CurState.

3.1 What to Do

Perform the following steps:

Begin with the template code provided online in part1_template.v.

Complete the state table and the output logic. Look for the question marks (?).

Simulate your part1 module with ModelSim to satisfy yourself that your circuit is working. Be prepared to justify that your test cases are enough to give con dence that your circuit is working. When you are satis ed with your simulations, you can submit to the Automarker.

Create a new Quartus project for your circuit. You will need a top-level module to make connections from the instantiation of your part1 module to the switches, pushbutton and LEDs of the DE1-SoC board. The connections used are shown in Table 1.

By using the pushbutton KEY[0] as the Clock input, you will be able to manually create clock pulses so that you can observe the transitions through the states.

Simulate your new circuit with the DE1-SoC connections with ModelSim to ensure that you have done the connections correctly.

Compile the project to generate a bitstream to make sure your code can be synthesized.

Download the compiled circuit into the FPGA chip. Test the functionality of the circuit by toggling the various inputs and observing the outputs.

3


,-.-/$

 01#$

!”#$

01)$

01#$

%”#$

01)$

01#$

01#$

&”#$

01)$

01#$

01)$

01#$

‘”

01#$

*”#$

#$

01)$ 01)$

+”

(“)$ )$

01)$

Figure 2: A state diagram for the FSM.


Figure 3: Verilog code for the FSM.

part1 Port Name

Direction

DE1-SoC Pin Name

Clock

Input

KEY[0]

Resetn

Input

SW[0]

w

Input

SW[1]

z

Output

LEDR[9]

CurState

Output

LEDR[3:0]

Table 1: Module part1 mapping to DE1-SoC pin names

Part II

Please note that there is a lot written here. Most of it is explanation and guidance, so please read carefully.

A nite state machine (FSM) on its own, like the one built in Part I, cannot do much and is not what you usually do with an FSM except to teach how to build an FSM. The primary use of FSMs is to act as the main control for digital systems that require functions like sequencing or responding in di erent ways to some stimuli. This part will show you how to use an FSM to do something more interesting than recognizing a pattern of bits. To help you focus on the FSM design, you are provided an entire datapath and example FSM that performs a computation. Your task will be to change the FSM to do a di erent computation.

Most non-trivial digital circuits can be separated into two main functions. One is the datapath where the data ows and the other is the control path that manipulates the signals in the datapath to control the operations performed and how the data ows through the datapath. In previous labs, you learned how to construct a simple ALU, which is a common datapath component. In Part I of this lab you have already constructed a simple FSM, which is the most common component used to implement a control path. Now you will see how to implement an FSM to control a datapath so that a useful operation is performed. This is an important step towards building a microprocessor as well as any other computing circuit.

In this part, you are given a datapath and an FSM that controls the datapath so that it computes A2 + C. Observe that the example code loads four values, despite using only two of the values in the computed result. This will give you a head start because you will need all four values loaded for the task you will do. Also, often when you inherit code, there might be parts of it that may not be used or be relevant because of how the code evolved. It is up to you to gure out what is relevant for the task you are given. Feel free to modify the code you are given, if you feel it is necessary to achieve the nal result required. Note that the provided template code has an additional ResultValid port. While it is not used in the sample design, it is required for compatibility with the auto-marker. It’s expected functionality will be described later in the handout.

Using the given datapath, you are required to implement an FSM that controls the datapath


Table 2: Register contents and control signals for computing A2 + C


step in this part of the lab. You will not need to write much Verilog, but you will need to fully understand the provided Verilog to make your modi cations and build a simulation script. Here’s how to read the code and what to look for:

The part2 module has two modules called datapath and control, which is the explicit partitioning of the control logic and the datapath logic. The code has been organized this way to make it very clear where the control logic is written versus the datapath logic. Most important is that you can clearly see what control signals coming from the control logic connect to the datapath.

Read the datapath code and identify all the components shown in Figure 4 when looking at the datapath module.

Identify all the control signals shown in Figure 4 and where they are generated in the control module. You should see that all the control signals are generated by the FSM in the control module.

Find the case statement that de nes the state table for the FSM. Also, nd the case statement that de nes the outputs that are set in each state. It is important that the state transitions and the output logic be in separate case statements. This makes it explicitly clear what logic is being generated from your Verilog. If you start mixing the state transitions with the outputs, the code becomes very challenging to understand, with the risk that the Verilog compilation will also do something you did not intend.

By looking at the state transitions and what outputs are set in each state, observe how the loading of the four registers is done and how the sequence of the loading is done according the speci cation. How does the FSM handle the setting and releasing of the input Go signal? Ultimately, the Go signal will be connected to a pushbutton on the DE1-SoC board, which is a mechanical device that runs in human time. If the clock is running at 50 MHz how many clock pulses might there be during the time that the Go signal is set high by a human? How does the FSM account for human time? Take this into account when doing your simulations.

You may nd it helpful to draw a state diagram for the FSM because you can then just modify it for Step 5.

Which states do the actual computation? Identify those states and nd the se-quence of control signals. Observe how the computation is done by using Figure 4 and seeing how that sequence of control signals does the required operation. Can you gure out what is being computed by looking at the FSM?

Table 2 shows a table of the sequence of register contents and control signals to do the computation of A2 + C. You should be able to see the control signals changing the same way in part2 template.v. in the controller state machine output logic. See the supplemental notes NotesForControlContentSignalsLab6.pdf for a further explanation of Table 2.


Simulate part2 template.v using ModelSim starting with the values in Table 5 and some other values of your choosing. Remember that the registers are only eight bits wide, so make sure all values will t in the correct range.

Observe the state register of the FSM so that you can watch the state transitions. This is usually helpful when things are not working as getting the control sequences correct is usually the hardest part of a design.

You should be able to use the same simulation script after you modify the circuit to do the new computation, except you may need to account for there being more operations being performed, so more simulation time will be needed.

Before making changes to any design, it is always good to start from a known state, which in this case is a working example and a working simulation script. You are pro-vided part2 template.v, which is working code and you will build the corresponding simulation script. After this step, you will have both before you start making changes to the code. If something goes wrong, go back to when the design last worked and gure out what change caused the error.

Following the model of part2 template.v and Table 5, build the table for computing Ax2 + Bx + C.

Draw a state diagram for your controller starting with the register load states provided in part2 template.v.

Modify part2 template.v to implement your controller. You should only need to modify the control module.

Simulate your circuit with ModelSim for a variety of inputs. Since you are modifying the controller it is recommended that you simulate the controller module by itself rst. Only when you are satis ed that it is working individually should you add it into the full design for a full system simulation. Why is this approach better? (Hint: Consider the case when your design has 20 di erent modules.) When you are satis ed with your simulations, you can submit to the Automarker,

Create a new Quartus project for your circuit. You will need a top-level module to make connections from the instantiation of your part2 module to the switches, pushbuttons, LEDs and HEX displays of the DE1-SoC board. The connections used are shown in Table 3.

Remember that the pushbuttons on the DE1-SoC board are active low.

Simulate your new circuit with the DE1-SoC connections with ModelSim to ensure that you have done the connections correctly.

To examine the circuit produced by Quartus open the RTL Viewer tool (Tools>Netlist Viewers > RTL Viewer). Find (on the left panel) and double-click on the box shown in the circuit that represents the nite state machine, and determine whether the state

part2 Port Name

Direction

DE1-SoC Pin Name

Resetn

Input

KEY[0]

DataIn

Input

SW[7:0]

Go

Input

KEY[1]

DataResult[7:0]

Output

LEDR[7:0]

ResultValid

Output

LEDR[8]

DataResult[3:0]

Output

HEX0

DataResult[7:4]

Output

HEX1

Table 3: Module part2 mapping to DE1-SoC pin names

diagram that it shows properly corresponds to the one you have drawn. To see the state codes used for your FSM, open the Compilation Report, select the Analysis and Synthesis section of the report, and click on State Machines.

The state codes after synthesis may be di erent from what you originally speci ed. This is because the tool may have found a way to optimize the logic better by choosing a di erent state assignment. If you really need to use your original state assignment, there is a setting to keep it.

Compile the project to generate a bitstream to make sure your code can be synthesized.

Download the compiled circuit into the FPGA chip. Test the functionality of the circuit.

Part III (Optional – For Bonus Marks)

In this part, you will have to build a complete datapath and corresponding state machine. We recommend that you still have two separate modules for the datapath and the control just to help you understand the separation of what is in a datapath and what is in a controller. In Part II, the control logic just generated control signals that were inputs to the datapath. In this part, you will see that there is a signal that needs to go from the datapath into the controller so make sure you account for it accordingly.

Division in hardware is the most complex of the four basic arithmetic operations. Add, subtract and multiply are much easier to build in hardware. For this part, you will be designing a 4-bit restoring divider using a nite state machine.

Figure 5 shows an example of how the restoring divider works. This mimics what you do when you do long division by hand. In this speci c example, number 7 (Dividend) is divided by number 3 (Divisor). The restoring divider starts with Register A set to 0. The Dividend is shifted left and the bit shifted out of the left most bit of the Dividend (called the most


signi cant bit or MSB) is shifted into the least signi cant bit (LSB) of Register A as shown in Figure 6.

The Divisor is then subtracted from Register A. If the MSB of Register A is a 1, then we restore Register A back to its original value by adding the Divisor back to Register A, and set the LSB of the Dividend to 0. Else, we do not perform the restoring addition and immediately set the LSB of the Dividend to 1. You may use the subtract ( ) and addition (+) operators in Verilog to perform the subtraction and addition. The 1 in the MSB of Register A means that the value in Register A after the subtraction is a negative number, meaning that the Divisor is larger than the original value in Register A. That is why Register A is restored by adding back the Divisor.

This sequence of steps is performed until all the bits of the Dividend have been shifted out. Once the process is complete, the new value of the Dividend register is the Quotient, and Register A will hold the value of the Remainder.

5.1 What to Do

The top-level module of your design should have the following declaration:

module part3(Clock, Resetn, Go, Divisor, Dividend, Quotient, Remainder, ResultValid);

Divisor and Dividend are the input values for the division and Quotient and Remainder are the outputs of the division with the ResultValid port being used to indicate that the output is valid, similar to Part II. When Divisor and Dividend are both valid, Go is set and released in order to simultaneously load both input values into registers.

When Go is released, input values should be loaded into registers in 1 clock cycle. Your circuit should then perform the division in exactly 4 cycles i.e. 1 cycle per bit of the Dividend. ResultValid should remain high and Quotient and Remainder should maintain their nal values until new input is provided, i.e., Go is set to 1. Resetn is a synchronous active low reset.

Perform the following steps.

Draw a schematic for the datapath of your circuit. It will be similar to Figure 6. You should show how you will initialize the registers, where the outputs are taken, and include all the control signals that you require. Do not forget the clock and resets.

Draw the state diagram to control your datapath. Check it by hand simulating the example shown in Figure 5. Hand simulation just means to work through the steps

                                           

Figure 5: An example showing how the restoring divider works.


Figure 6: Block diagram of restoring divider.

using your schematic and state diagram to check whether you can do the required operations before going through the e ort of setting up the simulator. This may not catch all bugs, but it is a good step to make sure you have a design that has a chance of working.

Draw the schematic for your controller module.

Draw the top-level schematic showing how the datapath and controller are connected as well as the inputs and outputs to your top-level circuit.

Write the Verilog code that realizes your circuit. Structure your code in the same way as you were shown in Part II.

Simulate your circuit with ModelSim for a variety of input settings, ensuring the output waveforms are correct. Start with Figure 5 as an example because it shows you all the steps with the values that should be in the registers at each step.

Create a new Quartus project for your circuit. You will need a top-level module to make connections from the instantiation of your part3 module to the switches, pushbuttons, LEDs and HEX displays of the DE1-SoC board. The connections used are shown in Table 4.

Remember that the pushbuttons on the DE1-SoC board are active low.

part3 Port Name

Direction

DE1-SoC Pin Name

Clock

Input

Clock

50

Resetn

Input

KEY[0]

Divisor

Input

SW[3:0]

Dividend

Input

SW[7:4]

Go

Input

KEY[1]

Divisor

Output

HEX0

Dividend

Output

HEX2

Quotient

Output

HEX4

Remainder

Output

HEX5

Quotient

Output

LEDR[3:0]

ResultValid

Output

LEDR[4]

Table 4: Module part3 mapping to DE1-SoC pin names

Simulate your new circuit with the DE1-SoC connections with ModelSim to ensure that you have done the connections correctly.

Compile the project to generate a bitstream to make sure your code can be synthesized.

Download the compiled circuit into the FPGA chip. Test the functionality of the circuit by toggling the various inputs and observing the outputs.

Submission

6.1 Part I and Part II

Please submit part1.v and part2.v. For module names and port names, please follow the provided templates.

6.2 Part III

You may optionally submit part3.v for bonus marks. For Part III, you need to submit a le named part3.v with the following module in it:

module part3(Clock, Resetn, Go, Divisor, Dividend, Quotient, Remainder, ResultValid);

