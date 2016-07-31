# GeneralizedBlockWorld
Generalized code to run playscript via input files

Unity v4
Allows for input file of list of chars, pawns, and marks
Allows for choice of script
Allows for manual testing of loading chars, pawns, and marks
Can run in any mode: baseline, nlp, rules, fdg, random
Intermission screen color is set by mode being run

Process:
Choose initialization file & click load characters
Choose script file, mode to run & click start play

Can also do single movements once the initialization file is loaded
Doesn't do much for speed difference right now
Requires initialization file and script file to have same names for objects, marks, and characters - same capitalization etc


Initialization file format (separated by tabs)
Type: C or P or M for type of object - S for speed
Speed: Slow, Med, Fast (only for S)
Name: uppercase name with no spaces
Start X Position
Start Z Position
Rotation 4 components (only for characters)
Holding Object: define prior to the character!! (only for characters)
Color: blue, purple, red, green, yellow, orange, brown, white (for pawns) or BLUEMat, REDMat, GREENMat, PURPLEMat (for chars)
Importance: 0 to 8 for chars only saying 0 = highest priority char to lowest - only for chars
Voice: Alex, Ralph, Bruce, Fred, Kathy, Vicki, Victoria, Agnes, Princess, Junior - only for characters

Script file format (separated by tabs)
File format / tab separated columns (some of this is because of how smart body’s bml commands were structured):
1 - Y or N - this line should be executed at the same time as the next line
2 - SPEAK or MOVE - determines whether it is a speech line or a movement line (easier parsing)
3 - <character name in CAPS> - who the current actor of this line of BML is
4 - <target name in CAPS> - what object or person is targeted by this BML (pickup what object? follow what person?), XXXXX is ok if no target
5 - <bml> - the actual bml for the command

**Note All character names are in upper case - preferably single unspaced word

BML examples:
1/2 way through scene in good “breaking” point, put this line for “intermission”:
N    BREAK    NOBODY    NOBODY    <?xml none></xml>

Speech (change highlights):
<? xml version="1.0" encoding="UTF-8" standalone="no" ? ><act><participant id="HAMLET" role="actor" /><fml><turn start="take" end="give" /><affect type="neutral" target="addressee"></affect><culture type="neutral"></culture><personality type="neutral"></personality></fml><bml><speech id="sp1" ref="" type="application/ssml+xml">Now get you to my lady's chamber,  and tell her,  let her paint an inch thick,  to this favour she must come.  Make her laugh at that.  Prithee,  Horatio,  tell me one thing. </speech></bml></act>

Movement (change highlights):
Pickup
<? xml version="1.0" encoding="UTF-8" standalone="no" ? ><act><participant id="GRAVEDIGGER" role="actor" /><bml><sbm:reach sbm:action="pick-up" target="SKULL1"/></bml></act>

Putdown
<? xml version="1.0" encoding="UTF-8" standalone="no" ? ><act><participant id="GRAVEDIGGER" role="actor" /><bml><sbm:reach sbm:action="put-down" target="SKULL1" /></bml></act>

Walk to point (or points) - x y x y x y x y… Use multiple points if want to put the walk path through a particular location, but still be a single walk command
<?xml version="1.0" encoding="UTF-8" standalone="no" ?><act><participant id="GRAVEDIGGER" role="actor" /><bml><locomotion target="-7.5 0 -10 20" type="basic" manner="walk" /></bml></act>

Walk to person / object - try to identify “marks” whenever possible over a point
<?xml version="1.0" encoding="UTF-8" standalone="no" ?><act><participant id="GRAVEDIGGER" role="actor" /><bml><locomotion target="GRAVE" type="basic" manner="walk" /></bml></act>

Follow person / object
<?xml version="1.0" encoding="UTF-8" standalone="no" ?><act><participant id="GRAVEDIGGERTWO" role="actor" /><bml><locomotion sbm:follow="GRAVEDIGGER" type="basic" manner="walk" proximity="50"/></bml></act>

Look at point (x y)
<?xml version="1.0" encoding="UTF-8" standalone="no" ?><act><participant id="GRAVEDIGGER" role="actor" /><bml><gaze target="10 36" /></bml></act>

Look at person / object - try to identify “marks” whenever possible over a point
<?xml version="1.0" encoding="UTF-8" standalone="no" ?><act><participant id="GRAVEDIGGER" role="actor" /><bml><gaze target="GRAVE" /></bml></act>

Point at person / object - try to identify “marks” whenever possible over a point
<?xml version="1.0" encoding="UTF-8" standalone="no" ?><act><participant id="GRAVEDIGGERTWO" role="actor" /><bml><gesture lexeme="POINT" target="GRAVE" /></bml></act>

Point at point (x y)
<?xml version="1.0" encoding="UTF-8" standalone="no" ?><act><participant id="GRAVEDIGGER" role="actor" /><bml><gesture lexeme="POINT" target="-5 62" /></bml></act>