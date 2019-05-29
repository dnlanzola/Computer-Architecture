.data
welcome:		.asciiz	"*****************************\n*** WELCOME TO PHOTOMIPS! ***\n*****************************\n"
inputQuestion:		.asciiz	"\nInput file name: "
outputQuestion:		.asciiz "Output file name: "
menu0:			.asciiz "Select a Filter Category:\n"
# BLUR MENU
menu1:			.asciiz "[1] Motion Blur.\n"
menuSub10:		.asciiz "\nSelect intensity:\n[1] Low.\n[2] Medium.\n[3] High.\n"

# COLORIZE MENU
menu2:			.asciiz "[2] Colorize.\n"
menuSub20:		.asciiz "\nSelect color:\n[1] Red.\n[2] Yellow.\n[3] Green.\n[4] Blue.\n[5] Pink.\n"

# FILTER NAME MENU
menu3:			.asciiz "[3] Quantizing.\n"
menuSub30:		.asciiz "\nSelect number of bits:\n[1] 128bit.\n[2] 16-bit.\n[3] 8-bit.\n"

# GAUSSIAN BLUR MENU
menu7:			.asciiz "[4] Hor. Blur.\n"
#menuSub70:		.asciiz ""

#EXIT
menu4:			.asciiz "[0] Exit.\n\n"
menu5:			.asciiz "[0] Previous menu.\n\n"
menu6:			.asciiz "Selection >> "

newLine:		.asciiz "\n"

imageName:		.space 128 		# NAME OF INPUT FILE
outputFileName:		.space 128 		# NAME OF OUTPUT FILE
fileHeader: 		.space 54 		# BITMAP HEADER INFO
bufferInput: 		.space 2		# BUFFER FOR MENU SELECTION


.text
main: 
	li $v0, 4			
	la $a0, welcome		
	syscall					# PRINT WELCOME MESSAGE
	
	li $v0, 4			
	la $a0, inputQuestion	
	syscall					# PRINT INPUT QUESTION

	li $v0, 8			
	la $a0, imageName	
	li $a1, 128				# READ INPUT FILE NAME (UP TO 256 CHARACTERS)
	syscall
	
	li $v0, 4			
	la $a0, outputQuestion	
	syscall					# PRINT OUTPUT QUESTION
	
	li $v0, 8			
	la $a0, outputFileName	
	li $a1, 128		
	syscall					# READ OUTPUT FILE NAME (UP TO 256 CHARACTERS)
	
	li $s0, 0       				# SET INDEX TO ZERO

removeNewLineInput:
	lb $a3, imageName($s0)    		# LOAD CHAR AT INDEX
	addi $s0, $s0, 1      			# INDEX = INDEX + 1
	bnez $a3, removeNewLineInput     	# LOOP UNTIL END OF STRING REACHED
	beq $a1, $s0, removeNewLineOutput 	# DO NOT REMOVE \n IF STRING = MAX SIZE
    
	subiu $s0, $s0, 2     			# If above not true, Backtrack index to '\n'
	sb $0, imageName($s0)    		# Add the terminating character in its place

removeNewLineOutput:
	lb $a3, outputFileName($s0)    		# LOAD CHAR AT INDEX
	addi $s0, $s0, 1      			# INDEX = INDEX + 1
	bnez $a3, removeNewLineOutput  		# LOOP UNTIL END OF STRING REACHED
	beq $a1, $s0, readImage			# DO NOT REMOVE \n IF STRING = MAX SIZE
    
	subiu $s0, $s0, 2     			
	sb $0, outputFileName($s0)    		


