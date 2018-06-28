#### M.S. Thesis Defense
## A Multi-Agent Simulation Tool for the Modern Grid

#### Holm Smidt
---

#### Digitalization

![Data is the new oil](assets/intro/data_oil.png)

+++

#### Machine Learning

![AI is the new electricity](assets/intro/ngAI.png)


+++

#### What about the energy grid?

+++

#### The status quo is what we know well.

![The current grid](assets/grid/oldGrid_2.jpg)

+++

#### How do we do what's new?

![The future grid](assets/grid/heco_newGrid.png)

 @size[0.5em](HECO Grid Modernization Plan)


---

#### We can @size[1.3em](@color[orange](design models)) and run @size[1.3em](@color[orange](simulations))

![VW testing scandal](assets/intro/vw_test.jpg)


+++

#### Objectives:

Develop and test a multi-agent simulation tool that
@ul

- models @color[green](agent-level behavior) and packages it as easy-to-deploy applications;
- enables lighweight @color[orange](communication) among (virtually) resource constrained devices;
- provides @color[red](administrative) monitoring, control, and management

@ulend

+++

#### System Architecture - Overview

![System architecture](assets/design/20180621_sys_architecture.png)

@size[0.7em](System services structured into the three implementation layers)

+++

#### Presentation Outline

@ol

- What are agents and how to model them.
- Sharing data among the right nodes.
- Using graph databases to keep track of agents and their relationships.
- Bringing everything together with the web application.
- Simulation outcomes.

@olend

---

## Agent Models
#### Agent Layer

+++

#### What defines an agent?

![Smart Grid Domain](assets/grid/20180620_NIST_conceptualModel.png)

@size[0.7em](NIST Smart Grid Domains)

+++

#### Use Case: Residential Demand Management & Response

The @color[orange](simulation goal) is to model residential energy usage based on individual appliances and then control the appliances in the case of a @color[orange](direct load control) (DLC) event.

+++

#### House application

@ul
- Python application
- Runs in real-time
- Publishes @color[orange](energy usage) and @color[orange](demand response availability) every 5 minutes
- Subscribes to @color[orange]control settings
- @color[green](Turns appliances on/off) based on a predefined schedule
- @color[green](Controls the temperature) of the house

@ulend

+++

<table>
  <tr>
    <th>Appliance</th>
    <th>Controls</th>
    <th>Power</th>
    <th>Schedule </th>
  </tr>
  <tr>
    <td>Lights</td>
    <td>fixed</td>
    <td>360 W</td>
    <td>5-8am, 6-11pm</td>
  </tr>
  <tr class="fragment">
    <td>Refrigerator</td>
    <td>fixed</td>
    <td>200 W</td>
    <td>12-1am, 5-6am, 8-9am, 11- 12pm, 3-4pm, 7-8pm </td>
  </tr>
  <tr class="fragment">
    <td>Entertainment (TV)</td>
    <td>fixed</td>
    <td>200 W</td>
    <td>5-10pm</td>
  </tr>
  <tr class="fragment">
    <td>Range/Oven</td>
    <td>fixed </td>
    <td>3500 W</td>
    <td>11-12pm, 6-7pm</td>
  </tr>
  <tr class="fragment">
    <td>HVAC</td>
    <td>variable </td>
    <td>3500 W</td>
    <td>12am-3am, 10-2pm, 5-11pm</td>
  </tr>
  <tr class="fragment">
    <td>Baseload</td>
    <td>fixed schedule </td>
    <td>250 W$^*$ </td>
    <td>N/A</td>
  </tr>
</table>

+++

#### Thermal model
`\[
\frac{dT}{dt} = \frac{-1}{C_{H}} \left( P_{HVAC} + \frac{T_H - T_a}{R_{TH}}\right)
\]` <br>
`\[
T_{H_i} = T_{H_{i-1}} - \frac{1}{ R_{TH} C_{H}} \int_{t_{i-1}}^{t_i} \left( R_{TH}P_{HVAC} + (T_{H_{i-1}} - T_a)\right) d\tau
\]`
![Thermal Model](assets/design/20180627_thermal_circuit_s.png)


+++

#### PI Controls

- control the HVAC output when the system is running

`\[
 c(t) = K_p  e(t) + K_i  \int_0^t e(\tau) d\tau
\]`

+++

![System architecture](assets/design/20180621_sys_architecture.png)

---

## Publish/Subscribe Messaging
#### Communication Layer

+++

## Publish / Subscribe Concept

![Pub/Sub Concept ](assets/design/20180616_aws_pub:sub.png)

