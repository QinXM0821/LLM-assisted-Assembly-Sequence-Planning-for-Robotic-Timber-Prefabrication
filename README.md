# LLM-assisted-Assembly-Sequence-Planning-for-Robotic-Timber-Prefabrication

## Research Problems
In conventional robotic structural assembly workflows, as illustrated in Fig. 1, robot engineers are required to manually extract structural geometric information from design models, plan assembly sequences, and program the robot accordingly. However, in construction-oriented structural assembly scenarios, structures often vary in size, geometry, and layout patterns. Even minor design changes require robot engineers to repeat the entire workflow of geometric interpretation, task sequencing, and robot programming. This strong reliance on manual intervention significantly reduces the level of automation and leads to low efficiency in robotic structural assembly applications. 

<img src="Figures for README/conventional workflow.png" width="60%">

Figure 1. Conventional Workflow of Robotic Structural Assembly

During manual timber assembly, workers can adjust a member’s position during placement to accommodate limited clearance. In robotic placement, unintended contact between the member being placed and previously assembled members may displace those members and cause assembly failure. This motivates the avoidance of geometric interference during insertion. In this study, the final insertion of a member is modelled as a short linear translation from an approach position to its target position. This movement is termed the slide-in motion, and its direction is termed the slide-in direc-tion. Zero-clearance insertion refers to a motion that requires the member to slide along a contacting surface of a previously assembled member without clearance. Such sliding may cause interference when dimensional and positioning errors are present. The member-level insertion criteria therefore exclude both geometric overlap and zero-clearance sliding contact during the motion, while permitting intended contact at the final assembly position.

The free-edge rule requires each member to have at least one side edge and one end edge fully exposed. Its slide-in motion must avoid geometric overlap and zero-clearance sliding with previously installed members, while allowing intended contact at the final position. Fig. 3 illustrates how assembly sequence and slide-in direction violate or satisfy the free-edge rule. In Fig. 3(a), installing the two vertical members before horizontal member 3 constrains both ends of it, violating the free-edge rule. Because it will cause zero-clearance insertion during the slide-in motion. In Fig. 3(b), candidate member 3 has exposed side and end edges, but its slide-in direction causes zero-clearance sliding against member 2. Fig. 3(c) retains the same partial assembly and changes the slide-in direction. Member 3 approaches from the right and moves leftward, allowing contact at the target position without the zero-clearance sliding. These examples demonstrate why both assembly sequence and slide-in direction must be considered to generate a valid assembly plan.

<img src="Figures for README/zero clearance insertion and slide in direction.png" width="60%">

Figure 2. Illustration of zero clearance insertion under inproper sequence (a) and safe slide-in direction under proper sequence (b)

## Proposed LLM-driven Task Sequencing Method for Robotic Structural Assembly

To address the aforementioned problems in robotic structural assembly, we proposed an LLM-driven task sequencing method for robotic structural assembly. As shown in Fig. 3, the proposed approach constructs LLM agents that take structured geometric information extracted from BIM design models as input and generate assembly sequence plans as output. 

<img src="Figures for README/LLM-assisted workflow.png" width="60%">

Figure 3. Proposed Workflow of LLM-assisted Robotic Structural Assembly

The technical pipeline of the proposed method is shown in Fig. 4. This pipeline is an automated workflow designed to transform structural design models into robot-executable assembly sequence plans. 

<img src="Figures for README/technique pipeline.png" width="40%">

Figure 4. Technical Pipeline of the proposed LLM-driven Task Sequencing Method for Robotic Structural Assembly

A generator-corrector architecture is adopted to improve reasoning reliability. As illustrated in Fig. 5, the proposed agent architecture consists of two core modules: a Generator and a Corrector. The agent has abilities to identify structural component roles, determine assembly sequences, and infer feasible slide-in directions. 

In this study, we developed customized prompts for the Generator and Corrector modules. The Generator focuses on identifying member roles, defining assembly sequence rules, and producing a preliminary assembly sequence plan. In contrast, the Corrector emphasizes error checking, sequence repair, and additionally determines the slide-in direction for each structural member.