readImage:
	li $v0, 13				
	la $a0, imageName			# OPEN FILE -> NAME = $a0 and loads name into $a0
	li $a1, 0				
	li $a2, 0				
	syscall
	move $s0, $v0					

	li $v0, 14				# READ INPUT FILE
	move $a0, $s0				
	la $a1, fileHeader			# STORE DATA
	li $a2, 54				# READS THE BMP HEADER
	syscall
	
	lw $s7, fileHeader+18			# SAVES WIDTH
	
	lw $s4, fileHeader+22			# SAVES HEIGHT
	
	lw $s1, fileHeader+34			# SAVES SIZE OF IMAGE

	li $v0, 9				# ALLOCATE MEMORY FOR 3 ELEMENTS (R,G,B)
	move $a0, $s1				
	syscall
	move $s2, $v0				# STORE ARRAY IN $s2
	
	li $v0, 14				# READ FILE INFORMATION
	move $a0, $s0			
	move $a1, $s2			
	move $a2, $s1		
	syscall
	

	move $a0, $s0				
	li $v0, 16				# CLOSE FILE
	syscall
	la $s3, bufferInput
	
menuSelection:
	la $a0 newLine	
	li $v0, 4		
	syscall 

	li $v0, 4			
	la $a0, menu0	
	syscall					# SELECT CATEGORY

	li $v0, 4			
	la $a0, menu1	
	syscall					# MOTION BLUR

	li $v0, 4			
	la $a0, menu2	
	syscall					# COLORIZE
	
	li $v0, 4			
	la $a0, menu3	
	syscall					# QUANTIZING
	
	li $v0, 4			
	la $a0, menu7	
	syscall					# GAUSSIAN BLUR
	
	li $v0, 4			
	la $a0, menu4	
	syscall					# EXIT
	
	li $v0, 4			
	la $a0, menu6	
	syscall					# SELECTION
	
	li $v0, 8
	la $a0, bufferInput
	li $a1, 2
	move $t7, $a0
	syscall
	
	lb $t6, 0($t7)	
	beq $t6, 49, subMenuBlur		# 1
	beq $t6, 50, subMenuColorize		# 2
	beq $t6, 51, subMenuQuantizing		# 3
	beq $t6, 52, gaussian    		# 4
	beq $t6, 48, end			# 0	

subMenuBlur:
	la $a0 newLine	
	li $v0, 4		
	syscall 
	
	li $v0, 4			
	la $a0, menuSub10	
	syscall				

	li $v0, 4			
	la $a0, menu5	
	syscall					# PREVIOUS MENU

	li $v0, 4			
	la $a0, menu6	
	syscall					# SELECTION
	
	li $v0, 8
	la $a0, bufferInput
	li $a1, 2
	move $t7, $a0
	syscall
	
	lb $t6, 0($t7)	
	beq $t6, 49, blurLow			# 1
	beq $t6, 50, blurMed			# 2
	beq $t6, 51, blurHigh			# 3
	beq $t6, 48, menuSelection		# 0
	

subMenuColorize:
	la $a0 newLine	
	li $v0, 4		
	syscall 
	
	li $v0, 4			
	la $a0, menuSub20	
	syscall	
	
	li $v0, 4			
	la $a0, menu5	
	syscall					# PREVIOUS MENU

	li $v0, 4			
	la $a0, menu6	
	syscall					# SELECTION
	
	
	li $v0, 8
	la $a0, bufferInput
	li $a1, 2
	move $t7, $a0
	syscall
	
	lb $t6, 0($t7)	
	beq $t6, 49, red			# 1
	beq $t6, 50, yellow			# 2
	beq $t6, 51, green			# 3
	beq $t6, 52, blue			# 4
	beq $t6, 53, pink			# 5
	beq $t6, 48, menuSelection		# 0
		
subMenuQuantizing:
	la $a0 newLine	
	li $v0, 4		
	syscall
 
	li $v0, 4			
	la $a0, menuSub30	
	syscall	
			

	li $v0, 4			
	la $a0, menu5	
	syscall					# PREVIOUS MENU

	li $v0, 4			
	la $a0, menu6	
	syscall					# SELECTION
	
	li $v0, 8
	la $a0, bufferInput
	li $a1, 2
	move $t7, $a0
	syscall
	
	lb $t6, 0($t7)	
	beq $t6, 49, one28			# 1
	beq $t6, 50, sixteen			# 2
	beq $t6, 51, eight			# 3
	beq $t6, 48, menuSelection		# 0


