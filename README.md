# 改进后的Turtle版本代码（直接复制到Replit）
import turtle
import random
import math

# 初始化
screen = turtle.Screen()
screen.setup(800, 600)
screen.bgcolor((100/255, 120/255, 50/255))
screen.title("保护色模拟 - Replit版")

# 捕食者（红色圆点）
predator = turtle.Turtle()
predator.shape("circle")
predator.color("red")
predator.shapesize(1.5)
predator.penup()

# 猎物类
class Prey:
    def __init__(self):
        self.x = random.randint(-350, 350)
        self.y = random.randint(-250, 250)
        self.color = (
            random.randint(50, 150)/255,
            random.randint(70, 140)/255,
            random.randint(40, 100)/255
        )
        self.survived = True
        self.t = turtle.Turtle()
        self.t.hideturtle()
    
    def draw(self):
        if self.survived:
            self.t.penup()
            self.t.goto(self.x, self.y)
            self.t.dot(10, self.color)
    
    def is_eaten(self, px, py):
        distance = math.sqrt((self.x - px)**2 + (self.y - py)**2)
        if distance < 30:
            self.survived = False
            self.t.clear()
            return True
        return False

# 主循环
prey_list = [Prey() for _ in range(20)]
generation = 0

def update_predator(x, y):
    predator.goto(x, y)

screen.onclick(update_predator)
screen.listen()

while True:
    for prey in prey_list:
        prey.draw()
        prey.is_eaten(predator.xcor(), predator.ycor())
    
    if all(not p.survived for p in prey_list):
        prey_list = [Prey() for _ in range(20)]
        generation += 1
    
    screen.update()
