import random
import time
import keyboard

from graphics import *

# configurations
WIDTH = 400
GRID_HEIGHT = WIDTH
HEIGHT = 470
TIMER = False
GAME = True
SCORE = 0
BONUS = 0
X = 70
Y = 30
RADIUS = 10
LENGTH = RADIUS * 2
PLAYER_LENGTH = 3
POISON_LENGTH = PLAYER_LENGTH
i = 0
k = 0
POINT_RADIUS = 5
POINTS = False
CHERRY_POINTS = False
KEY = "Right"
COUNTDOWN = 0

# set coordinations
C_X = 90
C_Y = 30
COORD_X = [10]
COORD_Y = [10]

while COORD_X[len(COORD_X) - 1] != (WIDTH - 10):
    C_X += 20
    COORD_X.append(C_X)

while COORD_Y[len(COORD_Y) - 1] != 390:
    C_Y += 20
    COORD_Y.append(C_Y)

RANDOM_X = random.choice(COORD_X)
RANDOM_Y = random.choice(COORD_Y)
CHERRY_RANDOM_X = random.choice(COORD_X)
CHERRY_RANDOM_Y = random.choice(COORD_Y)
POISON_RANDOM_X = random.choice(COORD_X)
POISON_RANDOM_Y = random.choice(COORD_Y)

#window setup
WINDOW = GraphWin("SNAKE", WIDTH, HEIGHT, autoflush=False)
WINDOW.setBackground(color_rgb(15, 15, 15))

# grid
LINE_X = 20
while LINE_X < WIDTH:
    GRID_X = Line(Point(LINE_X, 0), Point(LINE_X, GRID_HEIGHT))
    GRID_X.setOutline(color_rgb(25, 25, 25))
    GRID_X.draw(WINDOW)
    LINE_X += 20
LINE_Y = 20
while LINE_Y <= GRID_HEIGHT:
    GRID_X = Line(Point(0, LINE_Y), Point(WIDTH, LINE_Y))
    GRID_X.setOutline(color_rgb(25, 25, 25))
    GRID_X.draw(WINDOW)
    LINE_Y += 20

# snake banner
UI = Rectangle(Point(0, 400), Point(WIDTH, HEIGHT))
UI.setFill(color_rgb(102, 51, 0))
UI.setOutline(color_rgb(102, 51, 0))
UI.draw(WINDOW)
SNAKE_TITLE = Text(Point(WIDTH / 2, 420), "SNAKE")
SNAKE_TITLE.setTextColor("green")
SNAKE_TITLE.setSize(20)
SNAKE_TITLE.draw(WINDOW)
SCORE_TITLE = Text(Point(320, 424), "SCORE")
SCORE_TITLE.setTextColor("white")
SCORE_TITLE.setSize(10)
SCORE_TITLE.draw(WINDOW)
SCORE_UI = Text(Point(320, 435), SCORE)
SCORE_UI.setTextColor("white")
SCORE_UI.setSize(10)
SCORE_UI.draw(WINDOW)

# make player
PLAYER = {}
PLAYER[0] = Rectangle(Point(X - 20 - RADIUS, Y - RADIUS), Point(X - 20 + RADIUS, Y + RADIUS))
PLAYER[1] = Rectangle(Point(X - 40 - RADIUS, Y - RADIUS), Point(X - 40 + RADIUS, Y + RADIUS))
PLAYER[2] = Rectangle(Point(X - 60 - RADIUS, Y - RADIUS), Point(X - 60 + RADIUS, Y + RADIUS))

# make poison
POISON = {}