#################################################


#########blurLow############

		
	
blurLow:
		move $t6, $s2 			# LOAD IMAGE
		move $t4, $zero
	
		addi $t8, $zero, 30		# NUMBER OF PIXELS TO COPY OVER	
		j loadNewPixel

#########blurMed############
		
blurMed:
		move $t6, $s2 			# LOAD IMAGE
		move $t4, $zero
	
		addi $t8, $zero, 60		# NUMBER OF PIXELS TO COPY OVER			
		j loadNewPixel

#########blurHigh############
blurHigh:
		move $t6, $s2			#LOAD IMAGE
		move $t4, $zero
	
		addi $t8, $zero, 90		# NUMBER OF PIXELS TO COPY OVER	

					
loadNewPixel:	add $t7, $zero, $zero		# RESET COUNTER
						
		lb $t0, 0($t6)			# LOAD PIXEL
		lb $t1, 1($t6)
		lb $t2, 2($t6)
		
		sb $t0, 0($s3) 			# LOAD PIXEL
		sb $t1, 1($s3) 
		sb $t2, 2($s3) 
		
		addi $t4, $t4, 3
		bge $t4, $s1, writeImage
				
		add $t6, $t6, 3			# MOVE TO THE NEXT PIXEL (original array)
		add $s3, $s3, 3			# MOVE TO THE NEXT PIXEL (new array)
			
storePixel:	
		sb $t0, 0($s3) 			# COPY PIXEL (to the new array)
		sb $t1, 1($s3) 
		sb $t2, 2($s3) 
		
		addi $t4, $t4, 3
		bge $t4, $s1, writeImage
		
		addi $t7, $t7, 1		# INCREMENT COUNTER
	
		add $t6, $t6, 3			# MOVE TO THE NEXT PIXEL (original array)
		add $s3, $s3, 3			# MOVE TO THE NEXT PIXEL (new array)
		
		slt $t9, $t7, $t8
		bne $t9, $zero, storePixel
		
		j loadNewPixel
		
		

##########RED#########	
red:
	move $t6, $s2			# LOAD IMAGE
	move $t4, $zero

red_loop:
	li $t7, 0
	li $t8, 0
	li $t9, 255
	
					# LOAD RGB VALUES
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)
	
	div $t0, $t9
	mfhi $t0
	
					# STORE RGB VALUES
	sb $t7, 0($s3) #B
	sb $t8, 1($s3) #G
	sb $t0, 2($s3) #R
		
	addi $t4, $t4, 3		# INCREMENT COUNTER

					
	bge $t4, $s1, writeImage	# IF END OF ARRAY, ENDS LOOP
	add $t6, $t6, 3
	add $s3, $s3, 3
	
	j red_loop

		
yellow:
	move $t6, $s2			# LOAD IMAGE
	move $t4, $zero
	
yellow_loop:

	li $t7, 0
	li $t8, 255
	li $t9, 255
	
					# LOAD RGB VALUES
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)
	
	div $t1, $t8
	mfhi $t0
	
	div $t2, $t9
	mfhi $t1
	
					# STORE RGB VALUES
	sb $t7, 0($s3) #B
	sb $t0, 1($s3) #G
	sb $t1, 2($s3) #R
	
					# INCREMENT COUNTER	
	addi $t4, $t4, 3

					# IF END OF ARRAY, ENDS LOOP
	bge $t4, $s1, writeImage
	add $t6, $t6, 3
	add $s3, $s3, 3

	j yellow_loop	

##########GREEN#########
	
green:
	move $t6, $s2			# LOAD IMAGE
	move $t4, $zero

