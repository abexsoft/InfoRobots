# InfoRobots
A mod that introduces a walking mascot robot that patrolls factory floors to collect and visualize operational data.

This repository is for bug reports and discussions. If you have any questions or feedback, please post them in the Discussions tab.

## Overview
- Robots are spawned from a Base Station.
- Robots patrol along a Walking Path, starting from that Base Station.
- A Walking Path is created by connecting various nodes (Base Stations, Monitoring Nodes, and Walking Path Nodes).
- A robot's patrol route is created by sequentially selecting Monitoring Nodes within the Walking Path network connected to its home station.
- By linking equipment to a Monitoring Node, the robot can collect equipment data and bring it back to the Base Station.
- At the Base Station, this data is aggregated, allowing users to check production and consumption information for each item.
