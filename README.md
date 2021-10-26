# PD2map_CapitolSavingsBank
Template for overall files and info. Curently WIP.

"U.S. Capitol Savings Bank"
Full technical description + storyline

STORY:
Two plans:
1)Standard as common hacking'n'cracking'n'robbing. Like usual heist.
2)Insider's help as impersonation of company's deposit injection(fake of course) while temp storage is not suitable. Forсing to reshedule vault opening.

Plan A (stealth)
We starting in casing mode.
+ 1st objective: find server room, Bain wants to know details.
+ 2nd objective: hack Admin's PC in IT room. He is our hostage now. And it fails.
+ 3rd objective: find some terminal with open session to VaultControlSystem. Bain offers manager's PC. (May be switced to right one PC search, like in Big bank, later in dev, 10 seconds, then 'yes' or 'no, try another') We proceeding to his office and tieing him too. plus keycard to use later. Next is installation of something useful on his PC.
+ 4th objective: relaunch hack in server room. Now successful. Bain scans their system and detects what we facing here. Then makes a note what we need to do.
+ 5th objective: use or find two keycards on timelock and find codes for vault door somewhere in the branch.
+ event (75% chance in 15-40 seconds, if we placed keycards with interval > 2 seconds): GenSec calls us on security chief/manager's phone. We have 15 seconds to respond or alarm goes off. 1 second (or 0) action to patch Bain on line. He tells a 'short story' about what happened here and why timelock is activated 'occasionally'. 2 variants of phrases and answers, random selection in each of category. Marked in voicelines list file. Each has separation to good and bad ending.
  + (Both variants) Operator satisfied and tell one of phrases, we ending the conversation same way. Then we can continue. If not, operator sends 2 additional guards on a car (40% chance, if we has more than 1 civilians killed).
  + Variant 1 (breaking keycard panel) has option to make them fools with costuming and confirming the story. 
  + Variant 2 (faulty timelock test) is not, continuing is loud only. >>>Semi-fixed variations for this event. Described in dedicated section below.
+ 6th objective is wait for timelock and then enter the codes on drums (12 seconds action). And then open up the door with main handle (1 second action).
+ 7th objective - secure the loot and escape.
---(on loud) Differences is: 
a)lots of cops
b)requirement of proceed to stage(objective) 5 above anyway. Then Bain needs additional hack for sealing pins lockers and tells us to drill up drums sealing lock. This time we breaking also drums' teeths and can go straight to opening without codes. Variations about time of alarm triggering is described in voicelines file. In case of loud nearly from start, (anything below objective 5, I mean before VCS hack is complete and he detects locking system ACTIVATION), after hacking finished he just puts phrase about sealing is active and he needs to be reconnected for last hack (right after report about their vault system stats) and we must drill.
c)cops can kill the lights in office/customers secton. We need pull it back on and restart PC + reconnect Bain. (action -> additional 20 seconds of waiting time, like booting up and reconnecting him -> hacking progress bar continues)
d)additional gas attack as trap for us. Goes in 'safe' to defend areas to (literally) smoke us out. That is vault, toilets, server room and room2. 25 seconds then fades in 15 seconds, deals damage all 40 seconds. Or more with requirement to remove gas can from the roof.

Plan B (Insider asset which costs almost all points/expensive enough)
We starting in casing mode
+ 1st objective: walk to receptionist in lobby
+ 2nd objective: ask for info about deposit injection as company (scripted dialogue). She points us to managers office behind the customers hall ('Deals' sign is there).
+ 3rd objective: enter his office and put on the masks. (Careful, camera and direct view for civs might be there). We tieing him down and borrowing his keycard. (Optionally closing blinders, someone might do it even in casing mode)
+ 4th objective: patching Bain to the phone (same 1 second) and he gonna make a call to GenSec to inform about unplanned urge to open up the vault. 
Reason is 'big client' with a deposit injection, want to do it today, and TSU is not have enough space already for his stack of cash. Asks GenSec to move planned time a day early. Then we recieving clearance and new opening time(basically useless info for gameplay, might be complicated and low important to implement). Dialogue proceeds and ends as written in scripts file. Random variations are free.
+ 5th objective: same activating timelock with cards and search for door codes.
+ 6th and 7th objective - copied from plan A.

