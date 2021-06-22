# Overview
My AI is controlled by a sequence of steps: projectile detection, global safespots update, and safespot selection.

## Projectile Detection
the AI will determine the time instant and location that a projectile will impact the ground. The vertical motion of projectile can be modelled as a constant-acceleration motion, with gravity as the only acceleration, which is in the same line as vertical velocity. My idea for calculating the time to impact $t$ is by first finding impact vertical velocity $v_{yt}$, which, by conservation of energy, has the relation as (assume g points downward)

$$ 0.5mv_{yt}^2 - 0.5mv_y^2 = - mgy$$

The above equation can be then rearranged as 

$$v_{yt}^2 = 2gy + v_y^2$$

We can then compute terminal speed
$$ v_{yt} = \sqrt{v_y^2 + 2gy}$$
, (negative since it is pointing downward). Then the time to impact is
$$ t = \frac{\Delta v_y}{g} = \frac{v_{yt} - v_y}{g}$$
Since in x-direciton, there is no acceleration, the impact location is then 
$$x_{impact} = v_x * t$$

## Safespots Update
I implemented a vector of integers to discretize the x axis, and initialize each cell as 0s. I set a time offset parameter and I will only consider the projectiles within that time offset. If a projectile will explode and affect a range of cells, I increment the cells' values by 1. I also implemented a vector of floats that stores the corresponding impact time of each cell.

## Safespot Selection
With a series of safespots at corresponding time periods, I select the safespot to go by comparing the minimum distances to go until reaching a safe spot. There are two directions to compare: left and right. If by going to the left, the player will take a longer distance to reach safe spot than going to the right, the player will go to the right by maximum speed in this time step, and vice versa.

# Challenges
The biggest challenge for this assignment is to understand the logic for updating parameters with time step. I had to fully understand that the controller only needs to control the motion in a single timestep of the game, then can I make the right execution decision. 

# Hard Scenario Performance
In hard scenario, my AI can survive for an average around 30 seconds. The decreased performance is because there are two enemies shooting at higher frequency. The safespots in each time step then will become fewer, and the distance to travel to a safespot will become larger. Since player's moving speed is limited, if the distance to travel gets too large, the player cannot travel fast enough to reach the safespot. 
# Thoughts
This assignment makes me realize the importance of code review and understanding the big software architecture of a project. Before writing my own code, the intuition for the logic behind how the code is built upon is crucial. 