The LLM Agent is powered by OpenAI ChatGPT-o3 model via Responses API (standard processing tier), with the default temperature setting (1.0) and high reasoning effort enabled. 

<img src="Figures for README/generator-corrector architecture.png" width="40%">

Figure 5. Generator-Corrector Architecture of the LLM Agent

Timber structural models were built using Autodesk Revit 2025, including six wall-frame models. The complexity of the wall frame structures increased by enlarging structure dimensions, increasing the number of members, and introducing door-opening and window-opening. Fig. 6 shows the examples of wall 3 and wall 6.

<img src="Figures for README/wall frame structure model 3.png" width="20%"> <img src="Figures for README/wall frame structure model 6.png" width="35%"> 

Figure 6. Examples of design revit model wall 3 (left) and wall 6 (right)

Each Revit model was first converted to IFC4 format and then parsed by IfcOpenShell to extract geometric information in JSON format as shown in Fig. 7. The extracted geometric information includes the member identifier, overall dimensions (length, width, and depth), centroid coordinates, orientation vectors, and end-point positions.

<img src="Figures for README/extracted geo info JSON file from IFC.png" width="50%">

Figure 7. Example of Geometry Information in JSON Format

The Generator processes the input file *geometric_information.json* to produce two outputs: a *reasoning_process.txt* (Fig. 8) and a *preliminary_sequence_plan.json* (Fig. 9). The latter is enriched with additional attributes, specifically *step_no* and *member_role*.

<img src="Figures for README/generator output reasoning process TXT.png" width="70%">

Figure 8. Example of **Generator Output** of Reasoning Process in TXT Format

<img src="Figures for README/generator output sequence JSON.png" width="50%">

Figure 9. Example of **Generator Output** of Sequence in JSON Format

Subsequently, the Corrector processes the *preliminary_sequence_plan.json* and outputs a *reasoning_process.txt* (Fig. 10) and a *revised_sequence_plan.json* (Fig. 11). This revised plan is enriched with an additional attribute, specifically *slide_dir*.

<img src="Figures for README/corrector output reasoning process TXT 01.png" width="70%">
<img src="Figures for README/corrector output reasoning process TXT 02.png" width="70%">
<img src="Figures for README/corrector output reasoning process TXT 03.png" width="70%">

Figure 10. Example of **Corrector Output** of Reasoning Process in TXT Format

<img src="Figures for README/corrector output sequence JSON.png" width="50%">

Figure 11. Example of **Corrector Output** of Sequence in JSON Format

To further validate the accuracy of the LLM Agent output, a human-in-the-loop verification step is appended after the Corrector. Specifically, the raw text-based sequence plan is automatically rendered into a visual GIF animation accompanied by annotations, thereby enabling rapid and intuitive human verification. Figs. 12 and 13 illustrate the GIF animations of success and failure cases for Wall 3 and Wall 6, respectively.

<img src="Figures for README/success_wall3_assembly.gif" width="48%"> <img src="Figures for README/fail_wall3_assembly.gif" width="48%"> 

Figure 12. GIF Animations of a Success Case (left) and a Failure Case (right) of Wall 3

<img src="Figures for README/success_wall6_assembly.gif" width="48%"> <img src="Figures for README/fail_wall6_assembly.gif" width="48%"> 

Figure 13. GIF Animations of a Success Case (left) and a Failure Case (right) of Wall 6



<img src="Figures for README/revseq_wall6_r7_assembly.gif" width="60%">

Figure 14. An example of failed sequence plan for Wall 6

<img src="Figures for README/revseq_wall6_r2_assembly.gif" width="60%">

Figure 15. An example of success sequence but failed slide-in direction plan for Wall 6

<img src="Figures for README/revseq_wall6_r5_assembly.gif" width="60%">

Figure 16. An example of success sequence and slide-in direction plan for Wall 6
