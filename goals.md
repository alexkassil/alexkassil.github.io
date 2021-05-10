---
layout: page
title: My Goals
permalink: /goals/
---

I want my goals and progress to be public and visible to all!

## Cardio

I am happiest when I am exercising regularly! Here are my recent workout sessions

<iframe height='454' width='300' frameborder='0' allowtransparency='true' scrolling='no' src='https://www.strava.com/athletes/44003187/latest-rides/1e38fd16d370cd848916763c3a9cb209dc8dbbb7'></iframe>

<style>
.Progress {
  width: 100%;
  background-color: #ddd;
}

#bike {
  width: 10%;
  height: 30px;
  background-color: #04AA6D;
  text-align: center;
  line-height: 30px;
  color: white;
}
#run {
  width: 10%;
  height: 30px;
  background-color: #04AA6D;
  text-align: center;
  line-height: 30px;
  color: white;
}
</style>

### My goal is to run 250 miles this year!
<div class="Progress">
  <div id="run">0 miles ran in 2021</div>
</div>

### My goal is to bike 500 miles this year!
<div class="Progress">
  <div id="bike">0 miles biked in 2021</div>
</div>

### Lifting

I have finally returned to the gym after a long haitus due to covid. My main lifts are down, but my motivation is high! This year I want consistency.


Current Squat: 135 lbs

Current Deadlift: 195 lbs

Current Bench Press: 95 lbs

Current Overhead Press: 65 lbs

### Reading Chinese

I've been learning chinese and my favorite part is reading books. Here are the books I read and the books I am currently reading are marked with *. I hope to keep making a little progress every day!

<ol>

	<li>我的老师是火星人</li>
	<li>周海生</li>
	<li>花马</li>
	<li>秘密花园</li>
	<li>盲人国</li>
	<li>安末</li>
	<li>美好的前途（上）</li>
	<li>猴王的诞生</li>
	<li>*美好的前途（下）</li>
	<li>*女巫</li>
	
</ol>

### Reading & Listening to English

I've been listening to a lot of audiobooks and using Libby to get library books. Here are some I've read recently and am currently reading (marked with *). I hope to rekindle my childhood love for reading.

<ol>

	<li>The Way of Kings</li>
	<li>Words of Radiance</li>
	<li>Talking to Strangers</li>
	<li>The Trials of Morrigan Crow</li>
	<li>Steelheart</li>
	<li>The Poppy War</li>
	<li>Atomic Habits</li>
	<li>*Defining Decade: Why Your Twenties Matter And How To Make The Most Of Them Now</li>
	<li>*Oathbringer</li>

	
</ol>



<script type="text/javascript" src="../data.json"></script>
<script>
let run = 0.0;
let ride = 0.0;
let meters_in_mile = 1609.34;

for (var i = 1; i < data["length"]; i++) {
	var act = data[i];
	if (act["start_date"].slice(0, 4) === "2021") {
	   if (act["type"] === "Run") {
	   	  run += act["distance"]/meters_in_mile;
	   }
	   if (act["type"] === "Ride") {
	   	  ride += act["distance"]/meters_in_mile;
	   }
	}
}

var run_bar = document.getElementById("run");
run_bar.innerHTML = run.toFixed(2) + " miles ran";
run_bar.style.width = (run/250.0 * 100).toFixed(2) + "%";
var ride_bar = document.getElementById("bike");
ride_bar.innerHTML = ride.toFixed(2) + " miles biked";
ride_bar.style.width = (ride/500.0 * 100).toFixed(2) + "%";
</script>
