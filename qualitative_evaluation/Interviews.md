# Interview Transcript

_Interview type_: [Qualitative Surveys (Interview Studies)](https://github.com/acmsigsoft/EmpiricalStandards/blob/master/docs/standards/QualitativeSurveys.md)

_Duration_: ~60 minutes per participant

_Participant_: Robotics researcher with experience in ROS
_Recording_: transcript reconstructed from available audio and interviewer notes

## Introduction and Context

The interviewer introduced the goal of the session, namely to evaluate a methodology for robotic mission analysis combining process mining and visual analytics. The main concepts of mission analysis, process mining, and visual analytics were briefly explained.

The interviewer described the overall methodology, composed of a data preparation phase and an analysis phase. In the data preparation phase, activity tags are integrated into the robotic codebase, mission executions are recorded (e.g., using ROS bags), and the collected data are processed into a structured event log. In the analysis phase, a process model is discovered from the event log and enriched with contextual perspectives such as spatial information, energy consumption, and communication.

## Interview to Participant_1

### Q1: How would you assess the effort required to define and integrate activity tags for a robotic mission? In which scenarios do you think tagging would be feasible, and in which might it become impractical?

In my opinion, the effort required to integrate activity tags into a ROS-based system is not particularly high. From what I have seen, this mainly involves adding a small number of lines of code, and I think this could even be modularized to reduce duplication.

The main difficulty I see is not in starting an activity, but in tagging when an activity has actually finished. In many robotic systems, it is not always clear when an action ends, especially if one activity is interrupted by another or if activities overlap. In these cases, explicitly signaling the completion of an activity can become challenging.

That said, I think there are possible workarounds. For example, when a new activity starts, the previous one could be implicitly considered completed. So while tagging is generally feasible, it may become less practical in scenarios where activity boundaries are highly dynamic or ambiguous.

### Based on the process model, can you describe what happened during the mission?
Yes, based on the process model, I was able to understand what happened during the mission. The model clearly showed the sequence of activities and how they followed one another across execution runs. I could see which activities were repeated and how the mission progressed over time.

I found the representation particularly useful for understanding the sequential nature of the mission. This is important because, depending on how the system is implemented, the developer’s mental model does not always explicitly represent sequencing, for example, when using behavior trees or similar control structures.

One aspect that was slightly less clear to me concerned overlapping resource usage, especially the interactions between the drone and the tractors. I think this issue would become even more evident in scenarios involving homogeneous robots that perform the same activities.

### Did any of these perspectives influence or refine your understanding of the mission? If so, how?
Yes, the additional perspectives clearly refined my understanding of the mission.

The spatial perspective was useful to see where most of the actions took place and where the robots tended to concentrate during the mission. This helped me connect activities with specific areas of the environment.

The energy perspective was helpful for identifying which tasks were more energy-consuming. This is important because energy-intensive activities can potentially lead to mission failures or require replanning.

The communication perspective was particularly relevant in the multi-robot setting. Seeing lost messages immediately stood out as a potential problem, since even a single missed message can indicate coordination issues or failures in the system.

Overall, combining these perspectives with the process model helped me understand not only what happened, but also why certain behaviors occurred.