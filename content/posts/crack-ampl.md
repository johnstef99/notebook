---
title: "How to crack AMPL student version"
description: "How ot crack AMPL with reverse engineering"
tags: ['low-level', 'reverse-engineer']
date: 2021-06-17
---

## Pre requirements

* [Ghidra](https://ghidra-sre.org/)
* [Ampl for students](https://ampl.com/products/ampl/ampl-for-students/)

## Cracking

1. Open Ghidra and create a new project

   ![image](step_1.png)

2. Hit the key **I** to import a file to the project and select the ampl binary

   ![image](step_2.png)

3. Make sure you have the same options<br>

   ![image](step_3.png)

4. Ghidra will ask to analyze the code, hit yes and check the option
   **Decompiler Parameter ID**

   ![image](step_4_1.png)
   ![image](step_4_2.png)

5. After the analyze is done go to the toolbar **Search -> For Strings** and
   filter out the word demo. There you will find the message that comes up for
   the limiter student edition. Double click it.

   ![image](step_5.png)

6. You will see the address that the string is stored and the functions that
   reference it. Double click on the **FUN_XXXXXXXX** on the right.

   ![image](step_6.png)

7. This brings us here

   ![image](step_7.png)

   The comparison at `004d8323`, is what causes the check for the student
   limitation. You can see an arrow going from `004d832a` to `004d8336`, this
   jump is taking place when student licence is found, so we can redirect the
   jump to an other address. But where? We can see that if the jump never
   happens there is an other one 4 lines bellow that goes to `004d83a3`, that
   seems like the right address to go.

8. We need to apply this change on the binary, but if you try to change this
   file and **Export to binary** you will get an error when you try to run it.
   That is happening because we need to do the change on the line `004d832a` on
   the raw binary and not here. So hit **I** to import a file and select
   the ampl binary again. But this time make sure the **Format is Raw Binary**.

   ![image](step_8.png)

9. Now hit **G** and go to this address `000d832a` instead of `004d832a` because
   the **Binary** starts from `00000000` and the **ELF** from `00400000`. Now
   **right click on the line -> Patch Instruction** and change the address to
   point to `000d83a3`

   ![image](step_9.png)

10. Hit **O** to export the new binary and choose **Format -> Binary**.<br>

    ![image](step_10.png)

**Now you have a cracked ampl binary! :)**

## AMPL Solvers
You need to follow the same procedure for every solver you want to use, but
probably you only going to need cplex.
