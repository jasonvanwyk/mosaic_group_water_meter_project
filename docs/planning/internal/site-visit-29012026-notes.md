site visit conducted by Jason van Wyk Precept Systems
date: Wednesday 29 January 2026, time: 12:00pm, finnished 13:45
locations: 447 Anton Lembede Street Durban
client: Mosaic Group
Attendees: Tanya Dowley (part owner), Ryan (inhouse plumber), Sumir (inhouse IT Admin/technician) see email correspondecne for contact details

Building name: City Life
Number of Floors: 27, but we are only concerned with the apartments on the 1st 12 floors - the higher floors are for a different set of clients.

number of units: 48 per floor = 576 units for the 12 floors.
Building age: approx 60-years old.

Brief and Context:
Client is looking to install a water metering and monitoring system for the 1st 12-floors. this includes meters - one per apartment/unit and one bulk meter per floor. unit pipe size is 16mm, bulk meter pipe size on each floor is 50mm, the building has 1 riser throughout the entire building from the top down to the bottom which is 75mm. it is fed by the municiplaity - only one municipal feed and the municipal meter is on the pavement of the building - ground level. the municipla feed goes up to the top of the building and enters the building at the top. it then gravity feeds down the 75mm riser to the 15th floor, where it is then further pumped to the rest of the bottom floors. the municipality has access to its ground floor pavement meter and reads the meter monthly. The building management also reads the municipal meter every two weeks as well.

The current setup for water to the tennents; is that mosaic groups adds a flat rate of R400 to each tennents rent bill every month. problem is that there is abuse of this simple setup - and no motivation for tennents to use water sparigly and diligently - some not even reporting leaks - so some clients use and or waste a lot more than others. Problem 2: theres no visibility of actual water usage, and no leak detection/prevention. you cant measure what you cant see. problem 3; plain meters with no telementry would incur timetaking labout to go and read each meter manually - would also introduce human error and non-reliability - say if the person taking the readinsg did not come to work, no readings would be taken. problem 4: the building pays for the water regardless of what the tennents pay or use. problem 5; its one thing to control and manage your own property water, but it becomes a nightmare when there is goverment or municipal mandatory water restrictions - no way of forcing tennents to adhere and building get penalised instead of client. problem 6: friction between silos - water metering solution still needs to be captured manually into invoicing system - need api intergrastion to make that automatic and frictionless, and minimise disputes. problem 7: no way of throttling water if tenent does not pay for water or rent (note that this would involve a motorised ball valve and is not neccesarrily wanted by the client in phase 1 of the rollout - but may still want if costing is acceptable.), and there is also no way of remotley shutting down each uniot and or each floor's water in case of an emargancy leak. NB!! VERY IMPORTANT!! WARNING!! OUT OF SCOPE!! any proposed water metering solution does not include and should be seperated from and isolated form fire alarm and sprinkler system!! This system is pureley for tenent water usage metering, monitoring and control.
Problem: Prepaid meters are not wanted by the client - either too expensive or results in unwanted consequinces - i.e. tenents not using water to keep apartments clean, and building management does not know about it 


Property Manager onsite: Tanya Dowley

Evidence: see photo of riser and floor main pipe/valve/meter in pic /home/jason/Projects/mosaic_group/pics/city-life-29012026.jpeg

installation begin date - in Q1 2026 - Tanya sitting with costing in 1st week of Feb 2026

Desicion maker: Tanya + board of directors.

Tanya has allocated a budget, but we did not discuss it. she is very price sensitive. Tanya is very keen for phased rollout, starting with a pilot project for possibly one floor and 5 units. (jason concures - minimises risk to all parties.)

Jason did explain becuase the system is so highly modular and customisable, anything thin or combination of things is possible, from multiarchitecture, customised dasboards, phased as well as different types of pahsed rollout is possible. Jason explainded that the system topology would likey comprise of water meters from precsion meters (must use and is obliged to use precision water meters), nodes fro precept, gateway (precept), server and db (precept) motorised ball valve (if needed - precept - pcb boards must be desined with connectors to connect to motorised ball valve should it be needed - must be motorised ball valve ready) exact architecture and tech stack and comms protocol to be confirmed after proper site and area assesment. At this stage its just this building that needs this solution by mosaic group.

