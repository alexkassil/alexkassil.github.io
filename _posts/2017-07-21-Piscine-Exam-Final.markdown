---
layout: post
title: "Exam Final: Eight Hours of Fun"
date: 2017-07-21
categories: piscine
---

## The Final Exam and The Final Day (Day 26)
It's over. Almost one month of grueling, 12 hours a day, intense programming, I finished the piscine. What a month. It flew by, since every day I was so busy. Also I barely procrastinated and spent not that much time on my phone. I was more or less 100% focused on programming. I will speak more about my final thoughts in the next post. For now, I will just recap the last day.

### One Last Exam
This final exam was special, in that it was twice as long as the other three. Eight hours instead of the usual 4. It was also much longer, going up way past level 5 to level 10. I only (barely) made it to level 9. The first 6 questions were that of a normal exam, level 0 to level 5. I breezed through them. Each of these questions were worth 9 points, but if you got one wrong you would get a question of the same level but only worth 4 points.

#### Level 0
Print last param. Using only write, I had to display the last command line argument passed into the program. Very simple

#### Level 1
String copy. I had to write a function that takes two char arrays as inputs and copies the second's value into the first, and return the first. Quite simple.

#### Level 2
String compare. Return the ascii difference between two strings. I wrote a function that iterated through the two strings until a character differed/null was reached and then returned the difference of the current char. Pretty straightforward.

#### Level 3
Ft rrange. Using malloc, return an array filled with ints starting from input end and ending at input start. Not too hard, but definitely starting to ramp up in complexity. At this point I was about two hours in, and at 36 points.

#### Level 4
Reverse string by whitespace. This one was the first new problem for me. I had to write a program that takes a string delineated by white space and prints the string with the words reversed. `./rev_wstr "This is a test"` would output `"test a is This"`. I ended up doing this one recursively, printing each last word, then sending the last space to null and calling the function again.