green_loop:

	li $t7, 0
	li $t8, 128
	li $t9, 0
	
					# LOAD RGB VALUES
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)
	
	div $t1, $t8
	mfhi $t0
	
					# STORE RGB VALUES
	sb $t7, 0($s3) #B
	sb $t0, 1($s3) #G
	sb $t9, 2($s3) #R
	
					# INCREMENT COUNTER	
	addi $t4, $t4, 3

					# IF END OF ARRAY, ENDS LOOP
	bge $t4, $s1, writeImage
	add $t6, $t6, 3
	add $s3, $s3, 3
	

	j green_loop

##########Blue#########	
	
blue:
	move $t6, $s2			# LOAD IMAGE
	move $t4, $zero
	
blue_loop:

	li $t7, 255
	li $t8, 0
	li $t9, 0
	
					# LOAD RGB VALUES
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)
	
	div $t0, $t7
	mfhi $t0
	
					# STORE RGB VALUES
	sb $t0, 0($s3) #B
	sb $t8, 1($s3) #G
	sb $t9, 2($s3) #R
	
					# INCREMENT COUNTER	
	addi $t4, $t4, 3

					# IF END OF ARRAY, ENDS LOOP
	bge $t4, $s1, writeImage
	add $t6, $t6, 3
	add $s3, $s3, 3
	

	j blue_loop

##########Pink#########	
pink:
	move $t6, $s2			# LOAD IMAGE
	move $t4, $zero

pink_loop:

	li $t7, 180
	li $t8, 0
	li $t9, 255
	
					# LOAD RGB VALUES
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)

	# RED DIV 255
	div $t0, $t9
	mfhi $t2	
		
	
	# BLUE DIV 203
	div $t2, $t7
	mfhi $t0
	
	
	
					# STORE RGB VALUES
	sb $t2, 0($s3) #B
	sb $t8, 1($s3) #G
	sb $t0, 2($s3) #R
	
					# INCREMENT COUNTER	
	addi $t4, $t4, 3

					# IF END OF ARRAY, ENDS LOOP
	bge $t4, $s1, writeImage
	add $t6, $t6, 3
	add $s3, $s3, 3
	

	j pink_loop

#################################################
			
			
gaussian:
	move $t6, $s2			# LOAD IMAGE
	move $t4, $zero
	
	# Load RGB values of cell
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)
	
	add $t7, $t0, 0
	add $t8, $t1, 0
	add $t9, $t2, 0
	
	add $t6, $t6, 3 #move one cell
	
	# Load RGB values of cell
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)	
	
	add $t7, $t0, $t7
	add $t8, $t1, $t8
	add $t9, $t2, $t9
	
	div $t7, $t7, 2
	div $t8, $t8, 2
	div $t9, $t9, 2
	
	#store location of s3
	add $s6, $s3, 0
	
	# copy pixel (to the new array)
	sb $t7, 0($s3) 
	sb $t8, 1($s3) 
	sb $t9, 2($s3)
	
	add $s3, $s3, 3 #move one cell forward
	#increment counters to use next pixel	
	addi $t4, $t4, 3
	
gaussianLoop:

	#if we reach the end of the array, exit
	bge $t4, $s1, writeImage

	# Load RGB values of cell
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)
	
	add $t7, $t0, 0
	add $t8, $t1, 0
	add $t9, $t2, 0
	
	add $t6, $t6, 3 #move one cell
	
	# Load RGB values of cell
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)	
	
	add $t7, $t0, $t7
	add $t8, $t1, $t8
	add $t9, $t2, $t9
	
	add $t6, $t6, 3 #move one cell
	
	# Load RGB values of cell
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)	
	
	add $t7, $t0, $t7
	add $t8, $t1, $t8
	add $t9, $t2, $t9
	
	div $t7, $t7, 3
	div $t8, $t8, 3
	div $t9, $t9, 3
	
	# copy pixel (to the new array)
	sb $t7, 0($s3) 
	sb $t8, 1($s3) 
	sb $t9, 2($s3) 

	sub $t6, $t6, 3 #move one cell back
	add $s3, $s3, 3 #move one cell forward
	#increment counters to use next pixel	
	addi $t4, $t4, 3
	
	j gaussianLoop
		
		
