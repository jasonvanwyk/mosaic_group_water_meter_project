# **Comprehensive Engineering and Economic Analysis for Large-Scale Advanced Metering Infrastructure: Vertical Urban Deployment at 477 Anton Lembede Street, Durban**

The modernization of urban residential utility management requires a paradigm shift from reactive maintenance to proactive, data-driven infrastructure. At 477 Anton Lembede Street, Durban—a location characterized by the high-density residential profile of City Life and the surrounding City Life developments—the engineering requirements for an Advanced Metering Infrastructure (AMI) are dictated by extreme physical and electromagnetic constraints. Establishing a reliable, 15-story vertical network of 720 nodes necessitates a deep integration of low-power radio frequency (RF) engineering, ultrasonic metrological science, and decentralized server-side logic. To satisfy the mandate for a 5-minute reporting interval and a decade-long battery maintenance cycle, the proposed solution eschews high-energy protocols like WiFi and subscription-locked services like Sigfox or NB-IoT in favor of a privately hosted LoRaWAN architecture. This report provides an exhaustive blueprint for the deployment, analyzing the structural, electromagnetic, and economic variables inherent to the Durban Central Business District (CBD) environment.

## **Site Profile and Electromagnetic Environmental Analysis**

The structural reality of 477 Anton Lembede Street presents a formidable obstacle to traditional wireless communication. As a 15-story high-rise utilizing reinforced concrete and steel-frame construction, the building serves as a massive RF attenuator. Each floor and internal partition wall introduces significant signal degradation, often referred to as path loss. In a vertical residential environment where 48 units are clustered per floor, the density of structural barriers is exacerbated by the presence of bathroom and kitchen utility shafts, which are often lined with metallic piping or foil-backed insulation. Data indicates that an 8-inch reinforced concrete slab can introduce upwards of 23 dB of signal loss, while stone walls can cost approximately 12 dB of the link budget.1 For a signal to travel from a ground-floor gateway to a 15th-floor meter, it would theoretically encounter over 300 dB of attenuation through floors alone, which far exceeds the 150-160 dB maximum link budget of even the most sensitive LPWAN technologies.1

Furthermore, the external RF environment of the Durban CBD is one of extreme saturation. The site is a "concrete canyon," a term used in urban RF engineering to describe environments where high-rise buildings create narrow corridors that reflect and diffract signals, leading to multipath interference and significant fading. In the 2.4 GHz ISM band, the congestion is catastrophic, with 13 to 15 WiFi Access Points (APs) per floor contributing to a noise floor that effectively masks low-power signals.3 This congestion makes consumer-grade protocols like standard WiFi or ESP-NOW unsuitable for utility-grade reliability.5 The reliance on the sub-GHz 868 MHz band (as permitted by South African ICASA regulations) is therefore not optional; it is a fundamental requirement for achieving the necessary building penetration and overcoming the noise floor of the CBD.6

The following table summarizes the signal loss characteristics that must be accounted for in the RF link budget for City Life.

| Structural Component | Typical Thickness | Estimated RF Attenuation (dB) |
| :---- | :---- | :---- |
| Reinforced Concrete Floor | 200 mm | 23.0 |
| Interior Masonry Wall | 110 mm | 3.5 |
| Exterior Stone Wall | 200 mm | 12.0 |
| Steel Fire Door | 45 mm | 15.0 \- 18.0 |
| Wood Partition Wall | 75 mm | 2.8 |

The vertical nature of the 720-node deployment creates a unique "near-far" problem. Meters located in units directly adjacent to a gateway may transmit with excessive power, potentially desensitizing the gateway receiver to weaker signals from meters several floors away. This highlights the necessity for a protocol that supports sophisticated Adaptive Data Rate (ADR) and power control mechanisms.9

## **Comparative Metrological Analysis: Ultrasonic vs. Mechanical Systems**

In the South African urban water context, the "moving parts" failure of mechanical meters is a persistent operational risk. Municipal water in high-density CBD areas often carries grit, suspended solids, and mineral scale, particularly following infrastructure repairs or during periods of low reservoir levels. For a 720-unit high-rise, a failure rate of even 2% per annum in mechanical meters leads to significant billing disputes and high labor costs for physical replacements.

The "Gold Standard" for this vertical environment is the ultrasonic static meter. Ultrasonic technology, utilized by industry leaders such as Kamstrup and Sensus, relies on the transit-time difference of ultrasonic waves to measure flow.11 These meters have no moving parts in the flow path, making them inherently resistant to the abrasive grit found in local water supplies.11

### **Kamstrup flowIQ Series**

The Kamstrup flowIQ 2101 and 2200 series represent the pinnacle of residential submetering. These meters utilize bidirectional ultrasonic transducers to achieve a starting flow rate as low as 2 liters per hour, ensuring that even small leaks—a common issue in aging high-rise plumbing—are accurately recorded.11 The flowIQ 2200, specifically, incorporates Acoustic Leak Detection (ALD), which allows the meter to "listen" to the pipe network and alert the support team to potential bursts or leaks before they cause structural damage.15 For the Lead IoT Architect, this provides a secondary diagnostic layer beyond simple pulse counting.14

### **Sensus ECCUS and Cordonel**

The Sensus ECCUS is an ultrasonic meter designed for flexibility, integrating both LoRaWAN and Wireless M-Bus (wM-Bus) communication.13 Its R500 accuracy ratio remains stable throughout its 15-year design life, as there are no mechanical components to wear down and lose calibration over time.13 This stability is critical for high-density residential billing, where precision is a legal and financial requirement.

### **Lesira-Teq Smart Water Management Units (WMU)**

Lesira-Teq, a prominent South African OEM, offers the WMU series which can be retrofitted to existing pulse-output meters or integrated into fully smart prepaid units.16 Their hardware supports LoRaWAN and Sigfox and is designed for the harsh environmental conditions of South African utility shafts.16 The Lesira units include integrated manual shut-off valves in certain configurations, allowing the server-side logic to remotely disconnect units for non-payment or detected leaks.16

| Metrological Parameter | Mechanical (Piston/Disc) | Ultrasonic (Static) |
| :---- | :---- | :---- |
| **Wear and Tear** | High (Mechanical friction) | None (No moving parts) |
| **Grit Sensitivity** | Prone to jamming/failure | Immune to particulates |
| **Minimum Flow Rate** | 15 \- 30 L/h | 2 L/h |
| **Maintenance Cycle** | 5 \- 7 Years | 15 \- 20 Years |
| **Billing Accuracy** | Declines over time | Sustained over life |

The deployment at 477 Anton Lembede Street must prioritize ultrasonic meters to eliminate the high-touch maintenance cycle associated with the grit-laden water of the Durban CBD.11

## **Protocol Selection: Resolving the 720-Node Collision Problem**

Achieving a 5-minute reporting interval for 720 nodes in a single building requires a protocol that can handle high packet density while minimizing Time-on-Air (ToA). The three protocols evaluated for this vertical density were Wireless M-Bus, ESP-NOW, and Private LoRaWAN.

### **Wireless M-Bus (wM-Bus)**

Wireless M-Bus is a popular standard for submetering, but its architectural limitations in a high-rise environment are significant. While it operates in the 868 MHz band, its communication range is shorter than LoRaWAN, often requiring more frequent repeaters or gateways in a concrete-dense building.6 More importantly, standard wM-Bus lacks the advanced collision avoidance and ADR features found in LoRaWAN, which are essential for managing 720 devices talking every 300 seconds.

### **ESP-NOW (over 2.4 GHz)**

ESP-NOW, a proprietary protocol by Espressif, offers high data rates and low latency.19 However, its operation in the 2.4 GHz band is its primary disqualifier for City Life. The extreme WiFi congestion (13-15 APs per floor) would result in catastrophic packet loss as the 720 nodes struggle to find clear airtime.5 Furthermore, the energy required for WiFi-based radio transmission (\~100mA) is roughly three to four times that of LoRa radios, making a 10-year battery life on AA cells an engineering impossibility at a 5-minute interval.19

### **Private LoRaWAN: The Engineering Solution**

LoRaWAN is designed specifically for high-node-density environments with challenging propagation paths. It utilizes Chirp Spread Spectrum (CSS) modulation, which provides a significant processing gain, allowing signals to be demodulated even when they are below the noise floor.8 To manage the 5-minute reporting interval for 720 nodes, LoRaWAN employs several strategies:

1. **Adaptive Data Rate (ADR):** The network server monitors the SNR and RSSI of every uplink. For nodes with strong signals (e.g., those on the same floor as a gateway), the server commands the device to use Spreading Factor 7 (SF7). This minimizes Time-on-Air (approx. 61ms for a 10-byte payload) and reduces the probability of a collision with another node.9  
2. **Frequency Hopping:** LoRaWAN nodes hop across multiple channels (typically 8 channels in the 868 MHz band), spreading the cumulative traffic load and reducing the mathematical probability of two nodes transmitting on the same frequency at the same time.21  
3. **Orthogonality of Spreading Factors:** Packets transmitted simultaneously on the same channel but using different Spreading Factors (e.g., one node on SF7 and another on SF10) can often be demodulated successfully by the gateway, effectively increasing the total network capacity.23

The mathematical probability of collision (![][image1]) in an ALOHA-based network like LoRaWAN is given by ![][image2], where ![][image3] is the offered traffic load. By utilizing multiple gateways and the lowest possible SF settings, the value of ![][image3] is kept low, ensuring that the packet delivery ratio (PDR) remains above 95% even with 720 nodes.22

