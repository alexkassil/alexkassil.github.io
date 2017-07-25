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
Reverse string by whitespace. This one was the first new problem for me. I had to write a program that takes a string delineated by white space and prints the string with the words reversed. `./rev_wstr "This is a test"` would output `"test a is This"`. I ended up doing this one recursively, printing each last word, then setting the last space to null and calling the function again. Conceptually interesting, and nontrivial to implement.

#### Level 5
Brainfuck for the third time! This time I breezed through it in 15 minutes. It's crazy how something so complex to me on the first exam became so easy! I was even able to explain how brainfuck works/how to write an interpreter for it to others so they could pass brainfuck on the exam. Super easy this time around, satisfying to complete. ~3 hours in

#### Level 6 - Uncharted territory
Count alpha. Taking a string as input, print out the number of each letter present. So `./count_alpha "Hello, world!"` should display `"1h, 1e, 3l, 2o, 1w, 1r, 1d"`. This problem wasn't too difficult. I used a global char array with 26 elements to keep track of which letters had already been printed so as to not print any letter twice, and then iterated over the input displaying the number and letter. Kind of difficult, but also fun. ~4 hours in

I later learned from the cadets that these last 5 levels and the problems they hold are only used for the final exam of the C piscine. It was honestly jarring during the exam when I got to level 6. I though I would have to do multiple level 5 questions.

#### Level 7
Order by alpha and length. This one was long. You had to take the string given as input, and then print all the words in it sorted first by length and then by lexicographical order. For this problem I had to actually rewrite a level 4 problem, split whitespaces, in order to manage the given input. Hard and long. ~6 hours in

#### Level 8 Attempt 1
Count Island. The first problem I failed. For this one you had to design a program that takes a map as input (rectangle populated only by .'s and #'s), and number each island (represented as a group on touching #'s). My solution worked for the two given exampled, but didn't pass the tests. I believe this is because I didn't clear the buffer every time the program ran. Fun, but difficult. ~7 hours in.

#### Level 8 Attempt 2
Infinity Addition. For this program I had to take 2 strings representing valid integers as input, and return their sum. I ended up doing this recursively, adding (or subtracting) the number digit by digit with the carry recursively to push the first result way back into the stack and print it at the very end. While this problem wasn't crazy difficult, I was so exhausted and mentally drained I had a lot of trouble getting it to work. Before I submitted it with minutes left, I kept having seg faults for the input of -10 and 9. Thankfully the automated tests didn't catch that and I passed this problem. Very difficult due to physical and mental exhaustion. ~7 hours and 57 minutes in

#### Level 9
Graph diameter. Given a string with graph links, return the length of the largest circular route. Now, I've never done any work with graphs, and I only had three minutes left. So I was unable to even start this question.

#### Level 10 (Didn't get to this one)
MD5. Rewrite the MD5 hash algorithm. Yep, this one is hard. No one, as far as I know, has gotten this right.

I ended up with a 76/100. The highest score in my piscine was 81/100. I was very happy with what I got, it being the second highest. I definitely enjoyed this very long exam, but was brain dead afterwards.

### BBQ
Woohoo! Piscine is over. The pisciners plus some cadets all had a barque on the lawn. It was nice to relax and play volleyball and eat food and be done with the piscine.

### Viewing of The Hitchhiker's Guide to the Galaxy
To end the piscine, the movie club had a viewing of the movie that 42 got it's namesake from. Also it made clear a lot of the references during the piscine. Vogsphere, or the server we pushed all our work to, is from Hitchhiker's Guide to the Galaxy. So is Sastantua, Marvin, and 42! A great way to end a great month of learning.