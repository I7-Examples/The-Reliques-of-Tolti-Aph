"The Reliques of Tolti-Aph" by Graham Nelson

Include (- Serial "101025"; -).

[Some commentators on interactive fiction suggest that it arose directly from the invention of role-playing games in the 1970s, when the nineteenth-century tabletop wargame collided with the potent cult of J. R. R. Tolkien in American college campuses. It is certainly true that Will Crowther, co-author of what is generally regarded as the first true work of IF, had been an early player of E. Gary Gygax and Dave Arneson's prototype role-playing game "Dungeons and Dragons: Rules for Fantastic Medieval Wargames Campaigns Playable with Paper and Pencil and Miniature Figures" (1974) - later, more snappily, just D&D. On the other hand, surprisingly few IF authors in the last thirty years have adopted styles of play which resemble RPGs at all closely; and the book-keeping, the randomness, the simulationist nature, the volume of detail and the open-endedness implied by a "campaign" all make the implementation of a RPG in interactive fiction something of a technical challenge. So that is what we are going to do: or at least we will work through enough of a system of RPG-like rules to demonstrate the possibility.]


The story description is "It used to be said that there are two kinds of magic-user: those who have been to Tolti-Aph, and charlatans. It used to be generally understood that the attempt to prove oneself in the unforgiving society of Tolti-Aph was a bid for rapid level advancement or else romantic, thin-young-mage-in-midnight-black-robes death. The closer you get to the wilderness island vaguely marked 'Tholtaff' on the agate globe in your great-great-grandfather's study, the better the alternative sounds: settling down in some coastal village, perhaps, a little weathermongering, some polymancy, and helping out with the nets after a bad storm. Retire at maybe level 3, with most of your experience points gained from observing rare fish-based poisons carry off those villagers careless about gutting. Publish an awesomely tedious monograph on the correct usage of the 'untangle rigging' spell. You know, the good life."

The story creation year is 2005.

Release along with cover art, the source text, a website and a file of "Collegio magazine" called "Collegio.pdf".


Part I - Mechanics of a Simple Role-Playing Game

The story headline is "A W&W Scenario".[* W&W is "Woodpulp and Wyverns", the name for our imaginary RPG, which will be a simplified mixture of ingredients from the various late-1970s rulebooks: besides D&D, one might also mention "Tunnels and Trolls" by Ken St Andre (crazy name, crazy guy).] The story genre is "Fantasy". Use full-length room descriptions. Use MAX_STATIC_DATA of 150000.

Understand "help" or "hint" or "hints" or "about" as calling for help. Calling for help is an action out of world applying to nothing. Carry out calling for help: say "(This game is being run according to the rules of a simplified role-playing game which we will call Woodpulp & Wyverns, or W&W. The protagonist is a magic-user, whose main ability is to cast spells. We are not in search of riches but of experience: by collecting experience points, the protagonist may progress upwards in 'level': the SCORE command shows this. An INVENTORY (or I) lists the spells known as well as the items carried.

One's strength, STR, is drained both by casting spells and by being wounded, and - especially early on - it is important to husband this carefully: but the means of restoration can also be found. It will probably take several games to work out the best tactics of play. Tolti-Aph is not a safe place, so expect to be killed every now and again (i.e. with terrifying frequency - E.S.).

W&W play differs from conventional interactive fiction in that there is a great deal of randomised behaviour. Notations like 4d6+2 express the (virtual) dice rolls to be made: this one would involve rolling 4 six-sided dice, then adding 2 to the result. Combat is an especially unpredictable business, but note that one can often run away from a fight which is going badly.

It is possible to get yourself into some insanely hard fights, or to run risks of appalling strength loss, where you might get lucky, but will die nine times out of ten. This is almost always a sign that you've missed something. The designer intends that a sufficiently ingenious player of IF can win through with even the worst luck. Still, with life and death hanging on the roll of a die, the UNDO command has been withdrawn. SAVE is allowed, but an important early puzzle is to work out how to do it.

Lastly, this is one of the worked examples whose source text is included in full in the documentation for Inform 7, a system to create interactive fiction. About the first quarter lays out general rules for W&W play, and can be borrowed to write new W&W scenarios.)"


Chapter 1 - Fundamentals

Section 1(a) - Substance

A material is a kind of value. The materials are stone, wood, paper, magic-imbued wood, metal, clay, flesh, sustenance and air.[* The idea of anything being made of air caused some controversy when this game was being tested, particularly as it was also the material of bodies of water such as the Icefinger River. In the end, I put a sort-of explanation into the spoof magazine accompanying the game.] A thing has a material. Things are usually stone. People are usually flesh.

Substance relates a thing (called the item) to a material (called the stuff) when the material of the item is the stuff. The verb to be made of implies the substance relation.

[The verb phrase "to be made of" is on the face of it only a shorthand, but our biggest aim in writing the source text is to make it look like normal English prose - we think it will be more understandable, and therefore more likely to work first time, if it reads naturally.]

Section 1(b) - Dice

A die roll is a kind of value. 3d20+9 specifies a die roll[* RPGs are games of chance as well as skill, and the actual rolling of the dice is as much a part of the experience as the randomised outcome. The polyhedral four-, six-, eight-, ten-, twenty- and even hundred-sided dice ubiquitous to RPGs are pleasingly exotic in the hand, but also make possible a wide range of random distributions. W&W uses the traditional notation for such rolls (which evolved as the D&D rules went through successive printings, but became standard around 1980): "3d6+2" means we should roll three six-sided dice, add up the scores showing, then add 2.] with parts dice, sides (without leading zeros), adds (without leading zeros, optional, preamble optional).[* The optional nature of the adds part of a die roll means that "2d4", for instance, is also a legal form. Again, this is a convention imitating the tables which appear in printed RPG rulebooks. What Inform means by the preamble to the adds part is the plus sign: if this had not been specified to be optional, "2d4+" would be legal.]

To decide which number is roll of (dr - a die roll):
	let the total be the adds part of dr;
	say "[dr]: ";
	repeat with counter running from 1 to the dice part of dr:
		let the roll be a random number from 1 to the sides part of dr;
		unless the counter is 1, say ",";
		say the roll;
		increase the total by the roll;
	if the adds part of dr is 0 and the dice part of dr is 1, decide on the total;
	if the adds part of dr is not 0, say "+", adds part of dr;
	say "=", total;
	decide on the total.

Test rolling is an action out of world applying to one die roll. Understand "dice [die roll]" as test rolling. Carry out test rolling: say "For your own amusement, you roll "; let the total thrown be the roll of the die roll understood; say ", making [total thrown in words]." [* We added the pointless verb "roll" as a harmless convenience for testing that die rolls work: if the player types "ROLL 3D6+2", the game might reply "For your own amusement, you roll 3d6+2: 2,5,4+2=13, making thirteen."]

Section 1(c) - Percentages

A percentage is a kind of value. 100% specifies a percentage with parts parts per hundred.[* This looks a bit odd, but does make sense. Unusually, this notation involves only one numeric piece - the number before the percentage sign. We give this a name, "parts per hundred", in order to be able to convert percentages to ordinary numbers and back.]

Section 1(d) - Strength

A person has a number called strength. The strength of a person is usually 14.

Definition: A person is alive if its strength is 1 or more. Definition: A person is killed[* There are no natural deaths in Tolti-Aph.] if its strength is 0 or less.

