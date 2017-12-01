# MeanMax

As always when a contest end, I’m really impressed with the solutions the other players have. I often feel like I have good result by doing way more simple and classical things.

## Game engine
The first step was to have a good game engine to simulate the game… and I’ve been really bad at doing it. Even if I had just done CSB and we had a good referee, I had a lot of pain having a simulation without bugs.
To be sure that my simulation is correct I play against myself and check that the inputs given by CG are the same than what I simulate from the previous turn. I only achieved it on Friday.

## Fitness function
Then I had to code an evaluation function, it includes:
-	The score
-	The rage 
-	The distance of the reaper to the closest wreck
-	The distance between the destroyer and a close tanker with the max carrying water.
-	The distance between the doof and the closest opponent reaper 
-	The distance between the reaper and the destroyer

The final evaluation is then `myScore – (0.5*opponent1Score – 0.5*opponent2Score)` and I compute it at each step with a degressive coeff of 0.75  
With this fitness function and a simple MC, I could climb from mid silver to 10th legend in two pushes on Saturday. What a day!

## Genetic algorithm
Here is the time to thanks Magus for his post-mortem on Fantastic Bits. It was my first GA and I didn’t want to waste a lot of time on it so I just did the same thing as him.

## Dummies
The next improvement was to add dummies to simulate the enemies. They do the following actions:
-	Reaper: go to closest wreck with full speed if there is a unit staying on it or with -1.5*velocity if the wreck is clear
-	Destroyer: wait (here I had no good idea as this unit is very unpredictable, sometimes it will destroy a tanker if the reaper can go to it, sometimes it won’t)
-	Doof: go to the closest enemy reaper

The addition of the GA and the dummies leaded me to 1st place for a brief moment!

## Dealing with 1v1v1
This is maybe the only special thing I tried, even if I’m not sure it helped me a lot.  
When playing a 1v1v1 game, you must know which player(s) you want to consider/focus. This is not an easy choice. 
For example, if you’re 2nd in score, sometimes you want to focus the 1st player if you can catch him up, sometimes you just want to be sure that the 3rd player won’t pass you, and sometimes you want to focus both because the scores are close or because it’s too early in the game to guess who will win.  
I initially wanted to do something very complex but because of the lack of time I ended up with a simpler version: 

I first want to know the progress of the game. This is defined by: `progress = max (turn/200, maxPlayersScore/50)`  
I then determine the player I want to beat by calculating the closest opponent in score. (for example if the scores are 30 for me and 22 - 40 for the opponents, abs(30-22) < abs(30-40) so I’ll focus the one who has 22).
The coefficient given to the opponents are:  
- `0.5 + 0.5 * progress` for the opponent I want to focus  
- `0.5 - 0.5 * progress` for the other  

And when I compute the final evaluation, it is now `myScore – opponent1Score*coef1 – opponent2Score*coeff2`  
That way I will at the beginning of the game focus equally both players and at the end of the game only the closest opponent.

## Other improvements
During the last hours, I tried to change the magic numbers and a lot of other things in my evaluation function but nothing really worked (my final magic numbers are basically the same as the one in my first fitness function!). 
The only two improvements are:
-	Instead of focusing the closest reaper, my doof focus the reaper of the player I am focusing.
-	Instead of targeting the reaper directly, the doof targets the closest wreck from the reaper in order to bloc him.

The other players made a lot of progress during Sunday night and I finished 4th in a very close battle for the top!

## Final thoughts:
Once again, a really good contest, maybe complex for beginners but the theme was good and we were really playing a game corresponding to the theme :)  
About 1v1v1, I think I prefer 1v1 because you have a better control of the outcome of a game. However:
-	this was really well implemented with a good map, and there wasn’t a mix of 1v1 and 1v1v1 which is would have forced us to do multiple strategies.
-	All the recent contests were 1v1 contests so that’s nice to change.
-	It contributes to having a chaotic game in the Mad Max theme!

Thanks a lot to the creators !
