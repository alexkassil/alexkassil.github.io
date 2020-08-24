---
layout: post
title: "Notes of MOSS and Followups"
date: 2020-08-23
tags: MOSS Plagiarism Detection TAPS TMOSS
---

## Introduction

It seems like cheating is quite prevalent in computer science courses, where students, often desperate, overwhelmed, and near a deadline copy solutions or collaborate too much with others to cheat and submit work that isn't entirely their own. It is all too easy for students to copy and paste coding solutions from a friend or online github solution.

Just as long as students have been copying code, instructors have had counter measures in place to catch people who copy code. This post will talk about a prevalent system MOSS, how it works, and a few followups to it.

### [MOSS](http://theory.stanford.edu/~aiken/publications/papers/sigmod03.pdf)

Moss stands for Measure of Software Similarity and automatically detects similarity between two files of code. You can read more about MOSS [here](https://theory.stanford.edu/~aiken/moss/).

An interesting bit of Moss history is found in this [berkeley news article from 1997](https://www.berkeley.edu/news/media/releases/97legacy/11_19_97b.html) where it says Alex Aiken is a professor at Berkeley, about 10 percent of students were caught cheating by using Moss with Professor Grisworld's compilers course in UC San Diego, Professor Aiken's penalty for cheating was a 0 on the assignment plus a decrease of one letter grade, and Professor Aiken's long term goal was not to flunk students but deter students as a whole from cheating.

Moss and previous work deals with finding k-grams that match between documents, where a k-gram is a contiguous substring of length k. Previous copying detection before Moss removed irrelevant features from text, split text into many k-grams, selected a hashed subset of these k-grams to be a document's fingerprints, and see if those hashes match up with another document.

Example:

	Text:
		1. Alex Kassil
		2. Alexander F. Kassil
	
	Text with irrelevant Features Removed:
		1. alexkassil
		2. alexanderfkassil

	Text split into 5-grams:
		1. alexk lexka exkas xkass kassi assil
		2. alexa lexan exand xande ander nderf derfk erfka rfkas fkass kassi assil

	Imagining our subset selection took every other 5-gram,
	kassi or assil would match between the two documents and be the one fingerprint match

There are three desirable properties of copy-detection: whitespace insensitivity, noise suppression, and position independence.

For whitespace insensitivity, everything other than text is removed, text is lowercased, and for code all variable names are swapped with "V" to achieve variable name agnostic search.

For noise suppression, a value of k needed to be selected that isn't too small that it allows too many short commonalities to be found and not too long to never have matches.

For positional independence, choose fingerprint-s independent of position, one way of doing it is choosing hashes which equal *0 mod p*.

The Moss paper describes an algorithm for selecting which hashes should be the fingerprints, with the algorithm called **winnowing**. The algorithm slides a window across the hashes and repeatedly selected the minimum in the window to be added to the list of fingerprints.

![image from the Moss paper](/assets/winnowing.png)

The winnowing algorithm results in fingerprints with desirable properties like having a guarantee threshold t where matches longer than t are detected and a noise threshold k, where matches shorter than k are not selected.

So the Moss algorithm is an improvement of previous detection algorithms and is so prevalent due to it providing a nice web interface for all to use. Specifically for matching code the algorithm can easily ignore boiler-plate by fingerprinting the boilerplace with a special document ID that indicates any match with that fingerprint should be discarded. A simple elegant algorithm for plagiarism detection.

### [TAPS: A MOSS Extension for Detecting Software Plagiarism at Scale](http://lucylabs.gatech.edu/b/wp-content/uploads/2016/04/wip146-sheahenA.pdf)

[Code for this project](https://github.com/danainschool/moss-taps)

A **MOSS** **T**ool **A**ddressing **P**lagiarism at **S**cale. The original Moss project works great for single documents you want to check for plagiarism against each other and online solutions that might exist, like a one off job for a big course project. TAPS is an extension that works for doing a lot of important preprocessing when there are both multiple assignments in a class as well as multiple previous offerings of the course that have had similar/the same assignments.

The problem TAPS solves is it becomes quite cumbersome to check a current batch of assignments against each other as well as the previous ones, so TAPS:

1. Allows for Mixed languages, and separates them before submission to Moss
2. Deals with File Management, like zips and multiple depths of directories which need to be expanded and normalized before sent to Moss.
3. Filters, since a student shouldn't have checks between their current their nth assignment checked against their own (n - 1)th assignment, where matches are likely when assignments build upon each other.

Impressively this saved an instructor tons of time of organizing, submitting, and filtering class assignments for the purpose of software plagiarism detection by slashing the time from 50 hours to only 10 minutes.

### [TMOSS: Using Intermediate Assignment Work to Understand Excessive Collaboration in Large Classes](https://stanford.edu/~cpiech/bio/papers/tmoss.pdf)

[Code for this project](https://github.com/yanlisa/tmoss)

TMOSS is another extension to Moss, relying on the fact that students often have backups of intermediate assignment work, through say git or okpy. By utilizing these backups as well as the final submission for plagiarism detection, while time analyzing increases, TMOSS is "almost twice as effective as traditional software similarity detectors in identifying the number of students who exhibit excessive collaboration". Also of interest is the paper also finds "that such students [who cheat] spend significantly less time on their assignment, use fewer class tutoring resources, and perform worse on exams than their peers"

The heart of the paper is the algorithm below, which just adds comparison of intermediate backups of each student to each other students submission as well as online solutions.

![algorithm from the TMOSS paper](/assets/tmoss.png)

Interestingly enough the paper also examines how start day of assignment effects midterm/final scores, with HEC standing for hypothesized excessive collaboration, or students who are suspected of cheating/copying solutions. Students who started earlier and didn't cheat had higher exam scores, while cheaters had lower exam scores regardless of if they cheated or not.

![image from the TMOSS paper showing start day vs midterm score](/assets/tmoss-startday-graph.png)

## Conclusion

Moss allows for great plagiarism detection checks, and TAPS allows for ease of managing Moss submissions when there are repeat offerings of a course/assignments build on each other, and TMOSS allows for better checks if in progress work of assignments is available too.