def main():    

    global TIMER, SCORE_UI, SCORE, BONUS, PLAYER_LENGTH, POISON_LENGTH, X, Y, POINTS, CHERRY_POINTS, RANDOM_X, RANDOM_Y, CHERRY_RANDOM_X, CHERRY_RANDOM_Y, POISON_RANDOM_X, POISON_RANDOM_Y, KEY, COUNTDOWN, k, GAME

    while GAME:
        # score update
        SCORE_UI.undraw()
        SCORE_UI = Text(Point(320, 435), SCORE)
        SCORE_UI.setTextColor("white")
        SCORE_UI.setSize(10)
        SCORE_UI.draw(WINDOW)

        # generating new body blocks
        if len(PLAYER) < PLAYER_LENGTH:
            i += 1
            PLAYER[i] = PLAYER[i - 1].clone()

        # body following player
        PLAYER[0].undraw()
        for i in range(1, len(PLAYER)):
            PLAYER[len(PLAYER) - i].undraw()
            PLAYER[len(PLAYER) - i] = PLAYER[len(PLAYER) - i - 1].clone()
            PLAYER[len(PLAYER) - i].draw(WINDOW)

        # update player's head coordinate
        PLAYER[0] = Rectangle(Point(X - RADIUS, Y - RADIUS), Point(X + RADIUS, Y + RADIUS))
        PLAYER[0].setFill("green")
        PLAYER[0].setWidth(2)
        PLAYER[0].draw(WINDOW)

        # player movement
        if keyboard.is_pressed("Up") and KEY != "Down":
            KEY = "Up"
        elif keyboard.is_pressed("Left") and KEY != "Right":
            KEY = "Left"
        elif keyboard.is_pressed("Down") and KEY != "Up":
            KEY = "Down"
        elif keyboard.is_pressed("Right") and KEY != "Left":
            KEY = "Right"
        if KEY == "Up":
            Y -= LENGTH
        elif KEY == "Left":
            X -= LENGTH
        elif KEY == "Down":
            Y += LENGTH
        elif KEY == "Right":
            X += LENGTH

        # point
        if not points: # generates new point when eaten
            point = Rectangle(Point(RANDOM_X - POINT_RADIUS, RANDOM_Y - POINT_RADIUS), Point(RANDOM_X + POINT_RADIUS, RANDOM_Y + POINT_RADIUS))
            point.setFill("white")
            point.setWidth(2)
            point.draw(WINDOW)
            points = True
        if PLAYER[0].getCenter().getX() == point.getCenter().getX() and PLAYER[0].getCenter().getY() == point.getCenter().getY(): # when player eats the point
            point.undraw()
            PLAYER_LENGTH += 1
            POISON_LENGTH += 1
            SCORE += 200 + BONUS
            RANDOM_X = random.choice(COORD_X)
            RANDOM_Y = random.choice(COORD_Y)
            for i, _ in enumerate(PLAYER):
                if (point.getCenter().getX() == PLAYER[i].getCenter().getX() and point.getCenter().getY() == PLAYER[i].getCenter().getY()) or (CHERRY_POINTS and cherry_point.getCenter().getX() == point.getCenter().getX() and cherry_point.getCenter().getY() == point.getCenter().getY()): # regenerate x and y coordinate if they share the same coordinate as player and cherry
                    RANDOM_X = random.choice(COORD_X)
                    RANDOM_Y = random.choice(COORD_Y)
            for i, _ in enumerate(POISON): # regenerate x and y coordinate if point shares the same coordinate to other array of poisons
                if point.getCenter().getX() == POISON[i].getCenter().getX() and point.getCenter().getY() == POISON[i].getCenter().getY():
                    CHERRY_RANDOM_X = random.choice(COORD_X)
                    CHERRY_RANDOM_Y = random.choice(COORD_Y)
            points = False

        # cherry
        if COUNTDOWN == 150:
            COUNTDOWN = 0
            if not CHERRY_POINTS: # generates new cherry from countdown
                cherry_point = Rectangle(Point(CHERRY_RANDOM_X - POINT_RADIUS, CHERRY_RANDOM_Y - POINT_RADIUS), Point(CHERRY_RANDOM_X + POINT_RADIUS, CHERRY_RANDOM_Y + POINT_RADIUS))
                cherry_point.setFill(color_rgb(213, 0, 50))
                cherry_point.setWidth(2)
                cherry_point.draw(WINDOW)
                CHERRY_POINTS = True
        if CHERRY_POINTS:
            for i in range(2, 6): # cherry blinks between countdown 40 to 100
                if COUNTDOWN == 20 * i:
                    cherry_point.undraw()
                elif COUNTDOWN == 10 + (20 * i):
                    cherry_point.draw(WINDOW)
            if COUNTDOWN >= 100: # when countdown becomes 100, remove cherry and reset count down
                CHERRY_POINTS = False
                COUNTDOWN = 0
                CHERRY_RANDOM_X = random.choice(COORD_X)
                CHERRY_RANDOM_Y = random.choice(COORD_Y)
        if CHERRY_POINTS and PLAYER[0].getCenter().getX() == cherry_point.getCenter().getX() and PLAYER[0].getCenter().getY() == cherry_point.getCenter().getY(): # when player eats the cherry
            cherry_point.undraw()
            SCORE += 500
            CHERRY_RANDOM_X = random.choice(COORD_X)
            CHERRY_RANDOM_Y = random.choice(COORD_Y)
            for i, _ in enumerate(PLAYER):
                if (cherry_point.getCenter().getX() == PLAYER[i].getCenter().getX() and cherry_point.getCenter().getY() == PLAYER[i].getCenter().getY()) or (cherry_point.getCenter().getX() == point.getCenter().getX() and cherry_point.getCenter().getY() == point.getCenter().getY()): # regenerate x and y coordinate if they share the same coordinate as player and point
                    CHERRY_RANDOM_X = random.choice(COORD_X)
                    CHERRY_RANDOM_Y = random.choice(COORD_Y)
            for i, _ in enumerate(POISON): # regenerate x and y coordinate if cherry shares the same coordinate to other array of poisons
                if cherry_point.getCenter().getX() == POISON[i].getCenter().getX() and cherry_point.getCenter().getY() == POISON[i].getCenter().getY():
                    CHERRY_RANDOM_X = random.choice(COORD_X)
                    CHERRY_RANDOM_Y = random.choice(COORD_Y)
            CHERRY_POINTS = False

        # poison
        if POISON_LENGTH % 5 == 0: # generates a poison block each time the player size reaches the multiple of 5
            POISON[k] = Rectangle(Point(POISON_RANDOM_X - POINT_RADIUS, POISON_RANDOM_Y - POINT_RADIUS), Point(POISON_RANDOM_X + POINT_RADIUS, POISON_RANDOM_Y + POINT_RADIUS))
            POISON[k].setFill("green")
            POISON[k].setWidth(2)
            POISON[k].draw(WINDOW)
            POISON_RANDOM_X = random.choice(COORD_X)
            POISON_RANDOM_Y = random.choice(COORD_Y)
            for i, _ in enumerate(PLAYER):
                if POISON[k].getCenter().getX() == PLAYER[i].getCenter().getX() and POISON[k].getCenter().getY() == PLAYER[i].getCenter().getY() or (POISON[k].getCenter().getX() == point.getCenter().getX() and POISON[k].getCenter().getY() == point.getCenter().getY()) or (CHERRY_POINTS and POISON[k].getCenter().getX() == cherry_point.getCenter().getX() and POISON[k].getCenter().getY() == cherry_point.getCenter().getY()): # regenerate x and y coordinate if they share the same coordinate as player and point and cherry
                    POISON_RANDOM_X = random.choice(COORD_X)
                    POISON_RANDOM_Y = random.choice(COORD_Y)
            for i, _ in enumerate(POISON):
                if POISON[k].getCenter().getX() == POISON[i].getCenter().getX() and POISON[k].getCenter().getY() == POISON[i].getCenter().getY(): # regenerate x and y coordinate if new poison shares the same coordinate to other array of poisons
                    POISON_RANDOM_X = random.choice(COORD_X)
                    POISON_RANDOM_Y = random.choice(COORD_Y)
            BONUS += 50
            k += 1
            POISON_LENGTH += 1

        # game over requirements
        for i, _ in enumerate(POISON): # if player touches poison
            if PLAYER[0].getCenter().getX() == POISON[i].getCenter().getX() and PLAYER[0].getCenter().getY() == POISON[i].getCenter().getY():
                GAME = False
        for i in range(2, len(PLAYER)): # if player touches its own body or reach out of window
            if PLAYER[0].getCenter().getX() == PLAYER[i].getCenter().getX() and PLAYER[0].getCenter().getY() == PLAYER[i].getCenter().getY() or X < 0 or X > WIDTH or Y < 0 or Y > GRID_HEIGHT:
                GAME = False

        # FPS
        update(10)
        COUNTDOWN += 1


    # GAME OVER
    game_over = Text(Point(WIDTH / 2, 200), "GAME OVER")
    game_over.setTextColor("red")
    game_over.setSize(30)
    game_over.draw(WINDOW)
    update()
    time.sleep(2)
    WINDOW.close()

main()