## **Server-Side Logic and Private Network Management**

The "non-negotiable" constraint of zero third-party subscriptions necessitates the hosting of a private Network Server (NS) and Application Server (AS). ChirpStack is the recommended open-source stack for this deployment due to its MIT license, modularity, and extensive API for GUI integration.26

### **ChirpStack vs. The Things Stack (TTS)**

While The Things Stack offers a robust platform, its self-hosted open-source version can be complex to manage for a single-building deployment. ChirpStack is known for its horizontal scalability and ease of integration into custom dashboards.26 It provides a gRPC-based API that allows the Lead Architect to build a proprietary management interface for City Life, enabling features like live frame logging and remote valve control without external licensing fees.29

### **Deduplication and Multi-Gateway Management**

In a vertical high-rise, an uplink from a single meter on the 8th floor will likely be heard by gateways on floors 4, 8, and 12\. ChirpStack handles this through a deduplication window.31 When the first packet arrives, the NS opens a window (e.g., 200ms) to collect all copies of the same packet. It then selects the packet with the highest SNR to determine the best return path for any downlink commands, such as an ADR adjustment or a valve closure.31 This multi-gateway architecture provides inherent redundancy; if the gateway on floor 8 fails, the gateways on floors 4 and 12 will continue to receive the data, ensuring zero data loss.31

| Feature | ChirpStack (Private) | Public Sigfox / NB-IoT |
| :---- | :---- | :---- |
| **Monthly Subscription** | R0 | R15 \- R45 per node |
| **Data Ownership** | 100% (On-Premise) | Third-party cloud |
| **Downlink Latency** | Near-instant (Class A/C) | High (Network dependent) |
| **Customizability** | Full API access | Limited to provider tools |
| **Scale** | Unlimited by license | Subject to tier fees |

By utilizing ChirpStack, the Senior IoT Solutions Architect maintains full control over the metadata of the network, which is critical for the "granular diagnostics" required for the support team.26

## **The "Gold Standard" Architecture for 477 Anton Lembede Street**

The proposed architecture for City Life is a multi-gateway, high-density LoRaWAN network optimized for minimal Time-on-Air and maximal battery life.

### **Gateway Placement and Backhaul**

A single gateway is insufficient for 15 floors of concrete. The recommended deployment uses five industrial-grade gateways placed as follows:

* **Basement/Plant Room:** 1 Gateway (Covers ground level and basement utility rooms).  
* **Floor 4:** 1 Gateway.  
* **Floor 8:** 1 Gateway.  
* **Floor 12:** 1 Gateway.  
* **Roof:** 1 Gateway (Covers the top floors and provides external signal for perimeter water mains).

Each gateway must be connected via Power-over-Ethernet (PoE) to the building's core fiber network. Wired backhaul is essential because WiFi-based backhaul at this site would be subject to the same 2.4 GHz interference that disqualifies ESP-NOW.1 For extreme redundancy, gateways with integrated 4G/LTE failover should be utilized, ensuring that the network remains alive even if the building's primary ISP is offline.34

### **Spreading Factor and ADR Settings**

To achieve the 10-year battery life, the network must be tuned to keep as many devices as possible on SF7. The Time-on-Air for an SF7 packet is roughly 1/20th of an SF12 packet.24 The 5-minute interval for 720 nodes results in 2,016,000 uplinks per week across the building. If nodes are allowed to drift to SF11 or SF12, the collision probability increases significantly.22

The Lead Architect must configure the ChirpStack ADR algorithm with a conservative "Installation Margin" of 10 dB. This ensures that the server only commands a node to a faster data rate when there is a significant signal buffer, preventing the "flapping" of data rates that can occur in urban environments with fluctuating noise floors.10

### **Payload Optimization**

To minimize airtime, the data payload must be kept small. Instead of sending complex JSON strings, the meters should transmit a compact Binary Coded Decimal (BCD) or Hexadecimal payload representing the cumulative totalizer and a status byte (e.g., leak alarm, tamper flag). A 10-byte payload is sufficient for water metering and ensures the shortest possible Time-on-Air.22

## **Economic Analysis and Total Cost of Ownership (TCO)**

A 10-year TCO analysis for 720 nodes reveals that the labor cost of maintenance in South Africa is the most significant long-term variable.

### **Labor Costs in the South African Context**

The cost of hiring an electrician or plumber in South Africa for utility meter maintenance is substantial. Current data from platforms like Kandua indicate that standard hourly rates for residential electrical and plumbing work range from R450 to R850, with call-out fees between R650 and R950.37 For a 720-unit high-rise, even a simple battery replacement project requires approximately 180 man-hours (assuming 15 minutes per unit for access, replacement, and testing).

### **TCO Comparison: WiFi vs. Private LoRaWAN**