TECHNICAL DATA:
We need to save 2 keycards for vault door, 1 from manager(maybe give one to director too?), 2 stored in safes of manager and director. In plan B we have additional one in Security room.
We warned not to use it on timelock before Bain will do some tricks. Better just disable interaction on panels for that.
In case of some not good guy already wasted them anyway and we don't have enough cards in inventory, when it's ready to use safely:
(Triggered when ready to use timelock safely but no actions in 20 seconds and players have not enough/at all keycards in their inventory)
-Bain tell us that we are dumb lvl1 crooks and commands to withdraw.
-Then Bain getting an idea about hacking also keycard panels, which don't have cards inserted. And makes a scanners delivery in Twitch's van with 10 minutes waiting. It available from same drill bag (pickup action override, 3 items recieved if none cards inserted, 2 items if one already in place, then pickup of drill bag itself becomes available on alarm trigger)
Then we placing those scanners on panels near the vault door and on security providing server in IT room. When all devised placed -- 30 seconds and timelock activates. 
Next required 1 action: to remove scanners from vault (2 seconds). In case if GenSec guards will arrive.

Doors handling is simple, just manage unit sequences to cast door_anim_open and door_anim_close. 2 trigger for each door at handles positions, they will be switchable - one pushes linked animation, enabling other trigger for opposite action and becomes disabled. And vice-versa, 0.125 second to launch sequence, door is unlocked and no need to waste time. [Also we might use Close the Door + mod, but there is door's hit detection and managing affected unit after, with sync]
For Server room we need RotateUnit, because that glass door planned is just prop and breaking it is too risky in stealth, you know how much weird thing is sound processing in this game.
Plus randomly we can have manager, admin and bookkeeper sitting at director's office. A council event, long enough. Rarity of 8% and for now - infinite time; we might call they back to workplaces after ~20 minutes.

Costuming event: 
When GenSec car is in the way (4 minutes to arrive), we need to finish our tasks like drilling the door, bodybags in the corner, etc. And then we all must meet in Security room (at least on 3rd minute). As soon as all active players entered marked area (shows up when Bain finishes line about this trick, near the white shelve), script sequence must be activated. That means few actions:
1) FadeToBlack kicks in
2) Script picks the host. 
3) Script swapping his model to guard model
4) Selected player entering Civilian mode, and being respawned outside Security room. Doors must be set to can_interact_in_civilian during this phase. 
5) Security room's door switches state to closed if still opened. (We should preserve its state for CTD+, since there is open_from_inside override to omnidirectional, so fake guard can open it with lockpick type)
6) FadeToBlack ends.
Everyone except fake guard stays in the room while door is closed (can be opened, but not recommended to, 0.125 second action). Costumed heister must go to meeting point and wait inspection team. (after Fade disappeared new objective pops up for him: "Meet GenSec team at moneycounting room and show them our situation" and area appears in that room)
Guards walks in through one of exits, depends from arriving point, and stops in center between 2 doorframes. TSU door must not be in anim_open_door state. Audiolines automatically plays (Don't forget about semi-fixed variations) - like q1-a1-s1-a1-s1 / q2-a2-s2-a2-s2. Every question/answer/sentence per scenario (worker and testing) has randomization. Visually marked in voicelines file. At the end they got satisfied and leaving. Used guard saying last phrase (also at his head coordinates) and switch-area in Security becomes enabled again, with new objective. Then as soon as they reached their car they all despawning. We need to go back to Security room, open up the door staying in Casing mode of course, but civilian type has no masks and stuff anyway. And then we all again must be in trigger_area to make same script working. But this time:
1) FadeToBlack enabled. 
2) Security room's door switches state to closed again, arming our custom open trigger (or just [still] being affected by CTD+). 
3) Script picks up used player and swapping his model back to original. Switching back to heister_unmasked type(some advantage, not for Security room on cell1 and cell4 position, there's staff only area, no civilians should be there) BTW we mostly have it in public area's view. All of this may be useless. [Switch hiding spot to Archive?]
4) FadeToBlack ends.

---Basically for now we can just set those 40% chance of sending GenSec variant to 0%, we'll make 'dressing up minigame' later.
In case of someone joining while fake guard is active and w/o bots, newcomer's spawn needs to be overriden to Security room [Archive] instead of near the host. Bots at this phase need to be in forced waiting state.

SECONDARY INFO
Develop later, low priority.