#################################################
# thirtyTwoBit loop
one28:
	move $t6, $s2	#load the image
	move $t4, $zero

one28_loop:
	li $t7, 0
	li $t8, 255
	
	# Load RGB values
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)
	 
######Calculate new RGB Values########
	divu $t0, $t0, 2
	#addi $t0, $t0, 100
	
	divu $t1, $t1, 2
	#addi $t1, $t1, 100
	
	divu $t2, $t2, 2
	#addi $t1, $t1, 100
	
	#Store rgb values in the data buffer
	sb $t0, 0($s3) #B
	sb $t1, 1($s3) #G
	sb $t2, 2($s3) #R
	
	
	#increment counters to use next pixel	
	addi $t4, $t4, 3

	#if we reach the end of the array, exit
	bge $t4, $s1, writeImage
	add $t6, $t6, 3
	add $s3, $s3, 3
	
	#else jump to start of the loop
	j one28_loop

#################################################
# sixteenBit loop
sixteen:
	move $t6, $s2	#load the image
	move $t4, $zero

sixteen_loop:
	
	# Load RGB values
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)
	
	
######Calculate new RGB Values########
	divu $t0, $t0, 8
	#addi $t0, $t0, 100
	
	divu $t1, $t1, 8
	#addi $t1, $t1, 100
	
	divu $t2, $t2, 8
	#addi $t1, $t1, 100
	
	#Store rgb values in the data buffer
	sb $t0, 0($s3) #B
	sb $t1, 1($s3) #G
	sb $t2, 2($s3) #R
	
	
	#increment counters to use next pixel	
	addi $t4, $t4, 3

	#if we reach the end of the array, exit
	bge $t4, $s1, writeImage
	add $t6, $t6, 3
	add $s3, $s3, 3
	
	#else jump to start of the loop
	j sixteen_loop

#################################################
# eightBit loop
eight:
	move $t6, $s2	#load the image
	move $t4, $zero

eight_loop:
	li $t7, 0
	li $t8, 255
	
	# Load RGB values
	lb $t0, 0($t6)
	lb $t1, 1($t6)
	lb $t2, 2($t6)
	
	
######Calculate new RGB Values########
	divu $t0, $t0, 32	
	#addi $t0, $t0, 100
	
	divu $t1, $t1, 32
	#addi $t1, $t1, 100
	
	divu $t2, $t2, 32
	#addi $t1, $t1, 100

	#Store rgb values in the data buffer
	sb $t0, 0($s3) #B
	sb $t1, 1($s3) #G
	sb $t2, 2($s3) #R
	
	
	#increment counters to use next pixel	
	addi $t4, $t4, 3

	#if we reach the end of the array, exit
	bge $t4, $s1, writeImage
	add $t6, $t6, 3
	add $s3, $s3, 3
	
	#else jump to start of the loop
	#else jump to start of the loop
	j eight_loop

#####################################################################################


writeImage:		
	li $v0, 13			# OPEN FILE
	la $a0, outputFileName
	li $a1, 1	
	li $a2, 0
	syscall
	move	$t1, $v0
	
	li $v0, 15			# PREPARE DATA TO BE WRITTEN
	move $a0, $t1
	la $a1, fileHeader
	addi $a2, $zero,54
	syscall
	
	li $v0, 15			# WRITE OUTPUT FILE
	move $a0, $t1
	la $a1, bufferInput
	move $a2, $s1
	syscall
	
	move $a0, $t1			# CLOSE THE FILE
	li $v0, 16
	syscall

end:
