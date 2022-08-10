from email import message
from os import fspath
from pickle import BYTEARRAY8
import can 

def sendingmessage():
	while True:
		op1 = input("Choose the operation\n1.Hazard Flasher on\n2.Hazard Flasher off\nSelection:")
		if (op1 == '0'):
			break
		if (op1 == '1'):
			bus = can.interface.Bus(bustype='socketcan', channel='vcan0', bitrate=250000)
			msg = can.Message(arbitration_id=0x01, data=[0,25,0,1,3,1,4,1], is_extended_id=False)
			data = list(msg.data)
			print(data)
			print("Hazard flasher is on.")
			try:
				bus.send(msg)	
			except can.CanError:
				print ("Message not sent")
		elif(op1 == '2'):
			bus = can.interface.Bus(bustype='socketcan', channel='vcan0', bitrate=250000)
			msg = can.Message(arbitration_id=0xc0ffee, data=[0,25,0,1,3,1,4,1], is_extended_id=False)
			print("Hazard flasher off.")
			try:
				bus.send(msg)
			except can.CanError:
				print ("Message not sent")
		elif(op1 == '3'):
			id = input("id:")
			for i in range (0,9,1):
				data1 = list()
				datas = input("\nEnter data:")
				data1.append(datas)
			print(data1)
			bus = can.interface.Bus(bustype='socketcan', channel='vcan0', bitrate=250000)
			msg = can.Message(arbitration_id=id, data=data1, is_extended_id=False)
			try:
				bus.send(msg)	
			except can.CanError:
				print ("Message not sent")

def receivingmessage():
	
	bus = can.interface.Bus(bustype='socketcan', channel='vcan0', bitrate=250000)
	while True:
	
			message = bus.recv()
			print(message)
			id = message.arbitration_id
			data = list(message.data)
			if (data == []):
				print("data yok")
				print(message)
				continue
			if (id == 399):
				if(data[0] == 238 and data[1] == 233 and data[2] == 39):
					rpm = (data[6]+ (data[5]*256))*0.125
					print(rpm,"rpm")
					
				

		
op = input("Enter the operation:\n 1-Sending message \n 2-Receiving message\n")

if (op == '1'):
	sendingmessage()
elif(op == '2'):
	receivingmessage()
