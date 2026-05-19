# Ground Truth for Questionnaire-Based Evaluation Logs

We report the reference ground truth used for the questionnaire-based evaluation. The ground truth was defined for each synthetic event log and describes the expected findings for the five analysis tasks performed by the participants: control-flow, performance, battery, communication, and spatial workspace analysis.


---

## Log 1: Single Ground Robot Delivery Patrol

**Scenario name:** `single_ground_robot_delivery_patrol`  
**Domain:** single ground robot delivery patrol  
**Number of traces:** 60  
**Number of events:** 1969  

### Expected control-flow findings

The typical mission starts with the preparation and initialization of a single ground robot. The robot then performs navigation activities to reach the delivery or patrol area, executes the required delivery/patrol-related activities, and finally returns to the dock or base. The nominal process can be summarized as follows:

1. mission initialization;
2. robot preparation or deployment;
3. navigation toward the target area;
4. delivery and/or patrol execution;
5. return to dock/base;
6. mission completion.

The process includes deviations from the nominal path. In particular, some traces include an `obstacle_avoidance` activity, representing a route deviation caused by an obstacle encountered during navigation. Some traces also include a temporary stop during mission execution.

### Expected performance findings

The main performance-related issues are associated with route deviations and temporary stops. Traces including `obstacle_avoidance` are expected to show longer execution times than nominal traces, because the robot deviates from the planned route before continuing the mission. Temporary stops also introduce additional waiting or idle time during the mission.

Expected bottlenecks or delays include:

- longer navigation time when obstacle avoidance is triggered;
- temporary stop during mission execution;
- increased total mission duration in traces involving route deviation or recharge.

### Expected spatial workspace findings

The spatial workspace should mainly reflect the route followed by the ground robot between the dock/base and the delivery or patrol area. Activities related to navigation are expected to cover a broader spatial area, while docking, recharge, and mission initialization/completion activities should be concentrated near the dock/base.

Expected spatial findings include:

- dock/recharge activities localized near the base area;
- navigation activities distributed along the mission route;
- delivery/patrol activities concentrated around the target area;
- obstacle avoidance associated with a deviation from the nominal route;
- no multi-robot spatial overlap analysis is required, since only one robot is involved.

### Expected battery-related findings

Battery conditions affect mission execution in a subset of traces. The expected battery-related issue is a low-battery condition that forces the robot to return to the dock and recharge before completing or resuming the mission.

Expected battery findings include:

- presence of low-battery events or conditions;
- return to dock/base caused by low battery;
- recharge activity before mission continuation or completion;
- longer mission duration in traces affected by battery-related return and recharge.

### Expected communication findings

Since this is a single-robot scenario, no inter-robot communication issues are expected. Participants should not identify delayed or missing messages between robots. 

Expected communication finding:

- no delayed or missing inter-robot messages are expected.



---

## Log 2: Warehouse Multi-Robot Pick-Pack-Handover

**Scenario name:** `warehouse_multi_robot_pick_pack_handover`  
**Domain:** warehouse multi robot pick pack handover  
**Number of traces:** 65  
**Number of events:** 2861  

### Expected control-flow findings

The typical mission involves multiple robots cooperating in a warehouse workflow. The process includes picking items, transporting them, packing them, and performing a coordinated handover between robots or stations. A nominal execution can be summarized as follows:

1. mission/order initialization;
2. robot assignment or task allocation;
3. navigation to pick area;
4. item picking;
5. transport toward packing or handover area;
6. queue or waiting phase when the next resource/station is not immediately available;
7. coordinated handover;
8. packing or final handling;
9. mission/order completion.

The process includes multi-robot coordination points, especially around the handover phase. Some traces include delays before the coordinated exchange.

### Expected performance findings

The main performance issues are related to queueing and handover synchronization. The expected bottleneck is the `queue_wait` activity, which may occur when robots or stations have to wait before the next step in the warehouse process. Another relevant delay occurs before the coordinated handover, where synchronization between robots is required.

Expected performance findings include:

- `queue_wait` as a recurrent bottleneck;
- handover delay before coordinated exchange;
- longer case duration for traces involving queueing or delayed handover;
- possible waiting time caused by resource contention or synchronization between robots.

### Expected spatial workspace findings

The spatial workspace should reflect distinct warehouse zones. Picking activities are expected to occur in the picking/storage area, packing activities in the packing area, and handover activities in a shared or intermediate coordination area. Transport/navigation activities connect these regions.

