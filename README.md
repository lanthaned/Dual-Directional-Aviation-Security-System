# Dual-Directional Aviation Security System
### Three-Factor Cockpit Perimeter Protection Against Internal and External Breach

[![Status](https://img.shields.io/badge/Status-Draft%20v0.1%20%2F%20Work%20in%20Progress-orange)](https://github.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Field](https://img.shields.io/badge/Domain-Avionics%20%26%20Cyber--Physical%20Security-emerald)](https://github.com)

**Author:** Marianna Zdor  
**Curator/Academic Advisor:** Liubov Zdor  
**Date:** October 2026  
**Project Status:** Conceptual Framework (Work in Progress / Draft v0.1)  

---

## Abstract

Here we show an idea for protecting an airplane from an illegal takeover when the threat comes from inside the plane including a crew member. One of the main problems with modern airplane security is that the cockpit has almost full control over the cockpit door.

After September 11 2001 new rules were introduced for cockpit security with the main idea to keep the cockpit completely separated from the passenger hall. The cockpit door could stay locked for a long time with a deny switch.

However, the Germanwings 9525 crash in 2015 and the FlyDubai FZ1073 incident on September 30 2026 showed some problems with this system. The usual "two person rule" isn't always easy to use and especially when a sudden physical short distance fight happens.

This work presents a new system called Fail-Secure Bi-Directional Access. It uses three main security parts:

1. **Physical locking system:** The lock uses three 28 V DC solenoids in an "AND system". It also has electrical isolation and a hardware timer.
2. **Secret sharing system:** The main key is divided into three parts the 1st is given to the main flight attendant, the 2nd to junior flight attendant and the 3rd is placed in a secure compartment of passenger part of airplane. The place is known only by ground service and can only be opened by the ground flight human controllers.
3. **Edge-AI system:** An on plane system called "Semantic Watchdog" checks what's happening inside the plane during all the flight. It uses a small neural network, for example, Gemma 2B, and works on a separate plane computer. The AI checks different information, including vertical speed, voice pitch, transponder codes like 7500 and 7700, cockpit sounds and video from the cameras. The wide-angle cameras look into both directions and watch the area near the cockpit and the passenger cabin all the way to the back of the airplane, scanning at discrete intervals for energy and processing power economy, for example 2 fps. This helps the system see areas that normal cameras will miss.

The system can also notice if someone tries to damage or cover it. It can detect situations where people in the back of the cabin are forced to act like the cockpit has already been attacked by pilot, put simply, to simulate resisting a plane hijacking from pilot cockpit.

The collected information is digitally signed and sent through ACARS and CPDLC. The system follows a simple rule called "Fail-Default to Closed". If the cameras are damaged, covered or stop working the system treats this as a possible hijacking from passenger cabin. In this case the ground key can't be released and the cockpit stays protected until the plane lands.

---

## Interactive Simulations

*(Note: Click the links below to launch interactive engineering visualizations of the system's core protocols. **For mobile viewing, please rotate your device to horizontal / landscape orientation** to prevent text from overlapping the graphics).*

* 🚀 **[Simulation 1: History of the Airplane Security](./simulation1_history.html)**
* 🚀 **[Simulation 2: Unlocking protocol 3-out-of-3 (Shamir scheme)](./simulation2_shamir_scheme.html)**
* 🚀 **[Simulation 3: Sensor Tampering & Lockout Advisory](./simulation3_watchdog.html)**
* 🚀 **[Simulation 4: Dynamic Key Disclosure & Dual Quorum](./simulation4_ground_arbitration.html)**
* 🚀 **[Simulation 5: Full Human-in-the-Loop Architecture](./simulation5_Human_in_the_Loop.html)**

---

## I. HISTORY OF THE AIRPLANE SECURITY. WHY UNILATERAL CONTROL FAILS.

Modern cockpit security was created over the past two decades. Every new rule was made to solve each new threat but it also created another weak spot.

### I.1. CHANGES AFTER SEPTEMBER 11 2001. FAIL-LOCKED PARADOX.
Before September 11 2001, the walls between the cockpit and passenger cabin were mostly made to protect the plane if it lost pressure. The cockpit doors weren't made to stop bullets or strong physical attacks. They also weren't very strong against hitting them. Some doors had special panels that opened when the pressure would change. This helped keep the pressure more equal between different parts of the airplane and helped prevent big damage. At that time, protection from hijacking mostly depended on the crewmates following the right instructions and communicating with air traffic control.

The attacks on September 11 2001 showed a serious vulnerability of this system. After the attacks the FAA and ICAO introduced new security rules: one of them was FAR 25.795, which was also used as a base for the European CS-25.795 rules. The new rules needed much stronger cockpit doors, which had to protect the cockpit from bullets, explosions and strong physical damages. A new Cockpit Door Locking System (CDLS) introduced at that time was made to protect the door from guns, improvised explosive devices and strong hits at the lock.

The main idea was called "Fail-Locked": if the power wasn't working or someone tried to break the lock from the outside, the door stayed locked because of mechanical mechanisms inside the lock. The pilots have a three-position switch: Unlk, Norm, Deny. There is also a keypad on the passenger side of the door only for staff needs. The idea is pretty simple the cockpit is treated as the safe area while the passenger cabin is treated as the possible source of danger.

### I.2. CATASTROPHE GERMANWINGS 9525. TRUST BREAKAGE AND DENY SYSTEM.
On March 24 2015 during Germanwings 9525 flight on the Airbus A320 the second pilot, Andreas Lubitz put the flight height control system on 100 feet and set a maximum speed of open descent while his partner left the pilot cabin for lavatory.

The first pilot of the plane after coming back saw that the door was locked and tried to use access codes on the door panel. After it didn't help he was trying to contact the cockpit using the intercom as well. But Lubitz calmly moved to the Deny position and kept resetting the emergency opening timer. The crew members also tried to break the door with a crash axe and a service cart but all their attempts failed as well, because the armored door had a sandwich structure inside and remained intact even when the plane crashed into a mountain in the French Alps. All 150 people on board died.

This accident showed a serious problem of "Fail-locked system". If a person who causes the danger is already inside the protected cockpit area, then the strong protected door can stop protecting the crew from terrorists but instead become a barrier that protects the attacker from the innocent people outside (in the passenger hall).

After the tragedy regulators trying to prevent something similar from happening again suggested a temporary solution, which was described later in the EASA SIB 2015-04 safety bulletin. It recommended the "two person rule": when one pilot leaves the cockpit the main flight attendant must enter the cockpit and stay there until the second pilot returns.

### I.3. FLYDUBAI FZ1073 INCIDENT: FAILURE OF PROCEDURES DURING A SUDDEN ATTACK
However airlines soon had problems with this new rule. The final failure of this procedure happened on 30 September 2026. It happened on a Boeing 737 MAX № A6-FKF belonging to FlyDubai, when it was in flight FZ1073 from Dubai to Tel-Aviv with 174 passengers onboard.

About two hours after the take off, while the airplane was flying at about 34 000 feet and was near the borders between Jordan and Saudi Arabia, inside the cabin a sudden fight started with a knife used. One of the pilots had brought a knife onto the plane and suddenly attacked the second pilot, causing several wounds. He wanted to take over control of the plane.

The plane went into a steep dive, which was registered on FlightRadar as a loss more than 5,000 m in just a few minutes. The transponder first showed code 7700 which means a general emergency and then it showed 7500 that means illegal interference with the plane. That is why Israel fighter jets were sent into the air to intercept the plane immediately.

The co-pilot's strong resistance apparently helped prevent the attacking pilot from fully locking the door. Obviously it was only for this reason that entry into the cockpit using the external code became possible for cabin crew and two pilots from other airlines (who were traveling as passengers) to take control of the descending aircraft.

The rescue of flight FZ1073 and its landing in Tabuk in Saudi Arabia was mostly a statistical matter of luck but not evidence of an absolutely safe algorithm. That is why the problem of symmetrical safety system against hijacking remains critical.

---

## II. ARCHITECTURAL BREAKDOWN OF WEAKNESSES IN COCKPIT ELECTROMECHANICAL LOCKS (CDLS)

The Germanwings tragedy and the critical situation on FlyDubai FZ1073 showed the hardware and mechanical limits of modern locking systems certified under FAR 25.795.

### II.1. KINEMATICS OF THE EMERGENCY CODE AND DENY SWITCH.
The normal CDLS system on Boeing 737 and Airbus A320 planes works by waiting for a cycling response from the operator. When an emergency master code is entered on the outside keypad the system goes into a waiting mode and a high sound alarm starts inside the cockpit. At the same time an electronic timer starts countdown, that is why the delay time usually lasts from 15 to 60 seconds. If the people inside the cockpit are unconscious (for example because there is not enough oxygen after depressurization) after the delay the 28 V DC power is removed from the solenoids, unlocking the door of cockpit for about 5 seconds. However if the pilot moves the switch to Deny position immediately, the timer resets and the outside code panel is also blocked for a period of 5 to 20 minutes, depending on the airline settings.

The comparison of the two incidents (Germanwings and FlyDubai) helps to explain why the captain of flight 9525 could not enter the cockpit while the crew and passengers of flight FZ1073 managed to do it:
- During the Germanwings accident, Lubitz was sitting in his seat and controlling the panel, allowing him to toggle the DENY switch each time others tried to enter the cockpit from outside;
- On FlyDubai FZ1073 the attacker was fighting at the same time as trying to hold the control column forward so the plane could keep its steep dive. Because of the strong flight forces and the physical fight with the second pilot the attacker could not reach the door control panel and keep the Deny switch constantly pressed.

### II.2 MECHANICAL LIMITS OF THE CRASH AXE DURING HIGH G FORCES
The crash axe that is normally kept on a plane is not designed to break structures certified under FAR 25.795. The strong cockpit door is made from several layers including aramid materials like Kevlar, composite materials and panels with an aluminum honeycomb filling inside, absorbing a very large amount of energy. During the extreme fall the dive of FlyDubai FZ1073 was nearly vertical.

Under these conditions, using a mechanical tool against the door lock would be almost impossible - due to structural deformation at high aerodynamic speeds the opening handles get locked, which makes forced entry impossible.

### II.3. BIOLOGICAL VULNERABILITY OF HUMAN OPERATOR. PSYCHOLOGICAL PROBLEM, COERCION, AND THE ELEMENT OF SURPRISE.
Even if we consider the possibility of opening the cockpit door from the passenger cabin using external keys, it is obvious that the next fundamental questions of human vulnerability arise:
- In case only a flight attendant has a cockpit key, it is still possible for hijackers to take it away;
- None of the crew members, including pilots, are psychologically prepared for threats, torture or murder of passengers happening in front of them;
- The most trained pilots could not be absolutely ready for a sudden fight in the small cockpit, as a person can be put down just within seconds of professional close fight actions, leaving no chance for sending an emergency message.

That is why giving the control key to one person is not a safe idea.

---

## III. ECONOMIC AND OPERATIONAL EFFICIENCY OF EDGE AI.

The expansion of the standard safety procedures would require increasing expenses. For example:

### III.1. THE COST OF AIR MARSHALS (FAMS) AND THE INEFFICIENCY OF THE TWO PERSON RULE.
The system of armed air marshals shows a very little efficiency in preventing hijacking, while requiring large financial resources. An average ticket for the air marshal is 3,300 USD per one flight. An attempt to put air marshals on every flight would lead to an economic crisis. Moreover, even with an officer on board, it would do nothing to avoid the German Wings incident, as the problem was at the other - "blocked" - side.

However the "two person rule" increases the time of the cockpit door being unlocked as movements of flight attendants there and back will add "vulnerability window" of the cockpit. That is why this overload led to its cancelation in 2017.

### III.2. LIFE CYCLE COSTS ANALYSIS (LCC): ONBOARD CAPEX vs. ENDLESS OPEX.
Meanwhile the use of hardware and software combined would help reduce the recurring costs (OPEX) and focus on one time investment (CAPEX). With the average airplane lifespan of 20 years, the computer and preflight regular checking would cost less than one dollar per passenger.

---

## IV. ARCHITECTURE OF THE CYBER PHYSICAL UNLOCKING PROTOCOL 3-OUT-OF-3.

The following "Fail-Secure Bi-Directional Access" system has to remove the architectural monopoly of the persons inside the cockpit and excludes the vulnerability of the human psyche against terrorists' manipulation.

The door lock is modified using three independent 28 V DC solenoids working on the logical "AND principle": to unlock the door all three electrical key signals must be sent at once to power lines. Each line is controlled by its own optorelay with isolation strength of at least 2500 V. This prevents unauthorized unlocking of the lock by intentionally shorting the wire or applying high voltage from the outside.

The control of the solenoids is made as the Shamir secret sharing cryptographic protocol. During the preflight preparation a cryptographic server generates 3 unique code keys: s1, s2 and s3.

Following the Shamir scheme when onboard crew knows only two parts of the key doesn't give the hijackers the full master-key.

Because the recovery of the master-key can only be done when all the three parts are available at the same time, meanwhile, the key parts are set at the different parts of this flight system, so the onboard actions do not affect the receive of the real full key:
- s1 is stored on a cryptographic token of the senior flight attendant;
- s2 is stored on the independent hardware of a junior flight attendant;
- s3 is kept hidden inside the plane with a hardware security module (HSM). The place and unlocking mechanism of the storage is known only to ground security.

Such scheme guarantees that even if the hijackers get the access to s1 and s2, as long as s3 remains hidden, the hijackers cannot open the cockpit door.

---

## V. ONBOARD SEMANTIC SUPERVISOR (SEMANTIC WATCHDOG): LOCAL EDGE AI ARBITRATION.

The third share of the key (s_3) is securely hidden inside the passenger cabin. The role of the onboard semantic supervisor (Watchdog) is to monitor the cabin and verify the emergency situation before this key can be accessed.

### V.1. HARDWARE AND SOFTWARE BASES AND INFERENCE CHARACTERISTICS OF A CLASS MODEL GEMMA 2B.
This software is based on the AI model, for example, Gemma 2B created by Google DeepMind. The computing unit is located in the protected spot, isolated from public networks (including passenger Wi-Fi), having its own conductive cooling and working with a latency of less than 200 ms and consuming no more than 20 Watts.

### V.2. DUAL DIRECTIONAL OPTICAL ARRAY FOR CROSS ATTESTATION AND DETECTION OF THE INSIDER ATTACKS & COCKPIT ACOUSTIC PROFILE CVR.
The most important part is having cameras that look in both directions and send all information about the incoming threats. To reduce these threats the system uses a Dual Directional Cross Attestation Optical Array. As the cockpit door is the biggest point of the action in all hijackings, it's necessary to keep a camera view pointing there, to spot a dishonest pilot or terrorists right away. Another camera should be put on the other end of the plane and the middle section to look after the passengers and any signs of threatening or torture. Both cameras should have each other in the view distance to spot any damages or cover ups. This will also be counted as a hijacking attempt, sending an emergency signal.

Computing platforms of the SummitX class normally include a 4x4 video switch with support for capturing up to 4 independent 1080p video streams at the same time. The computing power of 28 TOPS allows the model to run inference on both video streams at the same time at 10 to 15 frames per second without delays on the data bus. We need only 2 fps.

If one camera notices some unusual activity it immediately checks the second one. If there are signs of torture, sharp objects or violence captured, a hijacking signal is sent: CABIN_COERCION_ATTACK.

The system analyzes sounds of violence, threats, tortures and fear as well using commands from EGPWS and TCAS systems. It detects all the abnormal actions of the passengers and crew mates to detect the possibilities of hijackers taking over the plane control. With the settings similar to FlyDubai FZ1073 flight, where there were fight noises and defensive behavior in the cockpit, an emergency command is generated: INSIDER_THREAT_COCKPIT.

### V.3. COMPRESSION OF THE EVIDENCE BASE AND CRYPTOGRAPHIC SIGNATURE OF THE STATE VECTOR FOR ACARS/CPDLC.
The speed of ACARS channels is several KB/s, making it impossible to send a raw video. That's why the Semantic Supervisor compresses the proof itself into a 64-bit binary status word. A pack of up to 200 bytes is sent to the land using the ACARS or CPDLC radio channels. Three ground operators have to receive a mathematically secured statement and make a decision whether to release the location of the s3-key or not. If two of them refuse to issue it, the key s3 is not issued.

---

## VI. PRINCIPLE OF NEGATIVE INFERENCE DURING PASSENGER CABIN SENSOR SABOTAGE.

During the flight, the Semantic Supervisors send a cryptographic heartbeat to the flight recorder: if a hijacking group in the passenger cabin tries to stop the cameras by breaking lenses or cutting the wires, it is immediately considered sabotage and report an emergency signal to pilots and ground security.

Because the cameras are separated and monitor each other, it is easy to notice any damage and suspicious activities, while the pilots still keep the control over the plane and can have an emergency landing in the nearest available airport.

---

## VII. CONCLUSION

The proposed three-factor key concept for the cockpit solves a major security problem in modern aviation. It solves the conflict between two sides: pilot cockpit vs. crew and passenger cabin separated by a single door, providing access to arbitrators on the ground. Most importantly, the system keeps the strict "Human-in-the-Loop" principle: the Edge-AI acts only as an onboard verifier, while all critical decisions and the final granting of access always remain in the hands of human operators.

---

## References

1. Federal Aviation Administration (FAA). (2002). Title 14 CFR Part 25: Airworthiness Standards: Transport Category Airplanes (FAR 25.795: Transport category airplanes flight deck security). Washington, D.C.: U.S. Government Publishing Office.
2. European Aviation Safety Agency (EASA). (2015). Safety Information Bulletin SIB No. 2015-04: Authorised Persons on the Flight Crew Compartment. Cologne: EASA.
3. Bureau d'Enquêtes et d'Analyses pour la sécurité de l'aviation civile (BEA). (2016). Final Report: Accident on 24 March 2015 at Prads-Haute-Bléone (Alpes-de-Haute-Provence, France) to the Airbus A320-211 registered D-AIPX operated by Germanwings (Report No. bea2015-0125.en). Le Bourget: BEA.
4. General Authority of Civil Aviation (GACA) / Flight Safety Records. (2026). Incident Preliminary Docket: FlyDubai Flight FZ1073 (Boeing 737 MAX 8, A6-FKF), September 30, 2026.
5. Shamir, A. (1979). How to share a secret. Communications of the ACM, 22(11), 612-613.
6. DeepMind / Google. (2024). Gemma: Open Models Based on Gemini Research and Technology. Technical Report.

---
Copyright (c) 2026 Marianna Zdor, Liubov Zdor. All rights reserved.