A person has a number called permanent strength.[* The strength you would have after a square meal and a good night's sleep.]

When play begins:
	repeat with the patient running through people:
		change the permanent strength of the patient to the strength of the patient.[* Specifying both the permanent strength and the (current) strength for everyone in the game would be tiresome. To simplify matters we assume that everyone begins the game in rude health, and that we can therefore deduce the permanent strength from the (current) strength.]

To restore the health of (patient - a person): change the strength of the patient to the permanent strength of the patient.

Understand "str" as a mistake ("STR is not a command, but a numerical measure of your current strength. It is constantly displayed in the status line bar at the top of the window, and you are (usually) notified of any changes.").[* A quick way to respond to a mistaken command by the player. Testing showed that some people misunderstood the HELP text's mention of STR, and that others never noticed the status line.]

Understand "diagnose" as a mistake ("You can check your STR at any time from the status line, the bar at the top of the window. It's currently [strength of the player].").[* Some of the Infocom games had a "diagnose" verb, and the play-testers asked for this to be added out of nostalgia.]

Section 1(e) - Experience and levels

A person has a number called level. The level of a person is usually 1.[* RPGs seem to like the idea of numbered levels, with a level 3 magic-user that bit more puissant than a level 2 one. I suppose this may derive from the vague sense of mystery swirling around the "degrees" of masonic lodges, the Benevolent and Protective Order of the Elks, and such.]

Table of Level Advancement
level number	experience points to progress
1		50
2		100
3		250
4		600
5		0

Every turn:
	let the current level be the level of the player;
	if the current level is 5, stop;
	let the target be the experience points to progress corresponding to a level number of the current level in the Table of Level Advancement;
	if the score is greater than the target:
		say "You progress to level [current level plus 1]!";
		increase the level of the player by 1.

Procedural rule: ignore the announce the score rule.[* This is the built-in rule which ordinarily handles SCORE commands. We need to throw it out because we are somewhat subverting the usual conventions by using the score to represent the total accumulated experience points of the player; which means we need to write our own rule to tell the player his current score, as follows.]

Carry out requesting the score:
	say "You are a level [level of the player] magic-user, with [score] experience point[s]. ";
	let the current level be the level of the player;
	let the target be the experience points to progress corresponding to a level number of the current level in the Table of Level Advancement;
	let the target be the target minus the score;
	if the current level is 5, say "It is not possible to progress further." instead;
	if the target <= 0, say "You will soon progress to the next level." instead;
	say "You need [target] [if the score > 0]more [end if]point[s] to progress to the next level." instead.

 When play begins, change the right hand status line to "LVL: [level of the player] STR: [strength of the player]".[* One becomes weaker by performing magic or being wounded, stronger by resting or being healed; one gains experience by living a busy life. These tallies matter a lot, so we make them prominently visible at all times on the status line - the bar at the top of the screen.]

Section 1(f) - Saving rolls

Calculating saving roll modifiers of something is an activity.[* Saving rolls are dice rolls made when the player tries to do something difficult, or avoid some hazard - hence "saving". They tend to be influenced by the context of play: whether the player carries some lucky charm, is presently regarded with disfavour by some demigod, and so forth. Each such influence applies a modifier to the roll: -7 for having just broken a mirror, for instance. We need to calculate all of this without knowing what these influences will be - we need to set up a general framework, in fact, and so we take the unusual step of creating an "activity". These normally handle Inform's housekeeping tasks, but we're perfectly free to create new ones, and the W&W rules contain several.]

The to-save roll is a number that varies.

To modify to-save roll by (amount - a number) for (reason - text):
	if the amount >= 0, say ", +[amount] for [reason]";
	otherwise say ", [amount] for [reason]";
	increase the to-save roll by the amount.

For calculating saving roll modifiers of the player:
	modify to-save roll by the level of the player for "your level";[* See how useful it is to be a higher-level player? This produces text such as "+3 for your level" at the crucial moment.]
	continue the activity.

To decide whether a saving roll of (target - a number) by (savee - a person) to (task - a text) is made:
	say "[if savee is the player]You need [otherwise][The savee] needs [end if]to make a saving roll of [target] or better on 1d20 to [task]: ";
	change the to-save roll to a random number from 1 to 20;
	say the to-save roll;
	carry out the calculating saving roll modifiers activity with the savee;
	if the to-save roll >= target:
		say " - succeeded.";
		decide yes;
	say " - failed.";
	decide no.

To decide whether a saving roll of (target - a number) by (savee - a person) to (task - a text) is lost:[* Thus we could write, say, 'if a saving roll of 15 by the player to "swim the rapids" is lost'...]
	if a saving roll of target by the savee to task is made, decide no;
	decide yes.

Chapter 2 - Magic

Section 2(a) - The phenomenon of magic

A spell character is a kind of value. The spell characters are offensive, defensive, healing and arcana. A valency is a kind of value. The valencies are targeted and untargeted.

A spell is a kind of value. The spells are defined by the Table of Enchantments.[* This is the first of several tables which mimic the style of typical RPG manuals. That mimicry makes no difference to the game as it is perceived by the eventual player, but it helps us to write the source text. Spells are going to cost a certain number of strength points to cast, and will come in various categories: some apply to some particular item (the "targeted" ones), others do not. Most need the caster to be holding something of a particular material nature: for instance, "the requirement of magic missile" evaluates to "wood". Note the blank entries given as "--", and the entirely blank columns at the end, which are used for calculations during play.]

Table of Enchantments
spell		nature	cost	requirement	emission		targeting		duration	duration timer	usage count
detect trap	defensive	1	air	"diffuse blue light"		untargeted	--	a number	a number
memorise		arcana	3	air	"a glowing symbol"		targeted		--	
fashion staff	arcana	2	wood	"woodpulp"		untargeted	--	
know nature	arcana	1	air	"probing rays"		targeted		--
make sanctuary	arcana	8	clay	"rainbow walls"		untargeted	--	
magelight		arcana	4	wood	"flickering white light"	untargeted	25	
mend		healing	2	flesh	"ghostly fingers"		targeted		--	
magic missile	offensive	6	wood	"a fiery red bolt"		targeted		--	
aerial shield	defensive	3	air	"green smoke"		untargeted	3	
silence		defensive	4	air	"a tiny white puff"		untargeted	2	
web		offensive	4	air	"a clingy white thread"	targeted		--	
ward magic	defensive	6	metal	"orange sparks"		untargeted	--	
exorcise undead	defensive	7	metal	"purest whiteness"		untargeted	--	
dragon sleep	healing	5	flesh	"little dancing lights"	targeted		--	
circumvent lock	healing	2	metal	"tendrils of metallic light"	targeted		--	
inner warmth	defensive	4	wood	"reddish glowing"		untargeted	4
ironbones		defensive	4	flesh	"red metallic beams"	untargeted	6
summon elemental	offensive	6	flesh	"blood-red mist"		untargeted	--

Understand "memorize" as memorise.[* For American players who habitually type "-ize" spellings. Note that this is an instruction to Inform that the text "memorize" can refer to the noun "memorise" in anything the player types - it isn't a specification of a command verb, even though it will later turn out that the name of a spell can indeed be used as a command.]

Section 2(b) - The capacity for magic

Memorisation relates various people to various spells. The verb to know (he knows, they know, he knew, it is known) implies the memorisation relation.

Understand "spells" as listing spells. Listing spells is an action applying to nothing. Carry out listing spells: reel off the spells.[* Like "DIAGNOSE", this command and its consequent action were added to satisfy the nostalgia of the playtesters.]

After taking inventory: reel off the spells; continue the action.

To reel off the spells:
	say "You have the following spells committed to memory:[line break]";
	repeat through the Table of Enchantments in cost column order:[* Each time we need to list the spells known by the player, we are requiring the computer to work out how to look through the table of spells in cost order, which is a little inefficient - but it causes no perceptible delay in play. We don't list the spells in the right order to start with because we can't be bothered, and because it would be problematic when we come to add extension rules later on; and we don't sort the table in cost order at the start of play because we are not allowed to disturb the ordering of a table which defines properties attached to values.]
		let this spell be the spell entry;
		if the player knows this spell:
			say "  [this spell] ([nature of this spell]): cost [cost of this spell] strength";
			unless the requirement of this spell is air, say ", requires [requirement of this spell]";
			say "[line break]".

Section 2(c) - Staffs, strength potions and scrolls

After examining something made of magic-imbued wood, say "Holding [the noun] reduces the strength cost of any spell by 2."

After printing the name of something made of magic-imbued wood while taking inventory: say " (magic-imbued)".

To decide which number is the strength cost of (incantation - a spell) to (wizard - a person):
	let the damage be the cost of the incantation;
	if the wizard is carrying something made of magic-imbued wood, decrease the damage by 2;
	if the damage is less than zero, let the damage be zero;
	decide on the damage.[* This phrase calculates the actual strength cost to a given person of casting a given spell, and as we see this cost is reduced if the person is carrying a "magic staff"-like item.]

A strength potion is a kind of thing. A strength potion has a die roll called additional strength. Instead of examining a strength potion, say "[The noun] is a [additional strength] strength potion." A strength potion is usually sustenance.

Instead of eating or drinking a strength potion:
	remove the noun from play;
	say "You swallow [the noun], gaining ";
	let the extra be the roll of the additional strength of the noun;
	increase the strength of the player by the extra;
	if the strength of the player is greater than the permanent strength of the player:
		change the strength of the player to the permanent strength of the player;
		say " - more than enough to restore you to full strength.";
		stop;
	say " strength points."

A scroll is a kind of thing. A scroll has a spell called the inscribed spell. A scroll is paper. The description of a scroll is "Careful to take only a glance, you see that [the item described] is inscribed with the runes of the [inscribed spell] spell." After printing the name of a scroll while taking inventory: say " (bearing the [inscribed spell] spell)".[* Scrolls will be the player's objects of desire, since they carry new spells to be memorised and added to the player's current repertoire.]

Section 2(d) - The casting action

Affectedness relates various people to various spells. The verb to be affected by implies the affectedness relation.[* We now have two verbs connecting people and spells: we can write statements like "Radagast knows summon elemental", or "Henry is affected by ironbones".]

Casting it at is an action applying to one spell and one visible thing. Rule for supplying a missing second noun while casting: change the second noun to the location.[* "Casting it at" is an unusual action because it can take two forms, having a sort of transitive and intransitive version: one casts "detect trap" with no target, but "magic missile" must be aimed at something. Inform permits actions to have syntax which leaves a noun optional, but requires that we supply any missing nouns so that by the time the action is processed nothing is missing. We construe a missing target for a spell as being the current location: thus, "detect trap" is actually a spell cast on the ambient surroundings, which seems reasonable enough. (Inform's built-in action "listening" does something similar.)]

Understand "cast [spell]" or "[spell]" as casting it at.
Understand "cast [spell] on/at [something]" or "[spell] [something]" as casting it at.

The current spell focus is an object which varies.[* The variable "current spell focus" typifies something which tends to happen in complex sequences of rules: later rules need to refer to something worked out in the course of an earlier rule. That means the sort of "let ..." variable will not do, because it is too temporary - it expires when the earlier rule finishes, so will be gone when the later rule is reached. Here we put the value into a permanent variable. This might be problematic if one casting action could be interrupted by another one, but here that never happens.] Before casting, change the current spell focus to the location. 

Check casting (this is the can't cast what you don't know rule):[* We're naming these rules so that the author of a W&W scenario could use procedural rules to suspend or alter them as needed.]
	if the player does not know the spell understood, say "You do not know the mystery of that enchantment." instead.

Check casting (this is the can't cast on the wrong sort of target rule):
	if the targeting of the spell understood is targeted:
		if the second noun is a room, say "That enchantment must be cast at something." instead;
	otherwise:
		unless the second noun is a room, say "That enchantment cannot be cast at anything." instead.

Check casting (this is the can't cast without strength rule):
	if the strength of the player <= the strength cost of the spell understood to the player, say "You are weak, and have not sufficient strength of mind." instead.

Check casting (this is the can't cast at yourself rule):
	if the player is the second noun, say "It is a cardinal rule of magic that no mage may cast an enchantment upon himself." instead.

Check casting (this is the can't cast without required materials rule):
	let the stuff be the requirement of the spell understood;
	unless the stuff is air:
		if the player is carrying something (called the focus) which is made of the stuff,
			change the current spell focus to the focus;
		otherwise say "To weave that enchantment, you must have something made of [stuff] in your hands." instead.

Check casting (this is the can't magically attack a warded person rule):
	if the nature of the spell understood is offensive and the second noun is a person:
		let the intended victim be the second noun;
		if the intended victim knows ward magic, say "With a contemptuous wave of orange sparks, [the intended victim] cancels your [spell understood] incantation." instead.[* The can't magically attack a warded person rule is a little sloppy: it does not sap the magical strength of the victim to cast the spell, which - if sauce for the wizard is sauce for the necromancer - it perhaps should.]

Effect is a rulebook.[*The carry out and report rules for the casting action are going to be surprisingly short - considering the vast range of possible outcomes when a spell is cast - and they do this by using a more powerful mechanism to allow for more flexible reporting than would ordinarily be possible: they create a new rulebook, "Effect", whose task is to carry out the spell. Effect rules are also expected to set "current spell outcome" to a suitable message about what happens, and (optionally) "current spell event" to a rule which carries out further consequences.]

The current spell outcome is an indexed text which varies. To record outcome (eventual message - text): change the current spell outcome to eventual message.

The current spell event is a rule which varies. To record event (eventual rule - rule): change the current spell event to eventual rule.

This is the no spell event at all rule: stop.

Carry out casting:
	record outcome "but then nothing obvious happens";
	record event the no spell event at all rule;
	change the strength of the player to the strength of the player minus the strength cost of the spell understood to the player;
	increase the usage count of the spell understood by 1;
	if the usage count of the spell understood is less than 3[* You gain experience points the first couple of times you cast a spell, but then the novelty wears off.]:
		award cost of the spell understood points;
		award cost of the spell understood points;
	if the cost of the spell understood is at least 5 and the current spell focus is something, remove the current spell focus from play;
	consider the effect rulebook.

Report casting:
	say "As you intone the words of the [spell understood] spell, ";
	if the current spell focus is something:
		say "[the current spell focus] ";
		unless the player is holding the current spell focus, say "vanishes as it ";
		say "releases ";
	otherwise:
		say "your fingers release ";
	say emission of the spell understood;
	unless the second noun is a room, say " at [the second noun]";
	say ", [current spell outcome]";
	if the player is affected by the spell understood:
		if duration of the spell understood is greater than 0:
			say ", which will last for [duration of the spell understood in words] turn[s]";
			change the duration timer of the spell understood to the duration of the spell understood;
			increase the duration timer of the spell understood by 1;
	say ".";
	consider the current spell event.

Every turn:
	repeat through the Table of Enchantments:
		let the remaining effect be the duration timer entry;
		decrease the remaining effect by 1;
		if the remaining effect >= 0, change the duration timer entry to the remaining effect;
		if the remaining effect is 0:
			now the player is not affected by the spell entry;
			say "(The [spell entry] spell wears off.)".[* For simplicity, and to keep storage requirements from going through the roof, time-limited spells can only be applied to the player.]

Understand "xyzzy" and "plugh" as a mistake ("Nobody believes those old children's stories about a colossal cave with magic words any more. Magic's something you spend years and years learning.").

Section 2(e) - Effect of standard spells in alphabetical order

Effect of casting aerial shield at:
	record outcome "and forms an almost invisible saucer-like shield over your head, which moves with you";
	now the player is affected by aerial shield;
	rule succeeds.

A thing can be trapped or untrapped. A thing is usually not trapped.

Instead of drinking a trapped strength potion:
	remove the noun from play;
	say "You quaff [the noun], but it is poison! And it harms you by ";
	let the extra be the roll of the additional strength of the noun;
	decrease the strength of the player by the extra;
	say " strength points.";
	if the player is killed, end the game saying "You have been poisoned".

Instead of putting a trapped strength potion on a weapon:[* This didn't occur to the author at all - one of the playtesters tried it, and then the idea seemed irresistable. Poisoning a weapon adds a whole extra die and +2 in adds, so there's quite a reward for anyone resourceful enough to think of this.]
	remove the noun from play;
	let the hit power be the hit dice of the second noun;
	let N be the dice part of the hit power;
	let S be the sides part of the hit power;
	let A be the adds part of the hit power;
	increase A by 2;
	increase N by 1;
	change the hit dice of the second noun to the die roll with dice part N sides part S adds part A;
	say "Carefully, you apply the poison from [the noun] to [the second noun], using it all up."

Understand "poison [something]" as a mistake ("How, exactly?").

A person can be slumbering or wakeful.

Effect of casting dragon sleep at a wyvern:
	if the second noun is wakeful:
		record outcome "and quite beguiles the wyvern which, though not properly speaking a dragon, is always flattered by any suggestion that it counts as one in the great scheme of things. Enormous snores soon result";
		award 4 points;
		now the second noun is slumbering;
	otherwise:
		record outcome "but, after all, the wyvern is already asleep";
	rule succeeds.

Instead of doing something to a slumbering wyvern, say "Best let sleeping wyverns lie."

Effect of casting detect trap at:
	if a trapped thing is visible:
		record outcome "and it swirls around you for a time before finally settling upon [the list of trapped visible things]. You give a grim smile";
	otherwise:
		record outcome "but it radiates away with a sense of calm normality";
	rule succeeds.

Effect of casting fashion staff at:
	now the current spell focus is magic-imbued wood;
	record outcome "growing scorch-hot in your hands as the wood is transformed into a magical hybrid, imbuing it with strength";
	rule succeeds.

Effect of casting inner warmth at:
	record outcome "and surrounds you with a cosy feeling of well-being";
	now the player is affected by inner warmth;
	rule succeeds.

Effect of casting ironbones at:
	record outcome "and fortifies your whole body";
	now the player is affected by ironbones;
	rule succeeds.

Effect of casting know nature at:
	if the second noun is a direction, record outcome "never to return";
	otherwise record outcome "which make a curious sort of examination and then trace the sigil of [material of the second noun] in mid-air";
	rule succeeds.

Effect of casting magelight at:
	record outcome "which bathe your upper body in radiance";
	now the player is affected by magelight;
	now the player is lit;
	rule succeeds.

Every turn: unless the player is affected by magelight, now the player is unlit.

Effect of casting magic missile at something:
	if the second noun is a person:
		record outcome "bursting in a splash of magical fire";
		record event the magic missile strike rule;
	otherwise:
		if the second noun is portable and the second noun is not scenery:
			remove the second noun from play;
			record outcome "which vapourises into a whiff of smoke";
		otherwise:
			record outcome "which shakes as it absorbs the blow, but withstands it";
	rule succeeds.

This is the magic missile strike rule:
	say "A magic missile does 1d6 damage per level of the caster, so: ";
	let L be the level of the player;
	let the strike power be the die roll with dice part L sides part 6 adds part 0;
	let the strike amount be the roll of the strike power;
	say " points of damage";
	now the second noun is hostile;
	make the second noun take the strike amount points of damage.

Sanctum Sanctorum is a room. "Here in the calm, still centre of the turning world, you find Sanctuary. It is a place to rest, to meditate. Through the hazy rainbow walls, you can vaguely see the [room outside from the Sanctum Sanctorum], but it is as unreal as a dream." Before going outside from the Sanctum Sanctorum, say "You blink as your eyes adjust to the harshness of reality - the unforgiving vividness of existence."

The rainbow outlines of your sanctuary are fixed in place. "Before you are the rainbow outlines of your sanctuary, which you alone can see, never mind enter." Instead of entering the rainbow outlines: move the player to the Sanctum Sanctorum. Understand "sanctum" as the rainbow outlines.

Understand the command "rest" as "sleep". Understand the command "dream" as "sleep". Understand the command "meditate" as "sleep". Check sleeping (this is the can't sleep outside of sanctuary rule): if the location is not the Sanctum Sanctorum and the location is not the Temple of Peace, say "This is not a safe enough place to recuperate. Nowhere exposed to attack is ever safe." instead. Check sleeping: if the strength of the player is the permanent strength of the player, say "You are hale and hearty, with no need of rest." instead.

Carry out sleeping: restore the health of the player. Report sleeping: say "You rest, soaking up the healing goodness of the sanctuary." Procedural rule: ignore the block sleeping rule.

Check saving the game: if the location is not the Sanctum Sanctorum and the location is not the Temple of Peace, say "(The game can only be saved when in an indisputably safe place. This is not that place. You will know it when you find it.)" instead.[* An exceptionally unpopular rule with players, this. But it does make the reward for those who discover "make sanctuary" all the sweeter.]

Effect of casting make sanctuary at:
	if the location is the Sanctum Sanctorum:
		record outcome "which - thankfully - dissipate harmlessly. There probably are necromancers in this world who could get away with a stunt like that, but you're not one of them.";
		rule succeeds;
	record outcome "which - sketchily at first, but with growing confidence of purpose - come together into the ghostly rainbow outlines of a sanctuary before you";
	change the outside exit of the Sanctum Sanctorum to the location;
	move the rainbow outlines of your sanctuary to the location;
	rule succeeds.

Effect of casting memorise at something:
	record outcome "which fades and recovers itself, in a curiously unsatisfying way";
	rule succeeds.
Effect of casting memorise at a scroll:
	unless the player is carrying the second noun:
		record outcome "which, not being in your hands, tantalises more than it reveals";
		rule succeeds;
	remove the second noun from play;
	now the player knows the inscribed spell of the second noun;
	record outcome "which crumbles into dust, filling your mind with thoughts of the [inscribed spell of the second noun] spell";
	rule succeeds.

Effect of casting mend at something:
	record outcome "which almost imperceptibly straightens and tidies itself, but with no real effect";
	rule succeeds.

Effect of casting mend at a person:
	record outcome "which pats [the second noun] on the head, to no practical benefit";
	rule succeeds.

Definition: A person is undead if exorcise undead is its defensive charm.

This is the exorcise undead removal rule:
	repeat with turned one running through the undead people in the location:
		remove the turned one from play;
		award the permanent strength of the turned one points.

Effect of casting exorcise undead at:
	if an undead person is in the location:
		record outcome "which, as if a kind of lassoo, loops around [the list of undead people in the location] and drags the unclean down beneath the earth";
		record event the exorcise undead removal rule;
		rule succeeds;
	record outcome "which throws a kind of lassoo around nothing in particular, then pulls tight until it disappears";
	rule succeeds.

Effect of casting silence at:
	record outcome "and a silence settles mufflingly around your feet";
	now the player is affected by silence;
	rule succeeds.

Effect of casting summon elemental at:
	record outcome "and there is a horrendous tearing sound from the very earth";
	move Zoorl to the location;
	now Zoorl is indifferent;
	rule succeeds.

Effect of casting web at a hostile monster:
	record outcome "and the silken strands drape themselves over [the second noun], impeding all movement";
	now the second noun is affected by web;
	rule succeeds.
After printing the name of a person who is affected by web when we are looking, say " (webbed)".
For calculating combat modifiers of a person who is affected by web:
	modify to-hit roll by -8 for "trying to fight through web";
	continue the activity.

Effect of casting web at:
	record outcome "but the silken strands fall away, half-hearted and half-meant";
	rule succeeds.

Chapter 3 - Combat

Section 3(a) - Monsters are People Too

A person has a spell called defensive charm.

A person has a text called hand-to-hand combat method. The hand-to-hand combat method of a person is usually "with bare hands". A person has a die roll called unarmed combat hit dice. The unarmed combat hit dice of a person is usually 1d2.

A monster is a kind of person. A monster can be hostile or indifferent. A monster is usually hostile. Definition: A monster is scary if its strength is 50 or more. Definition: A monster is feeble if its strength is 10 or less. Some kinds of monster are defined by the Table of Monstrous Beasts.

Instead of asking a monster about something: say "[The noun] is unreachable on most levels."

After printing the name of a monster while looking: say " (STR [strength])".

Table of Monstrous Beasts
name		strength	hand-to-hand combat method	unarmed combat hit dice	defensive charm
goblin		5	"pummeling its green fists"	1d4			--
gnome		2	"slapping its feeble arms"	1d2			--
centaur		9	"kicking its hooves"	1d6			--
werespider	7	"clicking its mandibles"	1d8+1			--
wyvern		75	"beating its clawed wings"	2d8+5			--
half-orc		9	"burrowing with its claws"	1d6			--
harpy		15	"swooping with its talons"	2d6			aerial shield
giant bat		8	"diving to bite"		2d4			aerial shield
basilisk		4	"staring with cursed eyes"	4d6			--
purple worm	7	"biting with acidic teeth"	2d6+1			--
gargoyle		10	"punching stone fists"	1d8+1			--
cave troll		14	"swinging enormous fists"	3d4			--
cave spectre	12	"throwing curses"		2d4+3			exorcise undead
marsh spectre	18	"throwing curses"		4d4+3			exorcise undead
skeletal warrior	25	"slicing with ghostly blades"	2d6+3			exorcise undead
green hydra	32	"ducking its heads in turn"	3d8+3			aerial shield
frost giant		28	"throwing deadly icicles"	2d8+4			inner warmth
elemental demon	35	"whirling with venom"	4d4+2			--

An elemental demon called Zoorl the Elemental Demon is indifferent.[* Oh, the ennui of the underworld. The practical effect of this line is to create Zoorl and leave him out of play at the start of the game: he is the demon brought into being by the "summon elemental" spell.]

Every turn:
	if Zoorl is in a room (called the haunt):
		unless the haunt is the location:
			remove Zoorl from play;
		otherwise:
			if Zoorl is indifferent and a hostile monster (called the victim) is in the location,
				make Zoorl strike a blow against the victim.

Effect of casting web at a wyvern:[* It turns out to be convenient to have at least one monster which is basically proof against anything the player can throw at it, and that's the wyvern. In testing, it turned out that "web" gave a lucky player a chance to defeat a wyvern, which was inconvenient for the plot. So...]
	record outcome "but the web is pathetically small compared to the great scale of the wyvern, and your efforts seem laughable";
	rule succeeds.

Section 3(b) - Weaponry

A weapon is a kind of thing. A weapon has a die roll called hit dice. Definition: A weapon is savage if its hit dice are 3d6 or more. A weapon has a text called technique. The technique of a weapon is usually "swinging". Some kinds of weapon are defined by the Table of Weaponry.[* This innocent-looking sentence is doing something rather sophisticated: it makes a new kind for every row of the table.] Weapons are usually metal. A spear is wood. A quarterstaff is wood. A lance is wood.

After printing the name of a weapon while taking inventory: say " ([hit dice] attack)". Instead of examining a weapon, say "[The noun] is a weapon with a [hit dice of the noun] attack."

A weapon has a number called the to-hit bonus.

Table of Weaponry
name		hit dice	technique
dagger		1d3	"stabbing"
quarterstaff	1d6	"swinging"
battle axe		1d8	"hewing"
short sword	1d6	"swinging"
broad sword	2d4	"swinging"
spear		1d6	"lunging with"
lance		1d8+1	"lunging with"



Section 3(c) - Armour

An armour is a kind of thing. An armour is wearable. Some kinds of armour are defined by the Table of Armoured Dress. Armour is usually metal. Leather jerkins are usually flesh.

Table of Armoured Dress
name		armour class
leather jerkin	1
ring mail		2
chain mail		3
plate armour	4

Check wearing an armour:
	if the player is wearing armour, say "You can only wear one piece of armour at a time." instead.

After printing the name of armour while taking inventory: say " (damage -[armour class])".

Instead of casting magic missile at armour which is worn by someone (called the victim): try casting magic missile at victim.


Section 3(d) - Combat-readiness

To decide whether combat proceeds:
	if a hostile alive monster (called the baddie) is in the location, decide yes;
	decide no.

Before reading a command:
	if combat proceeds, change the command prompt to "Combat> ";
	otherwise change the command prompt to "> ".

After waiting when combat proceeds: stop the action.

Check going (this is the need a saving roll to retreat rule):
	if combat proceeds:
		if a saving roll of 11 by the player to "disengage from the fight" is made, continue the action;
		say "So you are unable to break away." instead.

Check casting (this is the can't cast non-combat spells in combat rule):
	let the warlike nature be the nature of the spell understood;
	if the warlike nature is defensive, let the warlike nature be offensive;
	if combat proceeds and the warlike nature is not offensive:
		say "The nature of that spell is [nature of the spell understood]: unlike offensive and defensive spells - which are structured for use in combat - [nature of the spell understood] spells need a little peace and calm to be cast." instead.

A person can be surprised or ready. A person is usually ready.

Instead of attacking a monster:
	if the noun is hostile, say "You are already locked in combat." instead;
	now the noun is hostile;
	now the noun is surprised;
	now the player is ready;
	say "You make a surprise attack on [the noun]!" instead.

To mount an attack from (bad guy - a monster):
	now the bad guy is hostile;
	now the bad guy is ready;
	now the player is surprised;
	say "[The bad guy] attacks!"

Every turn when a hostile monster is in the location:
	if the player is ready, make the player strike a blow against the scariest hostile monster in the location;[* Recall that "scary" was defined in 3(a) above: "scariest" automatically acquires the meaning of "that which is most scary".]
	repeat with the opponent running through hostile monsters in the location:
		if the player is killed, stop;
		if the opponent is ready:
			if Zoorl is in the location, make the opponent strike a blow against Zoorl;
			otherwise make the opponent strike a blow against the player;
	now the player is ready;
	now all hostile monsters are ready.

Section 3(e) - Damage

The damage incurred is a number that varies.

Calculating damage modifiers of something is an activity. 

For calculating damage modifiers of a person (called the defender):
	if the defender is wearing armour (called the protector) and the damage incurred is greater than 1:
		say ": [the protector] absorbs ";
		let the absorbency be the armour class of the protector;
		if the absorbency >= the damage incurred:
			let the absorbency be the damage incurred;
			decrease the absorbency by 1;
		decrease the damage incurred by the absorbency;
		say the absorbency;
	continue the activity.

For calculating damage modifiers of a person (called the defender):
	if the defender is affected by ironbones and the damage incurred is greater than 1:
		say ": the ironbones spell halves the damage";
		change the damage incurred to the damage incurred divided by 2;
	continue the activity.

Dead body disposal of something is an activity.[* This is provided for the benefit of anyone using these rules who wants to make something special happen to dead bodies, which otherwise quietly vanish from the field of battle.]

To make (defender - a person) take (hits - a number) points of damage:
	let the original hits be the hits;
	change the damage incurred to the hits;
	carry out the calculating damage modifiers activity with the defender;
	let the hits be the damage incurred;
	if the hits <= 0:
		say " - so no damage is done.";
		stop;
	if the original hits are not the hits, say ", leaving [hits] to take";
	decrease the strength of the defender by the hits;
	if the defender is killed:
		say " - a fatal blow!";
		if the player is the defender:
			end the game saying "You have been killed in combat";
			stop;
		carry out the dead body disposal activity with the defender;
		if the defender is in the location, remove the defender from play;
		award the permanent strength of the defender points;
		repeat with the item running through things which are had by the defender: 
			move the item to the location;
	otherwise:
		if the player is the defender, say ", reducing your strength to [strength of the player].";
		otherwise say ", wounding [the defender] to a strength of [strength of the defender].".

Section 3(f) - Striking blows

The to-hit roll is a number that varies.

To modify to-hit roll by (amount - a number) for (reason - text):
	if the amount >= 0, say ", +[amount] for [reason]";
	otherwise say ", [amount] for [reason]";
	increase the to-hit roll by the amount.

Calculating combat modifiers of something is an activity.

For calculating combat modifiers of the player:
	modify to-hit roll by the level of the player for "your level";
	continue the activity.

To make (attacker - a person) strike a blow against (defender - a person):
	if the attacker is the player, say "You attack ";
	otherwise say "[The attacker] attacks ";
	if the defender is the player, say "you, ";
	otherwise say "[the defender], ";
	let the damage done be the unarmed combat hit dice of the attacker;
	let the instrument be nothing;
	if the attacker carries a weapon:
		let the instrument be the savagest weapon carried by the attacker;
		let the damage done be the hit dice of the instrument;
		say "[technique of the instrument] [the instrument]";
	otherwise:
		say hand-to-hand combat method of the attacker;
	change the to-hit roll to a random number from 1 to 20;
	say ", making an attack roll (1d20) of [to-hit roll]";
	carry out the calculating combat modifiers activity with the attacker;
	if the instrument is a weapon and the to-hit bonus of the instrument is not 0, modify to-hit roll by the to-hit bonus of the instrument for "wielding a blessed weapon";
	let the hits be 0;
	if the to-hit roll <= 2:
		say " - critical miss (2 or less): ";
		if the instrument is a weapon:
			if the attacker is the player:
				silently try dropping the instrument;
				if the player does not carry the instrument, say "you dropped [the instrument]!";
				otherwise say "[the instrument] shook in your hand!";
			otherwise:
				now the instrument is in the location;
				say "[the attacker] drops [the instrument]!";
		otherwise:
			decrease the strength of the attacker by 1;
			if the attacker is the player:
				say "you actually injure yourself!";
				if the player is killed, end the game saying "You have fumbled into an inglorious demise";
			otherwise:
				say "[the attacker] roars with impotent rage!";
		stop;
	if the to-hit roll <= 10:
		say " - miss (3 to 10): no damage done!";
		stop;
	if the to-hit roll <= 19:
		say " - hit (11 to 19), doing [damage done] of damage: ";
		let the hits be the roll of the damage done;
		if the hits is 1, say " point";
		otherwise say " points";
	otherwise:
		say " - critical hit (20 or more), doing [damage done] doubled of damage: ";
		let the hits be the roll of the damage done;
		let the hits be the hits multiplied by 2;
		say ", doubled up to [hits] points";
	let the counter-charm be the defensive charm of the attacker;
	if the defender is affected by the counter-charm:
		say ", soaked up by the [counter-charm] spell.";
		stop;
	make the defender take hits points of damage.

Section 3(g) - Missile weapons, lack of

Instead of throwing a spear at, say "There are no missile weapons in the combat rules for this game, which does seem a pity with a fine spear like that."

Instead of throwing something at, say "There are no missile weapons in the combat rules for this game, but I trust we could do better than [the noun] even if there were."


Part II - The Reliques of Tolti-Aph Scenario

Use undo prevention.[* Oh my, this was unpopular. The trouble was that play-testers would get into battle with some monster, then UNDO on every turn in which they didn't roll a hit. So I withdrew UNDO, though in fact on some interpreters there are ways to UNDO even if the game does not want to: but a majority of the testers felt that to UNDO was an inalienable right. I'm not sure whether I should capitulate or not, but it makes a better example of unusual game mechanics to leave this in, so that might as well be the deciding factor.]

When play begins, say "It used to be said that there are two kinds of magic-user: those who have been to Tolti-Aph, and charlatans. It used to be generally understood that the attempt to prove oneself in the unforgiving society of Tolti-Aph was a bid for rapid level advancement or else romantic, thin-young-mage-in-midnight-black-robes death. The closer you get to the wilderness island vaguely marked 'Tholtaff' on the agate globe in your great-great-grandfather's study, the better the alternative sounds: settling down in some coastal village, perhaps, a little weathermongering, some polymancy, and helping out with the nets after a bad storm. Retire at maybe level 3, with most of your experience points gained from observing rare fish-based poisons carry off those villagers careless about gutting. Publish an awesomely tedious monograph on the correct usage of the 'untangle rigging' spell. You know, the good life."

[The ruined city is divided into four quarters, north, south, east and west of a central Ziggurat: those five zones form the five chapters of the scenario. The player begins at the southernmost tip of the southern quarter, so that chapter comes first.]

Chapter S - Southern Quarter

Section S(a) - Turret, Longwall, Mound

The stubborn wiry trees are a backdrop. The stubborn wiry trees are everywhere. Instead of doing something to the stubborn wiry trees, say "The trees are everywhere, and while they are now the fabric of the city they are also in a sense foreign to it. They need not concern you." Understand "forest" as the stubborn wiry trees.

Longwall Street is a room. "Almost overgrown in the deep forest, collapsed and at once standing, the cyclopean east-west Longwall of the fallen city sinks here to a mostly flat, lichened stone clearing: it must once have been a customs post. To the west, an old wall turret like a broken chess piece still seems enterable by a dark doorway, whereas the eastern side of the wall's gap is only a shapeless ramp of masonry. Northwards, to what was once the interior, stubborn, wiry trees grapple with ancient pavings."

The player is in Longwall Street. The player knows detect trap, memorise, fashion staff, know nature and exorcise undead. The player wears a leather jerkin. The player carries a dagger.

The old wall turret is scenery in Longwall Street. Effect of casting mend at the old wall turret: record outcome "which lose their focus as they attempt to mend so huge a ruination"; rule succeeds. Instead of pushing the turret, say "Unsteady, perhaps. Likely to fall at the touch of your little finger, no."

Before going inside in Longwall Street, try going west instead. Before entering the old wall turret, try going west instead.

Writing it on is an action applying to one topic and one thing. Understand "write [text] on [something]" as writing it on. Understand "write on [something]" as a mistake ("To write, you must say what to write: for instance, WRITE HELLO WORLD ON WALL."). Check writing it on: unless the barbed feather is carried, say "You have nothing to write with." instead. A thing can be inscribable. A thing is usually not inscribable. Check writing it on: if the second noun is the player, say "Tattoos are frowned upon in the magic-user community." instead; unless the second noun is inscribable, say "To go writing all over [the second noun] would achieve nothing." instead.

The player is carrying a blank parchment. The blank parchment is inscribable. The deeply scored scroll is a scroll. Understand "parchment" as the deeply scored scroll. The inscribed spell of the deeply scored scroll is aerial shield. Carry out writing it on: if the second noun is the blank parchment begin; remove the blank parchment from play; move the deeply scored scroll to the player; say "The metal feather, with a will of its own, ignores the motion of your wrist and writes its own desires onto the parchment, which is transformed into a deeply scored scroll."; award 7 points; end if. The description of the blank parchment is "Just plain parchment paper, of no particular value or rarity. One of the tools of the magic-user's trade, though admittedly your mother also likes to use it for lining cake tins." The blank parchment is paper.

The dark doorway is an open door. "The doorway looks ordinary enough, but it's so difficult to be sure with the unaided eye." The dark doorway is scenery. The dark doorway is not openable. The dark doorway is west of Longwall Street and east of Turret Roundhouse. The dark doorway is trapped.[* Oh, the grief this trapped doorway caused. It's the first test case of the way this game is unlike conventional IF: because one can blunder through and, probably, just about survive to see the inside of the Turret, though one is then too weak to stand much chance of profiting from the experience. Players tended to assume that if they had got inside at all, then they must have "solved" the problem. Several hints were added to suggest otherwise. Another popular confusion was that players felt that "detect trap" ought also to defuse traps, so text was added to the spoof magazine which would in effect explain otherwise.]

After going through the trapped dark doorway:
	now the dark doorway is untrapped;
	say "Your passage through the doorway triggers off an old trap, of rusty blades swung across by a counterweight! A crude, one-time device, but this doesn't seem the time to sneer at its naive simplicity. You just [italic type]know[roman type] there was something you could have done to avoid this... You soak up ";
	let the damage be the roll of 4d4;
	say " points of damage";
	if the player is affected by aerial shield:
		decrease the damage by 3;
		say ", reduced only slightly (-3) by your aerial shield since the blades come mostly sideways";
	make the player take the damage points of damage;
	if the player is killed, end the game saying "You have been killed by a trap";
	otherwise continue the action.

Effect of casting mend at the untrapped dark doorway:
	now the dark doorway is trapped;
	record outcome "which gives a creaky metal groan that you really don't like the sound of";
	rule succeeds.

After going through the trapped dark doorway with the ornamental stone ball:
	now the dark doorway is untrapped;
	say "The rolling of the stone ball through the doorway triggers off an old trap, of rusty blades swung across by a counterweight! A crude, one-time device, which makes a surprisingly nasty noise, but does no actual damage.";
	continue the action.

A trestle table is a supporter in the Turret Roundhouse. "An old trestle table is [if the crumbs are on the trestle table]scattered with a few crumbs, suggesting that somebody has lived here quite recently[otherwise]wiped clean[end if]." The trestle table is wood and fixed in place. The food crumbs are scenery on the trestle table. The food crumbs are sustenance. Instead of taking the crumbs, say "They're too small and fiddly." The wafer of elven bread is a strength potion. The elven bread has additional strength 3d6. The elven bread is sustenance. Effect of casting mend at the crumbs: record outcome "and as you watch the fingers reshape the crumbs back into the wafer of elven bread from which they were once crumbled"; remove the food crumbs from play; move the wafer of elven bread to the trestle table; award 7 points; rule succeeds.

Outside from the Turret Roundhouse is Longwall Street.

Going south in the Longwall Street is departing Tolti-Aph. Going southeast in the Longwall Street is departing Tolti-Aph. Going southwest in the Longwall Street is departing Tolti-Aph.

Instead of departing Tolti-Aph:
	if the level of the player is less than 5, say "Oh, but it would be dishonourable to leave so soon. Also a little ridiculous, given the number of friends you told about your intentions: to come back a level 5 magic-user, or never return. And then there's the sea-sickness all over again." instead;
	say "Passing the Longwall and out into the wilderness forest, on the long trek back to the triple-kingdoms, you feel distinctly pleased with yourself. A level 5 magic-user, with genuine experience points to your name! Okay, so if anyone ever bowed to a level 5 mage, the light was bad: and you seem just as prone to trip over creepers and stumble into trees as ever you were: but you are now a force to be reckoned with, and if nothing else, you'll always have the memory of Tolti-Aph.";
	end the game saying "You have won".

The Stone Mound is east of Longwall Street. "The remains of the Longwall rise steeply up eastward of the curious ruined platform, and also lose their shape, becoming little more than a great stone mound. Here at the top, it looks as if the stonework has been shattered by lightning and not just slow old time, but that seems an unreal idea when the canopies of the forest stretch out in every direction against a clear autumnal-blue sky."

Instead of going nowhere from the Stone Mound, say "The only safe way down the Mound is back to westward." Down from the Stone Mound is the Longwall Street.

Before going to the Stone Mound: if the strength of the player is less than 3, say "You are too weak now to contemplate making such a climb." instead.

After going to the Stone Mound: say "It is a long, steep climb up the ragged eastern side of the Longwall breach, and costs you 1 strength point."; decrease the strength of the player by 1; continue the action.

A barbed feather is in the Stone Mound. "A barbed metallic feather, from a bird which must have been scaly and very large - also, disconcertingly, metallic - lies in the loose rubble." The feather is metal. The description of the feather is "The feather comes to a metal tip which is - there's no other other word for it - a sort of nib." After taking the feather for the first time, say "Taken. As the feather pulls clear, it occurs to you that the rubble it was sitting in is not any random cairn, but is all of the same kind of stone, and has smooth as well as roughened faces. It's thoroughly broken up now, whatever it was."

Understand "fly" or "fly [text]" as a mistake ("Hmm. 'Magicians Who Thought They Could Fly' is a local delicacy back home - little tomato-pancakes dropped into a skillet of olive oil.").

The ornamental stone ball is pushable between rooms. Instead of taking the ornamental stone ball, say "It's almost as big as you, and made of stone."

The loose rubble is scenery in the Stone Mound. Effect of casting mend at the loose rubble: remove the loose rubble from play; move the ornamental stone ball to the Stone Mound; record outcome "shaping it back into the ornamental stone ball which must once have stood atop the Longwall"; rule succeeds. Instead of taking the loose rubble, say "Whatever you came to this ruin for, it wasn't a handful of rubble." Instead of pushing the stone ball, say "Try saying which way to push it." After going from the Stone Mound with the ornamental stone ball: say "Wheeeeee!"; continue the action. Before pushing the stone ball to down, try pushing the stone ball to west instead. Understand the command "roll" as "push". Understand "stonework" as the loose rubble.

Turret Roundhouse is a room. "This broad round chamber forms the central well of the turret. Little light enters by the dark doorway from the east, but the crude steps upward presumably lead to a lookout." A scroll called the battered scroll is in the Turret Roundhouse. "Propped against one wall, almost as if it was meant for you to find it, is a battered scroll." The inscribed spell of the battered scroll is make sanctuary.

Turret Alcove is above the Turret Roundhouse. A container called the medicine chest is in the Turret Alcove. "The rough steps - probably not the original construction - end at a blank-walled alcove, bare except for a medicine chest." In the medicine chest is a strength potion called the green vial. The green vial has additional strength 1d6+2. In the medicine chest is a strength potion called the ochre vial. The ochre vial has additional strength 1d6+2. In the medicine chest is a strength potion called the crimson vial. The crimson vial has additional strength 1d6+2. The ochre vial is trapped.[* This was intended to keep players on their toes: detect trap! detect trap! always detect trap!] The medicine chest is closed and openable. The medicine chest is wood.

The arrowslit is fixed in place in the Turret Alcove. "A little light shows through a broad arrowslit in the northwestern wall of the turret, which is where the steps have wound around to." The arrow is a wood thing. Instead of examining the arrowslit for the first time: say "As you look curiously out of the arrowslit, there is a wheep! sound. Instinctively, you dodge. "; if a saving roll of 11 by the player to "sidestep the arrow" is made begin; say "The arrow whings past you and rattles against the far wall of the Turret."; now the arrow is in the Turret Roundhouse; otherwise; now the player carries the arrow; say "The arrow catches you in the shoulder"; make the player take 3 points of damage;[* It's almost certainly a good idea to be stoical and accept the 3 points of damage for the sake of getting something wooden which can be used as a spell focus later - so this is not a puzzle to be solved, but a treasure in disguise, really.] if the player is alive, say "You pull the arrow out ruefully."; end if. Instead of examining the arrowslit: say "You feel a certain reluctance to stick your head out of that a second time."

Section S(b) - The Journal and Orion

The player is carrying a new-looking journal. The journal is paper. Understand "new" as the journal. Instead of examining the journal: say "Magic-users are trained to keep a journal, lest their discoveries be forgotten, or possibly just to give them a vehicle for their self-importance which doesn't involve world subjugation. Yours is divided into entries named after stars in the constellation of Orion, so clearly there's no self-importance there.

(Try TURN JOURNAL TO RIGEL for a sample of the sort of twaddle you wrote in your distant student days, which finished, oh, three or four weeks ago now.)"

An Orion star is a kind of value. The Orion stars are defined by the Table of Journal Entries.[* The play-testers were distinctly piqued by the idea that they were supposed to remember the names of the stars of Orion from general knowledge, and transcripts showed that the most popular response was to try everything in the Wikipedia entry. The Table tries to cater for all the variant names used in popular references, but in any case contains no really crucial information: and as one reaches the more obscure stars, it communicates less and less. All the same, it was the play-testers' annoyance about Orion that led to the writing of the game's "feelie", the spoof Collegium magazine, which includes clues on Orion and excuses for many odd things about the rules.] Journal-turning it to is an action applying to one thing and one Orion star. Understand "turn [something] to [Orion star]" as journal-turning it to. Understand "look up [Orion star] in [something]" as journal-turning it to (with nouns reversed). Understand "turn to [Orion star] in [something]" as journal-turning it to (with nouns reversed). Check journal-turning: unless the noun is the new-looking journal, say "That has no star references." instead. Carry out journal-turning: say the corresponding journal entry of the Orion star understood; say paragraph break. Before writing: if the second noun is the journal, say "You have vowed not to write in this journal until your expedition is complete." instead.

Table of Journal Entries
star name		corresponding journal entry
betelgeuse	"13th Tortoise 346. Old Scrofulax at his worst today - double Memory, went on for what seemed like forever. Keeps losing segments of apple in his beard. But it wasn't useless, for once. For the first time we got scrolls to practice on, only scrolls with 'encourage watercress' inscribed on them but you have to start somewhere, like Scrofulax says, and anyway you wouldn't want to live in a world where watercress goes unencouraged, would you? So I did my very first 'memorise'. Didn't quite work, but for a while I could have encouraged some grass, I suppose. Like always, they made us do it twenty times over. And then they made us walk under this big talisman thing that wiped our minds empty again! How are we ever going to learn? 'Memorise' is supposed to give you the permanent ability to cast the spell written on the scroll, but huh, we got it for only an hour and a half."
rigel		"1st Leopard 342. First day at the Collegium! Big cold dormitory and HUGE books but yummy custard and Migstipple, in Class IVa, says he can do 'know nature' but he's a big liar and my grandfather can do 'exorcise undead' which is loads better."
bellatrix		"6th Camel 347. They took away our wooden amulets today. Funny, when you're a little squit, you see the big kids walking around with nothing on their wrists and you can't wait, but it feels - wrong. Turns out they weren't just wood, but magic-imbued wood, and they were doing all the spells, not us. Or anyway they provided all the strength. I never realised, but when you cast a spell, it costs you a bit of strength, and that's why they're all so obsessed with teaching us to measure our STR all the time. So they made us cast a 'detect trap' on our own, and Migstipple had to sit down and his face was dead white. Mood over dinner pretty subdued. This isn't going to be quite as easy as we all thought."
mintaka		"38th Swallowtail 347. Total exhaustion. Endless exercises with focuses. It seems that what counts isn't the focus itself, like it looks in all the paintings - the wizard holding up the silver lamp, and all that - but the material it's made of. And you need the right material or the spell won't work. Except, some spells don't need a focus at all! Or something. So tired I couldn't even hear what Babbington was saying at the end. When are they giving us our amulets back? When?"
alnilam		"2nd Leopard 348. Big day, worth a great big new star in the old journal, oh yes. My first really big spell. Can't remember why I chose 'exorcise undead' for my optional subject this year, except it sounded pretty eldritch. And today the first real casting. Shock of my life when the blunt old sword I'd picked up for a focus vanished right out of my hands! Migstipple gave me a vial of strength potion to get me back on my feet, and yeah, it turned out the zombie squirrel had been struck back down to the underworld which was way cool, but what happened to the sword? Thought I'd dropped it, which would have been just too lame. Babbington didn't even laugh, for once, was pretty decent about it. Turns out that a really big spell drains the focus right out of existence - and this was a really big spell. Still worried about my acne though."
alnitak		"14th Turtle 347. Don't know why I bother with this journal in the summer vac, but I guess it's just a habit now. Great-great-grandfather's place up in the mountains is not as ruined and desolate as I'd hoped, and it looks as if people tidy up so much that there's no chance of finding a few scrolls he left behind. The study's kind of interesting, though. No secret passages yet. Spun the globe a while, thinking it might do something. Doesn't, though. Reading a dusty old book called Reliques of Tolti-Aph, but I don't believe a word of it."
tabit		"The TABIT entry is crossed out almost immediately, since you decided that spelling it THABIT would be more sinister."
thabit		"42nd Piglet 346. Combat exercises, out in the rain. When they tell you in class that only defensive and offensive spells can be cast during combat, you look superior and think 'until me, anyway'. Turns out they're not kidding, it's harder to spell-cast when you're distracted by some cave troll. Not that we practice on real cave trolls. 'Very well, Mr Migstipple,' said Babbington wearily, 'but at least swing that club as if you have some idea what a blunt instrument is, would you?' Tchah."
algiebba		"29th Goshawk 346. They made us distinguish strength potions this morning. 'Would you say this was yellow, tan or ochre?' Got fed up and told Swarthwater the potion in question was definitely tannish-yellow and not yellowish-tan. Did not expect this to be the right answer."
mankib		"3rd Camel 346. Everyone still having trouble with colour-coded strength potions. Makes them angry that I know carminine from crimsonish from scarletesque. Crabheart brought me her juice to identify at dinner. I said it was applish-brown, and also poisoned. She pretended not to believe me, but Migstipple says he saw her cast 'detect trap' afterward to be sure."
hatsya		"2nd Tortoise 343. Odds exam in two days. Spent the entire afternoon making charts and trying to memorize. The older kids say there will be essay questions: when is a 1d10 weapon better than 2d3+1? When is it worse? Which would you wield against a gnome? Etc. Swear my head will explode. Wish everything used the same base, at least. What would be so wrong with it all being d20?"
nair al saif		"17th Mallard 348. Morning after the night before. No more lessons, no more ceremonies, no more custard. Can't quite get a handle on this. Thought Scrofulax didn't have to look quite so smug when he erased our experience points right back down to 0. I know, I know, the school ones aren't real experience, but they were hard enough to pick up, and now I'm just a level 1 nobody with NO EXPERIENCE AT ALL."
saiph		"29th Mallard 348. If this wretched forest goes on any longer, I'll turn into a tree myself. I mean, don't the gods ever think, right, I'd like some variety now, let's put a glacier down here and maybe a few sandy beaches?"
meissa		"31st Mallard 348. Nearly there, according to the sketch I made from the old globe. Seems the first I see of Tolti-Aph ought to be the Longwall, and that must be gigantic if the engravings in Reliques are anything to go by."
orion		"That's the whole constellation, not a single star."
orion nebula	"That's a nebula, not a star. Hence the name."
m42		"That's a nebula, not a star."
m43		"That's a nebula, not a star."
belt		"That's a collective name for several stars."
trapezium		"That's a collective name for several stars."

Section S(c) - The Fallen Tree and the Fullers

The Fallen Tree is north of Longwall Street. "The overgrown ground, sloping gradually downwards to the north, is interrupted here by the fallen trunk of a giant of a tree. The trunk seems to have partially demolished an old town-house to northeast."

The trunk of a giant fallen tree is scenery in the Fallen Tree. Instead of climbing or entering the trunk, say "No need to clamber on it." The trunk of a giant fallen tree is wood.

The partially demolished house is scenery in the Fallen Tree. Understand "town-house" as the partially demolished house. Effect of casting mend at the partially demolished house: record outcome "but it's just too huge a task. This whole idea of rebuilding Tolti-Aph, one house at a time, is very commendable, but perhaps not very realistic"; rule succeeds.

The skeleton is a supporter in the Fallen Tree.[* Supporter in the sense that it can prop things up, not in the sense of politics.] "Propped against the trunk in a parody of a sitting man, a skeleton - no, a woman's, to judge from the pelvis - shines white in the sunlight. The empty eye-sockets look up at the sky with a curious sort of expectancy." Effect of casting mend at the skeleton: record outcome "shaping flesh back onto her bones, until she sits - naked, since her clothes are not here to be mended - in front of you: almost, once, she blinks with disbelief, but the whole semblance of new life cruelly dissipates to merest air. It seems one cannot mend a skeleton, even with her own spell"; rule succeeds.

Instead of attacking or kissing the skeleton, say "That would violate a powerful taboo amongst your people: and besides, an alarming number of skeletons are not as dead as they seem."

A leather pack is in the Fallen Tree. "On the skeleton's lap is a leather pack." The pack is trapped. In the leather pack is a scroll called the immaculate scroll. The inscribed spell of the immaculate scroll is mend. The guardian harpy is a harpy. The pack is closed and openable. Instead of examining the trapped pack, say "It does seem... curious that the pack should be untouched, unlooted, when all else is gone, and even the skeleton's clothes have been taken." Instead of taking or opening the trapped pack: say "As your thieving fingers touch the dead woman's pack, a screaming noise almost throws you backward, and worse is to come: a harpy swoops down from the sky!"; now the pack is untrapped; move the guardian harpy to the location; mount an attack from the guardian harpy. The leather pack is flesh.

In the leather pack is a strength potion called the sea-blue vial. The sea-blue vial has additional strength 1d4+2.

A cursive diary is in the leather pack. The cursive diary is paper. The description of the cursive diary is "A diary written in a cursive hand, not taught in guild-schools these many centuries. The final entry makes you sigh:

No strength... so cruel to get so close...[line break]say your prayers little one,[line break]begone bleak days of drowning time -[line break]you loose-roaming clouds be with me -

She was clearly in a mystical state at the end. Strength collapse can do that to a magic-user."

Instead of going north from the Fallen Tree with the ornamental stone ball, say "It's hopeless: the stone ball cannot be rolled over the trunk."

Instead of going nowhere from the Fallen Tree, say "If it were not for the break in the wall to northeast, you would only be able to go north or south along the lane."

The Fullers' Town-House is northeast of the Fallen Tree.[* This southern end of the city - where cargoes arrived - ought to be where practical trades and crafts were carried on, but I wanted to avoid obvious trades. Fullers were people who cleaned and carded cloth from wool: it's a trade which died with the Industrial Revolution, but the surname lives on in England, most famously through the poet John Fuller - which may be why I also wrote the fullers a tiny poem.] "This may well be the most habitable room left in Tolti-Aph, though the craftsmen who built it - to judge by the frescos, fullers of cloth - were in no wise rich. And since then, even what little they could afford in the way of windows have been bricked solid."

Outside from the Fullers' Town-House is the Fallen Tree. Instead of going nowhere from the Fullers' Town-House, say "If it were not for the hole knocked in the wall by the recently-fallen trunk, this room would almost have been sealed completely tight, it occurs to you. It must be almost perfectly preserved."

A painting of a tobacco pipe is fixed in place in the Fullers' Town-House. "On one wall hangs a painting of a clay pipe, smoking tobacco." The description of the painting is "Written on the frame underneath, in common tongue, is THIS IS NOT CLAY." The painting is paper. Instead of taking the painting, say "It will not detach from the wall." Understand "picture" as the painting of a tobacco pipe.[* Cf. Magritte, of course, but the point of this little puzzle is that it tantalises the player with something made of clay - because the player badly needs a clay focus in order to cast "make sanctuary".]

Looking behind is an action applying to one visible thing. Understand "look behind [something]" as looking behind. Report looking behind: say "You see nothing unexpected behind [the noun]." Instead of looking behind the painting, say "It seems to be fastened to the wall by some sort of mechanism, so that you can't simply pull it away to look behind." Instead of pushing or pulling the painting, try looking behind the painting.

A cloth loom is a supporter in the Fullers' Town-House. "The centrepiece of the room is the cloth loom on which the wispy strands of wool were once washed, combed and woven into cloth." The stretched scroll is a scroll on the cloth loom. The inscribed spell of the stretched scroll is circumvent lock. Before casting memorise at the stretched scroll, say "Under the terms of the recent Sorcery Millennium Property Act, all magic-users are now hypnotised to make it impossible for them to learn certain spells connected with the breaking or circumvention of locks or other devices intended to protect property. Like all magic-users, you resent the implication that you are some kind of kleptomaniac, and regard this as an outrageous infringement of your personal liberty. Especially since these are the same people who go on and on about the right to bear 'apocalyptic fireball' wands! Tsk." instead.[* The Recording Industry Association of America may like to note that this annoyed the play-testers very much indeed.]

The description of the cloth loom is "Painted around the base of the loom is the score, it seems, for a song, in the traditional tablature notation." The tablature song score is scenery in the Town-House. Understand "sing song" as singing. Singing from is an action applying to one visible thing. Understand "sing [something]" or "sing from [something]" as singing from. Check singing from: unless the noun is the tablature, say "You might sing from a score, perhaps, but not [the noun]." Before singing in the Town-House: try singing from the tablature instead. Instead of singing from the tablature: say "You walk slowly around all four sides of the loom, singing the song - a round, it seems, and the sort of cheery number written by those engaged in basically repetitive crafts. A pretty little song, but you're not sorry your father apprenticed you to a magic-user instead."; display the boxed quotation
"Oh the fuller turns
And the world turns
So turns the day
And the fuller turns
So the leaf turns
That turns the world
So everything turns".

Instead of turning the tablature, say "You turn the song over in your mind."

The old tobacco pipe is clay. Instead of turning the painting for the first time: say "You turn the painting around, and are astonished to find behind it a secret cache of tobacco - hidden from the wives of the fullers, no doubt. It has all rotted except for one old pipe, which you pocket in your usual kleptomanic way."; now the player carries the old tobacco pipe; award 3 points. Instead of turning the painting, say "Nope - no more pipes." Instead of smelling the old tobacco pipe, say "Extraordinary how long the redolent odour survives, but even so it's just the faintest trace now." Understand "smoke [something]" as burning. Instead of burning the old tobacco pipe, say "It's a vile habit, frowned upon by the contemporary magic-user." Understand the command "spin" as "turn".

The Internal Cavity is a container. The modest scroll is a scroll in the Internal Cavity. The inscribed spell of the modest scroll is silence. The Temporary Cavity is a container.[* When the loom turns, anything inside is turned to the outside and vice versa, and this means exchanging the contents of two containers, in effect. The easiest way to do that is to use a third location for temporary storage, hence the Temporary Cavity, which has no physical existence in the player's world and so does not appear in play.]

The loom can be known to be broken. The loom is not known to be broken.

The cloth loom is trapped. Instead of turning the cloth loom:
	say "Your fingers finally locate the hidden catch which turns the loom, and would have enabled the fullers to work on both sides of the cloth. ";
	if the cloth loom is trapped:
		now the loom is known to be broken;
		say "But the mechanism is jammed in some way." instead;
	say "The mechanism turns smoothly";
	if something is on the cloth loom, say ", winding [the list of things on the cloth loom] out of sight";
	if something is in the Internal Cavity, say ", bringing [the list of things in the Internal Cavity] up into view";
	say " as the new top-side of the loom comes flat.";
	now all the things in the Internal Cavity are in the Temporary Cavity;
	now all the things on the cloth loom are in the Internal Cavity;
	now all the things in the Temporary Cavity are on the cloth loom.

Effect of casting mend at the trapped cloth loom:
	if the loom is known to be broken:
		record outcome "which reach into the mechanism of the cloth loom - it's not trapped at all, just full of enmeshed gearing, and soon the obstruction is eased";
		now the loom is untrapped;
		rule succeeds.

Understand "kick" or "kick [text]" as a mistake ("In case you are the kind of adventurer who kicks things when frustrated, we may as well agree right now that this will not help matters.").[* There was a certain amount of loom-kicking when this room was tested.]

Understand "weave" or "weave [text]" as a mistake ("Alas, the old crafts of the city are long gone, and it would only be sentimentalism to revive them now.").

Section S(d) - The Broken Lane and the Spiders

The Broken Lane is north of Fallen Tree. "This must once have been a north-south thoroughfare, but now it is as cracked as a fishmonger's face. Even the old paving has given out, and its place is taken by stolen tombstones."

Instead of going nowhere from the Broken Lane, say "Though the humps around the lane - half-masonry, half-tree boles - must once have been public buildings, they are impenetrable now. Perhaps further on to the north."

The stolen tombstones are scenery in the Broken Lane. The description of the stolen tombstones is "These tombstones have clearly been dug up, carted here and thrown carelessly down into something that somebody very careless might, charitably, call a roadway. Frankly, they are a mess, broken up like so much biscuit." Effect of casting mend at the stolen tombstones: record outcome "which make a half-hearted attempt to set all right, but the task is way beyond so basic a spell"; if the broken half-brick is fixed in place begin; now the broken half-brick is portable; move the broken half-brick to the player; end if; rule succeeds.

The broken half-brick is fixed in place. The half-brick is clay.[* Another tricky-to-find clay focus, in case the player didn't find the clay pipe.] Understand "brick" as the broken half-brick.

The flame-blackened door is west of the Broken Lane and east of the Sunken Courtyard East. The flame-blackened door is a closed openable door. "Set into the slab-sided [if the location is the Broken Lane]western cutting of the lane[otherwise]eastern apex of the courtyard[end if] is a flame-blackened door, no more than four feet high." The flame-blackened door is lockable and locked. The flame-blackened door is wood. The matching key of the flame-blackened door is the eight-legged key. Effect of casting mend at the flame-blackened door: record outcome "which gives off an air of superiority - in so far as doors can do such a thing - and somehow communicates to you that it [italic type]likes[roman type] being flame-blackened, thank you very much. Gives a door a certain air, a certain rakehell dash"; rule succeeds. The eight-legged key is metal.

The flame-blackened door is inscribable. Check writing it on: if the second noun is the flame-blackened door, say "In paintings of ruined cities, bronze-limbed shepherds stand around pointing nobly at the sunset while half-fallen viaducts and temples of perfect white marble complement the sward. Typically, they are not seen scrawling graffiti over the remains, and neither are you."

Knocking on is an action applying to one thing. Instead of knocking on something, say "That would accomplish little, beyond the pleasures of percussion." Understand "knock [something]" or "knock on [something]" as knocking on. Instead of knocking on the flame-blackened door, say "You knock, but - and perhaps this might have been foreseen, since the city has been abandoned for uncountable centuries - apparently nobody is home." Instead of taking the flame-blackened door, say "I sense that you'd really like to be the proud owner of this somewhat charred but otherwise presumably STILL WOODEN piece of wood: unfortunately, it is fixed in place."[* Wooden objects can be used as the focus for spells like "magic missile", so are much in demand.]

A room can be werespider-infested. Sunken Courtyard East, Sunken Courtyard North, Sunken Courtyard West and Sunken Courtyard South are werespider-infested. Effect of casting detect trap at in a werespider-infested room: record outcome "but really, the whole place - scuttling with eerie shadows and scratchings - is so obviously a trap that no spell could be needed to tell you so. Hideous, half-blind werespiders must be everywhere"; rule succeeds.

The Sunken Courtyard East is a room. "This is the eastern corner of a sunken courtyard, perhaps fifty yards on a side, which is surrounded by impenetrable walls of masonry so that it might as well be an arena. A disconcerting thought, to be sure: but not quite so disconcerting as the wispy strands you recognise as the tell-tale signs of werespider incursion. This is a dangerous place. The central fountain-grove lies west, while the shadowy edges of the courtyard extend northwest and southwest." Before going from Sunken Courtyard East for the first time, award 10 points.

The Sunken Courtyard North is northwest of the Sunken Courtyard East. "This northern apex of the Courtyard is ruinously overrun by webs, husks, and a whitened compote of bones and chitin." In Sunken Courtyard North is some ring mail.

The Sunken Courtyard West is southwest of the Sunken Courtyard North. "Is it a coincidence that this western apex of the Courtyard is the only one retaining some semblance of its original purpose? A flight of steps leads southward and up to what looks like an old workshop."

A person can be poisoned or unpoisoned. Every turn when the player is poisoned: if the location is the Sanctum Sanctorum begin; now the player is unpoisoned; say "With profound relief you realise that entrance into sanctuary has purged your blood of poison."; stop; end if; decrease the strength of the player by 1; if the strength of the player < 5, say "You feel weakened by some inner corruption. Only the peace of sanctuary can cure this."; if the player is killed, end the game saying "You have been poisoned".

The Apothecary's Workshop is above Sunken Courtyard West. The Apothecary's Workshop is south of Sunken Courtyard West. "Many hundreds of tiny bright eyes follow you from the shadows of this ancient workshop, which seems once to have been where the local apothecary mixed, ground and filtered his wares. At least, that was his day job; you have a sneaking suspicion that his hobby may have been the collection of exotic species of spiders..." A closed openable trapped transparent container called the glass bottle is in the Workshop. "Most of the detritus is rubbish, and almost surely deadly to touch, but a glass bottle with a scroll inside it catches your eye." In the glass bottle is a scroll called the cobwebbed scroll. The inscribed spell of the cobwebbed scroll is web. Instead of opening the bottle, say "There is no obvious way to open the bottle short of breaking it." Instead of attacking the bottle for the first time, say "As you heft the bottle, ready to smash it, you notice a tiny red spider inside - just for a moment - and, although it vanishes almost at once, it gives you pause for thought." Instead of attacking the bottle: now the player is poisoned; now the cobwebbed scroll is in the location; remove the bottle from play; say "As the bottle smashes almost into powder - the glass must be ancient - and the cobwebbed scroll falls out, you notice an almost imperceptible stinging sensation on your forearm." After dropping the bottle, say "Miraculously, it doesn't break."

Instead of inserting the glass bottle into the arrowslit: now the player is poisoned; now the player carries the cobwebbed scroll; remove the bottle from play; say "A wild arrow smashes the bottle almost into powder - the glass must be ancient - and the cobwebbed scroll falls out into your hands, so that you barely notice a slight stinging sensation on your forearm."

In the Apothecary's Workshop is a strength potion called the turquoise vial. The turquoise vial has additional strength 3d4+3.

A short sword called the short sword called Bliddforn is metal.[* Thus it is referred to in play as "the short sword called Bliddforn". Named blades with extra vorpalness are more or less obligatory for this kind of game.] The to-hit bonus of Bliddforn is 1.

The Sunken Courtyard South is southeast of the Sunken Courtyard West. "This southern apex of the Courtyard is ruinously overrun by webs, husks, and a whitened compote of bones and chitin." A dead goblin lord is in Sunken Courtyard South. The dead goblin lord is flesh. "The victims are not all from ages past, as the body of a dead goblin - a lordly one, at that, to judge from the cut of its loincloth - gapes stupidly at the world in a rictus of poison." Instead of taking the dead goblin lord, say "The body is far too heavy to move. Goblins are light on their feet, but heavy on their backs, no?" Instead of searching the dead goblin lord for the first time: now the player carries Bliddforn; say "Ransacking the body takes little time (just as well, given the surroundings): but it pays dividends as you retrieve Bliddforn, a short sword which must have been a goblin heirloom." Instead of examining the dead goblin, say "It certainly is much less repellent to examine the goblin than search it."

Effect of casting mend at the dead goblin lord:
	record outcome "but the corpse does no more than twitch convulsively, once";
	rule succeeds.

The Sunken Courtyard East is northeast of the Sunken Courtyard South.

The Sump of the Fountain-Grove is west of the Sunken Courtyard East.

The Sump of the Fountain-Grove is east of the Sunken Courtyard West.

The Sump of the Fountain-Grove is south of the Sunken Courtyard North.

The Sump of the Fountain-Grove is north of the Sunken Courtyard South.

Instead of going from a werespider-infested room to the Sump of the Fountain-Grove, say "As you gingerly approach the centre of the sunken courtyard, a restive threshing of the tendrils which drape around the fountain makes you abruptly aware that the undergrowth is - well. There are giant spiders, and then there are spider-giants. Never let one see you approaching!"

The spider tendrils of the undergrowth is a backdrop. The tendrils are in Sunken Courtyard East, Sunken Courtyard South, Sunken Courtyard North and Sunken Courtyard West. Instead of pulling the tendrils, say "It would be safer to pull the hair of the Princess Vl'tasha. (Slightly. She's thirteen and a psychopath.)" Instead of examining the tendrils, say "You really do not think it best to get any closer." Understand "spider-giant" or "giant spider" as the tendrils.

Five werespiders are in the Sump of the Fountain-Grove.[* The sump is not in fact visitable; it exists as a dwelling-place for the spiders while they wait to pounce.]

After going from a werespider-infested room to a werespider-infested room:
	say "You tread warily around the periphery of the sunken courtyard. ";
	if the player is affected by silence:
		say "But the silence charm holds true, and you do not disturb the werespiders.";
		continue the action;
	if a saving roll of 17 by the player to "tiptoe past the werespiders" is lost:
		repeat with legsy running through the alive werespiders:
			move legsy to the location;
			now legsy is hostile;
		say "The werespiders, with ears on all eight knees, are not fooled!";
	otherwise:
		say "Thank the gods for that.";
	continue the action.

 Rule for dead body disposal of a werespider (called the victim) when the location is a werespider-infested room: restore the health of the victim; move the victim to the Sump of the Fountain-Grove; continue the activity.

Section S(e) - The Icefinger River

The Gravel Track is north of the Broken Lane. "Even the makeshift paving gives out here, and the lane degenerates into a gravel track, rutted by carts and stamped all over with oddly inhuman prints of bare feet."

Understand "stamp" as jumping. Instead of jumping in the Gravel Track, say "You jump about on the gravel, disturbed to find that your footprints don't look all that human either."

Instead of going nowhere from the Gravel Track, say "Though the humps around the lane - half-masonry, half-tree boles - must once have been public buildings, they are impenetrable now. Perhaps further on to the north."

The rutted gravel stamped by inhuman footprints is scenery in the Gravel Track. Understand "track" or "foot prints" or "prints" as the rutted gravel. The description of the rutted gravel is "You trace circles in the rutted gravel with your toe, but to no lasting benefit."

A hostile goblin called Vish is in the Gravel Track. Vish carries a spear. "Vish, a goblin lookout armed with a spear, hisses in rage at the very sight of you desecrating his realm."

Instead of going to the Ford for the first time, say "Just a warning before proceeding: you might want to exercise a little caution. It looks as if the lane may descend a little into shallow water, and if the goblin encampment has a centre, that will be it."[* The reason for this warning is that the goblins are far too tough to take on until the player has succeeded in solving the early rooms well enough to get the "make sanctuary" spell, find something made of clay to use as a focus, and have enough strength left to perform the casting. The clue is meant to signal players that they ought to hang back and work on the early rooms before proceeding.]

Icefinger Ford is north of the Gravel Track. "A shallow, rapid torrent washes over the Lane from northwest to southeast here, and no forest creek by the feel of it against your ankles but a mountain spring that must have burst from the ground no more than a mile away. This is goblin-work."

In Icefinger Ford are four goblins, a bundle of kindling and a cracked cooking pot. The kindling is wood. The pot is clay. Instead of burning the kindling, say "The kindling is hopelessly too damp to catch light, even if you had something to light it with. Goblins [italic type]have[roman type] discovered fire, but they do keep forgetting the essential points." Effect of casting mend at the cooking pot: record outcome "but the pot isn't broken, as such: it was just badly made in the first place"; rule succeeds. 

The river Icefinger is a backdrop. The river is in the Icefinger Ford, the Divide and the Icefinger Source. Understand "water" or "torrent" as the river Icefinger. Instead of drinking the river, say "It is cold enough that you can feel each separate tooth in your jaw." The river is air.[* In some metaphysical sense. There is an excuse - I don't think we could call it an explanation - in the accompanying spoof magazine.]

The Riverbed Course is a region. Icefinger Ford, the Divide and the Source of the Icefinger are in the Riverbed Course.

Rule for calculating combat modifiers of the player: if the location is in the Riverbed Course, modify to-hit roll by -3 for "having your legs in water"; continue the activity.

For calculating saving roll modifiers of the player:
	if the location is in the Riverbed Course and we are going south,
		modify to-save roll by 10 for "splashing back out the way you came";
	continue the activity.[* This is to protect players who, despite the warning, march straight into the goblins and the middle of a rather difficult battle. Assuming they wish to run away, they need to make a saving roll to get out: failure to roll it means almost certain slaughter. That seems a little rough, so we give them a +10 bonus on this saving roll (making it virtually certain to succeed) so long as they only want to run back to the early rooms.]

Instead of going nowhere from the Icefinger Ford, say "The lane, of course, runs north to south; the shallow river washes through it from northwest to southeast."

The Divide is northwest of Icefinger Ford. "The Icefinger runs west to southeast through a cutting here which is so deep that the river flows almost underground." The majestic erratic outcrop of rock is in the Divide. The outcrop is fixed in place. "In the centre of the stream is a startlingly majestic erratic outcrop of rock. It must be older than the river, older yet than the city."

Instead of going up in the Divide, say "You would have to climb the outcrop."

Before climbing the erratic outcrop for the first time, say "Warning: you might be able to climb up, but even to try will cost you strength." instead.

Instead of climbing the erratic outcrop:
	if the strength of the player < 3, say "You are too physically weak to attempt it." instead;
	decrease the strength of the player by 1;
	if a saving roll of 15 by the player to "scale the erratic outcrop" is made:
		move the player to the Shrine of the Pinnacle;
	otherwise:
		say "Your attempt fails, and you land back with a jarring splash - the water is shallow and the riverbed unyielding.".

The Shrine of the Pinnacle is a room. "The flattened top of the erratic outcrop is, at once, both severely plain and unmistakeably sacred, and you sense that this must be a place sacred to one of the gods of the old city."

Praying is an action applying to nothing. Understand "pray" as praying.

Carry out praying: say "It would cheapen the appeal to the gods to make it in such a place as this."

Instead of praying in the Sanctum Sanctorum, say "This is where one comes to hide from the gods."

Instead of praying in the Shrine of the Pinnacle, say "You prepare yourself with due solemnity, and yet - and yet - you sense somehow that the proper words must be used, if the prayer is to be answered."

Praying to clouds is an action applying to nothing. Understand "you loose-roaming clouds be with me" or "pray you loose-roaming clouds be with me" as praying to clouds. Check praying to clouds: unless the location is the Shrine of the Pinnacle, say "Nothing could happen here." instead. Carry out praying to clouds: if the player does not know magic missile, award 12 points; now the player knows magic missile. Report praying to clouds: say "Communing in this special place with the swirling clouds - but was this not, a moment before, a clear autumnal sky - you feel seized with a sense of purpose and ability."

Instead of going down in the Shrine of the Pinnacle:
	if a saving roll of 15 by the player to "get down safely from the erratic outcrop" is made:
		say "You breathe a sigh of relief, then hiss a little as the Icefinger reminds you of the coldness of mountain water.";
	otherwise:
		say "You fall as you descend, taking ";
		let the damage be the roll of 1d6+1;
		say " points of damage.";
		decrease the strength of the player by the damage;
		if the player is killed:
			end the game saying "You have fallen to your death";
	move player to the Divide.

The Source of the Icefinger is west of the Divide. "The deep-cut channel becomes at last almost a cave, as hollowed walls of long-smoothed stone come abruptly to a halt. You might wade back east, but otherwise all ways are impassible."

A bronze grating is fixed in place in the Source of the Icefinger. "The Icefinger river bursts into existence through a bronze grating very securely fixed right across the rocky cave floor. You had hoped to descend this way - but it is not to be." Instead of opening the grating, say "When I said 'very securely', I had in mind 'more securely than you can break with your bare hands'." Instead of searching the grating, say "Water. Lots of water."

A goblin called Dronsh is in the Source of the Icefinger. "The goblin lord Dronsh, who had been fishing here (perhaps it is reserved for the upper castes?), is enraged by your presence, as goblins are wont to be." Dronsh is carrying a spear. Dronsh is carrying the eight-legged key. Dronsh has strength 12. Understand "goblin" and "lord" as Dronsh.

The description of the eight-legged key is "Whatever was Dronsh doing with this? It's not goblin manufacture. The loop at the end of the key is worked in tempered iron into a sort of eight-legged figure: not valuable, not pleasing to look at, but not easily forgotten either."[* We did work out some back-story to do with Dronsh having recently deposed the previous lord and sentenced him to death at the mandibles of the were-spiders, but it seemed neater to leave the explanations to the reader's imagination. Vish was mixed up in the intrigue too, but I forget how.]

Section S(f) - The Marshes

A marsh room is a kind of room. Marshy Hollow and Log Pontoon are marsh rooms.

The Marshy Hollow is southeast of Icefinger Ford. "Even the wall-sized slabs and boulders of the ruined city are disturbed here, hanging at crazy angles in a marshy, dangerously festooned swamp-hollow. The only possible safe route out of here is back upriver, to the northwest. Although a sort of pontoon of logs lies east, there's something you really, really don't like about the look of it."

The pontoon of logs is scenery in the Marshy Hollow. Instead of doing something to the pontoon of logs, say "The pontoon lies further east."

Before going up in the Marshy Hollow, try going northwest instead.

Instead of going nowhere from a marsh room:
	say "The marsh swirls as it sucks you into its turbulent belly.";
	end the game saying "You have drowned in marsh".

The Log Pontoon is east of the Marshy Hollow. "This platform - who built it, and why? - is like a log raft adrift on a river; but it is static in the midst of a deadly marsh, which if anything deepens on every side except to the west." Instead of praying in the Log Pontoon, say "The gods of such a place as this are - well, not the talkative kind."

In the Log Pontoon is a marsh spectre. A platinum pyramid is in the Log Pontoon. "Balanced on the pontoon, and straining its load-bearing capacity to the uttermost, is a waist-high pyramid of what looks like solid platinum." Instead of doing something to the platinum pyramid when combat proceeds, say "You cannot even get a good look at the pyramid so long as the spectre is haunting you." The platinum pyramid is fixed in place. The description of the platinum pyramid is "It seems to be some sort of altar, consecrated here to the hideous gods of the undead marshes. It is scrawled all over with incomprehensible sigils and glyphs, not at all well written." Understand "sigil", "sigils", "glyph" and "glyphs" as the platinum pyramid. The platinum pyramid is metal.

The platinum pyramid is inscribable. Carry out writing "begone bleak days of drowning time" on platinum pyramid: if the player does not know dragon sleep, award 12 points; now the player knows dragon sleep;[* In this sort of game the player basically progresses by collecting new spells, but it's boring getting them all from scrolls. So two spells can be acquired by praying to suitable gods, or in this case, to unsuitable ones: the clues are in the skeleton's diary.] say "After you scrawl these sacred words onto the pyramid with the feather, you step one (but only one) pace back. Communing in this special place with the hideous shadows of the dead - but was this not, a moment before, a deserted marsh - you feel seized with a sense of purpose and ability." instead. Understand "begone bleak days of drowning time" as a mistake ("Nice try, but nothing happens."). Carry out writing it on: if the second noun is the platinum pyramid, say "Even as you scrawl that on the platinum face of the pyramid, it seems the wrong thing to say, and your hand falters."

Chapter Z - The Central Ziggurat

The Ziggurat is north of Icefinger Ford. "The only distantly visible building of these ruins to survive, through its sheer vastness: the awesome Ziggurat, a stepped pyramid temple, rises up out of sight. This is clearly the destination of the old lane, which descends slightly to the ford in the south."

The stepped pyramid temple is scenery in the Ziggurat.[* We seem to be in central America now, whereas the early rooms were more medieval-Europe in flavour. Oh well.] Understand "ziggurat" as the stepped temple.

An indifferent wyvern called the green-scaled wyvern is in the Ziggurat. "A green-scaled wyvern [if the green-scaled wyvern is wakeful]coils its long neck as it gazes unblinkingly at you[otherwise]snoozes happily[end if], sunning itself on the steps of the Ziggurat. One of the fabled great beasts! You never thought to see one, and know only that it is highly territorial, and vastly too dangerous to provoke."[* Play-testers simply wouldn't take "no" for an answer, and kept on, and on, trying to defeat the wyvern. It is meant to be essentially impossible to kill, so that the only way to pass is with the "dragon sleep" spell, because this ensures that the player has solved the Southern Quarter area before proceeding - and that's necessary since, otherwise, the player will not be powerful enough to cope with what follows. In every draft of the game, the wyvern was made stronger and harder to defeat.] Understand "dragon" as the green-scaled wyvern. The description of the green-scaled wyvern is "You see nothing special about the green-scaled wyvern. It's just your everyday wyrm the size of a small house, when you come right down to it. Unremarkable."

Instead of going up from the Ziggurat in the presence of the wakeful green-scaled wyvern:
	if the green-scaled wyvern is indifferent, mount an attack from the green-scaled wyvern;
	otherwise say "The wyvern has every intention of stopping you!"

Instead of going nowhere from the Ziggurat, say "The ruins are impassibly overgrown on all sides. It suddenly strikes you as, well, uncoincidental that the only accessible route appears to lead right here."

Instead of going north from the Ziggurat, say "That would mean climbing UP the Ziggurat, if you dare."

The Exalted Throne is above the Ziggurat. "How many of the thousands who lived in Tolti-Aph were ever allowed up, here, you wonder, to this twenty-yard square plateau? The arch-priests and the kings, possibly - on feast days only; and the occasional snow-necked virgin on her way to a spaced-out meeting with [if the Robing Room has been visited]a lustful monarch[otherwise]an obsidian dagger[end if]. It all seems so impossibly long ago now."

The town of Tolti-Aph is scenery in the Exalted Throne. Understand "ruined" or "city" as Tolti-Aph. The description of Tolti-Aph is "Only the whitened bones of the ruined settlement of Tolti-Aph can be made out against the encroaching forest. Clearly this was never a city, even by the degenerate standards of what people hereabouts flattered themselves was a 'city'."

The stonework throne of the king is a door. "[if in the Robing Room]The oddly winding little passage leads back outside.[otherwise]Dominating the summit is a well-founded stonework building in the shape of a low-backed throne, as if it awaits the seating of a god. An oddly winding little passage leads to darkness inside." The stonework throne of the king is inside from the Exalted Throne and outside from the Robing Room. The stonework throne is open. The stonework throne is not openable. Understand "passage" or "building" or "winding" or "little" as the stonework throne. The description of the stonework throne is "Think small building, not large furniture." Instead of climbing the stonework throne, say "It seems odd to be frustrated by the last six yards of the climb, but you are. The throne's sides are perfectly smooth." Instead of going up in the Exalted Throne, try climbing the throne instead. Instead of jumping in the Exalted Throne, say "There are many easier forms of suicide in the town, if you just look around a little harder."

Understand the command "sit" as something new. Understand "sit on [something]" or "sit in [something]" as climbing.

The Robing Room is a dark room. "By the curious magelight, you see that this modest Robing Room - no doubt a place in which feathered head-dresses, jaguar-skins and the like were arrayed on the numinous chiefs before they showed themselves to the populace - is in fact painted on every side with highly 'artistic' images of succubi. And the floor is, well, a little more padded than might be thought strictly necessary."

The artistic images of succubi are scenery in the Robing Room. The description of the artistic images is "Centuries have passed since the painting of these images, and it is now possible to regard them as achieved artworks. You do this for a while." Understand "succubus" as the succubi.

Instead of sleeping in the Robing Room, say "Your memories of that day back in the Collegium, when the boys are all taken to see the true form of a succubus, and the girls to see the true form of an incubus, remain just a shade too vivid."

Instead of going down in the Exalted Throne, say "Every way from here is down, which is kind of the point the architects wanted to make. You get your bearings by figuring out that the wyvern was guarding the south face." The Ziggurat is south of the Exalted Throne. Instead of going outside in the Exalted Throne, say "This is about as outside as it gets."

Going northeast is going diagonally. Going northwest is going diagonally. Going southeast is going diagonally. Going southwest is going diagonally. 

Instead of going diagonally in the Exalted Throne, say "It would be perilous to clamber down the four angled corners. Stick to north, east, south or west."

Instead of praying in the Robing Room, say "There's only one religion in here, and it isn't the sort you pray to on your own." Instead of praying in the Exalted Throne, say "Once this place was sacred, certainly. Now it is merely high-up."

After going south from the Exalted Throne: say "You climb slowly down the steps to the end of the old north-south lane."; continue the action.

After going east from the Exalted Throne: say "You climb slowly down the shadowy steps and into a delightful twilit garden."; continue the action.

After going west from the Exalted Throne: say "You climb slowly down the grandly formal steps, the afternoon sun in your eyes, and at last reach ground level in a narrow channel."; continue the action.

Chapter E - Eastern Quarter

[The Eastern Quarter is really one enormous puzzle: the former Maze of Royal Beasts which, in the way of these things, is still operational. We begin with a single room, the Budless Grove, and the never-children who act as gatekeepers for the maze. You have to prove yourself to get in - we really want players to attempt this quarter last of all - and then you have to solve one or more of the maze quests to get out again.

The Quarter involves two unusual spells not found in the standard W&W rulebook, so these are defined here, simply by continuing the previous table. "Compel postulant" is unusual in that it is cast upon the player, not by him.

We also need a new material for the pseudo-real items found in the Maze, most of which cannot be brought out of it again.]

Table of Enchantments (continued)
spell		nature	cost	requirement	emission		targeting
compel postulant	arcana	20	air		"a ghostly brush"	targeted
force labyrinth	arcana	6	metal		"a spinning blur"	untargeted

The magical essence is a material.

Section E(a) - The Budless Grove

A room called In the Budless Grove is east of the Exalted Throne. "These eastward demesnes were once formal gardens: none more formal, in fact, since they comprised the Maze of the Royal Beasts, the traditional proving-ground for mages who wished to be advanced in the king's service. Of the original structure, nothing remains except the trellis-work archway to the east, the bordering hedge obscuring all sight of what may lie within, the stone bench - in fact, more or less everything survives, it seems."

The Exalted Throne is up from the Budless Grove.

A ring of ghostly never-children is a person in the Budless Grove. "But not perhaps by natural causes, for a ring of ghostly never-children, pallid things with shallow, violent eyes, join hands to dance and dance again around the bench." Understand "never" and "children" and "ghosts" and "pallid" as the never-children.

Instead of attacking the never-children, say "There is an abrupt shifting of scale. The giggling of the children grows to a booming thunder, from far, far above.

Abruptly you realise that this is a fight which may begin while you are the height of a blade of recently-cut grass. Each of the never-children, you'd guess, has a STR of maybe 4, and you doubt if they could deal out more than 1d4 of damage between them. On the other hand you now have a STR of 1/10th...

So it's a lucky thing that, treating the episode as a game, they allow you to return to normal size. You step back involuntarily."

Instead of kissing the never-children, say "You might get close enough. Whether you'd get back again..."

The description of the never-children is "As the name suggests, they were never children. They are not undead, for they never lived. They - well. Let's just leave it at that."

Instead of giving something to the never-children, say "'We doethn't want giftth!' they cry out in derision. This whole lisping thing is going to get old quite quickly. 'We demandth... thacrifithe.' Mmm." Instead of asking the never-children about, say "They yelp with amusement at the idea that they should answer your questions."

Understand "sacrifice [something]" as a mistake ("There are as many forms of sacrifice as there are minor cults. You will have to say how.").

The stone bench is a scenery supporter in the Budless Grove.

The Orange Flower of the Postulant is a thing. Instead of rubbing the Orange Flower, say "What, you think you can just rub it off? One born every minute." The Orange Flower is flesh.

Instead of entering the archway in the Budless Grove, try going east.

The cowardice count is a number that varies. The never-children can be satisfied or unsatisfied. The never-children are unsatisfied.

Instead of attacking a scroll:
	remove the noun from play;
	say "You tear up [the noun], and it loses coherence as it melts into thinnest air. Apparently only magic really gave it any substance, and that is now gone.";
	if the player is in the Budless Grove and the never-children are unsatisfied:
		now the never-children are satisfied;
		say "'The magic-uther tore up the [inscribed spell of the noun] all for uth!' one of the vile little creatures says, and the others make grudging gestures. 'All wight. Can come play.'".

Understand the command "play" as "sing". Understand the command "dance" as "sing". Instead of singing in the presence of the never-children, say "The merest look from them would be enough to stop you, but in fact they give you quite a lot more than the merest look. At any rate - you stop."

Instead of entering or climbing the stone bench:
	if the never-children are unsatisfied, say "'We don't let jutht anyone play!' giggles one of the never-children. 'Firtht, you have to sacrifithe something. Something no magic-uther would ever sacrifithe.'" instead;
	if the player is affected by compel postulant, say "The never-children glower at you." instead;
	now the player is affected by compel postulant;
	now the Orange Flower of the Postulant is part of the player;
	restore the health of the player;
	say "As you sit, one of the ghostly never-children dabs your forehead with a practised brush, painting a flower motif. They chant in a patty-cake-like rhyme, 'Postu- postu- postulant, Now you are compelled!' and, what with all the mad, scamp-like laughter, you spend so long thinking about how ghastly they are as well as ghostly that you realise far too late that the rhyme is actually a spell. You jump up again, feeling too stupid to feel angry.";
	change the cowardice count to 1;
	award 5 points.

Instead of casting magic missile at in the presence of the never-children, say "'Who'th got a magic mithile then!' lisps one of the never-children unbearably, and their cackles become so monstrously elongated that, well, somehow you change your mind. Between them, they may be the most powerful sorcerer you have ever met."

Every turn when the player is in the Budless Grove and the cowardice count is not 0 and the maze victory count is 0:
	increase the cowardice count by 1;
	if the cowardice count is 2, stop;
	if the cowardice count is 3, say "One of the never-children slaps you in the face. 'Coward!'";
	if the cowardice count > 3:
		say "The never-children slap you harder, and there is a nastier overtone to their taunting. 'Coward! You must enter the Labyrinth!' The blow has real effect";
		if the player is affected by aerial shield, say ", and your aerial shield is no hope when they're reaching up from below";
		let the slap strength be the cowardice count;
		decrease the slap strength by 3;
		make the player take the slap strength points of damage.

Before taking inventory: if the player is affected by compel postulant, say "Your forehead bears the Orange Flower of the Postulant, age-old symbol of one held by the 'compel postulant' spell."

Before going to the Exalted Throne when the player is affected by compel postulant:
	if the maze victory count is 0, say "Your forehead aches with greater agony as you take each step of the ziggurat, and you are eventually compelled to return." instead;
	now the player is not affected by compel postulant;
	remove the Orange Flower from play.

Instead of examining the Orange Flower:
	if the player carries something metal (called the reflective surface), say "In [the reflective surface] you see reflected the Orange Flower of the Postulant on your forehead, a pretty many-petalled flower like a saxifrage. You last saw one of these on, wait a moment, an old wood-panel in the unrenovated part of the Master Herbiarist's classroom. You used to look at this when he was collapsed by sneezing - why they appointed a Master Herbiarist with hay-fever, the gods only know - and now it comes back to you with an uneasy jolt: the brave young figure, Flower on forehead, setting off into the verdant maze, never seeing the three-necked Green Hydra waiting just behind the first corner." instead;
	say "It seems to be fuzzy, thin and brownish. No - wait! Those are your eyebrows."

The trellis-work archway is an open trapped door. "So-called because it is the work of Mrs Trellis of North Wales, an indefatigable witch."[* I really thought nobody would get this joke, but in fact everyone did.] The trellis-work archway is east of the Budless Grove and west of the Hedge Archway. The trellis-work archway is scenery. The trellis-work archway is wood.

Instead of going east in the Budless Grove when the player is not affected by compel postulant, say "'Wanth to play in our garden of [number of labyrinth rooms in words] beds,' giggles one of the never-children. 'Can't!' screams another, then another. 'Not unleth...' They laugh, if by laugh we mean a thoroughly ill-intentioned threat."

After going east from the Budless Grove: say "You try your very hardest to remember what used to happen to postulants, once they had crossed the archway. All you can recall from the Master-Sage's lessons is that some insanely powerful geas protected the Maze's secrets by ensuring that if anyone were ever to be told the story of a postulant, even in another world, even if the story were being told by some kind of mechanical contraption - that any who listened or read would immediately suffer DEATH should the postulant in the story come to that same unhappy end."; continue the action.

Section E(b) - The Maze of Royal Beasts

[The Maze is a complex labyrinth which builds itself, randomly, as it is explored, and which is also randomly stocked with treasures, monsters, magical artefacts and hazards. Just as the whole of ROTA is loosely indebted to the late 1970s role-playing games, so the Maze is indebted to a contemporary board game - floor game, really - called "Sorcerer's Cave", a classic of this lost genre.

"Sorcerer's Cave" was published by Philmar, and then Gibson's: the best-known edition is from 1978, but all editions are long out of print. (As a sort of homage, I bought a copy up on Ebay after writing ROTA.) The "Cave" is an underworld labyrinth which extends itself across the floor. When a player wishes to move off one cave tile, he draws a new tile from the pack and lays it down in the appropriate space - face up if the connection works, that is, if the caves have connecting passageways which meet at the join; but face down if it doesn't work, because the exit from the existing cave doesn't match an entrance on the new one. Some caves are mere passageways or junctions, but others open out and are inhabited: the player draws from the "encounters pack" to see what can be found in such caves. Staircases down lead to deeper and rougher levels: whereas a first-level cavern involves drawing only a single encounter card, a third-level cavern involves drawing three, and if you have ever drawn the Sorcerer and two Spectres simultaneously then you will know the true meaning of unfairness.

All of this is clearly very difficult to implement in Inform, which is why it is included in ROTA, which is after all meant to be a worked example.

What makes the original "Sorcerer's Cave" work so well is that it is elegantly balanced, as a result of over two years of obsessive testing and modification by the designer, Terence Peter Donnelly. The encounter cards are beautifully judged to counterpoise each other, and recur in endless unexpected combinations. Donnelly tells the story nicely in his article for "Games and Puzzles" magazine (Autumn 1980), which can be found on the web. I haven't stuck exactly to the original, or even nearly so, but the basic ingredients are mostly the same. I had a minor pang of conscience about this, but Donnelly himself says that he finally ended up dropping ideas from D&D into the "Cave" design, despite having started out "misguidedly" trying to avoid this. So I feel I am in good company.

On with the show, but let me encourage curious readers to seek out the original game, or indeed Donnelly's equally recommendable table-top sequel "Mystic Wood". (The fact that level 1 of our Maze is above ground and wooded is a nod to this sequel, as is the range of quests.)]

Section E(c) - Spatial coordinates

A spatial coordinate is a kind of value. <9,24,24> specifies a spatial coordinate with parts maze level, easting (without leading zeros), northing (without leading zeros). A room has a spatial coordinate called grid position. Definition: A room is unlocated rather than located if its grid position is <0,0,0>.[* Inform does not automatically construct un- prefixes: they're too unpredictable in English. For instance, in ROTA "undead" has a definition which is by no means equivalent to "not dead".]

To decide which number is the current maze level:
	let L be the maze level part of the grid position of the location;
	decide on L.

The previous maze level is a number that varies.

Every turn:
	if the location is a labyrinth room and the location is not the Hedge Archway:
		if the current maze level is not the previous maze level:
			if the current maze level is 1, say "You blink in the shadowy, but natural twilight, feeling a faint breeze on your face once again. You are back in the familiar hedge-maze at the surface.";
			if the previous maze level is 1, say "Underground, the labyrinth continues with clean-cut, magically lit passages.";
	change the previous maze level to the current maze level.

A direction has a spatial coordinate called vector. North has vector <0,0,1>. South has vector <0,0,24>. East has vector <0,1,0>. West has vector <0,24,0>. Down has vector <1,0,0>. Up has vector <9,0,0>.[* As this demonstrates, Inform allows us to add to or modify even the most fundamental built-in concepts.] Definition: A direction is vectorial if its vector is not <0,0,0>.[* For instance, "northeast" and "outside" are non-vectorial directions in this sense. The Maze occupies a cubical lattice and never has connections in those directions.] Definition: A direction is vertical if it is up or it is down.

To decide which spatial coordinate is the vector sum of (V1 - a spatial coordinate) and (V2 - a spatial coordinate):
	let L be the maze level part of V1 plus the maze level part of V2;
	let L be the remainder after dividing L by 10;
	let E be the easting part of V1 plus the easting part of V2;
	let E be the remainder after dividing E by 25;
	let N be the northing part of V1 plus the northing part of V2;
	let N be the remainder after dividing N by 25;
	if L is 0 or L is 9, decide on <0,0,0>;
	if E is 0 or E is 24, decide on <0,0,0>;
	if N is 0 or N is 24, decide on <0,0,0>;
	let the sum be the spatial coordinate with maze level part L easting part E northing part N;
	decide on the sum.[* All of that remainder-after-dividing stuff is to enable us to have relative vectors with negative entries, in effect. Thus a single move west is achieved by adding <0,24,0> to the position, because 24 is the same as -1 when we are dealing only with numbers in the range 0 to 24. However, the maze does not "wrap around": the legal grid positions are <L,E,N> where L must be 1 to 8, and E and N must be 1 to 23, with the special value <0,0,0> being reserved as a "no position at all" value.]

To decide which spatial coordinate is the vector difference of (V1 - a spatial coordinate) and (V2 - a spatial coordinate):
	let L be the maze level part of V1 minus the maze level part of V2;
	let L be the remainder after dividing L by 10;
	let E be the easting part of V1 minus the easting part of V2;
	let E be the remainder after dividing E by 25;
	let N be the northing part of V1 minus the northing part of V2;
	let N be the remainder after dividing N by 25;
	let the sum be the spatial coordinate with maze level part L easting part E northing part N;
	decide on the sum.

Section E(d) - Shapes of individual rooms in the labyrinth

Labyrinth shape is a kind of value. L9/1-1-1-1-1-1 specifies a labyrinth shape with parts open space, U, D, N, E, S, W.[* The "open space" part is a number indicating how expansive the cave is. 0 indicates a mere intersection of passages; 1 is a space large enough for encounters to occur, while higher values are reserved for specially large caves, as we shall see.]

To say cave title: if the current maze level is 1, say "Clearing"; otherwise say "Cave".
To say chamber title: if the current maze level is 1, say "Grove"; otherwise say "Chamber".
To say tunnel title: if the current maze level is 1, say "Footpath"; otherwise say "Tunnel".

Table of Shape Frequencies
shape		shape name				legend	frequency	shape count
L1/0-0-1-1-1-1	"[cave title] of Four Ways"			"-O-"	6%	a number
L1/0-0-0-1-1-1	"[chamber title] of East-South-West Ways"	"-O-"	2%
L1/0-0-1-0-1-1	"[chamber title] of North-South-West Ways"	"-O "	2%
L1/0-0-1-1-0-1	"[chamber title] of North-East-West Ways"	"-O-"	2%
L1/0-0-1-1-1-0	"[chamber title] of North-East-South Ways"	" O-"	2%
L1/1-0-0-1-1-1	"[chamber title] of East-South-West Ways"	"-O-"	2%
L1/1-0-1-0-1-1	"[chamber title] of North-South-West Ways"	"-O "	2%
L1/1-0-1-1-0-1	"[chamber title] of North-East-West Ways"	"-O-"	2%
L1/1-0-1-1-1-0	"[chamber title] of North-East-South Ways"	" O-"	2%
L1/0-1-0-1-1-1	"[chamber title] of East-South-West Ways"	"-O-"	2%
L1/0-1-1-0-1-1	"[chamber title] of North-South-West Ways"	"-O "	2%
L1/0-1-1-1-0-1	"[chamber title] of North-East-West Ways"	"-O-"	2%
L1/0-1-1-1-1-0	"[chamber title] of North-East-South Ways"	" O-"	2%
L0/0-0-1-1-1-1	"Four Ways"				"-+-"	6%
L0/0-0-0-1-1-1	"East-South-West Junction"			"-o-"	2%
L0/0-0-1-0-1-1	"North-South-West Junction"			"-o "	2%
L0/0-0-1-1-0-1	"North-East-West Junction"			"-o-"	2%
L0/0-0-1-1-1-0	"North-East-South Junction"			" o-"	2%
L0/1-0-0-1-1-1	"East-South-West Junction"			"-o-"	2%
L0/1-0-1-0-1-1	"North-South-West Junction"			"-o "	2%
L0/1-0-1-1-0-1	"North-East-West Junction"			"-o-"	2%
L0/1-0-1-1-1-0	"North-East-South Junction"			" o-"	2%
L0/0-1-0-1-1-1	"East-South-West Junction"			"-o-"	2%
L0/0-1-1-0-1-1	"North-South-West Junction"			"-o "	2%
L0/0-1-1-1-0-1	"North-East-West Junction"			"-o-"	2%
L0/0-1-1-1-1-0	"North-East-South Junction"			" o-"	2%
L0/0-0-1-0-1-0	"North-South [tunnel title]"			" | "	4%
L0/0-0-0-1-0-1	"East-West [tunnel title]"			"---"	4%
L0/0-0-1-1-0-0	"North-East Bend"				" \-"	4%
L0/1-0-1-1-0-0	"North-East Bend"				" \-"	2%
L0/0-1-1-1-0-0	"North-East Bend"				" \-"	2%
L0/0-0-0-1-1-0	"South-East Bend"				" /-"	4%
L0/1-0-0-1-1-0	"South-East Bend"				" /-"	2%
L0/0-1-0-1-1-0	"South-East Bend"				" /-"	2%
L0/0-0-0-0-1-1	"South-West Bend"				"-\ "	4%
L0/1-0-0-0-1-1	"South-West Bend"				"-\ "	2%
L0/0-1-0-0-1-1	"South-West Bend"				"-\ "	2%
L0/0-0-1-0-0-1	"North-West Bend"				"-/ "	4%
L0/1-0-1-0-0-1	"North-West Bend"				"-/ "	2%
L0/0-1-1-0-0-1	"North-West Bend"				"-/ "	2%

Section E(e) - Labyrinth rooms

A labyrinth room is a kind of room. A labyrinth room has a labyrinth shape called shape. A labyrinth room has a text called map legend. A labyrinth room usually has map legend "-+-". A labyrinth room has a number called minimum level. A labyrinth room usually has minimum level 1. A labyrinth room can be grovelike or ungrovelike. A labyrinth room is usually ungrovelike.

Definition: A labyrinth room is unplaced if its grid position is <0,0,0>.
Definition: A labyrinth room is unshaped if its shape is L0/0-0-0-0-0-0.
Definition: A labyrinth room is ascending rather than non-ascending if the U part of its shape is 1 and the maze level part of its grid position is not 1.
Definition: A labyrinth room is descending rather than non-descending if the D part of its shape is 1 and the maze level part of its grid position is not 9.
[* The point here is that if a cave with an up staircase is found on the top level, the staircase must be ignored: similarly a down staircase on the bottom level.]

After printing the name of an ascending labyrinth room, say " with Staircase Up".
After printing the name of an descending labyrinth room, say " with Staircase Down".

[At the start of play, the game contains a large stock of labyrinth rooms which are not committed to any shape or position, but are waiting to be joined to the maze. The previous section's table is used to assign shapes to the rooms, but respecting their relative frequencies.]

To calculate how many labyrinth rooms should have each shape:
	let total space be the number of unshaped labyrinth rooms;
	let total allocated be 0;
	let total frequency be 0%;
	if the total space is 0, stop;
	repeat through the Table of Shape Frequencies:
		change the shape count entry to 0;
	repeat through the Table of Shape Frequencies:
		let N be the parts per hundred part of the frequency entry;
		let N be N multiplied by the total space;
		change the shape count entry to N divided by 100;
		change the total allocated to the total allocated plus the shape count entry;
		let total frequency be total frequency plus the frequency entry;
	unless the total frequency is 100%,
		say "Oops: the frequency table totals up to [total frequency].";
	[Nevertheless, if the cave extent is not a multiple of 50 we have probably under-allocated due to rounding errors. We make random draws to put this right.]
	while the total allocated is less than the total space:
		let total frequency be 0%;
		let R be a random number from 1 to 100;
		let the weighted percentage be the percentage with parts per hundred part R;
		repeat through the Table of Shape Frequencies:
			let total frequency be total frequency plus the frequency entry;
			if the weighted percentage > 0% and the weighted percentage <= the total frequency:
				change the shape count entry to the shape count entry plus 1;
				change the total allocated to the total allocated plus 1;
				change the weighted percentage to 0%.

To give shape to the shapeless labyrinth rooms:
	[Now we change the count entries so that a count entry of X means that the Xth room is the first to have the characteristics of the present row. In particular, there is guaranteed to be a row with a count entry of 1.]
	let total allocated be 0;
	repeat through the Table of Shape Frequencies:
		unless the shape count entry is 0:
			let new value be the total allocated plus 1;
			let total allocated be total allocated plus shape count entry;
			change the shape count entry to the new value;
	[Now we go through the rooms, using the current row to set each one, and moving to the appropriate row whenever one of the start counts is hit. Since there is guaranteed to be a count entry of 1 somewhere, the changes can only be reached when a row has been chosen.]
	let total allocated be 0;
	repeat with blank room running through unshaped labyrinth rooms:
		let total allocated be total allocated plus 1;
		if there is a shape count of total allocated in the Table of Shape Frequencies then choose row with a shape count of total allocated in the Table of Shape Frequencies;
		change the shape of the blank room to the shape entry;
		change the map legend of the blank room to the legend entry;
		change the printed name of the blank room to the shape name entry.

Before casting make sanctuary at: if the location is a labyrinth room or the location is the Budless Grove, say "You hear the laughter of the never-children in your mind, and know that you might as well save your strength." instead.

Before listing nondescript items when the location is a labyrinth room:
	now all hazards are unmarked for listing;
	if a hostile monster is marked for listing, say "The [if the current maze level is 1]lawn[otherwise]cave[end if] is guarded by [a list of hostile monsters which are marked for listing].";
	now all hostile monsters are unmarked for listing.

Section E(f) - Solid Rock and labyrinth boundaries

["Solid Rock" is a room which is never visited, but which is used to indicate blocked-off routes: an exit leading to Solid Rock is considered blocked. Initially, when none of the labyrinth rooms have been "played" into the maze, all exits are blocked in this way. Solid Rock is also used as a special value meaning that no room is at a given grid position: thus "the room at <1,4,12>" might evaluate to a room present in the maze, or it might evaluate to Solid Rock.]

Solid Rock is a room.

To make all labyrinth exits lead to Solid Rock:
	repeat with blank room running through labyrinth rooms:
		let Sh be the shape of the blank room;
		if U part of Sh is 1, change the up exit of blank room to Solid Rock;
		if D part of Sh is 1, change the down exit of blank room to Solid Rock;
		if N part of Sh is 1, change the north exit of blank room to Solid Rock;
		if E part of Sh is 1, change the east exit of blank room to Solid Rock;
		if S part of Sh is 1, change the south exit of blank room to Solid Rock;
		if W part of Sh is 1, change the west exit of blank room to Solid Rock.

To decide which room is the room at (grid ref - a spatial coordinate):
	if grid ref is <0,0,0> then decide on Solid Rock;
	repeat with R running through rooms:
		if the grid position of R is grid ref then decide on R;
	decide on Solid Rock.

Section E(g) - Terra Incognita

[Like "Solid Rock", "Terra Incognita" is a room which is never visited. The rule is that a maze exit leads to TI if and only if it would take one to a grid position which is still inside the boundaries of the maze, but at which no room has yet been located. Except for the special case of the Hedge Archway joining the maze to the outside world, all exits in the maze lead either to Solid Rock (no way through), Terra Incognita (we try extending the maze if this exit is taken) or another room in the maze.

The "position ... at ..." phrase is crucial in establishing the game's mechanics, because it maintains these rules about exits, and checks to see whether adjacent rooms have exits which join up or not. Note that up and down staircases always work (the "vertical" directions), but are sometimes secret and invisible from the other side - a quirk of the original "Sorcerer's Cave" board game which lives on here.]

Terra Incognita is a room.

Egress relates a room (called the place) to a direction (called that way) when the room that way from the place is a room. The verb to exit (it exits) implies the egress relation.

To position (new room - a room) at (grid ref - a spatial coordinate):
	change the grid position of the new room to the grid ref;
	repeat with this way running through vectorial directions:
		let the further position be the vector sum of the grid ref and the vector of this way;
		[say "In direction [this way], the position would be [further position].";]
		if the further position is not <0,0,0>:
			let the further room be the room at the further position;
			if the further room is Solid Rock:
				if the new room exits this way then change this way exit of the new room to Terra Incognita;
			otherwise:
				let the reverse way be the opposite of this way;
				if this way is vertical or the new room exits this way:
					if the further room exits the reverse way:
						change this way exit of the new room to the further room;
						change the reverse way exit of the further room to the new room;
					otherwise:
						change this way exit of the new room to Solid Rock;
				otherwise:
					if the further room exits the reverse way, change the reverse way exit of the further room to Solid Rock.

Section E(h) - Exhausting the maze and retrieving rooms

[Terra Incognita is said to be "open" if there are still maze rooms left to discover, and "closed" if they have all been played, so that the maze is exhausted.]

Terra Incognita can be open or closed. Terra Incognita is open.

[When the maze has been exhausted, it often happens that several of the rooms played have never been visited. If the player tries going east into Terra Incognita, say, a new room is placed at the appropriate grid position. If that new room has a connection back west, all well and good. If it doesn't, the player is stymied, and the original map connection changed to Solid Rock. But the new room is not removed again: instead, it waits in the labyrinth to be discovered from another side. (Such rooms were played as face-down cards in the original board game.) But what if the player never does discover another way in? They then remain unvisited forever.

Except that they don't, because we follow a procedure called retrieval to take such rooms back into the pack for further playing if the pack ever runs out. Note that the Deep Pool Environs area is fixed: those few rooms stay put whether or not they are ever visited.]

Definition: A labyrinth room is retrievable if it is unvisited and it is not in the Deep Pool Environs.

To re-plumb (place - a room) to (way - a direction):
	let the connection be the room way from the place;
	if the connection is a retrievable labyrinth room, change the way exit of the place to Terra Incognita.

To retrieve inaccessible labyrinth rooms:
	repeat with maze area running through retrievable labyrinth rooms:
		re-plumb the maze area to north;
		re-plumb the maze area to east;
		re-plumb the maze area to south;
		re-plumb the maze area to west;
		re-plumb the maze area to up;
		re-plumb the maze area to down;
		change the grid position of the maze area to <0,0,0>.

Section E(i) - Exploring the labyrinth

Definition: A labyrinth room is acceptable if it is unplaced and its minimum level <= the current maze level.

Before going a direction (called this way) to Terra Incognita:
	if the location is unplaced or this way is not vectorial, say "Oops! You should only reach terra incognita from a placed room via a vectorial direction."; [* This never happens, of course: it was put in to test the game.] 
	if Terra Incognita is closed, say "The way is barred by some enchantment of the never-children." instead;
	let possibles be the number of acceptable labyrinth rooms;
	if possibles is 0:
		retrieve inaccessible labyrinth rooms;
		let possibles be the number of acceptable labyrinth rooms;
		if possibles is 0:[* I.e., if even retrieving unvisited rooms didn't help: now the maze really is exhausted.]
			now Terra Incognita is closed;
			say "In your mind, you hear the scornful laughter of the never-children. 'Magic-uther has run out of labyrinth!' And certainly the way is blocked." instead;
	if possibles > 0:
		let the new room be a random acceptable labyrinth room;
		let the current position be the grid position of the location;
		let the new position be the vector sum of the current position and the vector of this way;
		position the new room at the new position;
	otherwise:
		change this way exit of the location to Solid Rock;
	try going this way instead.

After going up to a labyrinth room from a non-ascending labyrinth room: say "You take the well-remembered secret staircase up."; continue the action.

After going down to a labyrinth room from a non-descending labyrinth room: say "You take the well-remembered secret staircase down."; continue the action.

Instead of going to Solid Rock:
	say "After a few strides, the way gives out into ";
	if the current maze level is 1, say "dense, impenetrable undergrowth";
	otherwise say "solid rock";
	say ". You step back again."

Section E(j) - The force labyrinth spell

[All players are given this spell as a sort of Get Out of Jail Free card, but it tends not to be as good as they imagine. It forces a map connection to work between a given pair of adjacent caves, and can be used if one is stranded by one of the Earthquake hazards.]

Instead of casting force labyrinth at when Terra Incognita is closed, say "The never-children laugh once again."

Before casting force labyrinth at:
	unless the location is a labyrinth room, say "That spell has efficacy only in the Maze of Royal Beasts." instead.[* This is a before rule, unconventionally, to stop players from wasting precious STR finding out what the spell is supposed to do.]

To force a path (way - direction) from (place - room):
	let the current position be the grid position of the location;
	let the new position be the vector sum of the current position and the vector of the way;
	if the new position is <0,0,0>, stop;
	let the new room be the room at the new position;
	if the new room is Solid Rock:
		if a labyrinth room is acceptable:
			let the new room be a random acceptable labyrinth room;
			let the current position be the grid position of the location;
			position the new room at the new position;
		otherwise:
			stop;
	now the new room is Earthquake-undamaged;
	change the way exit of the location to the new room;
	let the reverse way be the opposite of the way;
	change the reverse way exit of the new room to the location.

Effect of casting force labyrinth at:
	record outcome "the whole area before you fills with mist, momentarily leaving your mind a blur";
	if the room north from the location is Solid Rock, force a path north from the location;
	if the room east from the location is Solid Rock, force a path east from the location;
	if the room south from the location is Solid Rock, force a path south from the location;
	if the room west from the location is Solid Rock, force a path west from the location;
	continue the action.

Section E(k) - The Hedge Archway and the leaves

The current maze task is a thing that varies. The maze victory count is a number that varies.

The Hedge Archway is a labyrinth room with shape L0/0-0-1-1-1-0. The Hedge Archway has map legend "<H>". "The Maze is composed of solid, impenetrable hedges that might as well be dungeon walls, and are certainly too high to see over. Paths lead north, east and south into the labyrinth."

The branch like an extended hand is fixed in place in the Hedge Archway. "A branch, hanging out over the archway, seems almost like an extended hand: and you cannot help noticing, hanging down from the branch, [the list of leaves which are part of the branch]." Before doing something to the branch, say "The branch itself is too high above you." instead. The branch can be too high or almost too high. The branch is almost too high. The branch is wood.

Before going in the Hedge Archway when the branch is almost too high and ( noun is north or noun is east or noun is south ) , say "The hedge somehow closes to prevent you, pushing you back without pushing you. A moment later, it's as if nothing had happened." instead.

The plural of leaf is leaves. A leaf is a kind of thing. A leaf is usually magical essence. Some leaves part of the branch are defined by the Table of Task Leaves. The description of a leaf is "The leaf unrolls to reveal black writing on its open face: Your task, o mage, is to [task briefing]. Fail not in this quest: yet, if it must be so, you may tear this leaf to abandon your effort and return here (if you be not in combat at the time), at a permanent cost of 1 STR point. And remember always that if the maze be exhausted, you may never reach your goal... so do not tarry, do not meander."

Instead of examining a leaf which is part of the branch, say "It is rolled up, as if not quite opened by the sun, and although there is evidently writing inside, you can make little out except the word [clue word of the noun]." Instead of taking a leaf which is part of the branch, say "The branch is high enough up that its leaves are just slightly out of reach."

Understand "tear [something]" or "tear up [something]" or "tear [something] up" as attacking.

Instead of jumping in the Hedge Archway when a leaf is part of the branch:
	if the branch is too high, say "The branch is just too high, even at a jump. Perhaps your earlier pulling disturbed it." instead;
	now the branch is too high;
	let the grasped leaf be a random leaf which is part of the branch;
	now the player carries the grasped leaf;
	now the player knows force labyrinth;
	change the current maze task to the grasped leaf;
	if the grasped leaf is the mauve leaf, move Eurydice to the Island in the Deep Pool;
	say "Jumping up, you manage to catch [the grasped leaf] in your hand and it pulls away."

Instead of attacking a leaf:
	if combat proceeds, say "The leaf might as well be made of bronze. It won't tear." instead;
	remove the noun from play;
	say "You tear [the noun] across, feeling a sudden stabbing pain, and the world is momentarily hazy. When you reappear, even the hedges themselves are somehow shaking, adjusting, changing.";
	if a leaf is part of the branch:
		tidy the labyrinth for a fresh quest;
	otherwise:
		say "[paragraph break]But it seems there will be no more patience. Even as you reassemble at the archway, the never-children tear you limb from limb!";
		end the game saying "You exhausted the Maze of Royal Beasts".

Section E(l) - The four tasks

Table of Task Leaves
name		clue word	task briefing
a mauve leaf	"awaken"	"awaken Eurydice from her enchanted sleep on the Island in the Deep Pool"
a russet leaf	"visit"	"visit all [number of grovelike unvisited labyrinth rooms in words] remaining chambers and grove[s], and return"
a piebald leaf	"slay"	"slay a monster on level 4 or below, and return"
a pea-green leaf	"eye"	"find and bring back the Eye of God"

To win the maze task:
	if the player carries the current maze task, say "The leaf pulses with life in your hand. ";
	say "You have fulfilled your task ([task briefing of the current maze task]) in the Maze of Royal Beasts! You sense that, now, the never-children will not dare stand in your way.";
	award 100 points;
	increase the maze victory count by 1;
	remove the current maze task from play;
	if a leaf is part of the branch, tidy the labyrinth for a fresh quest;
	otherwise remove the branch from play;
	change the current maze task to the branch; [i.e., no task at all]
	unless the location is the Hedge Archway, move the player to the Hedge Archway.

The deepest slaying level is a number that varies. Rule for dead body disposal of a monster (called the victim) when the location is a labyrinth room: restore the health of the victim; return the victim to the pack; if the current maze level is greater than the deepest slaying level, change the deepest slaying level to the current maze level; continue the activity.

Every turn when the location is the Hedge Archway:
	if the current maze task is the piebald leaf and the deepest slaying level is greater than 3, win the maze task;
	if the current maze task is the pea-green leaf and the player carries the Eye of God, win the maze task;
	if the current maze task is the russet leaf and all grovelike labyrinth rooms are visited, win the maze task.

Eurydice is a woman. "Eurydice, the beautiful maiden you are bidden to rescue, turns out to be just eight inches tall. Or rather longways, since she is lying down asleep." Instead of kissing Eurydice, say "Mmm. I'm just going to pause for a moment to let the grotesqueness of that image sink in." Instead of attacking Eurydice, say "But she's so delicate." Instead of taking Eurydice, say "That would be... well. The taboos in your culture protect the faerie-folk from such abuse." Instead of doing something to Eurydice in the presence of a hostile monster, say "Eurydice is too well guarded to approach." Instead of turning Eurydice: say "As you turn Eurydice delicately around, and she comes to face north, she awakens with a start. 'Free! I am free!' And she flies away into the cavern, apparently never even having noticed you, much less offered any thanks."; win the maze task. Instead of pushing or pulling Eurydice, say "There is too little room on the Island to do much more than turn on the spot."

Effect of casting mindsift at Eurydice:
	record outcome "and you 'remember' lying down on this desolate rock, all hope gone with your strength, and resting: your last act to arrange yourself east-west, as no faerie can rest when turned north-south";
	rule succeeds.

Section E(m) - The fifty rooms of the inner labyrinth

LR0, LR1, LR2, LR3, LR4, LR5, LR6, LR7, LR8 and LR9 are labyrinth rooms.
LR10, LR11, LR12, LR13, LR14, LR15, LR16, LR17, LR18 and LR19 are labyrinth rooms.
LR20, LR21, LR22, LR23, LR24, LR25, LR26, LR27, LR28 and LR29 are labyrinth rooms.
LR30, LR31, LR32, LR33, LR34, LR35, LR36, LR37, LR38 and LR39 are labyrinth rooms.
LR40 and LR41 are labyrinth rooms.

LR0 has shape L0/0-0-1-1-1-0. LR1 has shape L0/0-0-1-1-1-0.[* There are several obviously special labyrinth rooms, with exotic names and behaviour. These two rooms are less obviously special. They are T-junctions pointing into the maze, which will be placed just north and just south of the Hedge Archway respectively, and their purpose is to maximize the initial connectedness of the maze - they guarantee the explorer initial access to five possible rooms rather than the three which the Hedge Archway alone would lead into, and this decreases the chances of a boringly quick game in which the maze is fully blocked off in a couple of rooms.]

The Temple of Peace is a labyrinth room with shape L0/0-0-1-1-1-1. "The cave walls soften, become less harsh: glisten with warm reflections: congregate with soft sounds. This is almost a temple, a place of such perfect peace that it feels almost like sanctuary." The Temple of Peace has map legend "<T>". The Temple of Peace has minimum level 2.

The Island in the Deep Pool is a labyrinth room with shape L1/0-0-0-0-0-0. "This rocky islet, in the centre of a deep pool of ice-cold mountain water, must be one of the most seldom-visited places in all of Tolti-Aph. There is no apparent way off." The Island in the Deep Pool has map legend "(O)".

The Dome of the White Bones is a labyrinth room with shape L3/0-0-1-1-1-1. "The walls of this spectacularly high cave are whitened and ridged, so that to stand in this natural arena is to imagine oneself inside the ribcage of a long-dead behemoth." The Dome of the White Bones has map legend "(B)". The Dome of the White Bones has minimum level 2.

A shoreline room is a kind of labyrinth room. The Western Shore is a shoreline room with shape L0/0-0-1-0-1-1. The Northern Shore is a shoreline room with shape L0/0-0-1-1-0-1. The Eastern Shore is a shoreline room with shape L0/0-0-1-1-1-0. The Southern Shore is a shoreline room with shape L0/0-0-0-1-1-1. The Northern Shore is northeast of the Western Shore. The Eastern Shore is southeast of the Northern Shore. The Southern Shore is southwest of the Eastern Shore. The Western Shore is northwest of the Southern Shore. The description of a shoreline room is "This cavern shelves away into a deep pool of pure, ice-cold mountain water, almost a subterranean lake. In the centre is clearly an island, but you can make out little more in this dim light. As the lake is far too deep to ford, the only routes are back away from the water, or diagonally around its edge."

Deep Pool Environs is a region. The Island in the Deep Pool, the Eastern Shore, the Northern Shore, the Western Shore and the Southern Shore are in the Deep Pool Environs.

The pure ice-cold lake of mountain water is a scenery backdrop. The ice-cold lake is in the Deep Pool Environs. Understand "deep" as the ice-cold lake. Understand "pool" as the ice-cold lake. Instead of drinking the pure ice-cold mountain water, say "It is cold enough that you can momentarily feel distinct sensations in each of your teeth." The ice-cold lake is air.

The rocky island is a scenery backdrop. The rocky island is in the Eastern Shore, the Northern Shore, the Western Shore and the Southern Shore. Instead of doing something to the rocky island, say "The rocky island is too far away."

Swimming is an action applying to nothing. Carry out swimming: say "There's no very appealing pool here."

Instead of swimming in the Deep Pool Environs:[* Play-testers generally assumed that this was the only way to reach the Island - which the player must of course do in order to win the "rescue Eurydice" quest. In fact, there are two other ways: to use the Magic Carpet to fly across the lake from any of the four shores, or to get lucky and find a staircase down from the room vertically above the island - or a staircase up from the room vertically below.]
	unless the player is affected by inner warmth, say "You step into the water, intending to swim, but twenty seconds later you are out again, yelping." instead;
	if the location is the Island in the Deep Pool:
		say "Protected from the icy chill by the inner warmth spell, you swim confidently across to the north.";
		move player to the Northern Shore;
	otherwise:
		say "Protected from the icy chill by the inner warmth spell, you swim confidently across to the island.";
		if the Island in the Deep Pool is unvisited, draw cards for the Island in the Deep Pool;
		move player to the Island in the Deep Pool.

Section E(n) - Setting up the labyrinth

To restore the labyrinth to its virgin state:
	make all labyrinth exits lead to Solid Rock;
	position the Hedge Archway at <1,1,12>;
	position LR0 at <1,1,11>;
	change the printed name of LR0 to "North-East-South Junction";
	position LR1 at <1,1,13>;
	change the printed name of LR1 to "North-East-South Junction";
	position the Island in the Deep Pool at <3,6,12>;
	position Western Shore at <3,5,12>;
	position Eastern Shore at <3,7,12>;
	position Northern Shore at <3,6,13>;
	position Southern Shore at <3,6,11>;
	repeat with the locale running through labyrinth rooms:
		let the locale shape be the shape of the locale;
		if the U part of the locale shape is 1, change the minimum level of the locale to 2;
		now the locale is grovelike;
		if the open space part of the locale shape is 0, now the locale is ungrovelike;
		if the N part of the locale shape is 1 and the E part of the locale shape is 1 and the S part of the locale shape is 1 and the W part of the locale shape is 1, now the locale is ungrovelike;
		if the N part of the locale shape is 0 and the E part of the locale shape is 0 and the S part of the locale shape is 0 and the W part of the locale shape is 0, now the locale is ungrovelike.

When play begins:
	calculate how many labyrinth rooms should have each shape;
	give shape to the shapeless labyrinth rooms;
	restore the labyrinth to its virgin state.

To tidy the labyrinth for a fresh quest:
	now the branch is almost too high;
	now Terra Incognita is open;
	repeat with maze area running through labyrinth rooms:
		unless the maze area is the Hedge Archway:
			change the grid position of the maze area to <0,0,0>;
			now the maze area is unvisited;
			repeat with trinket running through things in the maze area:
				if the trinket is not a backdrop and the trinket is not the player, move the trinket to Terra Incognita;
				if the trinket is a person, restore the health of the trinket;
	remove Eurydice from play;
	restore the labyrinth to its virgin state;
	unless the location is the Hedge Archway, move the player to the Hedge Archway;
	decrease the permanent strength of the player by 1;
	restore the health of the player.

Section E(o) - The solitaire game variant

[The Solitaire game was invented as a testing convenience: if, on the first turn, one types "solitaire", then the rest of ROTA does not exist and one plays the Maze as a self-contained game. I've left this in the final game as an "easter egg".]

The solitaire game count is a number that varies.

Preparing to maze is an action out of world applying to nothing. Understand "solitaire" as preparing to maze. Check preparing to maze: unless the turn count is 1, say "Solitaire maze play is only allowed if chosen on the opening round." instead. Carry out preparing to maze: change the solitaire game count to 1; change the level of the player to 3; change the permanent strength of the player to 16; restore the health of the player; now the player knows magic missile; now the player knows make sanctuary; now the player knows mend; move the old pipe to the player; now the player carries the arrow; now the player knows web; now the player knows aerial shield; move player to the Hedge Archway.

Before going west from the Hedge Archway when the solitaire game count is not 0:
	end the game saying "You have left the Maze of Royal Beasts" instead.

Section E(p) - Treasures of sorts

A reddish vial is a strength potion in Terra Incognita. It has additional strength 2d4+4. A brownish vial is a strength potion in Terra Incognita. It has additional strength 2d4+4. A yellowish vial is a strength potion in Terra Incognita. It has additional strength 1d8+2. A greenish vial is a strength potion in Terra Incognita. It has additional strength 3d6+6. The brownish vial is trapped.

An old wooden dowel rod and a broken-off handle are in Terra Incognita. The dowel and the handle are wood. In Terra Incognita are some chain mail, a lance and a battle axe. A dead badger is in Terra Incognita. "A dead badger, striped forehead lain over his legs, lies here. Nothing so terrible seems to have happened to him: only old age." The dead badger is flesh. Instead of eating the dead badger, say "Things are not that desperate yet: and let us hope they never will be." Effect of casting mend at the dead badger: record outcome "but look, if the spell were 'resurrect badger' it would have said so"; rule succeeds.[* This bric-a-brac is more useful than it seems: the wooden items make convenient "magic missile" foci, while the poor badger allows flesh-focus spells to be cast. There are badgers in the wood I most often go for walks in, and I'm rather fond of them: but they do look inexpressibly sad in death.]

A scrappy scroll is a scroll in Terra Incognita. The inscribed spell of the scrappy scroll is inner warmth.

A hazard is a kind of thing. Earthquake and Trap are kinds of hazard. In Terra Incognita are two Earthquakes and two Traps.

In Terra Incognita are two goblins, three gnomes, a frost giant, a green hydra, one centaur, two half-orcs, one harpy, one basilisk, one purple worms, two gargoyles, a cave troll, two cave spectres, two skeletal warriors and a giant bat.[* At one time there were more bats, and because many cards don't appear in the hedge maze on level 1, players tended to run into giant bats rather often: one of the play-testers remarked that it seemed a little sad to be wandering around making the giant bat extinct in one its last habitats. Her comments ended up, in mutated form, as the "Outdoorsman" column in the spoof magazine accompanying the game.]

Section E(q) - The seven artefacts

[The artefacts are not unlike those found in "Sorcerer's Cave", though all of the details are different. The Eye of God was differently cursed in the original: here it is dangerous to carry around, but one of the maze quests is exactly to carry it for a long way. Our version of the Ring is immensely powerful, but there is a catch: although the wearer becomes almost immortal on lower levels, he is likely to emerge back up to higher levels with dangerously low STR, so there is some danger of surviving a tussle with demons far below only to be knocked on the head by a gnome on level 1.

No equivalent to the Divining Rod and the Enchanted Map exist in the original board game: nor could they, really, since the players could always see the entire labyrinth laid out on the floor.]

An artefact is a kind of thing. An artefact is magical essence. An artefact has a number called minimum level. The minimum level of an artefact is usually 1. Every turn when the location is the Budless Grove and the player carries an artefact: say "You barely notice the disappearance of [the list of artefacts carried by the player], returned by magic into the labyrinth."; repeat with trinket running through artefacts carried by the player begin; return the trinket to the pack; end repeat.

Instead of inserting an artefact into something, say "The artefacts of the maze may not be concealed in any way."

The Charmed Flute is an artefact in Terra Incognita. "The Charmed Flute, a precious artefact, lies discarded here." The description is "When played, the Charmed Flute soothes any hostile creatures (other than the undead) to quiescence: but the Flute may be played only once before vanishing again into the labyrinth."

Musically playing is an action applying to one carried thing. Understand "play [something]" as musically playing. Check musically playing: unless the noun is the Charmed Flute, say "That is not a musical instrument you know how to play." instead.

Carry out musically playing:
	if a hostile monster is in the location:
		say "As you play an unearthly lullaby on the Charmed Flute, it is as if you soothe [the list of hostile monsters in the location]. You are now free to walk peaceably here; but the Flute, its task performed, vanishes away.";
		now all monsters in the location are indifferent;
		return the Charmed Flute to the pack;
	otherwise:
		say "You play a short passacaglia to pass the time. But in the absence of hostile creatures, there is no effect beyond the soothing of your tired soul.".

The Eye of God is an artefact in Terra Incognita. "The Eye of God, a baleful green gemstone, glares up at you." It has minimum level 3. The description is "Cursed and yet precious, the Eye of God is the strangest of all the artefacts of the labyrinth."

Rule for calculating combat modifiers of the player:
	if the player carries the Eye of God, modify to-hit roll by -2 for "carrying the Eye of God";
	continue the activity.

The Ring of Power is an artefact in Terra Incognita. "Discarded almost as if it were not the most puissant of all artefacts in the labyrinth is this Ring of Power." It has minimum level 2. The description is "On the third and lower levels of the labyrinth, the wearer of the Ring of Power is invulnerable from (most) harm inflicted by others when his or her STR falls below twice his or her own level." The Ring of Power is wearable.

The Talisman is an artefact in Terra Incognita. "Propped up in the centre of the space in front of you is the Talisman." It has minimum level 2. The description is "He who carries the Talisman will not be attacked by the undead." Instead of wearing the Talisman, say "This charm is carried, not worn."

Every turn when the player carries the Talisman and an undead monster is in the location:
	now all undead monsters in the location are indifferent;
	say "The Talisman wards off attack from [the list of undead monsters in the location]."

For calculating damage modifiers of the player:
	let the threshold be the level of the player;
	let the threshold be the threshold multiplied by 2;
	let the putative strength be the strength of the player;
	decrease the putative strength by the damage incurred;
	if the player is wearing the Ring of Power and the current maze level is greater than 2 and the putative strength is less than the threshold:
		say ": the Ring of Power on your finger glows fire-red";
		let the largest safe amount be the strength of the player;
		decrease the largest safe amount by the threshold;
		if the largest safe amount is less than 0, let the largest safe amount be 0;
		change the damage incurred to the largest safe amount;
		stop; [I.e., don't continue the activity: there's no point.]
	continue the activity.

The Magic Carpet is an artefact in Terra Incognita. "Rolled up in front of you is the Magic Carpet." It has minimum level 2. The description is "A useful artefact of the labyrinth: it enables the mage, once only, to move one position in any direction (north, south, east, west, up or down) regardless of walls or hedges. (Type RIDE CARPET NORTH, or how may you.)"

Carpet-riding is an action applying to one carried thing and one visible thing. Understand "ride [something] [direction]" as carpet-riding. Check carpet-riding: unless the noun is the Magic Carpet, say "That's not something you can ride." instead; unless the second noun is a direction, say "That's not a direction you can take the carpet in." instead.

Carry out carpet-riding:
	let the current position be the grid position of the location;
	let the new position be the vector sum of the current position and the vector of the second noun;
	if the new position is <0,0,0>, say "The carpet is unable to take you that way, which would lie outwith the labyrinth." instead;
	let the destination room be the room at the new position;
	let the novelty factor be 0;
	if the destination room is Solid Rock:
		if a labyrinth room is acceptable:
			let the destination room be a random acceptable labyrinth room;
			position the destination room at the new position;
			let the novelty factor be 1;
		otherwise:
			say "The carpet is unable to take you that way, which would lie outwith the labyrinth." instead;
	return the Magic Carpet to the pack;
	say "The Magic Carpet transports you one place [second noun] in the labyrinth, then melts away into air.";
	let the way be the second noun;
	let the reverse way be the opposite of the second noun;
	if the room the way from the location is Terra Incognita:
		if the room the reverse way from the destination room is Terra Incognita:
			change the way exit of the location to the destination room;
			change the reverse way exit of the destination room to the location;
		otherwise:
			change the way exit of the location to Solid Rock;
			change the reverse way exit of the destination room to Solid Rock;
	if the novelty factor is 1, draw cards for the destination room;
	move the player to the destination room.

An artefact called the Divining Rod is in Terra Incognita. "The Divining Rod, one of the artefacts of the Maze, lies before you." The description is "If waved, the divining rod will give the mage some idea of his location in the labyrinth."

Instead of waving the Divining Rod:
	say "The Divining Rod senses that ";
	if the location is the Hedge Archway, say "you are in the Archway. Well, there's a surprise." instead;
	unless the current maze level is 1, say "you are below ground, in level [current maze level] of the labyrinth; ";
	let the current position be the grid position of the location;
	let the budless position be the grid position of the Hedge Archway;
	let the eastward journey be the easting part of the current position;
	decrease the eastward journey by the easting part of the budless position;
	let the northward journey be the northing part of the current position;
	decrease the northward journey by the northing part of the budless position;	
	let the westward journey be 0 minus the eastward journey;
	let the southward journey be 0 minus the northward journey;
	if the eastward journey is 0 and the northward journey is 0:
		say "you are vertically below the Archway, ";
	otherwise:
		say "you have travelled ";
		if the eastward journey > 0, say "east by [eastward journey in words], ";
		if the westward journey > 0, say "west by [westward journey in words], ";
		if the northward journey > 0, say "north by [northward journey in words], ";
		if the southward journey > 0, say "south by [southward journey in words], ";
	let the way out be the best route from the location to the Hedge Archway;
	if the way out is a direction, say "and it pulls your hand to the direction [way out].";
	otherwise say "and it shakes in your hand."

The Enchanted Map is an artefact in Terra Incognita. "The Enchanted Map, one of the artefacts of the Maze, lies here for you to find."

To plot line (N - a number) of room (R - a room):
	if R is Solid Rock:
		say "     ";
		stop;
	if R is unvisited:
		say ":::::";
		stop;
	if R is Earthquake-damaged:
		say "#####";
		stop;
	if R is the Island in the Deep Pool:
		if N is 1, say "~~~~~";
		if N is 2, say "~~O~~";
		if N is 3, say "~~~~~";
		stop;
	if N is:
		-- 1:
			if R exits north, say "  |";
			otherwise say "   ";
			if R is ascending, say "/ ";
			otherwise say "  ";
		-- 2:
			if R exits west, say "-";
			otherwise say " ";
			say the map legend of R;
			if R exits east, say "-";
			otherwise say " ";
		-- 3:
			if R is descending, say " /";
			otherwise say "  ";
			if R exits south, say "|  ";
			otherwise say "   ".


Instead of examining the Enchanted Map:
	let grid ref be the grid position of the location;
	if grid ref is <0,0,0>, stop;
	let L be the maze level part of the grid ref;
	if L is:
		-- 1: let the design be "=-=-=";
		-- 2: let the design be "-=-=-";
		-- 3: let the design be "=+=+=";
		-- 4: let the design be "=====";
		-- 5: let the design be "=+o+=";
	if L is greater than 5, let the design be "#####";
	say "[fixed letter spacing]+[design][design][design][design][design]+[line break]";
	repeat with dN running from -1 to 1:
		repeat with stripe running from 1 to 3:
			say "#";
			repeat with dE running from -2 to 2:
				let E be the easting part of the grid ref;
				let N be the northing part of the grid ref;
				let L be the maze level part of the grid ref;
				let E be E plus dE;
				let N be N minus dN;
				let the offset ref be the spatial coordinate with maze level part L easting part E northing part N;
				let the offset room be the room at the offset ref;
				plot line stripe of room offset room;
			say "#[line break]";
	say "+[design][design][design][design][design]+[line break][variable letter spacing]".

Section E(r) - The encounters pack

[Here we populate the maze. When the player enters a new-found room which has open space, "cards" are drawn from the "encounters pack" to see what will be found there. The cards are actually the things (monsters, etc.) themselves and the pack is a container - though, like the Solid Rock and Terra Incognita rooms, it exists in a somewhat metaphysical way and is not actually to be found in the player's world.

We arrange these "cards" in a pack because dead monsters, used artefacts and played hazard cards need to be returned to the bottom of the pack. It would not do to simply draw randomly from the pool of out-of-world cards: then there would be a chance of immediately redrawing the Charmed Flute, say, only a turn after giving it up.

Although not much Inform code uses or notices this, the contents of a room or container are in fact organised as a list, and we sneakily use this as the ordering of the pack. Note that "return X to the pack", defined below, puts X at the bottom by removing all of the cards, putting X in, then putting all the cards back again - which sounds slow, but it's a trifling exercise for the computer, and in any case happens relatively seldom in play.]

The encounters pack is a container.

Definition: a thing is out-of-the-pack if it is in Terra Incognita.

When play begins:
	let the pack size be the number of out-of-the-pack things;
	let the total shuffled be 0;
	while the total shuffled is less than the pack size
	begin;
		let the next card be a random out-of-the-pack thing;
		move the next card to the encounters pack;
		let the total shuffled be the total shuffled plus 1;
	end while.

To return (this card - a thing) to the pack:
	move this card to Terra Incognita;
	now all things in the encounters pack are in Terra Incognita;
	now all things in Terra Incognita are in the encounters pack.

The previous location is a room that varies. Before going, change the previous location to the location.

After going to an unvisited labyrinth room (called the locale): draw cards for the locale; continue the action.

To draw cards for (locale - a room):
	if the open space part of the shape of the locale is 0, stop;
	let the draw count be the open space part of the shape of the locale plus the maze level part of the grid position of the locale;
	let the preferred artefact be the Enchanted Map;
	if the current maze task is the mauve leaf, let the preferred artefact be the Charmed Flute;
	if the current maze level is 1 and the preferred artefact is in the encounters pack:[* We rig the draw to make sure that the first thing the player encounters is a useful artefact: ordinarily the Map, but in the "rescue Eurydice" quest it's the Charmed Flute instead, since we picture the player as Orpheus.]
		move the preferred artefact to the locale;
		stop;
	if the current maze task is the pea-green leaf and the Eye of God is in the encounters pack and the number of unvisited labyrinth rooms is less than 10:[* In the quest to bring back the Eye of God, it would be a bit unfair for the Eye to remain at the bottom of the pack for the whole game: so once four-fifths of the maze is played out, the Eye of God is rigged to come up next.]
		move the Eye of God to the locale;
		decrease the draw count by 1;
	while the draw count is greater than 1 and something (called the top card) is in the encounters pack:
		move the top card to the locale;
		if the current maze level is 1:[* The garden maze does not suffer from trap-doors, earthquakes or the baleful presence of the undead.]
			if the top card is a hazard, return the top card to the pack;
			if the top card is a monster and the defensive charm of the top card is exorcise undead, return the top card to the pack;
		if the locale is the Dome of the White Bones and the top card is a hazard, return the top card to the pack;[* The Dome is a huge cave and likely to result in many cards being drawn, which means there is a good chance of a Trap or an Earthquake: when these did indeed happen in play-testing, the effect was unsatisfactory in the first case, unfair in the second. So the Dome has been made hazard-free.]
		if the top card is an artefact and the current maze level is less than the minimum level of the top card, return the top card to the pack;[* For instance, you won't find the Ring by wandering around the hedge maze.]
		if the top card is a monster, now the top card is surprised;
		decrease the draw count by 1.

Section E(s) - Traps and earthquakes

[Donnelly's original "Sorcerer's Cave" had four hazards, the others being Medusa and Mutiny, but neither works very well for a game played with a single character rather than a "party" acting as a team. Trap and Earthquake are essentially unchanged from the original.]

Hazard-playing is an action out of world applying to one thing.

Every turn:
	let the locale be the location;
	repeat with hazard card running through hazards in the locale:
		try hazard-playing the hazard card.

To make the player fall to (destination - room):
	if the destination is unvisited, draw cards for the destination;
	move the player to the destination.

Carry out hazard-playing a Trap:
	return the noun to the pack;
	let the position below be the vector sum of the grid position of the location and the vector of down;
	if the position below is <0,0,0>, continue the action;
	if the location is descending, continue the action;
	let the place below be the room at the position below;
	unless the place below is Solid Rock:
		say "You fall through a concealed Trap in the cave floor, to emerge back at...";
		change the down exit of the location to the place below;
		make the player fall to the place below;
		stop;
	if a labyrinth room (called the place below) is unplaced:
		say "You fall through a concealed Trap in the cave floor!";
		position the place below at the position below;
		change the down exit of the location to the place below;
		unless the place below is ascending,
			change the up exit of the place below to Solid Rock;
		make the player fall to the place below.

A room can be Earthquake-damaged or Earthquake-undamaged. A room is usually not Earthquake-damaged. Instead of going to an Earthquake-damaged room, say "That whole cave has collapsed in an earthquake, and is impassible."

Carry out hazard-playing an Earthquake:
	return the noun to the pack;
	unless the previous location is a labyrinth room, continue the action;
	if the previous maze level is 1, continue the action;[* An Earthquake causes a rockfall to block the room just left behind: that would make no sense up in the hedge maze on level 1.]
	if the player is in the previous location, continue the action;
	say "An Earthquake causes a rockfall back in [the previous location]!";
	now the previous location is Earthquake-damaged.


Chapter W - Western Quarter

Section W(a) - Cure baldness, mindsift and corrode

[The four quarters are in effect independent scenarios, but there's a certain amount of cross-talk: the new spells found in each quarter have to have interesting effects on the items and things in the other quarters. Here, at any rate, we create two new W&W monsters, and three new W&W spells.]

Table of Monstrous Beasts (continued)
name		strength	hand-to-hand combat method	unarmed combat hit dice	defensive charm
reptile	5	"striking at you"	1d6
priest	12	"jabbing you with martial artistry"	1d6


Table of Enchantments (continued)
spell		nature	cost	requirement	emission		targeting		duration	duration timer	usage count
mindsift		arcana	6	metal	"a tracery of silver lines"		targeted		--	
cure baldness	healing	4	flesh	"citrus-scented steam"	targeted
corrode	offensive	4	wood	"dense fog"	targeted


Effect of casting cure baldness at something:
	record outcome "but goes out again without achieving much more than a passing aroma of lemons";
	rule succeeds.
Effect of casting cure baldness at a person:
	record outcome "but passes again without noticeable effect - these spells never work";
	rule succeeds.
Effect of casting cure baldness at a werespider:
	record outcome "- a patently foolish proceeding considering that these things are hairy enough already";
	rule succeeds.

Effect of casting mindsift at something:
	record outcome "leaving you with a brief, soothing amnesia before your life presses back into your mind";
	rule succeeds. 
Effect of casting mindsift at skeleton:
	record outcome "and, to your surprise, find there a faint residue of that last fading desire to live - but without breath, memory does not last, and nothing more reaches you";
	rule succeeds.
Effect of casting mindsift at goblin lord:
	record outcome "but there is nothing specific left, only a burning sensation of singular unpleasantness";
	rule succeeds.
Effect of casting mindsift at a wyvern:
	record outcome "and are treated to a rapid succession of simple but powerful sensations - flying, lazing in a hot sun, a scorching heat in your throat. No magic, though";
	rule succeeds.
Effect of casting mindsift at the succubi:
	record outcome "but the memories are your own - if with a bit more sensation than you can usually summon up by just thinking about it. You feel suddenly warm";
	rule succeeds.
Effect of casting mindsift at the copper-scaled snake: now the player knows corrode; record outcome "extracting its recollections, simplified by its reptile brain: anger, and a much-decayed memory of the ruination spell it cast here in revenge - which, however decayed, seems to be yours now";
	rule succeeds.
Effect of casting mindsift at a person:   
	repeat through Table of Enchantments:
		if the second noun knows the spell entry and the player does not know the spell entry:
			let the extracted spell be the spell entry;  
			now the player knows the extracted spell; 
			record outcome "and from the whirling mass of thoughts you clutch hold of something powerful and unfamiliar";
			rule succeeds;
	record outcome "but there is nothing magical you can extract from that sudden rush of thoughts"; 
	rule succeeds. 

Effect of casting corrode at something metal:
	record outcome "which now looks a little redder";
	rule succeeds.
Effect of casting corrode at a portable metal thing:
	remove the second noun from play;
	record outcome "which rusts through and vanishes";
	rule succeeds.
Effect of casting corrode at a weapon when the weapon is not metal:
	record outcome "but doesn't do much damage because the weapon is not mostly made of metal"; 
	rule succeeds.
Effect of casting corrode at a person:
	record outcome "causing [the second noun] to break out in freckles"; 
	rule succeeds.

Section W(b) - The Dry Channel

Sandy Area is a region. The Dry Channel, the Temple Plaza, and the Knotted Room are rooms. The Dry Channel, the Temple Plaza, and the Knotted Room are in the Sandy Area.

The Dry Channel is west of the Exalted Throne. The Dry Channel is below the Exalted Throne. "Heavy stones line the walls [if the layer is in location]of this channel, now only about half as deep as it once was. Underfoot are many feet of sand[otherwise]and floor of this channel, though most of the floor is also covered by a thin layer of very fine sand[end if]. Once the walls were plastered, and some hints of the original wall-paintings remain." The thick door is west of the Dry Channel. It is a door. "A thick sliding stone door [if the thick door is closed]closes off[otherwise]stands open at[end if] [if in the Channel]the west end of the channel[otherwise]the ceremonial procession-way[end if]." The thick door is trapped. Instead of opening the thick door for the first time: say "You grip the handles on the door and pull sideways. Sand flows quickly through the crack. You force it closed again before you can be overwhelmed." Instead of closing the open thick door, say "The sand-slide seems to have wedged it permanently open."

The wall-paintings are scenery in the Dry Channel. Understand "paintings" or "plaster" as the wall-paintings. The description is "Fluid strokes of blue and black suggest the shapes of leaping dolphins, an octopus, and a single swimmer." Effect of casting mend at the wall-paintings: record outcome "seeming to search about for more bits of the original plaster, but there is none to reassemble"; rule succeeds.

The gritty floor is scenery in the Channel. Understand "sand" or "fine" as the gritty floor. The description is "There is sand between the cracks and scattered across the surface of the stones, which have been worn to a fine polish." Instead of pushing or turning the gritty floor, try looking under the gritty floor. Instead of looking under the gritty floor, say "The sand is too thin to be concealing anything of interest." Instead of touching the gritty floor, say "You brush aside a little of the sand, but to no interesting effect."

The flimsy scroll is a scroll. "The edge of a half-buried scroll emerges from the sand." The inscribed spell of the flimsy scroll is cure baldness. Effect of casting mend at the flimsy scroll: record outcome "erasing the more recent handwriting and replacing it with older letter-forms..."; change the inscribed spell of the flimsy scroll to mindsift; rule succeeds. Understand "half-buried" as the flimsy scroll.

Instead of casting memorise at the flimsy scroll when the inscribed spell of the flimsy scroll is cure baldness for the first time: say "You glance over the cure baldness spell and find yourself distracted by the shadowy evidence of older writing, once underlying it and now scraped away. But with another try you could no doubt get past that impediment."

After opening the thick door:
	now the thick door is untrapped;
	remove the gritty floor from play;
	move the layer of sand to the Sandy Area;[* This makes the sand simultaneously present in all three of the rooms which make up the Sandy Area region.]
	say "You open the thick door. From beyond comes a flood of sand so swift and deep that it threatens to bury you, and it is still coming..."
	
The description of the thick door is "Sturdy enough to hold back a very considerable weight." Before going through the closed thick door, say "It's not the kind of door you just thoughtlessly open and slip through - this thing takes some effort to shove aside." instead. Instead of turning the thick door, say "It opens by sliding, not turning." Instead of pushing the thick door: try closing the thick door. Instead of pulling the thick door: try opening the thick door. Instead of listening to the thick door, say "It is entirely silent."

Understand "pick [a locked thing]" as a mistake ("You have no rogue skills at all."). 

The layer of sand is a backdrop. The layer can be flowing or still. The layer is flowing. The description is "The layer of sand would come up to your knees, perhaps further, if you were standing at the bottom of it." Instead of going through the thick door in the presence of the layer when the layer is flowing: say "You cannot possibly fight your way upstream." Instead of going to the Exalted Throne in the presence of the layer when the layer is flowing: say "You head away from the torrent of sand, but it is already a struggle to move fast enough with the sand underfoot, and you don't make it to the base of the ziggurat." Effect of casting mend at the layer of sand: record outcome "smoothing the surface of the sand to perfect flatness"; rule succeeds. Instead of searching the sand: say "You turn over the sand looking for more detritus from inside, but find nothing of interest."

After deciding the scope of the player when the player is in the Sanctorum:
	if the rainbow outlines are in the Dry Channel:
		if we have opened the thick door, place the sand in scope.

Every turn when the layer is flowing:  
	if opening the thick door, rule succeeds;
	if the player can see the sand:
		now the sand is still;
		move the flimsy scroll to the Dry Channel;
		if the player is in the Sanctum:
			say "The sand flows up and around the walls of your sanctuary, but you, inside, continue breathing easily. Eventually the flood subsides."; 
		otherwise:
			if the player is affected by aerial shield, say "Your aerial shield is no help, when your legs, waist and chest are being overwhelmed. ";
			say "The sand keeps pouring around you until your face is covered by fine suffocating powder. It does ";
			let the damage be the roll of 10d6;
			say " points of damage"; 
			make the player take the damage points of damage;
			if the player is killed, end the game saying "You have died like a moth in an hourglass".

Section W(c) - The Goddess and the Snake

The Temple Plaza is west of the thick door. "In the middle of the square stands a colossal statue to the goddess of Water. It was once a proof of the truly astonishing power of the inhabitants of Tolti-Aph: in keeping with the function of its divinity, it was made of clearest glass. But something corrupted it, and now it is disintegrating back into sand. Nonetheless, the walls are still decorated with dedications from her grateful adherents.

To the north is a building marked with the rune symbolizing esoteric knowledge."

The broken arm is scenery in the Temple Plaza. The description is "Vast and inelegant: it is hard to convey a slender wrist and tapering fingers when each fingernail is wider than your head."

The dedications are scenery in the Temple Plaza. The description of the dedications is "Various messages thanking the goddess for purifying wells, sending rain, bringing home cousins thought lost at sea, causing the spoilage of a rival's ale... The usual sort of thing." Understand "walls" as the dedications.

The library building is scenery in the Temple Plaza. Understand "rune" as the library building. The description is "It looks tantalizing." Instead of entering the library building, try going north.

The colossal statue is scenery in the Temple Plaza. Understand "goddess" as the statue. The description is "The head and shoulders are gone, and one giant arm has broken off to lie in the sand. Its surface, too, is rough with particles preparing to slough off." Instead of praying in the presence of the colossal statue, say "She plainly cannot help you now." Effect of casting mend at the colossal statue: record outcome "polishing up the surface a little, but unable to achieve a genuine reconstruction"; rule succeeds.

The copper-scaled snake is a reptile in the Temple Plaza. It is indifferent. "A snake with copper scales coils around one immense glass finger of the goddess." The description of the snake is "It looks faintly aggrieved about something: there is too much intelligence in its eyes for the average reptile." The copper-scaled snake knows corrode. It is metal. Instead of kissing the snake: say "It draws its head back, refusing to bite. If you seek to poison yourself, you will have to find some more direct way." Instead of taking the indifferent snake: move the snake to the player; say "The snake coils itself about your wrist like an expensive bangle." Instead of taking the hostile snake: say "It is not in the mood." Instead of attacking the snake when the snake is carried by the player: say "That would be a bit awkward while you are still carrying it."  

Instead of asking the snake about something, say "It keeps its mysterious counsel." Instead of telling the snake about something, say "It cocks its head to listen." Instead of answering the snake that something: say "It seems to nod." Instead of asking the snake to try doing something, say "It glares back at you: apparently it does not take orders."

After printing the name of the snake while taking inventory: say " (surprisingly docile)".

Section W(d) - Knots

The Knotted Room is north of the Temple Plaza. "High above you is a round opening to the room up there[if the rope ladder is not in the location]. Whatever stairs or ladder might once have reached it is now long-since eroded or removed[end if]."

The ropes are fixed in place in the Knotted Room. "Hanging from the ceiling are hundreds of threads - some thick as rope, some gossamer - and each is knotted at intervals. If you knew their code, you could read histories in these, and bloody legends. As it is, they brush over you as you pass, but leave you unenlightened." Understand "knots" or "knot" or "thread" or "threads" as the ropes. Instead of pulling or taking the ropes: say "They are all too firmly secured to the ceiling." Instead of touching or examining the ropes, say "You run your fingers down the nearest filament, and read the following: bump, bump-bump, bump-space-bump..." Instead of tying something to the ropes: try tying the second noun to the noun. Instead of tying the ropes to something: say "They are an awkward thickness to tie to things. Other than themselves, that is." Instead of tying ropes to ropes: say "You try fashioning a ladder of sorts, but it does not entirely work: your knotwork skills are lacking, and the strands are too different in thickness and composition."

Effect of casting web at the ropes:
	remove the ropes from play;
	move the rope ladder to the location;
	record outcome "joining one to another with the cross-bars of a spiderweb";
	rule succeeds.
	
Effect of casting mend at the rope ladder:
	remove the rope ladder from play;
	move the ropes to the location;
	record outcome "clearing away the detritus of spider-silk and restoring the threads to their original pristine glory";
	rule succeeds.
Effect of casting mend at the ropes:
	record outcome "causing the knots to shift very slightly on the nearest strand. It's impossible to guess the effect, but one can hope that you have undone some centuries-old bowdlerization of a venerable text";
	rule succeeds.
	
Effect of casting mindsift at ropes:
	record outcome "which reveals to you a brief history of a hero, a god with eagle-wings, and a woman disguised as a frog";
	rule succeeds.
	
The rope ladder is a fixed in place thing. "A curious sort of rope ladder has been made of webbing and the threads that hang from the ceiling here."  Instead of climbing the rope ladder, try going up.

Instead of going up in the Knotted Room in the presence of the ropes, try climbing the ropes. Instead of jumping in the presence of the ropes: say "The hole remains far out of your reach."

Instead of climbing the ropes:
	if the strength of the player < 3, say "You are too physically weak to attempt it." instead;
	decrease the strength of the player by 1;
	if a saving roll of 25 by the player to "climb the ropes" is made:
		move the player to the Dim Garret;
	otherwise:
		say "You don't make it. This is really more the sort of thing you leave to fighters.".

Section W(e) - The Garret and the Archivist

The hole is an open unopenable door. It is scenery. Understand "opening" as the hole. It is above the Knotted Room and below the Dim Garret. The printed name of Dim Garret is "[if archivist is lit and archivist is in location]Brilliant[otherwise]Dim[end if] Garret". 
	
Instead of searching the hole in the Garret:
	say "You look down into the room below, hoping your makeshift ladder will hold up for the return trip."

Instead of searching the hole in the Knotted Room:
	if a lit thing is in the Garret, say "White light and odd shadows are visible through the hole, but you can make out no more than that.";
	otherwise say "It looks dark up there."

The description of the Dim Garret is "A round room[if the archivist is in location and archivist is lit], dirty with age[otherwise], very badly lit[end if], with a hole in the floor giving onto the room below." The line of cubbyholes is fixed in place in the Dim Garret. "[if fixed in place]Empty cubbyholes, made of decaying wood, line the walls: these might once have held strands of varying thickness and colour[otherwise]On the ground is a sort of box made of rotten wood[end if]." It is a container. Understand "cubbyhole" or "holes" or "nook" or "niche" or "niches" or "nooks" or "box" or "decaying" or "lidless" as the cubbyholes. The line of cubbyholes is wood. 

Instead of attacking the fixed in place cubbyholes: now the line of cubbyholes is portable; change the printed name of the cubbyholes to "box of rotten wood"; say "You smash the cubbyholes, which come away from the wall in fragments, leaving you with a sort of detached, lidless box made of rotten wood." 

Instead of attacking the portable cubbyholes: say "Haven't you done enough?"

Instead of casting mend at the portable cubbyholes when the cubbyholes are not in the location:
	say "To mend properly, they'd have to be set down in the room, since they reassemble into furniture."

Effect of casting mend at the portable cubbyholes: 
	now the line of cubbyholes is fixed in place;
	change the printed name of the cubbyholes to "line of cubbyholes";
	record outcome "reassembling a rickety but functional container";
	rule succeeds;

The archivist is a priest in the Dim Garret. He is indifferent. The archivist can be active or passive. "Crouched in the center of the room, surrounded by filaments of various weights and colours, is an archivist[if lit]. He glows with magelight[end if]." The description is "He is very thin, as though he has been preserved by drying, and wears small round spectacles[if lit]. He also glows with a flickering white light[end if]."  

Instead of going to the Garret when the player knows magelight and the archivist is unlit for the first time:
	say "You hesitate - after all, the old lunatic has probably gone out like a snuffed lamp by now, and if you head up there you may be required to re-light him."

Instead of attacking the archivist: say "You swing at him, but make no contact. 'Ah,' he says, 'you have come far too late to kill me, I fear.'"

Instead of going to a room in the presence of the unlit archivist when the player knows magelight:
	say "The archivist stops you with a sign that ripples in the air[if the archivist is affected by silence]. Apparently you will not be allowed to depart until you have done your part[otherwise]. 'Not until you have given me light - such was our compact,' he says[end if]."
 
Every turn:
	repeat through the Table of Enchantments:
		if the duration timer entry < 1, now the archivist is not affected by the spell entry;	
	if the archivist is not affected by magelight, now the archivist is unlit.

Report casting in the presence of the archivist:
	say "As you intone the words of the [spell understood] spell, ";
	if the current spell focus is something:
		say "[the current spell focus] ";
		unless the player is holding the current spell focus, say "vanishes as it ";
		say "releases ";
	otherwise:
		say "your fingers release ";
	say emission of the spell understood;
	unless the second noun is a room, say " at [the second noun]";
	say ", [current spell outcome]";
	if the archivist is affected by the spell understood:
		if duration of the spell understood is greater than 0:
			say ", which will last for [duration of the spell understood in words] turn[s]";
			change the duration timer of the spell understood to the duration of the spell understood;
			increase the duration timer of the spell understood by 1;
	say ".";
	consider the current spell event;
	now the archivist is passive;
	stop the action. 

Effect of casting mindsift at the archivist:
	record outcome "but his eyes rise to meet your gaze. [if archivist is affected by silence]He smiles and shakes his head, and you know that you have just involuntarily given him a glimpse of your own history[otherwise]He smiles. 'I wouldn't,' he warns you, in the dialect of your own hometown[end if]";
	rule succeeds.
Effect of casting web at the archivist:
	record outcome "but the strands wrap your own arms, pinning you in place, and then dissolve again with a gesture from the priest";
	rule succeeds.
Effect of casting magic missile at the archivist:
	record outcome "but the fire turns and threatens to engulf you instead. Only at the last moment a motion from him diverts it";
	rule succeeds.
Effect of casting mend at the archivist:
	restore the health of the player;
	record outcome "but the spell reflects at you, restoring your strength instead. [if archivist is affected by silence]The archivist's mouth opens in laughter you cannot hear[otherwise]The archivist laughs, a bright and pleased chuckle. 'You have promise. Almost I wish I could still take an apprentice,' he remarks[end if]";
	rule succeeds.
Effect of casting silence at in the presence of the archivist:
	record outcome "settling around the archivist, who makes a comment you are unable to hear";
	now the archivist is affected by silence;
	rule succeeds.
Effect of casting aerial shield at in the presence of the archivist:
	now the archivist is affected by aerial shield;
	record outcome "creating a shield of protection above the archivist";
	rule succeeds. 
Effect of casting magelight at in the presence of the archivist:
	now the archivist is affected by magelight;
	now the archivist is lit;
	record outcome "settling on the archivist";
	rule succeeds. 
Effect of casting cure baldness at the archivist:
	record outcome "but the spell rebounds on you. [if archivist is affected by silence]The archivist smirks silently[otherwise]'If I may venture an opinion informed by fashion many centuries past - the curls are unbecoming,' the archivist remarks[end if]";
	rule succeeds.	 

Instead of casting memorise at in the presence of the archivist:
	say "The archivist holds up a hand[if the archivist is affected by silence]. He points at his own forehead warningly[otherwise]. 'Don't, unless you wish me to be the one to learn the spell,' he warns[end if]."

Instead of giving something to the archivist:
	say "You hold out [the noun], but when the archivist reaches out his fingers pass through yours without even the chill that comes of touching a ghost."
	
Instead of showing a display listed in the Table of Archivist Reactions to the archivist:
	if the archivist is affected by silence, say "He shrugs, pointing to his silenced mouth.";
	say "[reaction entry][paragraph break]".
Instead of showing or giving the much-knotted string to the archivist:
	remove the much-knotted string from play;
	now the player knows magelight;
	say "The archivist leans close, murmuring to himself as he interprets the spell and commits it to memory - to your memory, that is."
	
The much-knotted string is a thing. The description is "At a guess, it is the equivalent of a spell scroll, holding some incantation of light."
	
Table of Archivist Reactions
display		reaction
stretched scroll	"He reads it over through the spectacles. 'Not entirely purposeless, I think,' he remarks."
blank parchment	"'I am surprised no one has yet written on such a promising surface,' he says."
cursive diary	"'Ah, pity,' he says. 'She had courage, but courage alone does not always suffice.'"

Every turn:  
	if the player can see the archivist and the archivist is active:
		if the archivist is affected by silence:
			say "The archivist keeps working, but you cannot hear a move he makes.";
		otherwise:
			if the archivist is lit:
				say "The archivist works swiftly over his threads, comparing colours.";
			otherwise:
				repeat through Table of Archivist Chatter:
					say "[reaction entry][paragraph break]";
					blank out the whole row;
					stop the action;
				say "The archivist's fingers move briskly over the strands, knotting here and there.";
	otherwise:
		now the archivist is active.
	

Instead of asking the archivist about a topic listed in the Table of Archivist Chatter: 
	if the archivist is affected by silence, say "He points at his silenced throat." instead;
	now the archivist is passive;
	say "[reaction entry][paragraph break]";
	blank out the whole row.

Instead of asking the archivist about something, say "'What? what? Oh, you talk nothing but nonsense,' says the archivist."

Instead of answering the archivist that something: try asking the archivist about it. Instead of telling the Archivist about something: try asking the archivist about it.
 
Table of Archivist Chatter
topic	reaction
"hello/hi/hey"	"'You again,' remarks the archivist, without looking at you. 'Or is this your first time? No matter.'"
"time/age/temporal/reflection"	"'The time spell takes all my strength,' he says. 'I maintain it, it maintains me, nothing left over.'"
"time spell/spell/time/scroll/temporal/age/infinity/infinite/eternity"	"The archivist holds up a peculiar scroll, twisted and stitched top to bottom so that it has no end or beginning, but as you reach out to touch it your fingers slip through. He tucks the scroll back inside his clothing. [paragraph break]'Not for you,' he says. 'It allows doing things and undoing them, going forward, going back... Such spells are not intended for us to discover, I think.'[paragraph break]Then he frowns, seeming to regret this revelation. There is a sticky sort of POP in the air around your head, and you forget -"
"task/job/knots/knot/work/threads/thread/rope/ropes/archive/archives"	"'You know,' says the archivist, glancing at you over the top of his spectacles. 'This task would be easier with a bit of light. The threads have colours as well as knots.'"
"meaning/purpose/subject/thread/threads/rope/ropes/knots/knot/archive/archives"	"'All day and night counting and comparing,' he mutters, perhaps to you or perhaps to himself. 'Comparing and counting. Oh, sometimes I go out and practice a little archery, I suppose. D'you know, a good high arrow from here can even reach the old Longwall turret? But counting! That's the thing.'"
"light/lights/light spell"	"'I'd perform the light spell myself, but I haven't the strength, nor the wood either.'"
"light/lights/light spell"	"'You could do it, of course,' he says. 'Learn the light spell, cast it for me. You could keep it afterwards, if you wanted. In your head. A payment in your mind, what think you?' He tilts his head this way and that, like a bird. 'Yes?'"
"light/lights/light spell/payment/learning/memorizing/memorising/casting"	"He selects a new strand and begins knotting it carefully, counting, muttering. You cannot hear the words. [string placement] " 
"darkness/dark"	"[if archivist is lit]He smiles. [otherwise]He works, squinting down at the strands and cursing the darkness. [end if]"
"help/assistance"	"[if archivist is lit]'Don't think I'm not grateful,' he says. 'But I must work now while the spell holds.' [otherwise]'You could help if you wanted to,' he says, 'but if not, I wish you'd go away.' He follows this with a sharp glance. [end if]"
"plaster/water/goddess"	"[if archivist is lit]'I do not know whether the goddess yet lives,' he says. 'Perhaps she has only moved away.' [otherwise]The archivist squints over his thread, muttering: 'Water as a liquid? or the goddess herself? Difficult to say without the colours... Would you say this was blue? or green? No, you won't be able to tell any better than I.' [end if]"
"plaster/water/goddess/blue/green"	"[if archivist is lit]'Grey-green is her colour, if you want to find the goddess in the strands.' But of course you wouldn't be able to make any sense of them anyway. [otherwise]'They should have put a lamp in here with me,' says the archivist. 'I told them - bad arrangement - temple bureaucrats were never very wise.' [end if]"
"smart/arrangement/bureaucrats/temple"	"'This was a bustling place once,' he says. 'You wouldn't believe... they're all gone now, of course, of course. Demanding tribute is a dangerous business to get into.'"
"tribute"	"'Folk get testy when their wells run dry,' he comments, counting. 'Apt to place blame - four, five, that's a drought for certain, and not just a dry spell.' He holds up the offending strand for you to see, but you make no more of its five-knot-row than he just told you."
"snake/snakes/copper-scaled snake"	"'Shrinks year by year as its curse works out,' he remarks, to the air. 'It won't be much more than a grub when the spell ends. I could look - but I don't like to go that far ahead.'"
 
To say string placement:
	if cubbyholes are fixed in place and cubbyholes are in location:
		say "Then he rises and tucks the strand into one of the cubbyholes; and the moment his hand leaves it, it grows old and dry and brittle as though it has been lying there for centuries. ";
		move much-knotted string to cubbyholes;
	otherwise:
		say "Then he rises and lays the string a few feet from himself; and when his hand leaves it, it grows old and dry and brittle as though it has been lying there for centuries. ";
		move much-knotted string to location.

Chapter N - Northern Quarter

Section N(a) - That Was Old School

After going north from the Exalted Throne: say "You climb down the steps to the far side of Tolti-Aph, which is still more tumbledown than the route in."; continue the action.

Old School Yard is north of the Exalted Throne. "This was never a place of education, except it was. Today an overgrown spoil-heap of fallen columns, maybe, but you are pretty sure it is the same yard engraved in the frontispiece of Reliques of Tolti-Aph.

Of the 'Old School' inn, notorious haunt of liars and braggarts, time has swept everything away except a few stone columns[if the jumble is in Old School]: and even those it has tumbled into an unruly clump, like the corn-stook of a drunken harvester[otherwise], which lie segmented all about you[end if]."

Up from the Old School Yard is the Exalted Throne. Instead of going nowhere from Old School Yard, say "It's sad how little remains of this once-thriving northern quarter of the city: little more than what you see."

The wooden trap-door is a door. "[if the jumble is in Old School]What must be an old wooden trap-door, perhaps for wine-barrels, is partially buried by all that masonry[otherwise]The cellar trap-door is now free of the rubble[end if]." The wooden trap-door is below the Old School. The other side of the trap-door is the Archive Chamber. The trap-door is openable and closed. Instead of opening the trap-door when the jumble is in Old School, say "Even the idea of lifting the weight of those columns is tiring." After going through the trap-door: say "You lose your footing, and half jump, half fall down into the cellar."; continue the action. The trap-door is trapped. [Only for the pun, though.] Understand "trap" and "trapdoor" and "door" as the trap-door. The trap-door is wood.

The jumble of school columns are fixed in place scenery in Old School. Understand "column" and "masonry" as the jumble. Instead of casting magic missile at the jumble, say "Extreme violence isn't the answer to this one." Understand the command "pry" as "open". Effect of casting mend at the jumble: record outcome "which - perhaps with an air of smugness - resists all attempt at straightening out"; rule succeeds.

Pushing or pulling the jumble is a Samson act. Instead of a Samson act for the first time, say "It all looks mighty precarious, with the last of the ceiling-work liable to fall, but if you're sure this is what you want..." Instead of a Samson act: say "There is a vast, booming series of reports, the inconsequential sound of these jackstraws of stone smashing down against each other: and then the last remaining slab of ceiling-brace falls. "; unless the player is affected by aerial shield begin; say "We could go through a charade of working out [if the player is affected by ironbones]210d8+50[otherwise]420d8+100[end if] damage, but let's simply wind this up in a dignified fashion."; end the game saying "You have been buried"; stop; end unless; say "Probably never has 'aerial shield' been put to so formidable a challenge, but the enchantment holds, and as the rubble settles all about the yard you half-collapse, trying to catch your breath.

That was Old School."; remove the jumble from play; award 14 points.

Section N(b) - Rescued!

The Archive Chamber is a room. "A cellar, but hardly the cellar of an inn: that must have been a subterfuge. Here were stored the knowledge of spells of particular power or importance, those the possessors did not want to entrust to mere scrolls. Once coffins lined both sides of this room, but most are broken and empty, the inhabitants turned to dust: in some cases quite recently, since they broke your fall. 

The single remaining coffin, sealed up by magical, half-transparent wood, contains a motionless figure."

The broken coffins are scenery in the Archive Chamber. The description is "Their seals are broken and their contents dust." Effect of casting mend at the broken coffins: record outcome "but cannot restore the many layers of ancient magic that sustained these intact"; rule succeeds. The broken coffins are wood.

The pair of infravision spectacles is in the Archive Chamber. "Dislodged by your fall, perhaps, a pair of infravision spectacles lies on the stone floor, the skull that once wore them being now only powder." The description of the spectacles is "You see nothing special about the infravision spectacles. No, really, you never did see anything special in them. Okay, so darker-leaning magic users liked to wear them around town in an I'm-too-cool-for-sunlight pose, but really." The spectacles are wearable. After looking when in darkness and the player is wearing the infravision spectacles, say "It's actually a myth that infravision spectacles enable you to see in the dark. They actually allow you to see light re-phasing around magical surfaces." The infravision spectacles are metal.

The single remaining coffin is transparent closed scenery in the Archive Chamber. It contains a motionless figure. The figure is a woman. The figure knows summon elemental. The description of the motionless figure is "Her face is hatchet-like, her brows straight in a way you have seen only in statues. But she breathes. Very slowly, but she breathes." Instead of opening the coffin, say "It is sealed with a powerful magic, protecting the life within." Instead of entering the coffin, say "Standing on the coffin would take you no nearer the ceiling, in real terms, and as for entering it, the thing is shut tight: quite apart from any considerations of human decency." The single remaining coffin is wood.

Instead of going up in the Archive Chamber, say "There is no apparent way of climbing up to the square hole in the vaulted ceiling of this cellar." Instead of going nowhere from the Archive Chamber, say "You explore each corner of the cellar, hoping to find a way out. But it is very well secured against infestation, ground-water, thieves and the like."

In the Archive Chamber is scenery thing called the square hole in the vaulted ceiling. The description of the square hole is "Through the square hole in the ceiling, you are tantalised by the fine afternoon sunshine: that etherial, simple perfection which is somehow appreciated only by those who see it from a prison cell." Understand "door" and "trap" and "trap-door" and "trapdoor" as the square hole. The square hole is air.

The Prison Visit is a scene. The Prison Visit begins when the player is in the Archive Chamber for the ninth turn. The Prison Visit ends when the player is not in the Archive Chamber.

Understand "tp [any room]" as teleporting. teleporting is an action out of world applying to one thing. Carry out teleporting: move player to noun.

Understand the commands "yell" and "holler" as "answer". Answering is making a rumpus. Singing is making a rumpus. Asking someone for something is making a rumpus. Instead of making a rumpus during the Prison Visit, say "No matter how loudly you call out, the sound seems to be deadened by the funereal walls of this burial chamber."

Every turn during the Prison Visit:
	say "[one of]Just as you are thinking that being imprisoned down here is no life for a young magic-user with so much to give (i.e., so much to take), you think you might hear a faint noise from above.[or]No, it must have been nothing.[or]That second faint noise must have been nothing too.[or]Possibly all that masonry is re-settling itself.[or]You begin to conjure up a mental picture of some ideal rescuer, padding about through the rubble up above, and about to stumble upon you.[or]A rescuer. Absolutely. Let's see. Nice smile, healthy outdoor tan perhaps, hint of sparkling excitement in the eyes, forty feet of rope and a grappling hook coiled over one shoulder. A bit in awe of magic-users, for preference. Yes, that would do very nicely about now.[or]Well, the noises bump on. Sounds almost like pacing, in fact.[or]Is it your imagination? Or could that be the sound of breathing?[or]Come to think of it, the thought of this imminent meeting makes you a little nervous. You fuss with your hair, look down at your fingernails, wince.[or]Any minute now. It'll be like one of those romantic-adventure poems. You'll trek off into the wilderness together.[or]That is, you'll trek off together if anything ever happens.[or]Oh, when is that divine smile going to appear at the hole in the ceiling?[or]Ah, such a metaphor for life, this.[or]Tum te tum. Tum tum te-tum.[or][run paragraph on][stopping]". [This prints each clause exactly once, advancing to the next on each subsequent turn, until it reaches the last clause, which it will continue to print every turn as long as Prison Visit is still happening. We say "run paragraph on" in the last slot because the natural tendency for a say statement is to print a blank link, which in this case we don't want, since nothing has been said.]

Effect of casting web at the square hole:
	if Prison Visit is happening, record outcome "but although a plausible spider's-web does drape itself across the hole for a while, it obviously wasn't spectacular enough to get the attention of the mysterious person above";
	otherwise record outcome "but although a plausible spider's-web does drape itself across the hole for a while, it doesn't last, and doesn't by any means hang a useful thread down to your level";
	rule succeeds.

Effect of casting magic missile at the square hole:
	if Prison Visit is happening:
		record outcome "and a spectacular firework of a missile is fired into the afternoon air. It must get the attention of anyone around, surely? And your heart surges as one end of a forty-foot coil of rope is thrown down, and a vague shape from above gives a muffled yell, and you step onto the grappling hook to be pulled up, up, above the old coffins and over the lip of the trap-doorway to...

Oh, well, isn't that just [italic type]exactly[roman type] like life? You leap backwards. So does your benefactor. The trap-door slams angrily shut, swallowing the rope as if it were so much spaghetti";
		record event the rescued from the archive rule;
	otherwise:
		record outcome "and a spectacular firework of a missile is fired into the afternoon air. It must get the attention of anyone around, surely? But then, in a ruined city, who's to see it";
	rule succeeds.

Instead of listening during Prison Visit, say "The trouble is, the harder you concentrate on listening, the more you hear your own heartbeat and breathing."

Glog the Troll is a cave troll.[* Glog probably has romantic feelings too, and is probably just as annoyed by this outcome as the player is. Life is so terribly cruel.]

This is the rescued from the archive rule:
	now the trap-door is closed;
	now the trap-door is locked;
	move Glog to the Old School;
	now Glog is surprised;
	move the player to the Old School.

Section N(c) - Rescuing!

Mutatis Mutandis is a room. "Here in the ashen grate of the world, you find what must, hundreds of years ago, have been the Sanctuary of another magic-user. The spell must long have decayed, after the death of its caster, or you would not have been able to get in. Through the faded, blackened walls, you can vaguely see the Robing Rolls, but it is as unreal as a nightmare." Before going outside from Mutatis Mutandis, say "You feel a surge of vitality."

A pungently red scroll is a scroll in Mutatis Mutandis. The inscribed spell of the pungently red scroll is ironbones.

Instead of sleeping in Mutatis Mutandis for the first time:
	let the depletion be the strength of the player;
	let the depletion be the depletion divided by 2;
	unless the depletion is 0, decrease the strength of the player by the depletion;
	remove the skeleton from play;
	award 150 points;
	say "Your sleep is uneasy. A strange parade of memories of another life, long, long ago. You become her, graduating as a magic-user, celebrating in the echoey cloisters of the Collegium, buying provisions in the market, setting out with a half-dozen friends into the wilderness, fighting off the beasts of the forest - as it goes on, and on, you sink deeper into her identity and your own strength is sapped. By the time her story ends, running out of strength in the middle of the old lane, much of your own power has seeped across to her, those many years ago.

You sit bolt upright, your pose suddenly an echo of her skeleton's. You are awake. And yet you have a tenuous memory, or vision, of her rising, walking away, leaving the city - so many hundreds of years ago. This makes no sense, and yet you feel fulfilled."

The blackened outlines of an old sanctuary are fixed in place. "Through the infravision lenses, you see the blackened outlines of an old sanctuary spell." Instead of entering the blackened outlines: move the player to Mutatis Mutandis. Understand "black" as the blackened outlines.

Every turn:
	if the player is wearing the infravision spectacles:
		move the blackened outlines to the Robing Room;
		change the outside exit of Mutatis Mutandis to the Robing Room;
	otherwise:
		remove the blackened outlines from play.


Chapter the Last - Not for release

When play begins, seed the random-number generator with 1234. [* A device used in testing. The trouble with heavily random games is that when faults are reported, it is sometimes all but impossible to recreate the circumstances, because the right combination of random chances never comes up. This option fixes that.]

Understand "/ [text]" as commenting on the transcript.[* A notation for play-testers to use. I prefer to be sent transcripts of play rather than bug reports, and this allows testers to type random remarks in as commands, so that they appear in the transcript.] Commenting on the transcript is an action out of world applying to a topic. Carry out commenting on the transcript: say "(Noted.)"

Understand "pilfer [spell]" as pilfering. Pilfering is an action applying to one spell. Carry out pilfering: now the player knows the spell understood; say "OK."

Understand "swiggle" as preparing to test. Preparing to test is an action applying to nothing. Carry out preparing to test: now the player knows magic missile; now the player knows make sanctuary; now the player knows mend; move the pack to the player; now the pack is untrapped; move the old pipe to the player; now the player carries the arrow; now the player knows web; now the player knows silence; now the player knows corrode; now the player knows aerial shield; move player to Throne; increase score by 250.

Understand "swoggle" as scanning for materials. Scanning for materials is an action applying to nothing. Carry out scanning for materials: repeat with the lump running through portable things begin; say "[The lump] - [material of the lump]."; end repeat.

 Understand "play [any hazard]" as hazard-playing.[* For testing the Earthquake and Trap hazards in the Maze, which took quite a lot of getting right.]