A WiFi-based solution (ESP-NOW) using standard AA batteries would likely require replacement every 18-24 months due to the higher power draw of the radio.19 Over 10 years, this results in five replacement cycles. In contrast, a LoRaWAN solution using high-quality Lithium Thionyl Chloride (Li-SOCl2) batteries and operating on SF7 can realistically achieve a 10-year lifespan.8

| TCO Component (10 Years) | WiFi / ESP-NOW | Private LoRaWAN |
| :---- | :---- | :---- |
| **Initial Hardware (720 Nodes)** | R432,000 (R600/node) | R864,000 (R1200/node) |
| **Infrastructure (GWs/Backhaul)** | R30,000 | R85,000 |
| **Battery Replacement Labor** | R495,000 (5 cycles) | R0 (10-year life) |
| **Battery Material Costs** | R125,000 | R45,000 (Initial) |
| **Subscription Fees (Managed)** | R0 (Self-hosted) | R0 (Self-hosted) |
| **Billing Loss (Meter Accuracy)** | R180,000 (Mechanical wear) | R0 (Ultrasonic stability) |
| **Total 10-Year TCO** | **R1,262,000** | **R994,000** |

The LoRaWAN solution, while having a higher initial CAPEX (approximately 2x the node cost), results in a total saving of over R260,000 over 10 years by eliminating the labor-intensive battery replacement cycles and reducing non-revenue water (NRW) through sustained ultrasonic accuracy.11

## **Redundancy and Maintenance Plan**

Ensuring the system is "low-touch" requires a multi-layered redundancy strategy that leverages both hardware and software features.

### **Gateway Failure Scenarios**

If the gateway on Floor 8 fails, the meters on that floor are not orphaned. Because of the vertical nature of the building, signals from Floor 8 will penetrate the concrete slabs to reach the gateways on Floor 4 and Floor 12\.2 The ChirpStack NS will detect the loss of the Floor 8 gateway and, through the ADR mechanism, will automatically command the nodes on that floor to increase their transmission power or shift from SF7 to SF8/SF9 to ensure they maintain contact with the remaining gateways.10 This "self-healing" capability is a core advantage of the LoRaWAN architecture.

### **Diagnostic Monitoring and Alerts**

To provide "granular diagnostics," the support team should utilize an integration between ChirpStack and a monitoring tool like Grafana.

1. **Gateway Health:** SNMP or MQTT-based monitoring of gateway CPU temperature, backhaul latency, and packet processing rates.30  
2. **Node Health:** Real-time tracking of Battery Voltage, RSSI, and SNR for every unit. An automated alert is triggered if a node's voltage drops below a threshold (e.g., 3.1V for a 3.6V cell), allowing for planned maintenance rather than emergency repairs.40  
3. **Frame Counter Validation:** ChirpStack tracks the Frame Counter (FCnt) for every uplink. A sudden jump in FCnt or a cessation of increments alerts the team to potential firmware issues or tampering.32

### **Meter-Level Resilience**

Meters like the Kamstrup flowIQ or the Lesira-Teq units include internal data logging. If the entire LoRaWAN network goes offline (e.g., during a catastrophic building power failure), the meters continue to count pulses and store the totalizer locally. Once the network is restored, the next uplink contains the current cumulative reading, ensuring no consumption data is lost and billing remains accurate.16

## **Strategic Summary and Client Rationale**

The transition to a private LoRaWAN-based AMI system at 477 Anton Lembede Street is an investment in infrastructure autonomy. The following points summarize the strategic advantages that should be presented to the client to justify this solution over third-party or WiFi-based alternatives.

### **Robustness in the Durban CBD**

Traditional WiFi and 2.4 GHz systems are designed for the home, not for utility-grade infrastructure in a dense CBD. The interference at City Life will render WiFi-based meters unreliable within months of deployment. LoRaWAN’s sub-GHz frequency and CSS modulation are specifically engineered to cut through the noise floor and penetrate the reinforced concrete of high-rise buildings, ensuring that every 5-minute reading is delivered.4

### **Complete Financial Autonomy**

By self-hosting the network on ChirpStack, the client is freed from the "subscription trap." Standard managed IoT services (Sigfox, cellular NB-IoT) often charge a per-meter monthly fee. For 720 nodes, a R20 monthly fee translates to R172,800 per year. Over a 10-year lifespan, the client would pay over R1.7 million in connectivity fees alone. The proposed private network eliminates this entire cost center, making the system significantly more profitable for the property owner.34

### **Future-Proof Metrology**

Ultrasonic meters are a one-time investment in 20-year accuracy. Unlike mechanical meters that jam on Durban's water grit and lose accuracy as their gears wear, ultrasonic meters remain as precise in Year 15 as they are on Day 1\.11 This ensures revenue protection and eliminates the labor-intensive "fail-and-replace" cycle of mechanical units.11

### **Scalability and Low-Touch Operation**

