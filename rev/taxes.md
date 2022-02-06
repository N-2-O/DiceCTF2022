# rev/taxes

In document DG1, the task is said to be split into 4 parts, each revealing a quarter of the flag. From the initial checks, we can work out easily that the flag contains ASCII values only.

## Part 1

In document DG4-A, the task is relatively straightforward XORing values together. The python script used to solve this part:

```python
string = ""
vals = [68, 105, 99, 101, 71, 97, 110, 103, 68, 97, 110, 99, 101, 71, 105, 103] #given key
xor_vals = [45, 7, 23, 86, 53, 15, 90, 11, 27, 19, 93, 21, 0, 41, 28, 2] #given ciphertext

for i in range(0, len(vals)):
	string += chr(vals[i] ^ xor_vals[i]) #XOR values and convert to ASCII representation

print(string)
```

The part of the flag is: `int3rn4l_r3venue`. This can be confirmed by determing the sha256 hash and comparing the one found in document DG1.

## Part 2

In document DG4-B, the task involves adding, ANDing, and multiplying. Analysing the steps reveal a repetitive nature so we can use loops and functions.

```python
#some values that are used repeatedly 
four = 1162037572
five = 1103515245
seven = 4294967295
nine = 12345
twelve = 2147483647
value = (four*five)&seven
hidden = 95 #ASCII underscore: random guess but most likely character to come after the part of the flag that we found
line = 0 #theres a value that is important that appears somewhere in the middle of calculations that need to be stored
mod1 = [] #important values that need to be stored to be used later
mod2 = []
mod3 = []
mod4 = []
mod5 = []
mod6 = []
mod7 = []
mod8 = []
mod9 = []
mod10 = []
mod11 = []
mod12 = []
mod13 = []
mod14 = []
mod15 = []
for i in range(0,65):
	value += nine
	value = value&twelve
	if i == 64:
		line = value
		#print("Line val:" + str(value))
	if i%16 == 0:
		hidden = hidden*value
		hidden = hidden&seven
		#print(hidden)
	elif i%16 == 1:
		mod1.append(value)
	elif i%16 == 2:
		mod2.append(value)
	elif i%16 == 3:
		mod3.append(value)
	elif i%16 == 4:
		mod4.append(value)
	elif i%16 == 5:
		mod5.append(value)
	elif i%16 == 6:
		mod6.append(value)
	elif i%16 == 7:
		mod7.append(value)
	elif i%16 == 8:
		mod8.append(value)
	elif i%16 == 9:
		mod9.append(value)
	elif i%16 == 10:
		mod10.append(value)
	elif i%16 == 11:
		mod11.append(value)
	elif i%16 == 12:
		mod12.append(value)
	elif i%16 == 13:
		mod13.append(value)
	elif i%16 == 14:
		mod14.append(value)
	elif i%16 == 15:
		mod15.append(value)
	value = value*five
	value = value&seven
	#print(value)

mods = []
mods.append(mod1)
mods.append(mod2)
mods.append(mod3)
mods.append(mod4)
mods.append(mod5)
mods.append(mod6)
mods.append(mod7)
mods.append(mod8)
mods.append(mod9)
mods.append(mod10)
mods.append(mod11)
mods.append(mod12)
mods.append(mod13)
mods.append(mod14)
mods.append(mod15)

target = [1799809632, 2410097199, 1862270976, 3128452582, 2326550240, 3466793581, 1785064448, 115003115, 3378973472, 3416715536, 2900951040, 2645089189, 1243834272, 2808954356, 272176128] #given target values

def brute_force_check(listVals, target, recurringVal):
	for index in range(35, 127): #try all ASCII values
		val = index
		for i in range(0, 4):
			val = val*listVals[i]
			val = val&seven
		#print("index val:\t" + str(val))
		value = recurringVal*five
		value = value&seven
		value += nine
		value = value&twelve
		recurringVal = value
		#print(value)
		value = value*val
		value = value&seven
		#print("Value with ascii: " str(value))
		if value == target:
			ascii = index
			print(chr(ascii))
			return recurringVal
	return 0

val = line
for i in range(0, 15):
	#print("mod " + str(i+1) + str(mods[i]) + "\ttarget:" + str(target[i]) + "\tvalue: " + str(value))
	val = brute_force_check(mods[i], target[i], val)
```

The part of the flag is: `_serv1ce_m0re_l1`. This can be confirmed by determing the sha256 hash and comparing the one found in document DG1.

## Part 3

This part of the flag most likely begins with the letter `k`
