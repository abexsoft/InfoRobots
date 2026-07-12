# InfoRobots
A satisfactory mod that introduces a walking mascot robot that patrolls factory floors to collect and visualize operational data.

This repository is for bug reports and discussions. If you have any questions or feedback, please post them in the Discussions tab.

### Overview
#### Basic
- Robots are spawned from a Base Station.
- Robots patrol along a Walking Path, starting from that Base Station.
- A Walking Path is created by connecting various nodes (Base Stations, Monitoring Nodes, and Walking Path Nodes).
- A robot's patrol route is created by sequentially selecting Monitoring Nodes within the Walking Path network connected to its home station.
- By linking equipment to a Monitoring Node, the robot can collect equipment data and bring it back to the Base Station.
- At the Base Station, this data is aggregated, allowing users to check production and consumption information for each item.
#### Stations
- A Base Station can also aggregate data collected by other stations.
- This allows stations to be structured hierarchically.
- Data Packer Stations and Data Unpacker Stations are facilities used to turn data into items and transport them over long distances via conveyors or other means.
- A Data Packer Station functions almost identical to a Base Station, but it outputs the information collected by its assigned robots onto a conveyor.
- A Data Unpacker Station unpacks and stores the information received from the conveyor. 
- It also provides that information to any robots patrolling this station.
- Currently, you can only build up to 20 Data Packer Stations. While I plan to increase this limit, it is finite under the current specifications.
#### Waypoint Nodes
- Walking Path Nodes are used solely to form the Walking Path. 
- Monitoring Nodes serve as robot destinations and are utilized for facility monitoring.
- You can change the monitoring range and the visibility of Monitoring Nodes in the global settings.
- The Robot-only Portal(v1.0.2) is a special node designed to connect distant nodes.
- Place two portals, select one of the portals and link them together.
- This feature unlocks upon releasing SPWN.
#### Robots
- When you select a robot, you can configure patrol routes, start/stop patrols, view the robot camera, and change skins. 
- For patrol route configuration, green 'Available Nodes' indicate that the node is currently targeted for patrol by a robot. 
- If you change the patrol route configuration, please press 'Start' to apply the changes to the robot.
- When you select the robot camera view, you can press [C] to switch between Fixed Camera and Mouse Look. 
- In Mouse Look mode, hold the right mouse button and drag to change the viewpoint.
- You can introduce your original character. [Here](https://github.com/abexsoft/InfoRobots/blob/main/custom_skin_guide_en_public.md) is how to do it.
### Keybinding
- [Insert] open Global Settings window
- [Esc] close window
