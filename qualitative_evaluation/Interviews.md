# Interview Transcript

_Interview type_: [Qualitative Surveys (Interview Studies)](https://github.com/acmsigsoft/EmpiricalStandards/blob/master/docs/standards/QualitativeSurveys.md)

_Duration_: ~60 minutes per participant

_Participant_: Robotics researcher with experience in ROS (see profiles.md for more info)

_Recording_: transcript reconstructed from available audio and interviewer notes

_Material Used_: `evaluation-slides.pdf` and `RoboTrace` tool

## Introduction and Context

The interviewer introduced the goal of the session, namely to evaluate a methodology for robotic mission analysis combining process mining and visual analytics. The main concepts of mission analysis, process mining, and visual analytics were briefly explained.

The interviewer described the overall methodology, composed of a data preparation phase and an analysis phase. In the data preparation phase, activity tags are integrated into the robotic codebase, mission executions are recorded (e.g., using ROS bags), and the collected data are processed into a structured event log. In the analysis phase, a process model is discovered from the event log and enriched with contextual perspectives such as spatial information, energy consumption, and communication.

## Participant_1: Interview

### EQ1: How would you assess the effort required to define and integrate activity tags for a robotic mission? In which scenarios do you think tagging would be feasible, and in which might it become impractical?

In my opinion, the effort required to integrate activity tags into a ROS-based system is not particularly high. From what I have seen, this mainly involves adding a small number of lines of code, and I think this could even be modularized to reduce duplication.

The main difficulty I see is not in starting an activity, but in tagging when an activity has actually finished. In many robotic systems, it is not always clear when an action ends, especially if one activity is interrupted by another or if activities overlap. In these cases, explicitly signaling the completion of an activity can become challenging.

That said, I think there are possible workarounds. For example, when a new activity starts, the previous one could be implicitly considered completed. So while tagging is generally feasible, it may become less practical in scenarios where activity boundaries are highly dynamic or ambiguous.

### EQ2: Based on the process model, can you describe what happened during the mission?
Yes, based on the process model, I was able to understand what happened during the mission. The model clearly showed the sequence of activities and how they followed one another across execution runs. I could see which activities were repeated and how the mission progressed over time.

I found the representation particularly useful for understanding the sequential nature of the mission. This is important because, depending on how the system is implemented, the developer’s mental model does not always explicitly represent sequencing, for example, when using behavior trees or similar control structures.

One aspect that was slightly less clear to me concerned overlapping resource usage, especially the interactions between the drone and the tractors. I think this issue would become even more evident in scenarios involving homogeneous robots that perform the same activities.

### EQ3: Did any of these perspectives influence or refine your understanding of the mission, and how did the combination of the process view and the visualizations affect your reasoning?
Yes, the additional perspectives clearly refined my understanding of the mission. In particular, they helped me identify where most issues and potential failures occurred.

The spatial perspective was useful to see where most of the actions took place and where the robots tended to concentrate during the mission. Being able to visually connect activities to specific spatial areas helped me reason about what the robot was doing and where.

The energy perspective was helpful for identifying which tasks were more energy-consuming. This is important because energy-intensive activities can potentially lead to mission failures or require replanning. This kind of information is not immediately visible from the process model alone, but it becomes much clearer when combined with visualizations that show battery discharge over time or per activity.

The communication perspective was particularly relevant in the multi-robot setting. Seeing lost messages immediately stood out as a potential problem, since even a single missed message can indicate coordination issues or failures in the system.

Overall, combining these perspectives with the process model helped me understand not only what happened, but also why certain behaviors occurred.

### EQ4: How would you normally analyze a mission like this using the tools or data you currently have?

Without an approach like this, I would typically analyze such a mission by manually inspecting logs. This would involve opening multiple terminals or log files to monitor different topics or components, either at runtime or after execution. In practice, this means checking printed logs, recorded messages on specific ROS topics, or raw rosbag data.

This type of analysis is time-consuming and fragmented, as information about activities, positions, battery levels, and communications is spread across different sources. While it is possible to retrieve all this information, it requires a lot of manual effort to correlate events and understand how different aspects of the mission relate to each other.

### EQ5: Do you think you could have reached similar conclusions using your usual tools or approaches? Why or why not?
Some conclusions could probably be reached creating ad-hoc scripts, but not as directly or efficiently. For example, spatial trajectories and battery levels can be analyzed by extracting and re-plotting the corresponding logs. However, this would not provide an explicit mapping between these data and the activities being executed.

In particular, mapping contextual information, such as position or battery level, to specific activities would require manual reconstruction. I would have to infer what the robot was doing at a given time by manually correlating timestamps across different logs. This makes the analysis more error-prone and significantly less immediate.

In contrast, having activities explicitly linked to contextual data allows this mapping to be done automatically. This makes it much easier to understand, for instance, what the robot was doing at a certain position or how the battery evolved during a specific task.

### Anything to add?
One additional feature that could be useful is a time-based visualization, such as a timeline or schedule view of the executed tasks. A representation where tasks are shown along a temporal axis—possibly color-coded by robot—could make it easier to reason about task duration and sequencing at a glance.

Such a timeline could also be combined with battery information, as battery consumption within a task may not be linear over time. While this information can already be derived from the existing visualizations and the process model, a dedicated time-based view could improve intuitiveness, especially for users primarily interested in temporal aspects. That said, I do not consider this a core missing feature, as the same information is already available through the current representations.