# Interview Transcript

_Interview type_: [Qualitative Surveys (Interview Studies)](https://github.com/acmsigsoft/EmpiricalStandards/blob/master/docs/standards/QualitativeSurveys.md)

_Duration_: ~40-60 minutes per participant

_Participant_: Robotics researcher with experience in ROS (see `profiles.md` for more info)

_Recording_: transcript reconstructed from available audio and interviewer notes

_Material Used_: `evaluation-slides.pdf` and `RoboTrace` tool

## Introduction and Context

The interviewer introduced the goal of the session, namely to evaluate a methodology for robotic mission analysis combining process mining and visual analytics. The main concepts of mission analysis, process mining, and visual analytics were briefly explained.

The interviewer described the overall methodology, composed of a data preparation phase and an analysis phase. In the data preparation phase, activity tags are integrated into the robotic codebase, mission executions are recorded (using ROS bags), and the collected data are processed into a structured event log. In the analysis phase, a process model is discovered from the event log and enriched with contextual perspectives such as spatial information, energy consumption, and communication which drives the reasoning step.

## Participant_1: Interview

### EQ1: How would you assess the effort required to define and integrate activity tags into a robotic mission? In which scenarios do you think tagging would be feasible, and in which might it become impractical?

In my opinion, the effort required to integrate activity tags into a ROS-based system is not particularly high. From what I have seen, this mainly involves adding a small number of lines of code, and I think this could even be modularized to reduce duplication.

The main difficulty I see is not in starting an activity, but in tagging when an activity has actually finished. In many robotic systems, it is not always clear when an action ends, especially if one activity is interrupted by another or if activities overlap. In these cases, explicitly signaling the completion of an activity can become challenging.

That said, I think there are possible workarounds. For example, when a new activity starts, the previous one could be implicitly considered completed. So while tagging is generally feasible, it may become less practical in scenarios where activity boundaries are highly dynamic or ambiguous.

### EQ2: Based on the process model, can you describe what happened during the mission?
Yes, based on the process model, I was able to understand what happened during the mission. The model clearly showed the sequence of activities and how they followed one another across execution runs. I could see which activities were repeated and how the mission progressed over time.

I found the representation particularly useful for understanding the sequential nature of the mission. This is important because, depending on how the system is implemented, the developer’s mental model does not always explicitly represent sequencing, for example, when using behavior trees or similar control structures.

One aspect that was slightly less clear to me concerned overlapping resource usage, especially the interactions between the drone and the tractors. I think this issue would become even more evident in scenarios involving homogeneous robots that perform the same activities.

### EQ3: Did the additional perspectives influence or refine your understanding of the mission? How did the combination of the process view and these visualizations affect your reasoning about the mission behavior?
Yes, the additional perspectives clearly refined my understanding of the mission. In particular, they helped me identify where most issues and potential failures occurred.

The spatial perspective was useful to see where most of the actions took place and where the robots tended to concentrate during the mission. Being able to visually connect activities to specific spatial areas helped me reason about what the robot was doing and where.

The energy perspective was helpful for identifying which tasks were more energy-consuming. This is important because energy-intensive activities can potentially lead to mission failures or require replanning. This kind of information is not immediately visible from the process model alone, but it becomes much clearer when combined with visualizations that show battery discharge over time or per activity.

The communication perspective was particularly relevant in the multi-robot setting. Seeing lost messages immediately stood out as a potential problem, since even a single missed message can indicate coordination issues or failures in the system.

Overall, combining these perspectives with the process model helped me understand not only what happened, but also why certain behaviors occurred.

### EQ4: How would you normally analyze a robotic mission like this using the tools, data, or practices you currently rely on?

Without an approach like this, I would typically analyze such a mission by manually inspecting logs. This would involve opening multiple terminals or log files to monitor different topics or components, either at runtime or after execution. In practice, this means checking printed logs, recorded messages on specific ROS topics, or raw rosbag data.

This type of analysis is time-consuming and fragmented, as information about activities, positions, battery levels, and communications is spread across different sources. While it is possible to retrieve all this information, it requires a lot of manual effort to correlate events and understand how different aspects of the mission relate to each other.

### EQ5: Do you think you could have reached similar insights or conclusions using your usual tools or approaches?
Some conclusions could probably be reached creating ad-hoc scripts, but not as directly or efficiently. For example, spatial trajectories and battery levels can be analyzed by extracting and re-plotting the corresponding logs. However, this would not provide an explicit mapping between these data and the activities being executed.

In particular, mapping contextual information, such as position or battery level, to specific activities would require manual reconstruction. I would have to infer what the robot was doing at a given time by manually correlating timestamps across different logs. This makes the analysis more error-prone and significantly less immediate.

In contrast, having activities explicitly linked to contextual data allows this mapping to be done automatically. This makes it much easier to understand, for instance, what the robot was doing at a certain position or how the battery evolved during a specific task.

### Anything to add?
One additional feature that could be useful is a time-based visualization, such as a timeline or schedule view of the executed tasks. A representation where tasks are shown along a temporal axis—possibly color-coded by robot—could make it easier to reason about task duration and sequencing at a glance.

Such a timeline could also be combined with battery information, as battery consumption within a task may not be linear over time. While this information can already be derived from the existing visualizations and the process model, a dedicated time-based view could improve intuitiveness, especially for users primarily interested in temporal aspects. That said, I do not consider this a core missing feature, as the same information is already available through the current representations.


## Participant_2: Interview

### EQ1: How would you assess the effort required to define and integrate activity tags into a robotic mission? In which scenarios do you think tagging would be feasible, and in which might it become impractical?
I don’t think it adds a lot of effort. The example shown looked like just a few lines of code, so overall I would say the effort is small and it can be done.

Tagging seems feasible when we talk about high-level activities and there are not too many of them. In that case it is easy.

A situation that could become problematic is when one activity happens in the middle of another activity, such as parallel or nested behavior. In those cases you might end up mixing information. Also, robotics control structures like behavior trees introduce reactivity: if something fails during an activity, the robot may start another activity immediately. In that case, logging only start and complete may not be enough, and it would be useful to include additional lifecycle states such as failure, suspended, or aborted.

Overall, if the engineer understands the activities well and can define them as reasonably atomic, I don’t see major issues.

### EQ2: Based on the process model, can you describe what happened during the mission?
Yes. From the model I can understand what was executed.

I interpreted it as a team of robots. First there is a takeoff, then the system explores the field. When a weed is found, a message is sent to inform others. Then someone receives that message and the weed position is communicated to a tractor. The tractor receives the information and goes there to remove the weed.

I find the “message send / message receive” distinction interesting because it supports understanding the communication steps. Also, the frequency information helps: small numbers indicate rare transitions, while larger numbers show the main flow of the mission. This is useful for getting an overview, and it also suggests that you can filter out infrequent behavior when you want to focus on the typical mission path.

### EQ3: Did the additional perspectives influence or refine your understanding of the mission? How did the combination of the process view and these visualizations affect your reasoning about the mission behavior?
Yes, these perspectives are useful. The process model itself is already interesting and, in a ROS context, would be a novelty to have as a summary of the mission. But typically I also rely a lot on plots to understand what happened, so I think these visualizations are necessary.

The most valuable aspect is the correlation between low-level data (space, battery, communication) and the high-level activities. The graphs help me understand what happened, and if they are connected to the activities, then they become much more informative.

Right now, the plots and the process model are not fully linked interactively (e.g., clicking an activity in the model to highlight the corresponding region in a plot), but even without that, having both views helps reasoning. If there were stronger visual linking (for example consistent color mapping or direct selection), it could support exploration even better.

Overall, the additional perspectives provide an extra layer of reasoning beyond the activity sequence alone, and they help interpret issues such as energy-heavy activities or communication-related behavior.

### EQ4: How would you normally analyze a robotic mission like this using the tools, data, or practices you currently rely on?
Other than an approach like this, I would analyze it by reading logs and trying to understand what happened. A better way to handle logs is usually plotting them, but it is still manual.

Another approach I use in my work is monitoring or requirements-driven analysis: having high-level specifications or monitors that can read the logs and detect violations, telling you which requirement failed. This is another way to analyze how a mission went, especially for failures.

So, in practice, it is mainly log reading, plotting, and possibly requirement-based monitoring.

### EQ5: Do you think you could have reached similar insights or conclusions using your usual tools or approaches?
No, not really, at least not to the same extent. If I only use log reading, it would be very hard to get an overall view of the mission like the one I can get from the process model.

Also, it would be much harder to spot edge cases, meaning rare transitions or infrequent behavior. Those are easier to see when the model shows frequencies and when you can filter.

I think the approach supports a more in-depth analysis and helps mission engineers understand what happened better than they would by manually looking at logs.

### Anything to add?
I think it is very nice and very helpful. I believe roboticists would be happy to have such tools, especially if they work out of the box.

I particularly liked the communication aspects. At the process level, I would like to see communication “between who and who”, i.e., more explicit separation of agents/robots in the model.

More broadly, I think robotic mission engineering can be approached bottom-up (as in your work), top-down (model-driven or requirement-driven), or with a hybrid approach. I find the hybrid direction interesting—for example combining behavior-tree-driven logging with process mining, and possibly integrating requirement monitoring on top of the discovered model.


## Participant_3: Interview

### EQ1: How would you assess the effort required to define and integrate activity tags into a robotic mission? In which scenarios do you think tagging would be feasible, and in which might it become impractical?
In terms of pure implementation effort, I don’t think it is particularly high. If you know what you want to tag, adding the tags is straightforward and can be done quickly. The technical part is simple.

The main difficulty is not the implementation, but deciding what to tag. Defining the right level of abstraction is the hardest part. You have to decide which activities are meaningful at a high level and which details can be ignored. This is more of a conceptual and design problem than a programming one.

I think tagging is feasible when there is a good understanding of the scenario and the mission, especially if the developer or a domain expert knows the system well. In those cases, it is easier to decide which activities are important to observe. On the other hand, tagging becomes impractical when there is little knowledge of the scenario and you do not fully understand what is important to monitor. In those situations, it is difficult to decide what should be tagged and at which level of abstraction.

Some guidelines or conceptual examples on how to choose the abstraction level could be helpful. The technical part is easy; the challenging part is the modeling decision behind the tags.

### EQ2: Based on the process model, can you describe what happened during the mission?
Yes, to a certain extent, the process model allows me to understand what happened during the mission. At a high level, it reconstructs the sequence of activities that were executed and how the mission evolved over time.

How much I can understand really depends on the complexity of the mission and on the abstraction level used for tagging. For relatively simple missions or when the abstraction level is well chosen, the process model gives a clear picture of the mission flow. I can see which activities occurred, in which order, and how often.

However, when the system becomes very complex or highly variable, understanding exactly what happened becomes harder. If the abstraction level is too low, the model can explode into something similar to a large state machine, which becomes difficult to read. In that case, I may understand roughly what happened, but not easily pinpoint what happened at a specific moment without further filtering or analysis.

In general, the process model is useful for reconstructing the mission at a high level, but its effectiveness depends heavily on the chosen abstraction.

### EQ3: Did the additional perspectives influence or refine your understanding of the mission? How did the combination of the process view and these visualizations affect your reasoning about the mission behavior?
Yes, they helped, especially because each perspective captures a different aspect of the mission.

I appreciate that these perspectives are kept separate rather than merged into a single visualization. If everything were shown at once, it would become unreadable. Having separate but coordinated views makes it easier to reason step by step.

That said, this approach is more suited for understanding and explanation than for fine-grained verification. It helps me see what happened and notice suspicious patterns, but it does not automatically tell me whether something went wrong according to a formal property.

### EQ4: How would you normally analyze a robotic mission like this using the tools, data, or practices you currently rely on?

My background is more in verification and monitoring.
Normally, I would start from an explicit idea of what the mission is supposed to do. I would have a mental or written specification of the expected behavior, and then I would analyze execution data to see whether the mission was satisfied or not.

With current tools, analysis is often either very high-level, such as checking whether the mission completed successfully, or very low-level, such as inspecting raw logs or trajectories. For example, if a mission ends because the battery is depleted, I would want to understand whether there were suboptimal movements or unnecessary actions that caused excessive energy consumption.

For example, in past projects, I had situations where robots adapted their strategies at runtime (e.g., replanning paths when obstacles were encountered). From raw logs or simple plots, it was difficult to understand whether those adaptations actually occurred or worked as intended.


### EQ5: Do you think you could have reached similar insights or conclusions using your usual tools or approaches?
No, not really. I am not aware of other approaches that provide this kind of high-level mission abstraction combined with contextual information.

With existing tools, I could eventually understand parts of the behavior, but getting a global picture of the mission and identifying rare or unexpected behavior would be much harder. In particular, understanding sequences such as navigation attempts, failures, and replanning steps would require significant manual effort.

Having an explicit process representation of what happened could confirm whether expected behaviors, like replanning after encountering obstacles, actually took place.

### Anything to add?
I find the approach useful, especially for explainability and postmortem understanding of robotic behavior. 
However, I also see its limits: some behaviors are difficult to judge visually, and understanding whether a specific action was correct may still require additional reasoning or formal properties.

In the long term, I think this approach could be complemented with property-based or requirement-based analysis. For example, associating property violations with specific transitions or activities in the process model could strengthen the analysis.

However, as a tool for reconstructing, exploring, and explaining what happened during a mission, I think this approach fills an important gap compared to existing tools.


## Participant_4: Interview

### EQ1: How would you assess the effort required to define and integrate activity tags into a robotic mission? In which scenarios do you think tagging would be feasible, and in which might it become impractical?

From what I know about ROS, the mechanism you used is common practice. Publishing information as a topic and recording it in the same pipeline should not add much burden for the developer, at least at the implementation level.

The main requirement is that you need to know where activities are triggered so you can place the tags at the right points. If you control the code, I think it should generally be feasible, because you should be able to identify where actions begin and end.

I struggled to find a clear case where it would be not feasible, assuming you have control of the code. If you own the codebase, you can usually tag it. Also, in some robotic architectures I’ve seen, tagging could potentially be generated automatically, for example, if there is a structured lifecycle management for execution (task-by-task), it might be easier to derive these tags directly from the code.

### EQ2: Based on the process model, can you describe what happened during the mission?

Yes, I find the process view quite intuitive for understanding activities and their order. I like that it communicates the mission behavior clearly as a sequence of steps.

One improvement I would suggest is adding explicit information about which node/robot executed each activity, because as it stands I infer it (e.g., I assume takeoff is performed by the drone), but it would help analysis if it were visible in the model.

I also appreciated the color coding, because if many things happen during the mission it helps to focus on what is “more expensive” (e.g., resource-heavy) or on potential bottlenecks. Overall, the visualization style feels familiar and intuitive.

### EQ3: Did the additional perspectives influence or refine your understanding of the mission? How did the combination of the process view and these visualizations affect your reasoning about the mission behavior?

Yes, the added perspectives refine what I get from the process view, especially energy consumption, because energy is often a critical aspect in robotics and it makes sense to correlate it with activities.

Regarding the space visualization, I understand the motivation, especially for cases like post-mortem analysis when something goes wrong (e.g., the drone crashes or gets stuck). In those situations, seeing patterns like “proper navigation vs. stuck behavior” can be very useful.

My main question on the space perspective was whether it would sometimes be better to visualize geographical location / map-based information rather than only space occupancy. I can imagine that whether this matters depends on the use case: if the robot always operates in the same place, maybe map-based views add less; but in other settings, they could be valuable. In general, if you have a real use case you can refine the visualization accordingly.

I also think it could be interesting to add process-model coloring not only by frequency/performance but also by energy/resource consumption, so the process view itself highlights where energy is spent (e.g., a dedicated “energy” overlay, analogous to existing frequency/performance views).

For communication, I found it useful to show received vs. lost messages. I wondered whether it would be possible to see in which specific cases messages were lost. As I understood it, right now that requires filtering case-by-case first, and then analyzing. That seems workable, but deeper drill-down could help.

Overall, I like the idea of correlating these lower-level perspectives with the process activities. The separation into distinct views also helps avoid overwhelming the user.

### EQ4: How would you normally analyze a robotic mission like this using the tools, data, or practices you currently rely on?

In my experience, I often use robot- or vendor-specific tools that provide domain-specific visualizations depending on the robot’s sensors and control architecture.

As a developer, if I need to understand control behavior during operation—especially for people working close to lower-level control (e.g., PID), it can be useful to look at things like the robot’s attitude over time, but this depends heavily on the system and the user.

In one system I used, the lifecycle was already organized into missions as a sequence of tasks sent to the robot, with some modes between tasks. In that case, you could reconstruct something like a process view from logs, or even generate it from the code. However, those tools did not provide a strong way to correlate the high-level activity sequence with additional perspectives like energy or communication in the way shown here.

So to me, the correlation across perspectives is a differentiator compared with the tools I’ve used.

### EQ5: Do you think you could have reached similar insights or conclusions using your usual tools or approaches?

Some aspects are similar to what I’ve seen elsewhere—especially bottleneck detection, which I recognize from other robotic mission design and log analysis tools.

However, I find the energy visualization particularly relevant, because even when energy was important in my past work, the tools did not always provide rich ways to visualize energy drops or battery-related issues in a way that connects directly to mission activities.

For communication, I don’t recall having comparable metrics like message loss visualization. I also realize that this is possible here because the system records that messages were sent in the first place, so you can reason about loss vs. receipt.

Another point I only fully realized during the discussion is that the tool is designed for multi-robot scenarios by design, which is not always the case for typical tools.

Overall, I don’t think I would reach the same combined conclusions as easily with my usual tools, especially because this approach combines the process view with multiple correlated perspectives.

### Anything to add?

I think it would be interesting to see an extension toward monitoring, not only post-mortem analysis, especially given the multi-robot and communication aspects.


## Participant_5: Interview

### EQ1: How would you assess the effort required to define and integrate activity tags into a robotic mission? In which scenarios do you think tagging would be feasible, and in which might it become impractical?
On paper the effort is low: you only add those tags in the code. From a purely implementation point of view, inserting topic publishes is straightforward in ROS.

The real challenge is distributed or collaborative scenarios. When the mission is spread across many nodes, classes, or codebases, it becomes harder to place tags correctly. Typical mistakes I can imagine are: syntactic/programming errors when adding tags (fixable);logging start/end at the wrong places (e.g., logging start of activity A at the end of A), which corrupts the semantics; missing tags because you don’t know where to place them and you therefore lose parts of the trace.

So it’s feasible when you control the code and understand the architecture. However, to reduce human error, it would be useful to provide best-practice patterns or anchor points in a reference ROS codebase so developers have concrete snippets to copy.

### EQ2: Based on the process model, can you describe what happened during the mission?
Yes. From the process model I see the expected high-level flow: the drone takes off, explores the field, detects a weed, sends the weed position, and tractors receive the position. The drone selects the nearest tractor and sends the instruction; the selected tractor moves and performs the cutting (cut grass), then returns to base. Eventually the mission ends.

One warning: the process graph aggregates activities across robots and does not automatically distinguish which resource executed which activity. That’s why filtering by robot (e.g., show only the drone) is important to disambiguate. When filtered to the drone perspective, the sequence is clearer.

### EQ3: Did the additional perspectives influence or refine your understanding of the mission? How did the combination of the process view and these visualizations affect your reasoning about the mission behavior?
They are useful enhancements. The mission sequence is understandable from the process model alone, but the additional plots give important details about robot behavior.
Space shows where the robot moved and helps distinguish good navigation from cases where the drone got stuck.
Energy highlights which tasks are energy-intensive and can reveal defective batteries that drain faster.
Communication shows counts of sent/received messages and lost messages; this can reveal communication issues.

These perspectives don’t change the high-level story, but they help pinpoint behaviors and anomalies that are not visible in the sequence alone (e.g., which task consumed the most energy or when messages were lost).

### EQ4: How would you normally analyze a robotic mission like this using the tools, data, or practices you currently rely on?
It depends on the available logs. Typically I would start by checking robot logs (warnings/errors/hardware issues).
If the system’s lifecycle is structured into missions/tasks, you can reconstruct similar sequences from logs or even from code.

But usual toolchains are fragmented: you inspect logs and then use separate tools for each perspective. The integrated correlation across process + space + energy + communication offered here is not common in my experience.

### EQ5: Do you think you could have reached similar insights or conclusions using your usual tools or approaches?
Partly. If you had exactly the same, comprehensive logs (including tags, battery, communication metadata), you could reach similar conclusions, but you would probably need to stitch together multiple tools and spend more time. If the logging is not structured (no tags), reproducing the same analysis is harder.

So this approach is faster and more comprehensive when the data are collected in the structured way you propose. With traditional, unstructured logs you could achieve similar results, but with more difficulty, more tools, and more manual correlation.


### Anything to add?
I suggested adding a graph-based communication visualization (a social/network or graph view): nodes = robots, edges = topics/messages. Right now the communication view reports counts per topic; a graph-based view would let you see who communicates with whom, which nodes are central (possible bottlenecks), and help spot overloaded robots or communication-pattern bugs without switching tools.