+++

## MQTT API Design

![Pub/Sub Concept ](assets/design/mqttTopics.png)

+++

#### Assume the following scenario:
@ul
- A home with `devid='dev_34'` is assigned to a DRA with `aggid='agg_1'`.
- The home is reporting an availability of 1 kW of load to shed by publishing to the `drna/agg_1/dev_34` topic
- The home is subscribed to the `set/drmode/dev_34` and `drnc/agg_1/dev_34` topic
- The iso is subscibed to the `drsim/events` topic
@ulend
+++

#### In the case of a DLC event: 
@ul
- The user chooses to curtail 100 kW of load immediately, which the web application therefore publishes to `drsim/events` with a payload of `{"type": "DLC", "amount": 100, "minutes": 10}`
- The iso receives this, and publishes `{"type": "DLC", "amount": 1, "minutes": 10}` to the `drnc/agg_1/dev_34` topic
- The home receives this, sheds it's load, and at the next time interval reports zero availability
@ulend
+++

![System architecture](assets/design/20180621_sys_architecture.png)


---

## Graph Databases for System Modelling
#### Administration Layer

+++

#### Cyber-Physical System Modeling

![Property Graph](assets/design/20180608_PropertyGraph.png)

+++

#### Implementation

![Overview](assets/design/20180623_cps_overview_s.png)

+++

![Node Level](assets/design/20180623_cps_hems_v2.png)

+++



---

## Web Application For Administration
#### Administration Layer
Demo
---

## Simulation Platform Evaluation
#### Results

---


## Use Case

Using a LEZETi Hybrid Solar PV Mini Split Air Conditioner for Produce Cooling

+++

| ![Top](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/Kahuku_ContainerTop.png)  | ![Inside](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/Kahuku_Inside.png) |
|:---:|:---:|
| Container Top View | Indoor Ducted Units |

+++

| ![LEZETI](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/Kahuku_LEZETi.png)  | ![Inside](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/Kahuku_MIAO.png) |
|:---:|:---:|
| Outdoor Unit | Control Unit |

+++


| ![Schematic View](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/EVAPI_Lezeti_control_diagram_DWG_MT-1.png) |
|:---:|
|Schematic view of dynamic system |


---

## Controller Design

_Objective:_ Find control gains Kp, Ki, and Kd that best control the system.

_Desired properties:_ Stability, low error, low energy usage.

_Proposed Method:_ Genetic search algorithm.


+++

## Background

* PID controller is the most common controller in the industry
* Can be tuned by hand or computer software (if dynamics are known); difficult when system dynamics are unknown, when there are time delays, and when there are changes in the environment, etc.

+++

## Background

* Numerous works on PID controller tuning using EV: genetic search, particle swarm optimization, differential evolution, ...
* Most works simply consider simulations

+++

## Challenges

How to interface with the system?

How to overcome time constraints? <br> (One passive heating & active cooling cycle takes about 20-30 minutes.)

+++

## Approach

IoT system design to communicate with learning agents over the network.

Network multiple learning agents.

Store environmental and control parameters of each agent in a Graph Database.

---

## Genetic Search Algorithm

#### Design

+++

| ![LEZETI](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/CollaborativeEV_Process_V1.png)  | ![Inside](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/CollaborativeEV_FitnessEval_V1.png) |
|:---:|:---:|
| Main Loop | Fitness Evaluation |

+++

## Fitness Function

| ![Schematic View](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/fitnessJ.png) |
|:---:|
| ![Schematic View](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/iae.png) |
| Fitness Function J, Integrated Absolute Error IAE, and consumed Energy E |

+++

## Graph Database

| ![Schematic View](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/ev_graphdb_2.png) |
|:---:|
| Graph representation in neo4j |

---

## Initial Implementation

+++

## Parameters

| ![LEZETI](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/ga_param.png)  | ![Inside](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/ga_seq.png) |
|:---:|:---:|
| GA parameters | Setpoint sequence |

+++

## Initial Results

| ![LEZETI](https://raw.githubusercontent.com/hlmes/ec/master/collaborativeEV/images/GA_run1_gen1_annot_2.png) |
|:---:|
| Sample run analysis, promising observations did not lead to satisfactory convergence |


---

## Conclusions

+++

## So far ...

* Infrastructure for genetic search over the network and across agents is in place
* Promising components: constructive selection, graph query
* Initial tests run with gained insights but little success

+++

## What's next ...

- Add **virtual nodes** to the graph database and update them through simulations
- Add actual learning agents
- Revisit **fitness function, mutation, and selection**