Expected spatial findings include:

- pick activities localized in storage/picking zones;
- pack activities localized in packing zones;
- handover activities localized in a shared exchange area;
- navigation/transport activities covering paths between warehouse zones;
- multiple robots may operate in distinct regions during pick/pack phases but converge around the handover area;
- spatial overlap is expected mainly around shared handover or queueing areas.

### Expected battery-related findings

Battery-related issues are not the primary anomaly in this scenario. Participants may observe normal battery consumption during robot movement and task execution, but the expected ground truth does not include battery as a main cause of mission deviation or failure.

Expected battery findings include:

- no systematic battery-related mission interruption;
- no expected low-battery return/recharge pattern as a main issue;

### Expected communication findings

Communication is relevant because the scenario involves multiple robots and coordinated handover. The expected communication issues are sparse lost or delayed coordination messages. These communication anomalies may affect synchronization and contribute to handover delays.

Expected communication findings include:

- delayed coordination messages between robots or between robots and warehouse coordination components;
- sparse lost messages;
- possible resend or recovery behavior, if represented in the log;
- communication issues mainly occurring around handover or coordination phases.


---

## Log 3: Search-and-Rescue UAV-UGV Team

**Scenario name:** `search_and_rescue_uav_ugv_team`  
**Domain:** search and rescue UAV-UGV team  
**Number of traces:** 58  
**Number of events:** 2565  

### Expected control-flow findings

The typical mission involves a heterogeneous team composed of UAV and UGV robots. The UAV is expected to support aerial search, mapping, or target localization, while the UGV performs ground navigation and intervention or inspection activities. A nominal execution can be summarized as follows:

1. mission initialization;
2. deployment of UAV and UGV;
3. UAV aerial search/mapping or target localization;
4. transmission of coordinates or target information;
5. UGV navigation toward the identified area;
6. ground inspection, support, or rescue-related action;
7. mission completion or return.

The process includes coordination between UAV and UGV. Some traces include lost coordinate transmission, requiring a resend, while others include rerouting of the UGV path. Sparse mission aborts are also expected.

### Expected performance findings

The main performance issues are associated with delayed response, coordinate retransmission, and UGV rerouting. Lost coordinate messages can delay the UGV response because the target location must be resent. Rerouting increases navigation time for the UGV. Mission aborts may appear as shortened or interrupted executions.

Expected performance findings include:

- delayed response after UAV coordinate transmission;
- longer execution time when coordinate transmission is lost and resent;
- increased UGV navigation time when rerouting occurs;
- sparse mission aborts;
- variability in case duration due to communication and routing issues.


### Expected spatial workspace findings

The spatial workspace should show complementary operating regions for UAV and UGV. The UAV is expected to cover a wider aerial search or mapping area, while the UGV operates on the ground and moves toward specific target or intervention areas. 

Expected spatial findings include:

- UAV activities covering a broader aerial search/mapping area;
- UGV activities concentrated along ground routes and target/intervention areas;
- UAV and UGV operating in related but not identical spatial regions;

### Expected battery-related findings

Battery may be monitored during the mission, especially for the UAV, but battery-related anomalies are not the main expected issue in this scenario. The primary causes of deviations are communication loss, delayed response, rerouting, and sparse mission aborts.

Expected battery findings include:

- no systematic low-battery return/recharge pattern as a main issue;
- battery information may support mission monitoring;
- battery should not be identified as the dominant cause of the observed anomalies unless explicitly present in a specific trace.

### Expected communication findings

Communication is central in this scenario. The expected communication anomaly is the loss of coordinate transmission from the UAV to the UGV, requiring the coordinate information to be resent. This can lead to delayed response by the UGV and can affect the overall mission timing.

Expected communication findings include:

- lost coordinate transmission;
- resend of coordinate or target information;
- delayed UGV response following communication loss;
- communication dependency between UAV detection/localization and UGV navigation;
- communication issues as a likely explanation for some performance delays.

---

## Summary | TL;DR

| Scenario | Expected main anomalies |
|---|---|
| `single_ground_robot_delivery_patrol` | Low-battery return to dock and recharge; route deviation via obstacle avoidance; temporary stop during mission |
| `warehouse_multi_robot_pick_pack_handover` | Handover delay before coordinated exchange; `queue_wait` bottlenecks; sparse lost and delayed coordination messages |
| `search_and_rescue_uav_ugv_team` | Lost coordinate transmission requiring resend; UGV rerouting; delayed response; sparse mission aborts |