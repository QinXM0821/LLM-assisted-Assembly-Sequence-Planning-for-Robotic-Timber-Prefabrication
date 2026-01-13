# LLM-Agent-assisted-Task-Sequencing-for-Robotic-Structural-Assembly

## Research Gaps
In conventional robotic structural assembly workflows, as illustrated in Fig. 1, robot engineers are required to manually extract structural geometric information from design models, plan assembly sequences, and program the robot accordingly. However, in construction-oriented structural assembly scenarios, structures often vary in size, geometry, and layout patterns. Even minor design changes require robot engineers to repeat the entire workflow of geometric interpretation, task sequencing, and robot programming. This strong reliance on manual intervention significantly reduces the level of automation and leads to low efficiency in robotic structural assembly systems. 

<img src="Figures/conventional workflow.png" width="60%">

Figure 1. Conventional Workflow of Robotic Structural Assembly

In practical applications of robotic structural assembly, the final insertion of a component is typically executed by moving it from an approach position to its final target via a small linear translation. In this study, this translational vector is referred to as the slide-in direction. A well-defined slide-in direction is essential for collision-free assembly and therefore serves as a critical constraint in assembly sequence planning. As shown in Fig. 2(a), under a given assembly sequence, zero-clearance insertion of member 3 will lead to collision. Fig. 2(b) shows a different assembly sequence. This slide-in direction is safe and the corresponding assembly sequence is collision-free.

<img src="Figures/zero clearance insertion and slide in direction.png" width="40%">

Figure 2. Illustration of zero clearance insertion under incorrect sequence (a) and safe slide-in direction under correct sequence (b)

## Proposed LLM-driven Task Sequencing Method for Robotic Structural Assembly

<img src="Figures/LLM-assisted workflow.png" width="40%">


<img src="Figures/technique pipeline.png" width="40%">

