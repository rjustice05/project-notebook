# Design notebook for week ending April 17, 2016

## Last week's critique

I thought a lot about Daniel's suggestions, I kind of liked it, but wasn't sure how well it would work with my current implimintation and if someone used rateProf('Weidermann', 15), rateCourse('CS111', 7), and rateProfAndClass('Weidermann', 'CS111', 10), how would I correctly interpret that? There might be sections of CS111 not taught by Prof Ben, or other courses taught by Prof Ben so I wouldn't want to just discard those values, but it would be unclear what the user intended. If you want a very specific section of a class currently, then you can just rate that section very highly and it has no impact on the ratings of other sections or courses taught by the same profs and I think this is clearer for my users. I did really like your suggestion about output format, but have not yet had a chance to implement it. Daniel gave me some sugestions about how to include my code, the input from the Scheduler App and there code and run it, I looked at some of it and think I understand from a technical point of view now, but I am not yet sure what is the best syntax for my users, so I haven't reached a final decision even though I spent some time reading and researching it.

## Description

This week was frusterating. It felt like I was crawling alonge at a snails pace. I found some errors when I realized I hadn't reached the correct layer in my embedded list because the Schedules list is a list of lists of possible schedules and value pairs. Each possible schedule list is a list of classes and values pairs, and each class is a list of all relevant information for the class. I don't know if there is an easier way to pass around all this information, but the layers of lists caused me a lot of problems this week because it was hard to reason about what part of the list I was accessing at any given time. I was having issues with several other errors for 2 hours this afternoon, when I tried to run my program and it just output 'False' and I kept making small modifications to my code until it finally began working, but I am not actually sure what the error was or how I fixed it.

I also played around with several possible ways for the user to interact with the language. I looked at ways to read in and parse a text file, ways to include files or use modulos to keep some methods private (which I will do after I have finished debugging), and ways to automatically run various functions and read and write to the command line more neatly (rather than just outputting an unreadable list of embedded lists). I was excited to get a small program running and outputting correctly, but for some reason it outputs several duplicate schedules.

From Terminal:

WL-206-109:CourseScheduler rebekahjustice$ /usr/local/bin/swipl -f SampleUserFile.pl
Please input you list of classes as generated by the scheduler app and click enter. 
|: [['CHEM023A','HM-01',3,[8,30,2016],[12,16,2016],['Van Hecke','Johnson','Vosburg','Hawkins'],[[0,9,9.833333333333334,'HM Campus, Shanahan Center, 2460'],[2,9,9.833333333333334,'HM Campus, Shanahan Center, 2460'],[4,9,9.833333333333334,'HM Campus, Shanahan Center, 2460']]],['CL 057','HM-02',0,[1,17,2017],[5,14,2017],['McFadden'],[[2,13.25,16.5,'HM Campus, Olin Science Center, B141']]]].

Welcome to SWI-Prolog (Multi-threaded, 64 bits, Version 7.2.3)
Copyright (c) 1990-2015 University of Amsterdam, VU Amsterdam
SWI-Prolog comes with ABSOLUTELY NO WARRANTY. This is free software,
and you are welcome to redistribute it under certain conditions.
Please visit http://www.swi-prolog.org for details.

For help, use ?- help(Topic). or ?- apropos(Word).

?- setPreferences.
                   Done                                                
Schedule Number: 1 has a preference value of 115
CHEM023A HM-01 110
CL 057 HM-02 5

Schedule Number: 2 has a preference value of 115
CHEM023A HM-01 110
CL 057 HM-02 5

Schedule Number: 3 has a preference value of 115
CHEM023A HM-01 110
CL 057 HM-02 5

Schedule Number: 4 has a preference value of 115
CHEM023A HM-01 110
CL 057 HM-02 5

Schedule Number: 5 has a preference value of 110
CHEM023A HM-01 110

Schedule Number: 6 has a preference value of 5
CL 057 HM-02 5

true  

## Questions

**What is the most pressing issue for your project? What design decision do
you need to make, what implementation issue are you trying to solve, or how
are you evaluating your design and implementation?**

I don't intirely understand how to modify my program to increase the efficiency or to get rid of the duplicate schedules that keep appearing. I need to invest more time in trying to correct that, which I intend to do in office hours tomorrow with Prof. Ben. The next big decision is determining how to remove the host language feel and giving it a more language-y feel. I would like to talk to Prof. Ben about this tomorrow as well, I feel like I didn't accomplish a lot this week because I have been really stuck trying to correct errors that arose when I tried to merge the two parts of my code together, and which I never actually entirely fixed last week.

**What questions do you have for your critique partners? How can they best help
you?**

I am hoping to meet with Prof Ben tomorrow to see if he can help me with my project. I need ideas for how to make my code more efficient and to figure out why I am generating duplicate schedules. Once I get that sorted out, I will continue to add small features to my language but will mostly shift my focus to removing the host language flavor. If you could help me find any resources online which migth allow me to remove some of the host language feel, that would be a tramendous help because I am at somewhat of a loss as to how I can take this from a Prolog program, to a unique language. Right now, when I look at it, it doesn't have a strong language feel at all. What are things you think I could modify to make it seem more like a language? 

One idea I had, was to add in my own error messages, but I am not sure how to do this, and unfortunately did not make a lot of progress on it this week, because I got stuck debugging and searching for errors in my code when I tried to combine the 2 pieces of my project (the first taking in a list of classes and rating things, and the second, generating schedules based on those ratings).

Right now a user's program looks like this:

:- consult(sourceCode).

setPreferences :-
	rateProf('Van Hecke', 10),
	rateClass('CL 057', 5),
	rateSection('CHEM023A','HM-01', 100),
	generateSchedule.

When they run it, they will be immediantly asked to paste in the schedule list generated by the Scheduler App and then press enter. They will then have to type 'setPreference.' and press enter to run their code. Do you have ideas for Prolog functions that would make this simpler and more intutive? Would it be better to make the user write a line of code in which they pass the name of a file with the Scheduler App generated list inside to a method I define which reads in the input?

Or to add a line of code to the user file, such as:

initilization(setPreferences).

so setPreferences runs without them typing any additional commands, and then perhaps inside the body of setPreferences they could have an additional command that calls the method to read in the input classes and initialize everything. A user program would therefore look more like this:

:- consult(sourceCode).
:-initialization(SetPreferences).

setPreferences :-
	inputClasses('SchedulerClassList.txt'),
	rateProf('Van Hecke', 10),
	rateClass('CL 057', 5),
	rateSection('CHEM023A','HM-01', 100),
	generateSchedule.

Which might be harder for the user to remember, but would perhaps have a more language-like feel.

**How much time did you spend on the project this week? If you're working in a
team, how did you share the labor?**

I spent about 8 hours working this week:
- 4 hours on Saturday
- 4 hours on Sunday


