# Flow Rule Timeout Manager (SDN using Ryu + Mininet)

## Overview
This project implements timeout-based flow rule management using Software Defined Networking (SDN). A Ryu controller dynamically installs flow rules and removes them after inactivity using idle timeout.

---

## Objectives
- Configure idle timeout for flow rules  
- Automatically remove expired rules  
- Demonstrate flow lifecycle  
- Analyze network behavior  
- Validate consistency through repeated tests  

---

## Technologies Used
- Mininet  
- Ryu Controller  
- OpenFlow 1.3  
- Ubuntu 24.04  

---

## Topology
- 1 Switch (s1)  
- 3 Hosts (h1, h2, h3)  
- Remote Controller (Ryu)  

---

## How It Works
1. A packet arrives at the switch  
2. Switch sends PACKET_IN to controller  
3. Controller installs a flow rule (match-action)  
4. Rule includes idle_timeout = 10 seconds  
5. If no traffic, the flow expires automatically  
6. Next packet triggers the controller again  

---

## Running the Project

### Start Controller
python ryu/ryu/cmd/manager.py timeout_controller.py

### Start Mininet
sudo mn -c  
sudo mn --topo single,3 --controller=remote --switch ovsk,protocols=OpenFlow13  

---

## Testing

### Generate traffic
h1 ping -c 2 h2  

### Wait ~10 seconds  

### Run again
h1 ping -c 2 h2  

---

## Flow Table Verification
sudo ovs-ofctl -O OpenFlow13 dump-flows s1  

---

## Expected Output

### Controller Logs
- PACKET_IN → flow installed  
- FLOW EXPIRED → rule removed  

### Behavior
- First packet → higher latency  
- Subsequent packets → faster  
- After timeout → latency increases again  

---

## Key Observations
- Flow rules are dynamically installed  
- Idle timeout removes inactive flows  
- Flow lifecycle is clearly visible  
- Behavior is consistent across runs  

---

## Conclusion
This project demonstrates timeout-based flow rule management using SDN. The controller dynamically manages network behavior using OpenFlow match-action rules and idle timeouts.

---

## Author
Karnati Uday Keerthan Reddy
