"Paper Route" by Paul Broyles.
The release number is 3. [Updated from version 2 to compile under 6M62 by altering the "printing" command, which was in conflict with part of the Standard Rules, to "laserprinting".]

[This is a comment. It's there only for people reading the source code; no one actually playing the game will see it. I'll use comments like this to explain what things in the code do. Square brackets OUTSIDE quotation marks will always contain comments, though square brackets INSIDE quotation marks have a special meaning you'll see below.]

Include Rideable Vehicles by Graham Nelson. [This statement includes an extension--an outside tool that allows Inform to do things it couldn't normally do. Inform comes with a number of extensions pre-installed, and more can be downloaded. Extensions must be included at the beginning of the file.]

Release along with an interpreter. [This instruction means that, when we press the RELEASE button in Inform, it will output files that make the game playable on a website.]
Release along with the source text. [This instruction means that the website associated with the game will allow readers to look at the full source code.]

[Inform uses a metaphor derived from books to organize its source code--a metaphor intended to emphasize the idea of authorship over programming. There are no set rules about what books or chapters should be included, or what order they should come in; you may organize your text however you want. I've chosen to give a separate book to each major component of the world. But you could equally build your structure around locations (as I do at the Chapter level of Book 2) or around the plot. You could even avoid subdivisions altogether, which would be fine in a small game but would make it challenging to read a large one. The point is to choose a structure that makes sense and allows you to organize your writing so you can find and edit things easily. In practice, you have some flexibility: I didn't finalize my structure until after I was done writing the game.]

Book 1 - Objects

[I strongly advise that you skip this section and come back to it after you've read Book 2. This Book has to come first because of the New Types listed below. In that section, we're telling Inform about some new kinds of object that it doesn't know about. We have to tell it about those types before we create any objects belonging to them, which we'll do in describing the bedroom. But the techniques we're using here are more advanced than some of what follows, and this section will probably be more intelligible after you're familiar with the Settings.]

Chapter 1 - The Paper

The paper is a thing. The description is "It's hard to know after such a late night if it's any good, but it exists, and that's what matters. [if the paper is stapled]The pages are neatly stapled together; it looks beautiful and ready to turn in.[otherwise]The loose pages are stacked haphazardly.[end if]". The paper is nowhere. The paper can be queued. The paper can be printed. The paper can be found. The paper can be stapled. The paper can be late. [Some of the actions we'll introduce in Book 2 require the existence of the paper. From the perspective of the player, the paper doesn't exist until it's printed; she creates it by printing it. But it's not possible to create a new object in the middle of an Inform game (except using complicated techniques). Instead, we tell Inform that the paper exists, but that it's nowhere; you already saw above how we move it to the printer when we're ready for it. The designers of the language compare this to theatre: the paper is a prop kept off-stage until called for by a scene. 
	The "can be" statements tell Inform that the paper has certain properties. None of these are true when the game begins, but in the course of play, we give the paper each of these properties.]
Understand "pages/papers" as the paper. [This allows the player to type reasonable synonyms for the paper.]

Chapter 2 - Object Types

Section 1 - New Types

[In this section, we define several new kinds of objects. We create specific objects of each kind later in the game, and they automatically have all the characteristics we assign here.]

A window is a kind of thing. A window is always scenery. A window is always fixed in place. A window can be open or closed. A window is usually closed. A window can be openable or unopenable. A window is usually openable. [This definition is borrowed from Â§11.17 of the Inform documentation. The authors of the documentation invite you to borrow examples without credit, but for our class, you should note any borrowings in comments; a citation like the previous sentence is fine.]

A bed is a kind of container. A bed is always enterable. A bed is always fixed in place. [Later, we make two beds, but Inform doesn't feature a built-in category of beds. It's useful to have this vocabulary because both beds which share the property of being ENTERABLE, so it would be nice to have a faster way of doing it. We can do so using the IS A KIND OF instruction, which allows us to create our own kinds. Now we can create new beds using IS A BED. The ENTERABLE property simply means a person can get into it. You can make anything enterable, so you have to consider what makes sense; you wouldn't want to allow the player to enter the ID card, for instance. Note that I've made the bed a supporter because you might want to sit on it, or put something on it, as well as getting into it; however, in this game, nothing special will happen when you do.]

A swiper is a kind of thing. Swipers are never portable. [Some doors and objects are controlled by using your swipe card. We need some way to control them: here, we create a kind of object called a swiper, and give it the swipable property. Note that "swipable" has no meaning whatsoever to Inform, and we could have used any word we wanted. By creating this property, we can now write rules about swipe boxes. "Portable" is a property we've already met; since all swipers are fixed in place, we specify this.]

Does the player mean swiping the ID card in a swiper: it is likely. [This offers the game some clarification. If the player issues a vague command involving swiping, it may be unclear what the player intends to swipe with, or what she intends to swipe it in. This line tells the game that unless there's a good reason to believe differently, the player is probably talking about swiping the ID card in something of the kind SWIPER. So just typing a command like "swipe card" in the hallway will trigger the hallway swipe box or in the computer lab will trigger the printer pay station, exactly as the player would probably expect.]

Section 2 - Object Properties

[Here, we assign new properties to existing kinds of objects, which lets us tweak the behavior of every object of a type.]

A door is usually scenery. [It's very awkward to have doors habitually show up in room descriptions. Designating doors as scenery prevents this; the word USUALLY allows us to exempt individual doors, as we do in the office building. (If I didn't want to exempt any doors, I would say ALL instead.]
A door can be knocked on. A door is usually not knocked on. [Here we give doors an optional property, allowing us to record and look up whether any given door has been knocked on. Note that we must set this property ourselves; it doesn't happen automatically.]

A person can be beaten. A person is usually not beaten.

A person has a number called annoyance. Annoyance is usually 0. [This allows us to quantify something about an object--in this case a person. We give every person a numeric annoyance level, which we can then manipulate.]
Definition: A person is not-annoyed if his annoyance is 0.[This line and the next ones let us translate that annoyance level into words by defining custom adjectives. We can then make decisions based on whether a person is somewhat-annoyed or not annoyed.]
Definition: A person is somewhat-annoyed if his annoyance is 1 or more.
Definition: A person is very-annoyed if his annoyance is 3 or more.

Book 2 - Settings

Part 1 - Your Dorm

Chapter 1 - Your Dorm Room

Your dorm room is a room. "[if the player is in your bed]The [ceiling] of your dorm room stares down at you, as it does most mornings. Out of the corners of your eyes, you make out a sea of chaos around you. The plain, unmarked ceiling is reassuring by comparison.[else]Your room is a mess--hardly surprising, given the all-nighter you pulled. [Books] and [papers] are strewn everywhere. To the north, [your room door] leads to the main hallway.[end if][run paragraph on]" [The first sentence creates a new location and gives it the name "your dorm room." Note that it's mandatory to tell Inform that this is a room; it can't infer it from the name.
		The passage in quotation marks is called an INITIAL DESCRIPTION; it will be displayed the first time the interactor visits the location, and subsequently whenever the interactor LOOKS.
		In the description I place certain words in square brackets. This is a special way of referring to objects inside descriptions that prevents them from being listed separately. Note that we have to create these objects later or Inform will display an error message.
		Finally, note that this description doesn't change unless we specifically change it. That's fine in this game because the interactor isn't allowed to clean up or to remove any of the items we mention, but if the condition of the room could change, we'd have to allow for that.]
		
Section 1 - Simple Objects

The desk is in your dorm room. "In a corner by [your dorm window], your desk is the epicenter of the chaos." It is not portable. The description is "The sturdy wooden desk is barely visible under the [mess], but at least you can still see your [computer]." [A SUPPORTER is a special kind of object that you can put other things on top of. We wouldn't be able to put anything on the desk if we didn't define it as a supporter. It's important to specify that it's NOT PORTABLE, because otherwise the interactor would be able to pick up the desk and carry it around--probably not something you're up to after an all-nighter! Note that the initial description I assign here does not actually appear; a rule in Inform I haven't yet managed to track down apparently suppresses the initial description of a supporter when we're describing something on it.]

The ID card is an object. It is on the desk. "You can see your ID card half sticking out under the piles of stuff on your desk." The description is "Your student ID card shows the world who you are, and how bad you looked in pictures as a first-year. You can swipe it to access restricted rooms or to pay for things." [You can think of objects as being just like what we think of as objects, though in reality they have a big and complicated defintion. We could also just have said "The ID card is on the desk" and Inform would have assumed we were talking about an object, but it's a good habit to be explicit. We've given the card a description, which an interactor can see by EXAMINING it. Interactors generally expect to be able to examine most things in the game, so get in the habit of writing descriptions. Since an interactor might not realize that the card can be swiped, we use the description to signal this--a good idea, unless we wanted that to be a puzzle. We'll tell the game what happens when you swipe in another section.]

Your dorm window is a window in your dorm room. It is closed. The description is "Outside the window, the sky is alarmingly bright, and you can see people lounging about on the grass. Boy, are you late![if the window is open] A cool breeze drifts in, helping you wake up a little. The sounds of fun tantalize you, but right now, you have more important things to do." [I bet you can figure out what this does! Notice that if statements do not require an else; here, we add to the description if the window is open, but do nothing if it's closed."]
After opening the window, say "You push the window open, enjoying the cool rush of air in your face. From outside, you can hear sounds of laughter: someone, somewhere, is having a good time. Unlike you." [Again, you should be able to figure this out. While INSTEAD took the place of an action, AFTER lets the action complete, then does something else. In this case, we want to provide a message after the action is complete describing what happens. Note that messages like this usually replace the default messages built into Inform, though the rules about that are complicated.]
After closing the window, say "You push the window shut and the sounds die away."

Your bed is a bed in your dorm room. The description is "[if the player is in your bed]It cradles you comfortably. Soft blankets gently cocoon you, while the pillow caresses your... NO! You're not going back to sleep![else]The bed's sheets and blankets lie rumpled and disarrayed where you flung them when you got up, and you know what? It still looks inviting.[end if]".
Instead of entering your bed, say "If you get back in that bed, you will fall asleep, and you won't turn your paper in, and you will fail, and your whole life will be ruined! You don't really want that, do you?" [The wording of the INSTEAD statement may seem strange, but Inform understands actions that lead to your being inside something as "entering": an interactor who types "get in bed" will receive this response.]

Your roommate's bed is a bed in your dorm room. The description is "Isn't it odd how one's own bed just seems different from anyone else's? It's like that: as tired as you are, your roommate's bed just doesn't look as comfortable. Also, it's currently quite full of [your roommate]." [As above, note that we can get away with this description only because your roommate will never leave the bed in the course of the game; otherwise we would have to change the description.]
Instead of entering your roommate's bed, say "You've been roommates for a while, but you're not THAT close."
[**If you're paying attention, you might have noticed that right now there's no way to interact with your roommate. Even though your roommate is in your room, characters are a little different from scenery, so I've decided to deal with them in their own section.]

Does the player mean doing something to the your bed: it is likely. [This statement helps us control how the game reacts to commands. There are two beds in the room: the one called "your bed," and another to be introduced shortly called "your roommate's bed." Without this sentence, every time the interactor tries to do something to a bed, Inform will ask which one--and because of quirks, it's hard to tell the parser that you mean your own bed. This sentence tells the parser that it's relatively likely for the interactor to try doing something with her own bed, i.e. a random command applied to a bed should probably be applied to "your bed." The interactor can still do things to the roommate's bed by specifying that bed.]

Section 2 - Scenery

Some books are scenery in your dorm room. The description is "Piles of books lie where you discarded them as you finished. Some are still open." Instead of doing anything except examining to the books, say "You're finished with those; there's certainly no time to add anything else to your paper now." [This should all look familiar. The only new thing here is that we've defined the books as SCENERY. Scenery is a special kind of object. If other objects are like props in a play, scenery is like the set. It cannot ever be taken, and it isn't included in the room descriptions Inform generates automatically, though we can manually include it in our room descriptions, as we did above. It's a good idea to use scenery for things that you need to describe but that aren't important and can't be interacted with. In this case, interactors who read our room description might think they need to do something with the books. By including them as scenery, we're able to describe them so they can be EXAMINED, and to let the interactor know they aren't actually important.]

The ceiling is scenery in your dorm room. The description is "The ceiling is plain, unmarked, and utterly uninteresting, yet you're somehow capable of staring at it for hours at a time in the morning."

Some papers are scenery in your room. The description is "Bits of paper are strewn around the room. Some contain your notes, others portions of drafts. Some have bizarre sketches you can't remember doodling. Ah, 3 A.M." Instead of doing anything except examining to the papers, say "You fiddle with the papers, but you're pretty sure a full copy of your paper is not among them."

A mess is scenery in your dorm room. The description is "You can barely stand to look at this mess." Instead of doing anything except examining to the mess, say "You just can't face it right now."

Section 3 - The Computer

The computer is a device on the desk. "Your computer sits in the middle of the [desk], a blank spot at the eye of the storm." It is switched off. [These sentences do a lot of work. A DEVICE is a special kind of object that can be turned on or off. We explicitly say that it's switched off to set its condition at the start of play. Because we don't have to move the computer around, we don't have to say explicitly that it's on the desk, but this would be important if we wanted to be able to put it in different places.]
The description of the computer is "It's a slightly battered laptop that you use to do almost all your work. [if the computer is switched off]It's off right now. How come the computer gets to sleep?[else if the computer is switched on]The screen glows with a familiar warmth.[else]It is SchrÃ¶dinger's computer, neither off nor on, yet somehow both. Are you a physics major?[end if][if the paper is found and the paper is not queued and the computer is switched on] Your paper is displayed on the screen. Perhaps you should try [bold type]printing the paper[roman type]?[end if]". [The state of the computer will change once the interactor turns it on, so we want to be able to describe it differently. That's what the bracketed IF instructions do. Note that we could just have done "if the computer is turned on / else", as any computer that is not on is off; I included the third description only to show you how you could use the "if" instruction when there are more than two conditions.]
Understand "laptop" as the computer. [In our description, we described the computer as a laptop, so it would be perfectly reasonable for an interactor to refer to it in that way. This instruction tells our program that any mentions of a laptop refer to the computer.]
Instead of taking the computer, say "You're pretty sure your professor wouldn't just accept it if you handed in your whole computer; you'll need a copy of your essay on paper." [We could just have said the computer isn't portable, but this might have puzzled an interactor who could very reasonably expect to carry it around. Besides, maybe we'd want the interactor to be able to take it later in the game. The INSTEAD instruction allows us to interrupt Inform's default behavior and replace it with something else; in this case, we display a message.]
Instead of doing anything except examining or switching on to the computer when the computer is switched off, say "You poke grumpily at some keys, but nothing happens. Oh, it's still turned off. No wonder."
Instead of using the computer, say "You know, there are so many ways to use a computer! You could check Facebook, or play a game, or... No. What you need right now is your paper. You know it's in there somewhere; you just aren't sure where."
Instead of searching the computer:
	say "Your fingers fly across the keys as exciting music swells in the background. You found the paper! You are a hacker genius! Or at least you know how to work the search box. Now, to get it out of the computer...";
	now the paper is found.
	
Section 4 - The Door

A door called your room door is north of your room. It is lockable and unlocked. The description is "What is there to say, really? It's a door. It lets you in and out of your dorm room. Right now it is [if your room door is open]open[else]closed[end if]." [Doors are a special concept in Inform, and to explain this line, I have to digress for a minute. By longstanding convention, the locations in interactive fiction are laid out according to compass directions: north, east, south, and west, as well as northeast, northwest, southeast, and southwest. (Don't think of these as literal compass directions; they just describe the position of places relative to each other.) We connect rooms by specifying their direction from each other, and interactors move among them by going in a specific direction. Doors are a special kind of object that connects rooms. By default, they are openable and closable; you can also make them lockable (which we will see shortly). To create a door, you specify what direction it's in, just as if you were making a new room.]
Before locking your room door, say "There's no time to bother with locking your door now! Also, you have no idea where under your piles of stuff you might find your keys." [Every other time we've wanted to avoid an action, we've used INSTEAD. Why BEFORE here? When the interactor instructs the parser to lock the door, the first thing the parser is going to do is check to see whether the person has a key matching the door. The answer to that is no, because there is no such key in the game. So, the parser will not get as far as INSTEAD, because first it will give a message that you don't have the key. We want the reader to get a more informative response, so we return this response before the game checks on those things. By default, BEFORE, like INSTEAD, prevents the action from occurring.]

[That will about do it for the dorm room. We've written a lot of descriptions and rules, because we want to be prepared for things an interactor might try. But notice that there's lots we haven't written about. We could have provided a desk chair, for instance, and made the interactor sit down to use the computer, but that would be distracting more than enriching. In a real dorm, your roommate would have a separate desk, but that doesn't matter to the scene we're writing. We haven't described any art, decorations, or personal items, because for our game, what matters about the room is that it's messy. And this is one of the most detailed rooms in the game, since an actor who's just getting started will spend a good bit of time here. So as you design your own rooms, think about how the details you choose affect the experience, and make sure they serve the story.]

Chapter 2 - The Dorm Hallway

Dorm hallway is a room. It is north of your room door. "The hallway runs from one end of the dorm to the other, lined with identical [room doors]. At one end, to the west, is the [computer lab]. To the east, a door leads outside. [Your room] is to the south." [Now that we're ready to create the next room, we specify its position in relation to the door we just made. Basically, the dorm hallway is the room to the north of your dorm room, with your room door in between.]

Section 1 - Doors

Some room doors are scenery in the dorm hallway. The description is "The room doors look mostly alike, although some students have decorated them."
Instead of doing anything except examining to the room doors, say "You'd better not bother your neighbors; none of them has got your paper!"

A lockable door called the computer lab door is west of the dorm hallway. It is closed and locked. The description is "The door to the computer lab is a fancy glass door, designed to show off the technical riches within.[if the player is in the dorm hallway] A little black swipe box sits next to it.[end if]". [Note that a door exists in both of the rooms it connects. In this case, you only need to swipe into, not out of, the computer lab, so we add a condition that will mention the swipe box when the player is in the hall, but not the lab.]
Instead of unlocking the computer lab door with something, say "You'll have to swipe your ID card in the appropriate swipe box."

The main door is a closed door. It is east of the dorm hallway. The description is "The main door of the dorm is a heavy steel door, which looks as if it's designed to keep mischief out. Or in. You aren't sure which."

Section 2 - Swipe Box

The lab swipe box is a swiper in the dorm hallway. The description is "This small box lets you swipe a student ID to unlock the door to the computer lab. The light on the box is currently [if the computer lab door is unlocked]green[else]red[end if]." [Note that SWIPER is one of the new types introduced in Book 1 above. We use this type in order to identify what objects pair with the ID card and what don't.]

After swiping the ID card in the lab swipe box:
	now the computer lab door is unlocked; [We use NOW to make a change in the state of the world. By saying this, we declare, whatever the door was before, at this moment, it becomes unlocked."]
	say "The door to the computer lab unlocks with a click."
[Woah! New formatting. After the interactor swipes to unlock the door, we need to do more than one thing, so we make a list. We follow our AFTER rule with a colon, indicating that there's a list to come. Each command in the list must be indented. Instead of writing a period at the end of a sentence the way we're used to, we end commands that are part of the last with semicolons, indicating that there's more to come. The very last command in our list ends with a period in order to show that the list is over.]

After swiping the ID card in the lab swipe box when the computer lab door is unlocked:
	say "The door is already unlocked."

Section 3 - Movement Control

Instead of going east in the dorm hallway when the paper is not carried, say "There's no point leaving without your paper! Your professor is quite firm about deadlines, and he's not going to take any excuses." [We've met the INSTEAD rule before, but here we add a wrinkle with the WHEN clause. This lets us make the parser react differently under different circumstances. In this case, we want to prevent the interactor from leaving the building before getting the paper, to keep things from getting too complicated. (Thus, getting the paper becomes a puzzle to be solved before moving on to the next area.) So we use the WHEN clause to set a condition: we apply the INSTEAD only when the interactor doesn't have the paper. Note that this will also prevent the interactor from getting the paper, dropping it, then leaving. Basically, the paper is a ticket out of the building.]

Chapter 3 - The Computer Lab

A room called the computer lab is west of the computer lab door. "The computer lab is full of [stacks of cash]--except the stacks of cash take the shape of shiny new computers. On a table near the front there's a [printer] with a [pay box] next to it. There's a [stapler] on the table as well. The large glass door leads back east to the main hallway."

Section 1 - Scenery

Some stacks of cash are scenery in the computer lab. Instead of doing anything to the stacks of cash, say "Come on, don't you know what a metaphor is? Maybe they need to invest some of that technology budget in English classes." [So, I made a joke in my room description. A very literal-minded interactor might take it too seriously and try to do something with the cash. By default, the parser would report that there are no stacks of cash--true, but not very helpful. It's better to explain what went wrong. We can't do that without actually creating the stacks of cash--ironically, the very thing we're trying to tell the interactor don't exist. To make sure the interactor can't do ANYTHING with them, we declare them to be invisible--that means they won't show up in room descriptions.]

Some computers are scenery in the computer lab. The description is "The computers that fill the room are gorgeous, with large, bright, glassy screens. You think you can see last term's tuition over there on the far side of the room."
Instead of doing anything except examining to the computers, say "Your paper isn't on any of those computers, so you don't see how it can do any good."

Section 2 - The Table

The table is a supporter in the computer lab. It is not portable. The description is "The wooden table feels incongruously low-tech in this room buzzing with computation. On top of it you can see [the list of things on the table]. There's a small drawer in the center." [Only one new concept here, and it should be pretty self-evident.]
The drawer is a closed openable container that is part of the table. The description is "[if the drawer is closed]It's closed.[else]The drawer is open.[end if][if the drawer contains nothing and the drawer is open] It's empty.[else if the drawer is open] Inside, you see [the list of things in the drawer].[end if]" [We don't actually have to define the drawer as part of the table, since we can't really do anything with the table, but if the table were portable it would be important to define that relationship so that if we moved the table, the drawer would come along.]

Some staples are an object. They are in the drawer. The description is "A small, nondescript row of staples."

Section 3 - The Stapler

The stapler is a a container. It is on the table. The description is "It's a perfectly ordinary stapler.[if staples are not in the stapler] It seems to be empty right now.[end if]".
Instead of inserting something into the stapler when the noun is not the staples, say "The only thing that goes in there is staples."
Does the player mean stapling something with the stapler: it is likely.

Section 4 - The Printer

The printer is a container on the table. It is not portable. The description is "This printer is enormous! Clearly when it comes to technology, size does matter. [if the paper is in the printer]Several pages are sitting in the tray.[else]The tray is empty.[end if]". [A CONTAINER is a new kind of object for us. Basically, it's an object that can contain other objects. When the interactor successfully prints the paper, we're going to place the paper in the printer, so making the printer a container lets us do that. But note that by default, interactors can add and remove things to containers freely, which means right now the player could put anything into the printer. We're going to fix that in the next step.]
Instead of inserting something into the printer, say "You can't really say what has possessed you to want to put [the noun] in the printer, but in any case, you aren't sure how you'd do that." [The words "the noun" mean the first noun that follows the verb, meaning those words will be replaced by whatever the interactor tries to put in the printer.]
Instead of doing anything except examining to the printer, say "Actually, you can't do anything with the printer itself. You have to have a computer to print anything, and even the pay station is a separate device. Technology, huh?"

Section 5 - The Pay Box

The pay box is a swiper on the table. The description is "The pay box is a screen with a place to swipe a card next to it. It keeps track of what people have tried to print, and holds their documents for ransom until they've left behind some more money in the computer lab. [if the paper is queued]Hey! Your paper is in the list! It ought to print if you swipe your card.[else]There's nothing on the screen right now.[end if]".
After swiping the ID card in the pay box when the paper is queued:
	say "With a whirring sound, the printer roars to life and spits out a series of white pages, face down. You hardly dare to hope...";
	now the paper is not queued;
	now the paper is printed;
	now the paper is in the printer.
Instead of swiping the ID card in the pay box when the paper is printed, say "You've done that now! It's not as though having a second copy will help; you just need to get it there."
After swiping the ID card in the pay box when the paper is not queued, say "Nothing happens. You feel as if you missed a step."

Section 6 - Movement Control

Instead of going east in the computer lab when the stapler is not on the table, say "You should put the stapler back on the table where you found it."

Part 3 - Outdoors

Outdoors is a region. Grassy area, Construction zone, Road, and Office lawn are in Outdoors. [Regions are a way of grouping rooms together. By defining Outdoors as a region, we can now make decisions based on whether or not a given room is outdoors--something that will come in handy as we're going to introduce a bicycle, which should only be ridden outdoors. It might make sense to define regions for the dorm and the office, but I haven't done so since they aren't useful in this short game.]

Chapter 1 -  Grassy area

A room called Grassy area is east of the main door. "A large grassy expanse lies in front of the dorm. A quiet footpath meanders north toward your professor's office. A street runs to the northeast."

The book is scenery in the Grassy area. The description is "It's big and expensive-looking and full of highlights. You can't tell what class it's for." Instead of doing anything except examining to the book, say "You don't recognize it, so you're surely not taking that class. You don't need it."

Section 1 - Motion Control
	
Instead of going to Road when the player is not on the bicycle, say "It's way too dangerous just to run out into the road! Vehicles only."

Instead of going to Road when the player is not on the bicycle for the third time:
	say "To hell with it. Throwing caution to the wind, you stride out into the road and start off in the direction of [if the location is Grassy area]your professor's office[otherwise]your dorm[end if]. The sudden, excruciating pain catches you quite off-guard as your body soars through the air. You awake in the hospital. The car that hit you broke both your leg and your pelvis; largely immobile for months, you're forced to take a medical withdrawal for the remainder of the term.";
	end the story finally saying "YOU ARE HOSPITALIZED". [This instruction ends the game. The last message displayed will be the message we specify here; we could also omit the "saying...," as happens below, and the game would say "THE END." Immediately before that message, some other text will be displayed, as defined below.]
	
Instead of going to a room that is not in Outdoors while the player is on the bicycle, say "You can't ride the bicycle indoors!".

Chapter 3 - Construction zone

A room called Construction zone is north of grassy area. "A [footpath] stretches to the north, running directly between your dorm and your professor's office building. Currently, that path is blocked by a bright orange [barricade]. Past the barricade, the path is torn up; enormous [construction equipment] looms over the cratered way. Walls prevent passage to either side; the only way to go is back to the south."

Instead of going north in construction zone, say "No, the path has been utterly demolished by whatever construction they're doing. You'll have to find another way."

A footpath is scenery in construction zone. The description is "Once a pleasant little footpath that made a nice shortcut that wound its way across a rarely-used part of campus, it has been completely destroyed by whatever campus construction push is going on these days. You haven't heard what they're planning to build, but you have a sinking feeling that it won't be as nice as what's there." Understand "path" as the footpath.

A barricade is scenery in construction zone. The description is "You know those little orange cones, not even knee high, that people put up when there's a bottomless hole or deadly quagmire or something? This isn't one of those. It's a hulking concrete barrier almost as high as your shoulders, painted screaming orange, and stretching the whole way across the path. Whoever put it up is very determined: you're not going back there." Understand "barrier" as the barricade.
Instead of climbing the barricade, say "It's too tall, and the sides have no handholds. You'd never make it."

Some construction equipment is scenery in construction zone. The description is "You can never remember the names for all the different kinds of construction equipment, but it hardly matters: all of them are here, and huge, and yellow. They've completely destroyed the path."
Instead of doing anything except examining to the construction equipment, say "You can't get to the equipment, since you can't cross the barricade. Even if you could, you're pretty sure messing with it would get you arrested or expelled. You're not sure which would be worse at this point."

Chapter 4 - Road

A room called Road is northeast of grassy area. "The road through campus is lightly traficked. It bends to the northwest, and stretches behind to the southwest."

Instead of dismounting when the player is on the bicycle and the location is Road, say "It would be crazily dangerous to get off the bike in the middle of the road! You stay put."

Chapter 5 - Office lawn

A room called Office lawn is northwest of road. "You are standing in a small grassy area in front of a [white house], with a wooden [front door] leading in. Yes, it's an old house. It's also an office building. Just one of the oddities you might run into on a campus that's slowly eating up its town."

The white house is scenery in office lawn. The description is "It's a fairly small white house with a [front door] made of wood. It looks old. It feels allusive."

A closed lockable locked door called the front door is north from office lawn. The description is "A black wooden door breaks the white smoothness of the front of the house. There's no swipe box here, just an old-fashioned lock." The front door has matching key the office key.
Instead of going inside when the player is in office lawn, try going north. [When presented a house, it's very reasonable to want to go inside it. Here we are using compass directions, but we can translate an effort to go inside into going north instead. Note that we need to do the same thing in reverse in the adjoining rool, as we do below.]

The office key is an object. "A small key is lying [if the key is in Outdoors]in the grass[otherwise]on the floor[end if]." The description is "It's a small, very plain-looking key." The office key is nowhere. [The if statements in the initial description are necessary because the player might carry the key indoors and then drop it, which would add it to the locale description.]
	
Does the player mean unlocking the front door with the key: it is likely. Does the player mean opening the front door: it is likely.

Part 4 - Office Building

Chapter 1 - Foyeur

A room called Foyeur is north from the front door. "Along one side of the hall is a row of [mailboxes], labeled with the [names] of various professors. Opposite the mailboxes, you see a [closed door] to the west. A hall stretches on to the north."

Section 1 - Doors and Scenery

A door called the closed door is west from the foyeur. It is closed and locked. The description is "The [nameplate] bears a name you don't recognize."

The nameplate is scenery in the foyeur. The description is "It's not your professor."
Some names are scenery in the foyeur. The description is "So many different names! Thank goodness you recognize your professor's among them."

The mailboxes are an open container in the foyeur. The mailboxes are not openable. They are not portable. Understand "box/boxes/mailbox" as the mailboxes. The description is "The mailboxes are really more like little cubbies. You quickly find the one with your professor's name on it. It's empty, just like all the others."
After inserting the paper into the mailboxes, say "You put you paper in the box bearing your professor's name. You really hope he finds it." [It's important to use AFTER here because the action of inserting changes the game world: before doing this, the paper is not in the box; after doing it, it is. Using "instead" would prevent that from happening, so that the player would still have the paper in his inventory.
	The phrasing "inserting... into..." may seem unnatural, but this is the way Inform words the action of putting something inside something else. I had to check the documentation to find that out. The player can just type "put paper in mailbox," but we have to word it this way for Inform to understand.]

Instead of going outside in the foyeur, try going south.

Chapter 2 - Office hall

A room called Office hall is north of Foyeur. "A short hallway stretches toward the back of the building, where it ends in a [small window] overlooking a parking lot."

An object called small window is scenery in Office hall. The description is "Through the window you can see an expansive parking lot, bustling with cars." Understand "parking lot" as the window. Instead of opening the small window, say "It's bolted shut."

A door called unmarked door is west of Office hall. It is closed and locked. It is not scenery. "A closed door with no nameplate is set into the west wall." The description is "The door is absolutely plain, with no nametag or decorations. It appears the office is unoccupied. The door is shut tight."

A door called professor's door is east of Office hall. It is closed and unlocked. It is not scenery. "A door to the east leads to an office. [if the professor's door is closed]It is closed.[otherwise]Inside, you can see your professor.[end if]". The description is "It has your professor's name on it."
After opening the professor's door when professor's door is not knocked on:
	say "You open the door. Your professor glares at you from within. 'It isn't very polite to open someone's door without knocking.'";
	increment the the annoyance of the professor.
Does the player mean knocking on the professor's door: it is likely.

After knocking on the professor's door, say "'Come in,' a voice calls from within."

Chapter 3 - Professor's office

A room called Professor's office is east of professor's door. "The professor's office seems to overflow with learning. [Bookshelves] line the walls, filled with tomes bearing obscure and incomprehensible titles."

Bookshelves are scenery in the professor's office. The description is "Spines of many colors line the shelves. You can't even begin to guess what some of the obscure titles mean." Understand "books/tomes/bookcases" as the bookshelves.

The professor's desk is an scenery in the office. The description is "The desk has several books open, as well as two stacks of papers. Yet somehow it's infinitely neater than your own desk."

Instead of searching the professor's desk, say "However brazen you might feel[if the paper is nowhere] now that you've handed in your paper[end if], you don't dare rifle through the desk with your professor there watching you."

Book 2 - Characters

[This section concerns people. Note that there are two ways to define people in Inform. We can define them as Person, which is a generic term for any living organism that can act on its own: if there were animals in this game, we would declare them as people. We can also make a Man or Woman, which signifies a human person with a particular gender. Inform will give men and women the pronouns that typically correspond. I've defined some characters, including the character, in a gender-neutral manner and so make them people; others with specific gender are more precisely defined as men or women. Man and woman make no assumptions about age; male and female children would also be considered men and women.]

Part 1 - Your Roommate

Your roommate is a person. "Your roommate is sprawled in bed, snoring." Your roommate is in your roommate's bed. The description is "You've been roommates for the better part of a year now. You've never known anyone else with such an incredible talent for sleeping--a talent on prodigious display at this very moment."

Instead of doing anything except examining to your roommate, say "You try to rouse your roommate. For a moment, hope dawns in the form of half-opened eyes. But then: snoring. It's no good; you'll have to get the paper on your own."

Part 2 - The Hard-Working Student

The hard-working student is a woman in the Grassy area. "A student lies sprawled on a blanket on the grass, [if the hard-working student is beaten]unconscious[otherwise]studying[end if]." The description is "She has a look of intense concentration on her face as she studies her [book]."

Chapter 1 - Dialogue

[The series of INSTEADs that follow illustrates one approach to dialogue using Inform. The player can type ASK _____ about _____, and if it is one of the topics we have defined, receive a response. This system works for simple exchanges, but becomes unwieldy if there are many topics, and almost unusable in complicated circumstances where conversations branch or characters remember what has already been said. In those cases you will want to seek out a more advanced system for dialogue; several strong extensions have been written for that purpose. There are also systems slightly more sophisticated than this approach that use tables for dialogue; you will find examples in the Inform documentation.]

Instead of asking the hard-working student about "bicycle/bike/cycle":
	say "'If it's that much of an emergency, I'd let you borrow it,' she tells you. 'But I'd need you to leave something important with me so I can be sure you'll bring it back.'";
	now the bicycle is requested.

Instead of asking the hard-working student about "bus", say "'It comes every couple of minutes,' she answers. 'Just wait around; it'll show up.'"

Instead of asking the hard-working student about something, say "'Don't bother me unless it's important,' she hisses. 'I'm trying to study!'"

Chapter 2 - Actions

Instead of attacking the hard-working student:
	say "Feeling desperate and aggressive, you hurl yourself at the student, fists flying. She goes out like a light. Let's see her stop you from taking that bicycle now! But despite your bravado, your stomach twists nervously.";
	now the hard-working student is beaten.

Instead of giving something to the hard-working student, say "She looks at you quizically. 'Why are you trying to give me that?' she asks, handing it back.".

Instead of doing anything except examining to the hard-working student when the hard-working student is beaten, say "It's too late now that you've knocked her out."

Instead of giving the ID card to the hard-working student when the bicycle is requested:
	say "'Yeah, that should work,' she says, smiling. 'You'll have to come back if you want to eat. Okay, you can borrow my bicycle.' Your ID card disappears into her pocket.";
	now the hard-working student has the ID card; [This line is very important: it transfers possession of the ID card to the hard-working student character, both removing it from the player's inventory and allowing us to check whether the student has it later. There are other ways of accomplishing the same thing--for example, making the "give" action succeed instead of pre-empting it with INSTEAD.]
	now the bicycle is not requested;
	now the bicycle is granted.

Part 3 - The Grad Student

The grad student is a man in office lawn. "A graduate student [if the grad student is beaten]lies unconscious[otherwise]sits[end if] on the grass in front of the building[if the grad student is not beaten], his nose in a book[end if]." The description is "He's utterly [if the grad student is beaten]unconscious[otherwise]absorbed in his reading[end if]."

Chapter 1 - Dialogue

Instead of asking the grad student about "door/key/office/building/lock":
	say "'Oh, sure, I can let you in,' he says. He rises, goes over to the door, and fumbles in his pocket for a moment before producing the key with a look of triumph. 'Here we go!' he exclaims, turning the key in the lock. Or trying to. It's stuck. With mounting frustration you watch as he fiddles with the knob, but finally, the door unlocks with a click. The student heaves a sigh and returns to where he was sitting before, looking forlornly at the book.";
	now the front door is unlocked.
	
Instead of asking the grad student about "professor", say "'So brilliant!' the grad student rhapsodizes. 'That's who I came here to work with.'".

Instead of asking the grad student about something, say "He grunts indistinctly, not looking up from his reading."
	
Chapter 2 - Actions

Instead of attacking the grad student:
	say "Something inside you snaps. All of your exhaustion and frustration are too much to take, and your body fills with violent energy. You pull back your fist. The poor surprised [a noun] doesn't even have time to see it coming before your hand connects with an unyielding skull.[paragraph break]Ouch! You're so stung by the pain searing through your hand that it's a moment before you notice that [the noun] is now sprawled on the ground, unconscious. After a moment, you notice that a key has fallen out of [regarding the noun][possessive] pocket.";
	now the grad student is beaten;
	now the office key is in the office lawn.
	
Part 4 - The Professor

The professor is a person in the professor's office. "The professor sits behind the large desk, looking intently at a book." The description is "Right now, your professor looks [if the professor is very-annoyed]very annoyed[otherwise if the professor is somewhat-annoyed]a little annoyed[otherwise]quite focused[end if]."

Does the player mean doing something to the professor while the location is the professor's office: it is likely.
	
Chapter 1 - Dialogue

Instead of asking the professor about "extension":
	say "'It's too late for that now,' your professor growls, glaring. 'You should have asked days ago.'";
	increment the annoyance of the professor.
	
Instead of asking the professor about "grade" when the paper is not nowhere, say "'You'll have to hand it in first!' the professor exclaims, looking surprised."

Instead of asking the professor about "paper":
	if the paper is late:
		say "'It's past due. You'd better hand it in right away,' the professor snaps.";
	otherwise if the paper is nowhere:
		say "'You've already turned it in,' the professor replies. 'Now I've got to grade it.'";
	otherwise:
		say "'You have it finished, I hope?' The professor raises an eyebrow. 'I'll take it now.'".

Instead of asking the professor about "box/boxes/mailbox/mailboxes/cubby/cubbies", say "The professor looks surprised. 'I check them every now and then, but there's no way for me to know when someone puts something in there. That's why I tell you to hand in your work directly to me.'"

Instead of asking the professor about something:
	if the paper is nowhere:
		say "'I've got your paper, so there's nothing else we need to talk about,' the professor tells you. 'You should head on home.'";
	otherwise:
		say "The professor looks at you. 'What I'd really like to talk about is your paper. Where is it?'".
		
Chapter 2 - Actions

Instead of giving the paper to the professor:
	say "You nervously hand over the paper. The professor glances down at the front page. [if the paper is not stapled]'Couldn't you even bothered to staple it?' [end if]With a sigh, the paper disappears somewhere into the professor's voluminous desk.";
	if the paper is not stapled, increment the annoyance of the professor;
	now the paper is nowhere. [In this case, we're not going to give the player access to the paper again after it's been turned in, so the simplest thing to do is to make it cease to exist. This saves us the trouble of making the desk a container and making sure the player cannot somehow access the paper through an error.]

Instead of showing the paper to the professor, say "The professor glances at the paper but appears unimpressed. 'You're just going to have to bite the bullet and hand it in.'"

Instead of attacking the professor:
	say "You land the first blow, but the professor is out of the office and on the phone before you can land a second. It's astonishing how quickly campus police arrive. Before you even fully understand what's happening, you're banned from campus pending a disciplinary hearing and booked into a holding cell. When you return to campus for your hearing months later, the whole thing is over quite quickly[if a person is beaten], especially when your history of violent behavior comes to light[end if]. You started your day just trying to hand in a paper; now, you're never going back to college. And you still have a criminal trial to look forward to. Perhaps you have not made the best choices.";
	increment the annoyance of the professor;
	end the story finally saying "YOU ARE JAILED".

Book 3 - Vehicles

Chapter 1 - The Bicycle

The bicycle is a rideable vehicle in Grassy area. The description is "It's a bit battered, but it looks dependable." [The kind "rideable vehicle" is added by the extension we included at the beginning. While Inform does include a built-in vehicle class, which we'll use shortly to create a bus, using that class would have the unfortunate effect of describing the player as being "in the bicycle." Rideable vehicles fixes that problem.] Understand "bike/cycle" as the bicycle.
The bicycle can be requested. The bicycle can be granted. [Here, we are assigning possible properties to the bicycle. Inform doesn't know what the words "requested" and "granted" mean, but having said this, we can tell it that the bicycle is requested or granted, and the game will remember; we can then ask it, and use that to make decisions.]
Instead of taking the bicycle, try mounting the bicycle. [As with any object, the bicycle is by default portable. But it doesn't make sense for the player to take the bicycle, so we redirect this action to getting on the bicycle instead.]

Instead of mounting the bicycle when the ID card is not held by the hard-working student and the hard-working student is not beaten, say "'Ahem.' The student has torn herself away from her book and is glaring at you. 'That's [italic type]mine[roman type]. You aren't [italic type]stealing[roman type], are you?" [This line represents one of the challenges of organizing source text, for it could just as easily be a part of the student's entry.]

Chapter 2 - The Bus

The bus is a vehicle. "A bus from the campus transit system sits next to the curb." The description is "It's one of those fancy eco-friendly buses that runs on natural gas. The sign says it's heading to [if the bus is in Grassy area or the bus is outbound]the office building[otherwise]the dorm[end if]."
The bus is in Office lawn. [This tells Inform where to put the bus first; it will move on its own from there, as specified below.]
The bus can be either inbound or outbound. The bus is outbound. [While before we've defined some optional properties, here we define an either/or property: the bus will always be either inbound or outbound. We must change this status as appropriate.]

Instead of going a direction while the player is in the bus, say "Let the bus driver handle the navigation. The only thing you have to do is get off at the right stop." [Inform by default treats vehicles as "driven" by the player, so that the player's movements cause the vehicle to move. Since the bus is autonomous, we override that behavior by preventing movement while the player is on the bus.]

Instead of entering the bus when the player does not have the ID card, say "You have to have your ID card to be able to board the campus busses."

The driver is a person in the bus. He is scenery. The description is "The driver is focused on the road."

Every turn: [Unlike everything else in our game, the bus has a life of its own: it makes its circuit between the office and the dorm whether the player is interacting with it or not. EVERY TURN rules let us specify things that happen every turn, no matter what. If we wanted to perform these actions only when certain conditions are met, we could use an EVERY TURN WHEN... rule instead.]
	if the bus is in Office lawn for the third turn: ["for the third turn" in this case means for the third turn IN A ROW. So, the bus will arrive, wait two turns, and depart.]
		now the bus is inbound;
		try the bus going southeast; [You've seen this before, but I haven't explained it yet. TRY is how we tell something in the game to do something during code; as the Inform documentation says, the word "try" is meant to indicate that the action may not be successful -- that is, something might prevent the bus from going southeast.]
	otherwise if the bus is in Grassy area for the third turn:
		now the bus is outbound;
		try the bus going northeast;
	otherwise if the bus is in the road:
		if the bus is inbound:
			try the bus going southwest;
		otherwise:
			try the bus going northwest.

Instead of getting off the bus: try exiting. [Inform interprets "get off" to mean stop standing atop something. But an interactor might very well want to "get off bus," so we redirect that action.]
Instead of taking the bus when the player is not in the bus: try entering the bus. [Likewise, a player might try to "take bus," so we redirect that.]

Book 4 - Advanced Rules

[While I try to describe what these rules do, they are relatively advanced techniques compared to everything else we see, and you may have to do some research if you need to modify the behavior of your game in similar ways.]

Chapter 1 - Bedroom Description

Rule for reaching outside your bed: [This rule controls what happens when the player is in the bed and tries to interact with something outside the bed. In this case we want the player to get up before doing anything else, so we deny access to anything outside the bed. Remember, the bed is a container, so we imagine the player trying to reach outside that container.]
	say "You'll have to get up first.";
	deny access.
Rule for printing the locale description when the player is in your bed: stop. [We modified the initial description of Your Room so that it would say something different when the player was in the bed. But left to its own devices, the game will still go on to list items in the room, according to its default behavior. We don't want this, as the player in the bed shouldn't see anything by default, so we stop the locale description from printing.]

Chapter 2 - Bus Description

After the bus going when the player is in the bus: try looking. [This line resolves a problem caused by making the bus autonomous. When a player moves from room to room, it's typical to print a description of the room entered. But that doesn't happen automatically when the player is in something else that moves, like a vehicle. This command, the equivalent of the player's typing LOOK every time the bus moves, solves the problem.]

Instead of looking when the player is in the bus: [By default, looking inside a vehicle is no different from looking when outside it. But it doesn't make sense for the player to have a full view of the scenery when inside the bus, so we can replace that description with something different. My approach is very inelegant, but it works, and honestly, I haven't found a better one yet.]
	follow the room description heading rule; [We need this line in order to get the nice bolded heading telling us where we are, since we're replacing the normal action of LOOK.]
	say "The bus is fairly empty. Outside the windows, you can see [if the bus is in Office lawn]the office building[otherwise if the bus is in Grassy area]your dorm[otherwise]the road through campus[end if].".
	
Chapter 3 - Reading Commands

[The rules that follow change the way the game processes commands, removing any definite article. When we're talking abotu objects, the parser ignores articles anyway, so "take ID" and "take the ID" are equivalent. But that's not true of dialogue, which matches text: without the following rules, "ask student about bus" will produce the desired response, but "ask student about the bus" will not. These examples are borrowed from Emily Short, "When in Rome 1: Accounting for Taste," Release 3 (http://inform7.com/learn/eg/wir1/index.html).]

After reading a command:
	while the player's command includes "the":
		cut the matched text;
	while the player's command includes "a":
		cut the matched text.

Book 5 - Story

Part 1 - Setting the Scene

The player is in your bed. [This line specifies where the player character is when play begins. If we didn't have it, the character would by default be in the first room described--which is correct--but not in the bed.]
	
When play begins: [This allows us to specify what will happen at the beginning of the game. We'll use it here both to set the initial position of the character and to display a brief prologue.]
	say "Burning the midnight oil, they call it! Midnight's a normal night for you, but this paper you've been writing for your professor has taken you many hours past that. After a miserable, exhausting night, you crawled into your bed for a few hours of rest before handing the beast in. You aren't having class today, but your professor said you should bring the paper to his office and hand it in to him by the time your class would normally begin. Morning approaches...".

Part 2 - Paper Rush Scene

Paper Rush is a scene. [Scenes are a rich tool, offering much greater narrative control than you see here. Here, I use the scene to add some pressure: once the first set of puzzles is complete and the player has made it outside, we start a timer, and the paper is then late if the player can't get it in on time.]

Paper Rush begins when the player is in Grassy area. "You feel a sudden surge of panic. You glance at your wrist, but of course you never wear a watch. Still, you have this sudden feeling that you're running out of time. You'd better get the paper in fast!"

Paper Rush ends when Paper Rush has been happening for 15 turns.

When Paper Rush ends:
	say "You can hear bells in the distance, their melodious chime floating across campus on the breeze. The deadline is here. [run paragraph on]";
	if the paper is nowhere or the paper is in the mailboxes:
		say "You breathe a sigh of relief at having gotten your paper in.";
	otherwise:
		say "A wave of crushing disappointment floods through you as you realize you've failed to get the paper in on time. Still, you must press on: a late penalty is better than no grade at all.";
		now the paper is late.

Instead of waiting while Paper Rush is happening, say "Time passes. But can you really afford to waste time when your paper is due so soon?". [To add flavor to the scene, we can change how one of the actions works while it's going on.]
	

Part 3 - The End

When play ends: [This will happen whenever the game ends for any reason. The one constant among our endings is that in ever case the player should receive a report on the grade, so we can include it here. It's often used for reporting scores (which is the function the grade serves here), or for boilerplate final text.]
	say the final grade.

Instead of going south in Foyeur:
	say "You leave the office and trudge back to your dorm[if the hard-working student has the ID card and the hard-working student is not beaten], stopping along the way to collect your ID card[end if]. You arrive to find your roommate [one of]still blissfully unconscious--the lucky fool[or]gone, apparently having decided to go to class for once[purely at random]. Exhausted, you sink into your soft bed, ready to get some rest at last.";
	if any person is beaten:
		say "[line break]You're jerked awake a short time later by a sharp pounding at the door. Your head throbs as you stumble blearily over to open it. You feel very small next to the towering figures silhouetted against the too-bright light of the hallway. 'We've got some questions for you,' one of the police officers says gruffly. 'About your attack[if more than one person is beaten]s[end if] on [the list of beaten people].' Your stomach sinks. You're certainly not going to back to sleep now.[paragraph break]The process drags on over several months. In the end, you don't face criminal charges, but you're dismissed from the university, and you're working to pay a hefty financial settlement to the student[if more than one person is beaten]s[end if] you assaulted. In retrospect, failing your paper doesn't sound so bad.";
		end the story finally saying "YOU ARE EXPELLED";
	otherwise:
		end the story finally.

Chapter 1 - Grading

To say the final grade: [This creates an action we can then use elsewhere, producing the text we string together here.]
	let the penalty be the annoyance of the professor;
	if the paper is late or the paper is in the mailboxes:
		increment the penalty;
	say "Several weeks later, you finally hear about your paper. [run paragraph on]";
	if the paper is on-stage and the paper is not in the mailboxes:
		say "You didn't hand it in to the professor or leave it in the mailbox, so of course you get a zero.";
	otherwise:
		if the paper is in the mailboxes:
			say "When you turned it in, you left it in your professor's mailbox. Since the professor didn't check the box until well after the deadline, it was marked late. [run paragraph on]";
		otherwise if the paper is late:
			say "You handed it in late, so you received a penalty. [run paragraph on]";
		if the annoyance of the professor is greater than 0:
			say "You get the feeling that by the time you handed in your paper, your professor was [if the professor is somewhat-annoyed]a little[otherwise]very[end if] annoyed with you, and it showed in the grading. [run paragraph on]";
		say "In the end, after all that work, your grade on the paper is [the letter grade corresponding to a deduction of the penalty in the Table of Grades]."

Table of Grades [It's often convenient to assemble information in a table; the instructions above include an example of how to look something up in such a table. Here, we're translating deductions into the letter grade that will be the final score.]
Deduction	Letter grade
0	"A"
1	"B+"
2	"B-"
3	"C"
4	"D"
5	"F"

Book 6 - Actions

[In this section, we define custom actions, allowing the player to do things that aren't already built into Inform. The syntax for creating commands is moderately advanced and I don't explain it fully below; consult the documentation for more information.]

Part 1 - Stapling

Stapling it with is an action applying to two objects. Understand "staple [something] with [something]" as stapling it with. [This unfortunate syntax lets the verb take both a direct and an indirect object. In this case, we staple one thing with a second (i.e. the stapler).]
Instead of stapling something with something when the second noun is not the stapler, say "I'm pretty sure you can only staple something with a stapler."
Instead of stapling something with something when the noun is not the paper, say "Why would you ever want to staple [the noun]?"
Instead of stapling something with something when the staples are not in the stapler, say "You close the stapler several times, but it doesn't seem to be working. It must be empty."
After stapling the paper with the stapler:
	say "The stapler closes on the paper with a satisfying click. There, your pages will stay together.";
	now the paper is stapled.

Part 2 - Printing

Laserprinting is an action applying to one visible thing. Understand "print [something]" as laserprinting.
After deciding the scope of the player while laserprinting, place the paper in scope. [This is a fairly complicated instruction. Normally when the player asks to print the paper, the game will complain, because the player is trying to do something to the paper, but the paper isn't in the room. Adjusting the scope of the paper is a way around that, but we have to be careful, or the player will be able to do other things to the paper as well.]
Instead of laserprinting something when the computer is not visible, say "Your mother may accuse you of living online, but you don't really, so you can't print anything without having your computer in front of you."
Instead of laserprinting something when the computer is switched off, say "You know, they call them electronic devices because they need electricity. Without that, they're just big boxes of... Oh, anyway, there's no way to print with the computer off."
Instead of laserprinting something that is not the paper, say "The only thing you have due today is that paper, so there's no point printing anything else!"
Instead of laserprinting the paper when the paper is not found for the first time, say "You'll have to find your paper before you can print it."
Instead of laserprinting the paper when the paper is not found, say "You still haven't found the paper. Have you tried searching for it?".
Instead of laserprinting the paper when the paper is queued, say "You've already printed it once; doing it again won't help.[if the paper is carried] And you already have it! Let's go![else if the paper is queued] Now you just have to figure out where it went![else if the paper is printed] Just take it with you and go.[end if]".
Instead of laserprinting the paper:
	now the paper is queued;
	say "The progress bar on your computer ticks away with excruciating slowness, but at last it hits 100%. There! It's done![paragraph break]You strain your ears for the whirring sounds of paper and ink nozzles. Nothing. Err...[paragraph break]It slowly dawns on you that you don't actually own a printer. But it's clearly printed to somewhere. If only you weren't too tired to remember where."
Does the player mean laserprinting the paper: it is very likely.

Part 3 - Other Custom Actions

Swiping it in is an action applying to two things. Understand "swipe [something] in [something]" as swiping it in. [While there are lots of words Inform already knows, "swipe" isn't one of them, so we have to define it. The first sentence creates the action--we always refer to actions using present participles. "Applying to one thing" tells us that the verb expects a direct object: we can't just swipe; we have to swipe something. The second sentence specifies how the interactor will carry out the action, by typing the command, "swipe the thing." Note that so far, the action does nothing, and the player can swipe absolutely anything.]

Searching for is an action applying to one visible thing. Understand "search for [something]" as searching for. Instead of searching for something, say "You hardly know where to begin. Instead of searching for something, why not say where you want to look? Then you could start to narrow it down. For instance, you might [bold type]search your bed[roman type]..."
After deciding the scope of the player while searching for, place the paper in scope.

Using is an action applying to one thing. Understand "use [something]" as using.
Locking is an action applying to one thing. Understand "lock [something]" as locking. [The LOCKING action in Inform takes two objects; we create a special one-object version to issue a response of the player asks to lock the room door, which has no key.]

Knocking on is an action with past participle knocked, applying to one thing. Understand "knock on [something]" as knocking on. After knocking on a door, say "There is no answer."
Carry out knocking on a door: now the noun is knocked on. [The CARRY OUT rule specifies the content of an action. In most examples, I've used INSTEAD rules, which actually prevent an action from taking place and do something else instead. Some Inform programmers would argue that using INSTEAD in this fashion is bad form, though I've chosen it because it's simpler. But this demonstrates a more robust way to build and modify actions: when we CARRY OUT knocking, the game considers it to have actually occurred, and in the course of doing it, gives the specified noun the "knocked on" property.]

Part 4 - Responses to Commands
	
Instead of sleeping, say "There's nothing you'd like better than to shake off the weight of this cruel and thankless world and slip into the Morphean embrace of inky night, but if you do that, you'll never hand your paper in. Or finish this game."

Talking to is an action applying to one thing. Understand "talk to [something]" as talking to. Instead of talking to something, say "Perhaps you should try [bold type]ask[roman type]ing [the noun] about something instead." [This is largely superfluous, but lets us give a hint to a player who's using the wrong commands.]