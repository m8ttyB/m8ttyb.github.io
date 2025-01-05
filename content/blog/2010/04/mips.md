---
title: "MIPS"
date: 2010-04-05
draft: false
tags: ["coding", "college"]
---
Spring is in the air (finally) in Colorado and it doesn't look like
we're going to get *huge* amounts of snow anytime soon. I was dinking
around my file system to see what files I'm willing to delete thinking
to myself, "can I clear up any mind clutter?" Well I was able to delete
quite a bit of my old *adventures* from the past. However, I came across
a bit of MIPS code that I wrote, I almost deleted it until I remembered
somewhat recently I was able to help some poor student with his homework
on one of the Ubuntu forums.

For posterity I've decided to ensconce this knowledge into a blog post.

**Note to students:** don't blindly copy this code, make sure you
understand what its doing and why.

```mips
############################################
# Sums up the numbers (words) from 1 to 20
# Prints the numbers and then the sum
############################################
.text
.align 2
.globl main

main:
  la $t0, animas
  li $s0, 20
  jal addition
  jal print
  b end
addition:
  lw $t1 ($t0)
  move $a0, $t1
  li $v0, 1
  syscall
  li $v0, 4
  la $a0, comma
  syscall
  add $t3, $t1, $t3
  addi $s0, -1
  addi $t0, 4
  bgt $s0, $zero, addition
  jr $ra
print:
  li $v0, 4
  la $a0, msg
  syscall
  move $a0, $t3
  li $v0, 1
  syscall
  jr $ra
end:

.data
animas: .word 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20
msg: .asciiz "nThe sum is: "
comma: .asciiz ","

#########################################
# moveblk(&src,&dest,size)
# Transfers a block of words of "size"
# to a new destination
#########################################
.text
.align 2
.globl main

main:

  la $a0, src
  la $a1, dest
  li $s0, 4 	# the size
  jal moveblk
  b end
moveblk:
  li $t5, 0	# counter
  la $t0, src
  lw $t1 ($t0)	# src
  sw $t1 ($t2)	# dest

  addi $t0, 4
  addi $t2, 4
  addi $t5, 1
  beq $t5, $s0, end
  jal moveblk
end:

.data
src:.word 1,2,9,6
dest .word
###################################
# sortNums - Sorts the #'s stored
# in registers $a0, $a1, $a2
###################################

.text

.align 

.globl main

main:
  li $a0, 5
  li $a1, 2
  li $a2, 1
  jal sortNums
  jal print
  b end
sortNums:
  blt $a0, $a1, else
    move $t0, $a0
    move $a0, $a1
    move $a1, $t0
  else: blt $a1, $a2, else2
    move $t0, $a1
    move $a1, $a2
    move $a2, $t0
  else2: blt $a0, $a1, done
    move $t0, $a0
    move $a0, $a1
    move $a1, $t0
  done: jr $ra
print:
  li $v0, 4
  move $t0, $a0
  la $a0, msg
  syscall
  move $a0, $t0
  li $v0, 1
  syscall
  li $v0, 4
  la $a0, comma
  syscall
  move $a0, $a1
  li $v0, 1
  syscall
  li $v0, 4
  la $a0, comma
  syscall
  move $a0, $a2
  li $v0, 1
  syscall
  move $a0, $t0	# set $a0 to previous state
  jr $ra
end:
.data
msg: .asciiz "nThe sorted #'s: "
comma: .asciiz ","

#################################
# isEven function		#
#################################
.text
.align 2
.globl main

main:
  li $a0, 25		# var to test
  li $s0, 2
  jal isEven
  jal print
  b end
isEven:
  div $a0, $s0
  mfhi $t0
  beq $t0, $zero, even
  li $v0, 1 		# is odd
  jr $ra
  even: li $v0, 0
    jr $ra
print:
  move $t0, $v0
  li $v0, 4
  la $a0, msg
  syscall
  li $v0, 1
  move $a0, $t0
  syscall
  jr $ra
end:

.data
msg: .asciiz "The output is: "
```