The system is designed for a Senior Architect to "set and forget." With automated self-healing through ADR, high-availability server clusters, and native redundancy across five gateways, the support team only needs to intervene when alerted to specific hardware faults or tampering. This low-touch model is essential for maintaining a 720-node network without a large dedicated staff.10

In conclusion, the integration of Kamstrup or Lesira-Teq ultrasonic hardware with a private ChirpStack-managed LoRaWAN network represents the optimal balance of CAPEX and OPEX for 477 Anton Lembede Street. It solves the collision problem, overcomes the structural attenuation of the concrete high-rise, and delivers a decade of reliable data without the burden of third-party subscriptions.

#### **Works cited**

1. LoRaWAN Gateway Placement Checklist \- CHOOVIO IoT Solutions, accessed January 30, 2026, [https://www.choovio.com/lorawan-gateway-placement-checklist/](https://www.choovio.com/lorawan-gateway-placement-checklist/)  
2. Gateway for covering a large concrete building \- best practices \- The Things Network, accessed January 30, 2026, [https://www.thethingsnetwork.org/forum/t/gateway-for-covering-a-large-concrete-building-best-practices/39813](https://www.thethingsnetwork.org/forum/t/gateway-for-covering-a-large-concrete-building-best-practices/39813)  
3. LoRaWAN vs WiFi: Which is Better for Smart Building Communication? \- Forest Rock, accessed January 30, 2026, [https://www.forestrock.co.uk/lorawan-vs-wifi/](https://www.forestrock.co.uk/lorawan-vs-wifi/)  
4. Wi-Fi vs. LoRaWAN for Environmental Monitoring: Which is Right for You? \- Dickson data, accessed January 30, 2026, [https://apac.dicksondata.com/wi-fi-vs-lorawan-for-environmental-monitoring-which-is-right-for-you/](https://apac.dicksondata.com/wi-fi-vs-lorawan-for-environmental-monitoring-which-is-right-for-you/)  
5. ESP-NOW vs LoRa range : r/esp32 \- Reddit, accessed January 30, 2026, [https://www.reddit.com/r/esp32/comments/j7l4fy/espnow\_vs\_lora\_range/](https://www.reddit.com/r/esp32/comments/j7l4fy/espnow_vs_lora_range/)  
6. LoRaWAN & M‑Bus for Smart and Efficient Metering \- ista SE, accessed January 30, 2026, [https://www.ista.com/template/technologies/lora-mbus/](https://www.ista.com/template/technologies/lora-mbus/)  
7. A Comparative Study of LoRaWAN, SigFox, and NB-IoT for Smart Water Grid \- SciSpace, accessed January 30, 2026, [https://scispace.com/pdf/a-comparative-study-of-lorawan-sigfox-and-nb-iot-for-smart-3gz85360s4.pdf](https://scispace.com/pdf/a-comparative-study-of-lorawan-sigfox-and-nb-iot-for-smart-3gz85360s4.pdf)  
8. LoRaWAN Smart Water Metering \- Kamstrup, accessed January 30, 2026, [https://www.kamstrup.com/en-en/insights/lorawan-smart-water-metering](https://www.kamstrup.com/en-en/insights/lorawan-smart-water-metering)  
9. Design and Evaluation of Multi-Floor Lorawan ... \- RSIS International, accessed January 30, 2026, [https://rsisinternational.org/journals/ijriss/uploads/vol9-iss12-pg429-441-202512\_pdf.pdf](https://rsisinternational.org/journals/ijriss/uploads/vol9-iss12-pg429-441-202512_pdf.pdf)  
10. Adaptive data-rate (ADR) \- ChirpStack open-source LoRaWAN® Network Server documentation, accessed January 30, 2026, [https://www.chirpstack.io/docs/chirpstack/features/adaptive-data-rate.html](https://www.chirpstack.io/docs/chirpstack/features/adaptive-data-rate.html)  
11. flowIQ® 3100 \- Kamstrup, accessed January 30, 2026, [https://www.kamstrup.com/en-en/product-centre/flowiq-3100](https://www.kamstrup.com/en-en/product-centre/flowiq-3100)  
12. flowIQ® 4200 \- Kamstrup, accessed January 30, 2026, [https://www.kamstrup.com/en-en/product-centre/flowiq-4200](https://www.kamstrup.com/en-en/product-centre/flowiq-4200)  
13. Sensus ECCUS® Smart Ultrasonic Water Meter | Xylem South Africa, accessed January 30, 2026, [https://www.xylem.com/en-za/products--services/metrology-equipment-for-utilities/meters/eccus-water-meter/](https://www.xylem.com/en-za/products--services/metrology-equipment-for-utilities/meters/eccus-water-meter/)  
14. MULTICAL® 21 / flowIQ® 21xx \- Kamstrup, accessed January 30, 2026, [https://www.kamstrup.com/en-en/product-centre/multical-21](https://www.kamstrup.com/en-en/product-centre/multical-21)  
15. Kamstrup water solutions | Smart water metering, accessed January 30, 2026, [https://www.kamstrup.com/en-en/water-solutions](https://www.kamstrup.com/en-en/water-solutions)  
16. Water Management Unit: Technical Brochure \- Lesira-Teq, accessed January 30, 2026, [http://lesira.co.za/wp-content/uploads/2023/05/WMU-Technical-Brochure-2.pdf](http://lesira.co.za/wp-content/uploads/2023/05/WMU-Technical-Brochure-2.pdf)  
17. Smart Water Meter \- Lesira-Teq, accessed January 30, 2026, [https://lesira.datadigital.co.za/service/smart-water-meter/](https://lesira.datadigital.co.za/service/smart-water-meter/)  
18. Smart Prepaid Meter: Technical Brochure \- Lesira-Teq, accessed January 30, 2026, [http://lesira.co.za/wp-content/uploads/2023/05/SPM-Brochure-.pdf](http://lesira.co.za/wp-content/uploads/2023/05/SPM-Brochure-.pdf)  
19. Data transmission reliability over ESP-NOW protocol in indoor environment, accessed January 30, 2026, [https://developer.espressif.com/blog/reliability-esp-now/](https://developer.espressif.com/blog/reliability-esp-now/)  
20. Performance Evaluation of LoRa Networks in a Smart City Scenario \- ResearchGate, accessed January 30, 2026, [https://www.researchgate.net/publication/314278433\_Performance\_Evaluation\_of\_LoRa\_Networks\_in\_a\_Smart\_City\_Scenario](https://www.researchgate.net/publication/314278433_Performance_Evaluation_of_LoRa_Networks_in_a_Smart_City_Scenario)  
21. How Often Can LoRaWAN® Devices Send Data? \- tektelic, accessed January 30, 2026, [https://tektelic.com/expertise/how-often-can-lorawan-devices-send-data/](https://tektelic.com/expertise/how-often-can-lorawan-devices-send-data/)  
22. How many gateways are needed to support 1000 nodes sending 10 bytes per 5 minutes?, accessed January 30, 2026, [https://www.thethingsnetwork.org/forum/t/how-many-gateways-are-needed-to-support-1-000-nodes-sending-10-bytes-per-5-minutes/5527](https://www.thethingsnetwork.org/forum/t/how-many-gateways-are-needed-to-support-1-000-nodes-sending-10-bytes-per-5-minutes/5527)  
23. Predicting LoRaWAN Capacity | Semtech, accessed January 30, 2026, [https://www.semtech.com/uploads/technology/LoRa/predicting-lorawan-capacity.pdf](https://www.semtech.com/uploads/technology/LoRa/predicting-lorawan-capacity.pdf)  
24. Spatial Issues in Modeling LoRaWAN Capacity, accessed January 30, 2026, [http://lpwan.conf.citi-lab.fr/pres/duda.pdf](http://lpwan.conf.citi-lab.fr/pres/duda.pdf)  
25. Maximum Lora nodes per GW : r/LoRaWAN \- Reddit, accessed January 30, 2026, [https://www.reddit.com/r/LoRaWAN/comments/1g28eue/maximum\_lora\_nodes\_per\_gw/](https://www.reddit.com/r/LoRaWAN/comments/1g28eue/maximum_lora_nodes_per_gw/)  
26. In-Depth Comparison of LoRaWAN Network Servers: ThinkLink, TTS, ChirpStack, Loriot, and Actility | by Manthink\_LoRa | Medium, accessed January 30, 2026, [https://medium.com/@manthinkmt/in-depth-comparison-of-lorawan-network-servers-thinklink-tts-chirpstack-loriot-and-actility-9bbb55a6866c](https://medium.com/@manthinkmt/in-depth-comparison-of-lorawan-network-servers-thinklink-tts-chirpstack-loriot-and-actility-9bbb55a6866c)  
27. In-Depth Comparison of LoRaWAN Network Servers: ThinkLink, TTS, ChirpStack, Loriot, and Actility \- DEV Community, accessed January 30, 2026, [https://dev.to/manthink/in-depth-comparison-of-lorawan-network-servers-thinklink-tts-chirpstack-loriot-and-actility-45l8](https://dev.to/manthink/in-depth-comparison-of-lorawan-network-servers-thinklink-tts-chirpstack-loriot-and-actility-45l8)  
28. Chirpstack vs TTN (The Things Network) \- Rokland, accessed January 30, 2026, [https://store.rokland.com/blogs/news/chirpstack-vs-ttn-the-things-network](https://store.rokland.com/blogs/news/chirpstack-vs-ttn-the-things-network)  
29. Introduction \- ChirpStack open-source LoRaWAN® Network Server ..., accessed January 30, 2026, [https://www.chirpstack.io/docs/index.html](https://www.chirpstack.io/docs/index.html)  
30. Architecture \- ChirpStack open-source LoRaWAN® Network Server documentation, accessed January 30, 2026, [https://www.chirpstack.io/docs/architecture.html](https://www.chirpstack.io/docs/architecture.html)  
31. Late Arriving Duplicate Uplink Packets \- ChirpStack Community Forum, accessed January 30, 2026, [https://forum.chirpstack.io/t/late-arriving-duplicate-uplink-packets/24127](https://forum.chirpstack.io/t/late-arriving-duplicate-uplink-packets/24127)  
32. ChirpStack Deduplication Issue with Multiple Gateways, accessed January 30, 2026, [https://forum.chirpstack.io/t/chirpstack-deduplication-issue-with-multiple-gateways/23934](https://forum.chirpstack.io/t/chirpstack-deduplication-issue-with-multiple-gateways/23934)  
33. Gateways lose connection to server and Deduplication error \- Chirpstack Forum, accessed January 30, 2026, [https://forum.chirpstack.io/t/gateways-lose-connection-to-server-and-deduplication-error/21581](https://forum.chirpstack.io/t/gateways-lose-connection-to-server-and-deduplication-error/21581)  
34. Private vs. Public LoRaWAN Networks: What to Choose for Resource Management Projects, accessed January 30, 2026, [https://jooby.eu/blog/private-vs-public-lorawan-networks-what-to-choose-for-resource-management-projects/](https://jooby.eu/blog/private-vs-public-lorawan-networks-what-to-choose-for-resource-management-projects/)  
35. Battery Calculator for Strips LoRaWAN sensors \- Sensative AB, accessed January 30, 2026, [https://sensative.com/sensors/strips-sensors-for-lorawan/sensative-strips-lora-battery-calculator/](https://sensative.com/sensors/strips-sensors-for-lorawan/sensative-strips-lora-battery-calculator/)  
36. LoRa® Calculator \- Semtech, accessed January 30, 2026, [https://www.semtech.com/design-support/lora-calculator](https://www.semtech.com/design-support/lora-calculator)  
37. Inverter & Battery Repair Costs in SA: Troubleshooting & Restoring Your Backup Power System \- Kandua, accessed January 30, 2026, [https://kandua.com/cost-guides/inverter-battery-repair-costs-in-sa-troubleshooting-restoring-your-backup-power-system](https://kandua.com/cost-guides/inverter-battery-repair-costs-in-sa-troubleshooting-restoring-your-backup-power-system)  
38. Electrician Prices 2026: Detailed Guide to Electrician Call Out Rate and Electrical Services in South Africa \- Carports Pretoria, accessed January 30, 2026, [https://thirtyone.co.za/electrician-prices/](https://thirtyone.co.za/electrician-prices/)  
39. Cost Guides | Latest Home Maintenance Prices in South Africa \- Kandua, accessed January 30, 2026, [https://kandua.com/cost-guides](https://kandua.com/cost-guides)  
40. How to Choose a Long Life Smart Meter Battery?, accessed January 30, 2026, [https://www.longsingtechnology.com/smart-meter-battery/](https://www.longsingtechnology.com/smart-meter-battery/)  
41. LTE-M vs Private LoRaWAN® Connectivity Cost – Which Is Lower? \- tektelic, accessed January 30, 2026, [https://tektelic.com/expertise/lte-m-vs-private-lorawan-connectivity-cost-which-is-lower/](https://tektelic.com/expertise/lte-m-vs-private-lorawan-connectivity-cost-which-is-lower/)  
42. Comparison LoRaWAN and Wi-Fi HaLow Study Case for Smart Metering in Urban Area, accessed January 30, 2026, [https://www.researchgate.net/publication/383092352\_Comparison\_LoRaWAN\_and\_Wi-Fi\_HaLow\_Study\_Case\_for\_Smart\_Metering\_in\_Urban\_Area](https://www.researchgate.net/publication/383092352_Comparison_LoRaWAN_and_Wi-Fi_HaLow_Study_Case_for_Smart_Metering_in_Urban_Area)  
43. Help with Packet Loss of Data and ADR Algorithm \- Chirpstack Forum, accessed January 30, 2026, [https://forum.chirpstack.io/t/help-with-packet-loss-of-data-and-adr-algorithm/23155](https://forum.chirpstack.io/t/help-with-packet-loss-of-data-and-adr-algorithm/23155)  
44. Smart Meter Battery Life: How Long Do They Really Last?, accessed January 30, 2026, [https://www.ytl-e.com/news/quarterly-publication/how-long-do-batteries-last-in-a-smart-meter.html](https://www.ytl-e.com/news/quarterly-publication/how-long-do-batteries-last-in-a-smart-meter.html)  
45. LoraWan Theory \- rpi4cluster.com, accessed January 30, 2026, [https://rpi4cluster.com/lorawan-rpi4-theory/](https://rpi4cluster.com/lorawan-rpi4-theory/)  
46. Smart Water Meter | PDF \- Scribd, accessed January 30, 2026, [https://www.scribd.com/document/372609039/Smart-Water-Meter](https://www.scribd.com/document/372609039/Smart-Water-Meter)

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAYCAYAAAAYl8YPAAABEElEQVR4XmNgGAWDCpgC8W4gfgfE/4H4OpQPwgeh4vOBWASmgRiwjAFimA6auCYQf2OAGE40eAzET9AFoeAiA8QidXQJbECPAaJ4MboEEPAwQFwGwoJoclhBKQPEsCR0CSDIYIDINaFL4AK7GCAaVJDEQK5IAOJXQNwOxExIcjgByBs/GSCGgQJ5HxA/YoB4axYQGyKUEga+DLjDi2QwiQFiWDy6BDngLgPEMAl0CSwgD4gXAvFUIPZGk2NQYoAYdApdAguoB+IZUPZkIJ4Ck7AG4hNA/IsBYthzBkjge8EUoAFQzH5nwMwdZAEXBkgOoQpQZYCELQwoAHEOEp9kUMwACataIM4FYk5U6VFALAAAqBw3JObyl8UAAAAASUVORK5CYII=>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAHQAAAAYCAYAAAArrNkGAAAC8UlEQVR4Xu2ZWahNURjH/5QMIVOZSohEIg8KtyQ8yOzBE3UNhWRM8qRbHngwZR7rJoUHmR6McSkJRRkKSQmlJHlBlPj/77eOs87qHPceunvvc1q/+nX3+tY+p33Xt9f69toHiEQiLcJwepI+pQdop8LuRobSY/QU3Urr6Hi60D8pkg0aaG/ahp6BJc5nA31Cx3ixGvqLjvRiVUErOiAMVhAd6Ec63bVrYYlq7dpL6Bva17V9HoaBSqYPnU2v0bNBXyWzkb50x13oJ7oo313AzDDgMxo2OPoC3SHPXFvecvF62iP3gRRZRR/TvfQHqieh7ehruti1l8L+P83if+YELKEq1D4qyl9hCc4SX5DdhM6CzbhSdsuf2sgRutprb6N3vLaYR/fA8rAD+aW5JG/puzDoeARL9pCwI0WynNByUK2c644nur9r6T137KMn2+dhsBgjYAk7HnaQjrAZKrsGfWlSDQkdS3fSCU5tXURP2OSa49pCy+9Nut+LlWQ9LKHFivAyWN+msKMJdsE+11xVr8sh7YR2h43JFXqbXkZ5N7z2nEqaPwb6jhx6gj9NH9CLdDNs+zLFO6ckV2FfOMiL6eIW0A90C5qxZidMmgmdBBuX+V5MdU03f+poSf0OS6gK7g3Y/kdL7GE6Kn9qplBCz4XBBNBNr/HSg4vQflg7Bc2ifrmT0mQGLJnF6meWKSehy5HfijWltkR/QzNR46U3O3piPUTXwOpeJtgNu8DasOM/KbeGNtjHmo0Sej4MJsAl+hnZK0F/eAUb0F5hRxG0sdf7xn10WtCXNErohTCYAPX0RRgknZGB96sDYcm8H3YUoY4edMfa4Da1NLUkbek3eh1Ww5JE715VQ4d5Me3P79JxXixRamAXoNdLSuh7WP2Y6p/koSdeDWD4FilptPHWdetacku1Vhhdux7ukmI7/QnbF2qHcBT2q0nFMBn2JilSJQyGzYQc/ekKrx2pQNbBaqce1VfS9oXdkUgkEolEIpXFb1l+tpETpaI1AAAAAElFTkSuQmCC>

[image3]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA8AAAAYCAYAAAAlBadpAAAA8ElEQVR4XmNgGAVUAfJAXAfE+4H4HhAfB+LLQJwEle8BYnsoGwXUAvE3ID4PxB5AzA4VZwbiJiA+DZXngIrDwTwg/g/EDUDMhCoFBixA/BiIt6FL5DEgNOIDO4C4CFlADIg/AvFdBoQzcYEOIFZDFuhjgNiKYiKx4DoDRLMeugQhAAoEkMav6BJQAIoqkDwyBoU6HIDi8DcD9hCGgUdA/AuINdElahggJjqgicOABQNEfj26BAiAnL4TiN8DcQwQKwMxKwMkYXgC8Qkg/g7EcTAN6ADk5HQgPgLELxkgNoHoyUAsAsSBQCwEVz0KhhIAAASOL7vx+xPaAAAAAElFTkSuQmCC>