Cost of water triggered the need for a solution. current water bill is R1.2m/month current municipal water tarrif is R65.91/kL and monthyl water usage is 10500kL/month - but that is for the entire building including all the floors above the 1st 15 floors.

a person called barry reads the main municipal water meter twice a month.

Leaks are currently only detected visually or by someone reporting it.

Tanya does want to buill the tenents for water usage.

What does success look like? control of amount of water each tenent uses, leak detection/control/prevention. minimised friction between siloed systems (water-monitoring software/system and invoicing system) 

Feature Priority:
h= high, m = medium, l= low:

real-time mopnitoring: h
leak detection alerts: h
per-unit-billing: h
historical reports: h
tennent portal: m (whatsapp/email could be considred a portal)
mobile app: l
billing integration: h
automated readings: h
tamper detection: h
preaure monitoring: l

decision process:
Final decision: Tanya
Who else needs to approve: Board of directors
Procument process: Tanya  

There is no deadline driving the implementation or integrationm - but tanya sitting with pricing 1st week feb 2026. would like to begin q1 of 2026

budget is already allocated - tanya did not share it.

note: motorised ball valves would need a poer source -possible 12v - do research on how to achive this would poe work? ac access per floor and then centralsed 12v dc supply to each meter? risks?

phased payment is prefered.

contraints and concerns:
Mosaic wants least interuption to tennents, least labour installation costs, least infrastrcuture setup ie. ethernet cabling is too much of an infrastructure cost. mosaic would inform tenentts of water supply disrution during installtion. estimated 1 hour installtion time per unit. Ryan will install plumbing of meter, jason will build and configure the system, Sumir will learn from jason and help with installing nodes. presicion will supply meters, jason will procure motorised balled valves if needed. jason will procure all other tech - nodes, gatways, servers, db etc. 

Hosting prefrences: 49%leaning towards on-prem, 51% leaning towards clound - hypervisor like aws google cloud, azure etc (research others and best options)

System needs a central dashboad, non tenentant facing. needs rbac for admin and users.o

the network has a ubiquity/unifi ecosystem, with 13-15 ap's per floor, and a ubiquity unify secure gateway. The site has a static ip (not yet given to jason), ap's have both 2&5g frequency. WiFi is offered to each tenent - they get given a password. siste has smart managed unify switches per floor. has vlans configured - sumir can give jason a vlan for the water-monitoring system. the building is supplied with 1gb fiber from isp vox

All correspondence to mosaic must be sent to Tanya at this stage only. Tanya will email jason Ryan and Sumirs contact details - see email comms.

Municipal mainline supply is 110mm pipe.

the building reticulation system is not rind feed. the riser (only 1) is accesible. water enters each apartment above each aprtments front door in each passage. pipes and cable run along the passage ceiling and cables in a cable basket along the celing. each unit has a shut-off valve in the passage - so there is an isolation valve per unit. and there is space for a meter at entry. water can be shut off per riser on floor best time for shut-off is during working hours when tennents are out of the building 10am-2pm

leaks seem to be seldom in the building - but, there is no way for buiilding management to detect leaking toiltes ot taps inside apartments, unless they are reported. common leak points are angle joints. Estimated water loss is not easliy estimated.

IT contact Sumir.
all ethern t cable inside building it cat6 solid copper. entire building has coverage.
the comms room / server room is not a dedicated server room, but an office where switch sits on shelf. sumir says that there is rack availibility for server / gateway.

There is UPS power available on site, all to the comms room and to all managed switches. For LoRa or IoT feasibility, the building construction is a mixture of concrete, brick and steel. It has no basements, there is rooftop access for the antenna.

as far as gateway placement options go the comms room does have power it does have ethernet and it is secure the rooftop has power it has ethernet and it is also secure for hosting preferences they would prefer cloud 51% leaning or leaning 49% towards on-prem hybrid they do want integration with the existing cltm erp tenant management system - must research what this system is and api caperbilities etc - who provides the software and are they will ing for api - whos the contact, are there documetation for api - what does there api requires etc.
