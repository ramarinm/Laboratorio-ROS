# Laboratorio 3: Robótica de Desarrollo-Introducción a ROS
Integrantes: Fernando Cárdenas Acosta & Raúl Ignacio Marín Medina

## Objetivo del Laboratorio:
Este laboratorio tiene como objetivo principal el familiarizarse con el proceso de setup, uso e implementación de conceptos básicos en ROS (Robotic Operative System) así como el uso de comandos fundamentales de ROS en conjunto a diferentes softwares como MATLAB a través de conexión de nodos y el uso de Python para el movimiento interactivo de la tortuga del repositorio "Hello_Turtle". La tor

## TurtleSim en MATLAB:

Mov X
![WhatsApp Image 2024-04-14 at 23 26 21_cf6b3fe5](https://github.com/ramarinm/Laboratorio-ROS/assets/124843458/7366cb7e-41fd-41c3-a587-41ded4d33a13)

MovXY
![WhatsApp Image 2024-04-14 at 23 26 48_d03158db](https://github.com/ramarinm/Laboratorio-ROS/assets/124843458/6df1181e-be0e-4979-9a72-760b4fab492e)

MovXYGiro
![WhatsApp Image 2024-04-14 at 23 27 22_bfcefdc9](https://github.com/ramarinm/Laboratorio-ROS/assets/124843458/8eb321a2-f699-4861-96bc-b97120130cd9)

Pose
![WhatsApp Image 2024-04-14 at 23 28 06_bf186573](https://github.com/ramarinm/Laboratorio-ROS/assets/124843458/fc29c635-24ff-4577-92ab-714300c3631a)

Shutdown
![WhatsApp Image 2024-04-14 at 23 28 36_2c7d6ba7](https://github.com/ramarinm/Laboratorio-ROS/assets/124843458/a9355ff3-cb43-4c70-89af-1aa36e579704)

## Código Matlab turtlesim_1.m

%%
rosinit; %Conexi´on con nodo maestro
%%
velPub = rospublisher('/turtle1/cmd_vel','geometry_msgs/Twist'); %Creaci´on publicador
velMsg = rosmessage(velPub); %Creaci´on de mensaje
%%
velMsg.Linear.X = -1; %Valor del mensaje
send(velPub,velMsg); %Envio
pause(1)
%%
velMsg.Linear.Y = -1; %Valor del mensaje
send(velPub,velMsg); %Envio
pause(1)
%%
velMsg.Angular.Z = 1;
send(velPub,velMsg); %Envio
pause(2)
%% 
suscrPose = rossubscriber("/turtle1/pose",'turtlesim/Pose');
suscrPose.LatestMessage
%%
rosshutdown

## TurtleSim en Python: 

## Código Python para archivo myTeleopKey.py

#!/usr/bin/env 

import rospy
from geometry_msgs.msg import Twist
from turtlesim.srv import TeleportAbsolute, TeleportRelative
import termios, sys, os
from numpy import pi

TERMIOS = termios

def getkey():
    fd = sys.stdin.fileno()
    old = termios.tcgetattr(fd)
    new = termios.tcgetattr(fd)
    new[3] = new[3] & ~TERMIOS.ICANON & ~TERMIOS.ECHO
    new[6][TERMIOS.VMIN] = 1
    new[6][TERMIOS.VTIME] = 0
    termios.tcsetattr(fd, TERMIOS.TCSANOW, new)
    c = None
    try:
        c = os.read(fd, 1)
    finally:
        termios.tcsetattr(fd, TERMIOS.TCSAFLUSH, old)
    return c

def move():
    while not rospy.is_shutdown():
        c = getkey()
        if c=="w":
            rospy.init_node('robot_cleaner', anonymous=True)
            velocity_publisher = rospy.Publisher('/turtle1/cmd_vel', Twist, queue_size=10)
            vel_msg = Twist()
            vel_msg.linear.x = 1
            vel_msg.linear.y = 0
            vel_msg.linear.z = 0
            vel_msg.angular.x = 0
            vel_msg.angular.y = 0
            vel_msg.angular.z = 0
            velocity_publisher.publish(vel_msg)

print("Programa Fer y Raul")
move()
