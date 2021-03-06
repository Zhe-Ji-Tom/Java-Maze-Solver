# Java-Maze-Solver
This program is a console-based maze solving in Java with BFS, DFS, A*.




Maze structure
======
In this implementation, Mazes consists in a matrix of Squares.

Here is the orthogonal reprensentation of a Maze:
```
o---> X [Columns]
|
v
Y [Lines]
```




Solve mazes
======

Firstly, see how to [load a maze](#load-a-maze) from a .txt file or [create one](#create-a-maze) directly from code.

Next, refer to "[Use a solver](#use-a-solver)" to begin solving when your Maze is all set.

Like any solver, you can also [set your own shift order](#changing-the-shift-order). This program use cardinals as reference and the default one is North-East-South-West.




Load a maze
------
You can load a maze from a .txt as an argument in Maze class constructor, like this:

```Java
Maze myMaze = new Maze("./path/to/my/maze/myMaze.txt");
```
[Example in Main.java line 25][1]

The file must be written within this form :

```
-----------------e-
--xxxxxxxxxxxx-----
-------------x-----
----s--------x-----
-------------x-----
-------------------
-------------------
```
The number of characters in the first line will be the maze number of columns and the number of lines... the number of lines.

Every character is a square of the maze and is read like this:
* `s` : maze starting point
* `e` : maze ending point
* `x` : wall
* Other ones (whatever they are) are empty squares.





Create a maze
------
It's also possible to create a maze directly from code by following those steps, but this is the most laborious method.

__1. In Main.java, create a starting and a ending square:__
```Java
Square start = new Square(1, 0, "S");
Square end = new Square(3, 4, "E");
```
Square constructor arguments: `Line (int), Column (int), Identifier (String)`

`S` stands for starting point and `E` for ending point.

__2. Create a new Maze:__
```Java
Maze myMaze = new Maze(6, 5, start, end);
```
Maze constructor arguments : `Number of lines (int), Number of columns (int), Starting point (Square), Ending point (Square)`

__3. Create walls:__

(If you don't want walls, you can skip this step)

Use the `setMazeWall()` method:
```Java
myMaze.setMazeWall(2, 0);
myMaze.setMazeWall(3, 0);
myMaze.setMazeWall(1, 2);
myMaze.setMazeWall(2, 2);
...
```
Arguments: `Wall line pos (int), Wall column pos (int)`





Use a solver
------
You have 3 solvers available, each one corresponding to one algorithm:
* AStarSolver
* BFS_Solver
* DFS_Solver

Simply use
```Java
BFS_Solver bfsSolver = new BFS_Solver(myMaze);
DFS_Solver dfsSolver = new DFS_Solver(myMaze);
AStarSolver aStarSolver = new AStarSolver(myMaze, true);

System.out.println(bfsSolver.solve());
System.out.println(dfsSolver.solve());
System.out.println(aStarSolver.solve());
```

Note that `AStarSolver` takes one other boolean argument to specifies if you want your maze to be solved by A* with Manhattan or Euclidean heuristic.

Set to `true` to use Manhattan heuristic. Euclidean heuristic will be used otherwhise.




Changing the shift order
------
Shift order is defined with a char vertor of size 4, each letter corresponding to a cardinal

Here's an example to set the order West-East-North-South
```Java
char[] order = {'W', 'E', 'N', 'S'};
myMaze.setOrder(order);
```




Example of resolution : A* + Euclidean heuristic
=====
```
Path: [2, 12](23.0)(End) <- [1, 12](23.0) <- [1, 11](22.414213562373096) <- [1, 10](22.23606797749979) <- [2, 10](21.0) <- [2, 9](21.0) <- [2, 8](21.0) <- [1, 8](20.12310562561766) <- [0, 8](19.47213595499958) <- [0, 7](19.385164807134505) <- [0, 6](19.32455532033676) <- [1, 6](18.08276253029822) <- [2, 6](17.0) <- [3, 6](16.08276253029822) <- [3, 5](16.071067811865476) <- [3, 4](16.06225774829855) <- [4, 4](15.246211251235321) <- [5, 4](14.54400374531753) <- [6, 4](13.94427190999916) <- [6, 3](13.848857801796104) <- [6, 2](13.770329614269007) <- [6, 1](13.704699910719626) <- [6, 0](13.649110640673518) <- [7, 0](13.0)(Start) 
Path length: 23
Number of nodes created: 112
Execution time: 0.001 seconds
     0   1   2   3   4   5   6   7   8   9   10  11  12
   ╔═══╤═══╤═══╤═══╤═══╤═══╤═══════════╤═══╤═══╤═══╤═══╗
0  ║   │   │   │   │   │   │ *   *   * │   │   │   │   ║
   ╟───┴───┼───┤   └───┴───┘   ┌───┐   ├───┼───┴───┴───╢
1  ║       │   │             * │   │ * │   │ *   *   * ║
   ╟───┐   ├───┼───┬───┬───┐   ├───┤   └───┘   ┌───┐   ║
2  ║   │   │   │   │   │   │ * │   │ *   *   * │   │ o ║
   ╟───┤   └───┼───┼───┴───┘   ├───┤   ┌───┬───┼───┼───╢
3  ║   │       │   │ *   *   * │   │   │   │   │   │   ║
   ╟───┼───┐   └───┘   ┌───┐   ├───┤   └───┴───┴───┼───╢
4  ║   │   │         * │   │   │   │               │   ║
   ╟───┼───┼───┬───┐   └───┘   ├───┼───┐   ┌───┬───┼───╢
5  ║   │   │   │   │ *         │   │   │   │   │   │   ║
   ╟───┴───┴───┴───┘   ┌───┬───┼───┼───┘   └───┴───┼───╢
6  ║ *   *   *   *   * │   │   │   │               │   ║
   ║   ┌───┐   ┌───┐   └───┴───┼───┤   ┌───┐       ├───╢
7  ║ S │   │   │   │           │   │   │   │       │   ║
   ╟───┼───┤   ├───┤   ┌───┐   └───┘   └───┘       └───╢
8  ║   │   │   │   │   │   │                           ║
   ╟───┴───┘   └───┘   └───┘   ┌───┬───┐   ┌───┬───┬───╢
9  ║                           │   │   │   │   │   │   ║
   ║   ┌───┬───┐   ┌───┬───┐   └───┴───┘   └───┴───┴───╢
10 ║   │   │   │   │   │   │                           ║
   ║   └───┼───┼───┼───┼───┤   ┌───┬───┬───┬───┬───┬───╢
11 ║       │   │   │   │   │   │   │   │   │   │   │   ║
   ╚═══════╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╧═══╝

```




Class diagram
======


![alt image](https://i.imgur.com/Zz69FIL.png)


About the author
======
My name is Gabriel, I'm a french student in Video Games Development in Canada







[1]:https://github.com/Gaderr/Maze_solver/blob/a6ac782c316adee15d8029a12531a7fcd5f659ef/src/Main.java#L25
