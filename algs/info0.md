# Monty Hall problem

```
The answer to the Monty Hall problem is that you should always switch doors to maximize your chances of winning the car.
By switching, your probability of winning increases to two-thirds (2/3), while sticking with your original choice only gives you a one-third (1/3) chance.
This is because your initial choice has a 1/3 chance of being correct, meaning the car is behind the other two doors 2/3 of the time.
When Monty, who knows where the car is, opens one of the other doors to reveal a goat, he concentrates that 2/3 probability onto the single remaining unopened door. 
Here's why switching works:
Initial Choice: When you first pick a door, you have a 1/3 chance of selecting the car and a 2/3 chance of selecting a goat.
Monty's Action: Monty then opens one of the other two doors to reveal a goat. He always does this, and he never opens the door with the car.
The Power of Information:
If you picked the car initially (1/3 probability): Monty can open either of the other two doors, both of which have goats. If you switch, you will lose.
If you picked a goat initially (2/3 probability): The car is behind one of the other two doors. Since Monty must reveal a goat,
he is forced to open the other goat door, leaving the car behind the door you can switch to. If you switch, you will win. 
In summary: Since you have a 2/3 chance of picking a goat initially, you have a 2/3 chance of winning the car by switching doors.
The 1/3 chance of picking the car initially means you will lose by switching only 1/3 of the time. 
```

# Two-Door Riddle

```
To solve the two-door riddle, you must ask one guard,
"If I were to ask the other guard which door leads to freedom, which door would they point to?".
Both the truth-telling guard and the lying guard will indicate the same door – the one leading to death.
You should then choose the opposite door to find your freedom. 
Here's why this works:
If you ask the truth-teller:
They must truthfully tell you what the liar would say.
The liar would point to the death door, so the truth-teller will also point to the death door. 
If you ask the liar:
They must lie about what the truth-teller would say.
The truth-teller would point to the freedom door, so the liar will lie and point to the death door. 
In both scenarios, the door pointed to is the path to death, so you choose the other door to reach freedom. 
```

# Angle between 2 vectors (atan2)

- https://stackoverflow.com/questions/21483999/using-atan2-to-find-angle-between-two-vectors
- https://wumbo.net/formulas/angle-between-two-vectors-2d/
- https://gamedev.stackexchange.com/questions/203305/using-atan2-vs-dot-product-to-get-an-angle-in-2d-games
- https://maththebeautiful.com/angle-between-points/
- euclideanspace.com/maths/algebra/vectors/angleBetween/
- https://www.jwwalker.com/pages/angle-between-vectors.html

- https://github.com/alvmiller/vectors_angle

```c
/*
   We always can move vector's start dot to (0,0)
   and recalculate vector's coordinates
   
   A(Ax,Ay) B(Bx,By) -> V(Vx,Vy) : Vx=Bx-Ax Vy=By-Ay
   
   (v1,v2)=|v1||v2|cosL=v1x*v2x+v1y*v2y
   [v1,v2]=|v1||v2|sinL=v1x*v2y-v1y*v2x
   tgL=sinL/cosL=(v1x*v2y-v1y*v2x)/(v1x*v2x+v1y*v2y)=RES1/RES2
   L=atan2(RES1,RES2)
   
   Currently, each vector should be started from (0,0)
   
   reset; gcc -Werror vector00s_angle.c main.c -lm -o main; ./main
 */
```














