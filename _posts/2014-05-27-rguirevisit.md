---
layout: post
title: R gui Revisited
tags: [R, gui]
---

A couple months ago I wrote about switching my class from using SPSS to one that uses R with a gui frontend ([original post](http://educate-r.org/2014/02/03/Rgui/)).  Since the semester has wrapped up, below are my thoughts on how the course went with respect to the students using R.

#### Things that went well
***********
Many students by the end were starting to understand the power of R (even through a gui structure).  Most were able to create new variables, run statistical analyses, produce basic graphs or figures, and even understand a few basic commands. They also enjoyed that the language and program were free and they could bring their laptops to class when we were going to talk about doing something new in R.

I also did not hear any stories of a gui system crashing (which was one complaint of *Deducer* in the comments of my last post).  I had students using either *Deducer* or *Rcmdr*, but whenever possible steered them toward *Deducer* as it is a bit more user friendly (in my opinion) and uses *ggplot2* by default for graphics.  Although *Deducer* did crash on me once while demonstrating something during class, I did not hear any students complain about it.

During the last two weeks of the course when more assignments surrounding basic inferential methods were due, students were becoming much more confident and better able to navigate the R gui menus.  I do also think that the students did appreciate the simpleness of the R output, only giving you what you ask for (as compared to SPSS that gives you extremely more than you ask for). One part of any gui system is to figure out the menu structure and understand the basic language of the menu.  Once I was able to point students to the correct menus, give them correct names for statistical procedures, and also point out specific R terminology they were much more comfortable doing things on their own (and even on occasion trying new things on their own).

#### Things that went poorly
***********
Using both *Deducer* and *Rcmdr* in the class.  I did not want to do this initially, but was forced to do it as I was unsure initially why some students were getting an error (it is based on the Java version installed).  I should have spent more time trying to transition students to *Deducer* after figuring out the error, however did not want to have them learn a new system after starting to learn *Rcmdr*.  

Another sticky spot was that I did not have students learn *Markdown*, therefore students were copying and pasting output into Word. If anyone has tried this in the past, it usually does not format the best.  In the future (definitely for a PhD level course) I would have students learn *Markdown* as it would likely not take more than an hour.  

Students using *Rcmdr* had difficulty created dichotomous variables from a continuous variable.  With *Rcmdr*, one needs to know some basic R commands (like *ifelse*) in order to create dichotomous variables.  This is not something that I spent much time going over in class and was a definite stumbling block for many students.

An unfortunate aspect of any course where students need to learn software (or just any type of material in general) is that some students do not feel the software is something they are going to use down the road.  As a result, a handful of students seemed to be a bit more obstinate regarding the use of R (and my guess would have been a similar reaction to using SPSS).

Lastly, I did not have assignments that focused specifically on learning R.  If I ever teach a course of this level again (which may not occur at my new job at the University of Iowa) I would definitely have more regular small assignments to accomplish more data manipulation tasks, such as creating a new variable, turn a variable into a factor, etc.

#### Summary
*********
In general I was pleased with using R and more specifically using a R gui for the the course.  The scope of the class was not to teach students a statistical language and for many of these students this was likely their last research/statistics course. This gave me one shot to try to get them familiar enough with the program to be an option for them in the future.  I do feel that the students became familiar enough with R during the course to be able to use it for basic data manipulation and analysis in the future, but are still tied to the gui system.

With that being said, if I ever teach a similar course in the future, I would likely create a shiny app that allows students to interact in the browser instead of using the R gui. The scope of the course is focused more on interpretation of the output rather than learning the statistical package to get the output, so the shiny app would work well (I'm imagining a Tinkerplots-esque look and feel). I would also recommend for anyone who has students and a course at a similar level and you choose to use a gui system for R, to use *Deducer*.  Once the initial setup bottlenecks are worked out it is much easier for the students to learn and use (and is more similar to SPSS if they use that program in the future).