--Queueing simulation--
We will have 2 bidirectional pathes, like H&T, same quiet driveway near the avenue crossing with another street, quiet place in bunch of trees and mostly living area. From one path peoples not so often will enter bank and make 3 ways of travelling: 
+ 1st stage for all -- ticket dispencer. 
  + Rarely NPCs will walk to receptionist and then their destination is only manager's room and one point in Insurance unit. Corporative agents, huh. (Probably we may do it randomly, peoples have free choosing of where to inform about visit - Ticket machine or Receptionist, just make it not more than 9%, most of customers gonna use dispencer)
+ 2nd stage is random decision of 2 navlink groups - Waiting area and Counters area. 
+ 3rd stage is transition from Counters or straight from Waiting area (rare) to Tellers unit. There is also decision, where NPC can go from waiting in Lobby, Tellers or Units in 2nd hall.
+ 4th stage is waiting in queue IF that's presented in choosen to go window (1,2 and rarely 4, that's currency exchange, 3rd gonna be unused). Make that waiting right near Tellers chance not high, you can be called when ready to free unit after all, logically calling system and ticket-to-serve displays is planned. We will tweak every chances later for balance) 
  + Maybe NPCs don't use queueing, just walking to free unit (with staff waiting there) in runtime.
+ 5th stage is leaving (through same navpoints?) to main entrance and then continuing travel on street-path to last point for despawning. In case of Tellers unit first NPC must turn left, make a step and turn left again. To leave right nearby dividers via free space (dedicated onedirectional navlink on left side of dividers marked walkway, for civs only), then back to unified travel navlinks. If someone stays behind in queue, he must walk forward and then talk with operationist wit hanims, ~2-6 minutes in front of worker behing the glass. (behaviour of every NPC choosed for Tellers unit)

--Random staff traveling--

Rare enough trough time passing events.
Some workers may go to some other spots. Like walk to printer and back after 10-15 seconds, same to shevle or another desk, etc. We will tweak time and chance values later. In addition one-two workers from random workplaces might with pretty much low chance go to WC (one guy only for this variant, ~1 minute there) or Cafeteria (15-18 minutes there, small chain: sink->fridge->microwave->seats to table and 'eats'-> some small talking later -> back to workplaces)
Additions for that - they will use navpoints to Break room no matter lockpicked the door or still locked. (Planned to use following mechanics to other suitable doors, that's common reinforced and wooden doors) Note: this way door should not save state. [Has been lockpicked? Guys leaving from break and closing it with keys again.]

 *Door passind mechanics (Likely will be implemented): if NPC decides to use path trough the door and closes to it, door will automatically recieve command from unit_sequence to cast opening animation. When NPC passed it, closing animation must be casted. Also small delay for him, before opening and closing animations. We already have that proximity script in Capitol Art Gallery, but for lasers and without short-stops.
Guards will close back with keys/just close door to storage, but will check it pretty rarely. Same for roof access door and main service entrance. 
-----------------------------
Manager needs to be hidden behind his desk, civs might enter and/or see. If no manager in room in normal state, they gonna wait in 3xchairs and leave after 5 minutes. (Back to reception? To another unit then? Decide later) 

We might add right one pc search, then manager's pc and terminals in office will be used. Make 2 workplaces empty but launched(?) This will use 15 seconds to check a computer, then 3 seconds to activate process bar. Bain is working.
-----------------------------
The GenSec car leaving not have seating up and depart animations, need to be done with just despawning. Someway we need force player to go back and don't look at this. Also rest of the team from cameras might see that ugly 'glitch'.
If door's handle is destroyed by your silent judge - alarm from guard who detects that. (How to implement?) Same for vault door - one/two guards will use navpoints only in main area, might be left alive for pager limit, not gonna use deposit-corridor area, can't see the door. Will walk only in lobby, main Hall, 2nd Hall external side. Just opened-vault-door bounded alert triggers on more closer points, where they never gonna step. Same points will be used to travelling staff which scared of opened vault door.

Bain will cast 2 phrases about wasting keycards and send the van after, if at safe-to-use moment + 20 seconds we not have 2 keycards total in inventory. We can take one more somewhere, but the van will come. All inserted cards then will cancel 'take devices' objective. In case of presence he just reminds about opening.
-----------------------------
That's why GenSec allows to open up vault early. Scheduled day is tomorrow or something, but here's client with another stack of dollars. Why not to open it today and put everything meant to be in also today? Left side in to put inside, right meant to be send back to customers when they come. 30%, something is already taken from previous vault handling day.
