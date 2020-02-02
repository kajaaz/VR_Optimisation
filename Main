from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
import numpy as np
from itertools import product, combinations
import math

import serial
from drawnow import *
import atexit

plt.ion() #dire à matplotlib de travailler en mode interactif

x0=1
y0=1
z0=1

serialArduino = serial.Serial('com4', 115200)
fig = plt.figure()


def makefig():
    ax = fig.gca(projection='3d')
    ax.set_aspect("equal")
    ax.scatter(0, 0, 0, color="y", s=500)

    # draw sphere
    u, v = np.mgrid[0:2*np.pi:20j, 0:np.pi:10j]
    x = np.cos(u)*np.sin(v)
    y = np.sin(u)*np.sin(v)
    z = np.cos(v)
    ax.plot_wireframe(x, y, z, color="k")
    
    # draw a point
    ax.scatter([xn], [yn], [zn], color="r", s=100)



while True:
    while (serialArduino.inWaiting()==0):
        pass

    arduinoString = serialArduino.readline().decode('utf-8')
    
    dataArray = arduinoString.strip().split(',')
    
    L = [] #créer une liste qui contient un nombre raisonnable de points à tracer
    
    if len(dataArray) >= 3:
        
        pitch = float( dataArray[0] )
        roll = float( dataArray[1] )
        yaw = float( dataArray[2] )
        dataFloat = [ pitch, roll, yaw ]
        
        x3 = x0*np.cos(pitch)*np.cos(roll)+y0*np.sin(pitch)*np.cos(roll)-z0*np.sin(roll)
        
        y3 =x0*(np.cos(pitch)*np.sin(roll)*np.sin(yaw)-np.sin(pitch)*np.cos(yaw))+y0*(np.sin(pitch)*np.sin(roll)*np.sin(yaw)+np.cos(pitch)*np.cos(yaw))+z0*np.cos(roll)*np.cos(yaw)
        
        z3 =x0*(np.cos(pitch)*np.sin(roll)*np.cos(yaw)+np.sin(pitch)*np.sin(yaw))+y0*(np.sin(pitch)*np.sin(roll)*np.cos(yaw)-np.cos(pitch)*np.sin(yaw))+z0*(np.cos(roll)*np.cos(yaw))
        
        norme = math.sqrt(x3**2+y3**2+z3**2)
        
        xn=x3/norme
        yn=y3/norme
        zn=z3/norme
        
        drawnow(makefig)
        plt.pause(.000000001)
        

# memo couleur : c=bleu ciel , m=mauve , y=yellow , k=noir , w=white
