# Animation_SnakeGameSimple
A simple automatic snake game animation on Python.
# 1. Main ideas
## 1.1. A classic, retro snake game we all played years ago
One of the earliest games in the history, Snake, just as simple as its name, is a game where you control an entity resembling a moving snake who leaves trail behind its path. There will be "preys" randomly appear on the arena, and you 1must hunt them as many as possible while skillfully avoid the arena border, as well as the snake’s own body. With this animation project, I plan to make a Snake game that:
- The arena is a two-dimensional space annotated with ’.’ symbols.
- The snake will have 2 parts: the head, annotated with ’A’, and the body, annotated with multiple ’1’. In this game, the snake is being controlled automatically by algorithm.
- The prey, annotated by ’P’, will be randomly generated anew each time the snake eats the older prey. The animation stops when snake finishes eating all MAX PREY preys.
## 1.2. Variables
### 1.2.1. Arena’s initial stats:
- ARENA_HEIGHT and ARENA_WIDTH: height and width of the arena respectively. Here I set them 10 each.
- arena: a two-dimensional list used to store data about the arena, the snake, and the prey. Initially arena is populated with None value.
### 1.2.2. Snake's initial stats:
- snake_length_init: initial length of the snake. Here I set it 4.
- snake_tail_init_x and snake_tail_init_y: initial coordinate of the "tail" - final block of snake’s body, from which I will build the whole snake. I set it on the final line, horizontally centered.
- snake_location: a list I used to store coordinates of all blocks of the snake. First element will store tail’s coordinate, all the way to final element, which stores head’s coordinate.
### 1.2.3. Prey's initial stats:
- prey_location_x and prey_location_y: storing coordinates of the preys throughout animation’s life cycle. Here I initially set it at the tail of snake so I can trigger "prey_check == False" and generate new prey. I will talk about prey_check later.
- MAX_PREY: required prey number to end the game. Here I set it 5.
- prey_count: the number of preys that snake has caught. Animation stops when MAX_PREY == prey_count.
Next, I will explain about the functions.
## 1.3. Functions
### 1.3.1. arena_generate():
Generate a blank arena, by populating arena with ’.’
### 1.3.2. snake_generate_origin():
Generate initial state of snake from initial coordinate of tail snake_tail_init_x and snake_tail_init_y. The symbol of head is ’A’, and body blocks (including tail) are ’1’. After marking snake’s blocks into arena, I will save their coordinates into snake_location as [x, y] type.
### 1.3.3. snake_generate():
Update snake’s location and state throughout animation’s life cycle.
### 1.3.4. prey_check(prey_location_x, prey_location_y):
Check if prey’s location falls into snake’s body or head in some random cases. The function returns "False" if it actually falls, and "True" if it’s not (prey’s location is legal).
### 1.3.5. prey_generate():
Randomly generate the prey, with location checked by prey check. As mentioned above, I initially set prey’s location at the tail of snake so this function, upon being called, will always trigger "prey_check == False" and will always generate a new prey.
### 1.3.6. print_arena():
Print out the arena with snake and prey data included.
### 1.3.7. snake_move(direction):
Simulate the snake’s crawling motion, with direction = {’N’, ’S’, ’W’, ’E’} representing {"North", "South", "West", "East"} respectively. After moving snake’s head to new block, if coordinates of head and prey are different (the snake is still on the way to its prey), this means length of snake is unchanged, and so remove the old tail (second-last body part next to the old tail is now the new tail). But when the snake eats its prey (head and prey have same coordinates), I will not remove the tail block, and so the length of snake will increase by 1.
### 1.3.8. snake_navigation():
This is the snake’s auto-navigation function towards the prey. I prioritize the directions in this order: N -> S -> E -> W. This means there are still some bugs, such as the snake can crawl over his body. I will try to fix it later.
# 2. Game cyle
## 2.1. Initialization
Firstly, I will generate initial state of animation in this order:
1. Generate data of initial arena, snake and prey with arena_generate(), snake_generate_origin() and prey_generate().
2. Print the arena data with print_arena(), along with a blank line and a title for aesthetic and clarification purposes.
3. Wait for 5 seconds before entering main animation loop.
## 2.2. Main animation loop
After finishing initialization, the animation loop begins. Condition of this loop is "the snake has not yet finished eating all required prey (prey_count < MAX_PREY)". Inside the loop, in order:
1. Navigate the snake and move towards the prey with snake_navigation(), then save its new location with snake_generate().
2. Wait for 1 second and then print the updated arena data with print_arena().
3. Then, the next loop will begin, and our snake gradually moves towards its prey. After the snake reaches its prey, I will increase number of preys caught by 1, and generate a new prey with prey_generate(). The whole prey-hunting process repeats, until our snake fills his belly with enough preys.
