<h4>Piping System Calculation Workflow</h4>

Purpose of this "exercise" is to show how to use scripts from "Piping System Calculation", located [here](https://github.com/ddjokic/Piping-System-Calculation). Hence, you will need those scripts.</p>

<h5>Equation</h5>
In this calculation Headloss-Flow equation are used in following format:</p>
**H=AQ^2+B** </p>
"A" and "B" in above equation are calculated based on friction loss in pipe and local losses (fittings) and height difference between "entrance and exit" hights of pipe. Script to calculate pipeline losses, based on Darcy-Weisbach friction loss formula can be found [here](https://github.com/ddjokic/Headloss-in-Pipe), together with some useful data, but any kind of spreadsheet, other script or rule of thumb calculation can do the job.</p>

<h5>Background</h5>
Serial lines - flow through all serial lines is equal; equivalent line should be composed by summarising headlosses for each pipe for equal flow.</p>
Paralel pipes - flow is divided through pipes; equivalent line equation can be derived as sum of headlosses for equal flow.</p>

<h5>Description of system</h5>
System consist of Suction Line "0P", Pump "P" and 3 pipes on pressure side of the pump - refer to "Sketch 1". Pipelines "13" and "12" are parallel connected to each other, as flow is divided between them in node "2". Pipeline "P1" is in serial connection with "12" and "13" as flow going through that line is equal to sum of other two lines: </p>
**QP1=Q12+Q13** </p>

<h5>Step 1 - Pipeline loss equation</h5>
Input and complete results for each pipeline are given in text file "pipelinetag-results.txt", located in this folder. Headlosses:</p>
* pipeline "0P": "Headloss= 0.000065Q^2+ 0.000000Q +26.000000"</p>
* pipeline "P1": "Headloss= 0.000068Q^2+ 0.000000Q +-1.000000"</p>
* pipeline "12": "Headloss= 0.000414Q^2+ 0.000000Q +-0.750000"</p>
* pipeline "13": "Headloss= 0.001444Q*^2+ 0.000000Q +1.000000",</p>
where Q is flow in cubic meters per hour [cum/h], as implemented here.</p>
Graphs for Headloss, Reynold Number and Velocity, as function of Flow for each pipeline can be found in png format in this folder. Graphs, as output of script "dwheadloss.txt" are given as dxf-files as well, as method is graphical in nature. You may opt to bring above equations to your CAD system of choice on different way.</p>

<h5>Step 2 - Getting pump equation</h5>
In catalogues pumps are given as Flow-Headloss curves or Flow-**Pressureloss** curves. In a case of later, look for which fluid Pressure loss is given. It is water, usually, but there are exemptions. </p>
To get pump **Flow-HEADloss** equation script "pump.m" may be used or any other way. Take care about consistency of the units.</p>
Pump used here:</p>
**H=-0.00021Q^2+120**
where H (headloss) is in [m] and Q (flow) in [cum/h].

<h5>Step 3 - Getting equivalent pump equation</h5>
Pump, before getting to delivery side, already used some "power" to overcome headlosses in suction line. To calculate remaining "power" or, rather, remaining delivery capability, script "eq_pump.m" should be used. </p>
Script is based on fact that pump and pipe headloss curves are, in a nutshell, of the same form, once one of them is multiplied by "-1" - refer to above equations. Script subtracts from pump curve suction line losses (suction losses) and presents them in pump curve format. </p>
In this case, result is:</p>
**"H=-0.000275Q^2+-0.000000*x.^1+94.000000"**

<h5>Step 4 - Piping system calculation </h5>
It is remaining to feed all bits in final script, "pipeloop.m". Let repeat above results, on one place:</p>
<h6>Parallel Pipes "12" and "13":</h6>
Pipe 12 - A = 0.000414; B = -0.75</p>
Pipe 13 - A = 0.001444; B  = 1.00</p>
<h6> Serial pipe "P1":</h6>
Pipe P1 - A = 0.000068; B = -1.00</p>
<h6>Equivalent Pump:</h6>
A = -0.000275; B = 94</p>
Not happiest chosen pump, but will do.
Upon execution of the script, two files are created, with same data: "Loop-TestLoop.dxf" and "Loop-TestLoop.png". Files in "pdf" format are posted by me, as illustration of workflow.

<h5>Step 5 - Getting results </h5>
Short playing in dxf file, which is given here as pdf file, gives working point of the pump and piping system as:</p>
Q=428.4 cum/h</p>
H=43.5 m, which can be confirmed by putting Q in pump equation.</p>
For serial pipe, line tagged as P1, it is obvious from graph: </p> 
Q=428.4 cum/h </p>
H=11.5 m </p>
From same graph, results for both parallel lines is:
Q=428.4 cum/h</p>
H=32.1 m</p>
Now, knowing that Flow in "sub-loop" of paralel lines is sum of flow through individual pipes from same pressure, going back to Q-H diagrams for individual pipes, given in "headloss-12.png" and "headloss-13.png", for Headloss 32.1 m:</p>
Pipeline "12": Q=281.7 cum/h</p>
Pipeline "13": Q=146.9 cum/h</p> 
**Quick check:** Q12+Q13=281.7+146.9=428.6 cum/h vs 428.4 cum/h, which flows through P1. Not bad.</p>
Velocity and Reynolds number can be calculated from result files for each pipeline, where those are given as function of Q.</p>
Total time spent: 22 min, mostly on measuring in dxf file.

<h5>Note</h5>
You, probably, noticed that HS curve and, hence, resulting curve is "bumped" nearby "0" headloss. That is result of composing serial curve: sum of headlosses for equal flow. Hence, when headloss is "0" in this case, one curve is in between "0.75" and "1" and the other above "1". Kind of insight new "brute force" algoritms can not give.</p>
Just for reference: in reality, I would change a pump, probably by using some similarity approach. Will add 10 minutes more to the workflow.

<h5>Edit Feb-2013</h5>
Added numerical solution, file, "pipeloopnum.m" into main repo. It is <u>not finished, yet and can give unexpected results</u>.

<h5>Disclaimer</h5>
There are no any kind of warranty that, in this case, above described method  will work. It is more legacy to "paper and pencil" engineering approach.</p>
<p></p>
© D. DJokic, 2013  