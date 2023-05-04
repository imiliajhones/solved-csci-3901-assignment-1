Download Link: https://assignmentchef.com/product/solved-csci-3901-assignment-1
<br>
<h1>Problem 1</h1>




Goal

Get practice in decomposing a problem, creating a design for a program, and implementing and testing a program.




Background

Recommender systems use past behaviours of others to give you suggestions on what has worked for others in the past, based on your own current context.  Amazon books, for example, looks at the set of books that you have already purchased and compares your reading list with the reading list of others.  It then aggregates the purchases of others who also bought many of the same books as you and recommends to you those books from the aggregate that you haven’t already purchased.  Typically, you don’t make a recommendation unless there is enough data to suggest that it’s a meaningful recommendation.




In this assignment, we will do a simplified version of a recommender system.  In our case, you will be recommending courses to take at the university based on what other students have taken.




Problem

Write a class that accepts the list of courses that other students have taken in the past and then allows us to make three queries on that data.  I will provide a “main” program that will then let a user invoke the operations on the data.




The normal use of your class will have the following sequence:

<ul>

 <li>Read data from a file</li>

 <li>Do any of the following, in any order, a number of times o Provide a list of courses and then ask for the best X recommendations of next courses to take (where X is a parameter)

  <ul>

   <li>Provide a list of courses and have the method print to the screen the number of students with any combined pair of the courses (more details later)</li>

   <li>Have a method print to a file the number of students with the pairs of courses for any pair of courses (more details later)</li>

  </ul></li>

</ul>




We will not add data interactively to the set of courses.




Your class will be called “CourseSelector” and will include the following methods:

<ul>

 <li>Integer read( String filename ) – read in the contents of the file to the object. Each line represents the set of courses for a student.  Return the number of data rows read.  The data should replace any information already in the class.  Individual courses in a line will be separated by spaces.</li>

 <li>ArrayList&lt;String&gt; recommend( String taken, int support, int recommendations ) – return a list of “recommendations” courses for me to take, given that I have already taken the courses in “taken”. Only report back if the recommendations are based on at least “support” other students.  More information on how to select the courses appears later.</li>

 <li>boolean showCommon ( String courses ) – print to the screen a 2d array where each course in “courses” has a row and a column (in the order in which they appear in “courses”). The value at the intersection of a row and a column is the number of students who have taken both courses.  More information on the output format later.  Return True if you were able to make the computation and False if you encountered any error condition.</li>

 <li>boolean showCommonAll ( String filename ) – print to file “filename” a 2d array where each known course has a row and a column (in sorted order). The value at the intersection of a row and a column is the number of students who have taken both courses.  More information on the output format later.  Return True if you were able to make the computation and False if you encountered any error condition.</li>

</ul>




In all methods, treat course numbers in a case invariant way, so csci3901 and CSCI3901 should be treated as the same course.




<h2>Functionality</h2>

recommend( String taken, int support, int recommendations)

The method will return the courses that have been taken most frequently with the ones that you provide as the “taken” parameter.




The basic logic is as follows:

<ol>

 <li>Find all of the students who have taken all of the same courses as provided in “taken”.</li>

 <li>If there are at least “support” students then

  <ol>

   <li>List all of the courses that these students have taken, other than those in “taken”.</li>

   <li>For each course in point 2a, count up how many students have taken that course.</li>

   <li>Report back the most frequently taken courses, up to “recommendation” suggestions.</li>

  </ol></li>

</ol>

This logic is provided as a simple guide to understanding the method.  You are allowed to re-do the logic however you want.




If you don’t have enough students to make a recommendation, then return a null ArrayList.




If you do have recommendations to return, then the returned ArrayList should have at most “recommendations” entries in it.  The courses should be provided in descending order of frequency.  Two courses with the same frequency can be in any order.




There is one situation where the ArrayList can have more entries: when you reach “recommendations” entries and there are other courses with the same frequency that you haven’t reported.  Rather than choose between which of these last courses to return, report all of the ones with that frequency.




For example, suppose that the outcome of computation 2c is the following list of courses:

<table width="368">

 <tbody>

  <tr>

   <td width="94">Course</td>

   <td width="274">Number of students who have taken it</td>

  </tr>

  <tr>

   <td width="94">CSCI1100</td>

   <td width="274">12</td>

  </tr>

  <tr>

   <td width="94">CSCI2110</td>

   <td width="274">10</td>

  </tr>

  <tr>

   <td width="94">CSCI2112</td>

   <td width="274">8</td>

  </tr>

  <tr>

   <td width="94">CSCI2134</td>

   <td width="274">8</td>

  </tr>

  <tr>

   <td width="94">CSCI3110</td>

   <td width="274">5</td>

  </tr>

  <tr>

   <td width="94">CSCI3120</td>

   <td width="274">4</td>

  </tr>

 </tbody>

</table>




If “recommendations” is 2 then you would return CSCI1100 and CSCI2110.

If “recommendations” is 3 then you would return CSCI1100, CSCI2110, CSCI2112, and CSCI2134 (since the last two both have the same frequency).

If “recommendations” is 5 then you would return CSCI1100, CSCI2110, CSCI2112, CSCI2134, and CSCI3110.




showCommon( … )

Both methods with this method name compute a pairwise popularity matrix.  Pairs of courses that are both taken frequently shouldn’t be scheduled at the same time, so we want to know the pairs of courses that are both taken often.




The basic logic is as follows:

<ol>

 <li>Create a 2d array with one row and one column for each course that we are given.</li>

 <li>Consider the courses of each student in our system.

  <ol>

   <li>Find the rows/column number for each of the courses.</li>

   <li>For each pair of courses, add 1 to the entry in the 2d array for the pair. Do not pair a course with itself.</li>

  </ol></li>

 <li>Print the resulting matrix.</li>

</ol>

This logic is provided as a simple guide to understanding the method.  You are allowed to re-do the logic however you want.




When you print the matrix, you do not print the column names.  For each row of the matrix, print the course name and then the integer from each column.  Each of these bits to print will be separated by a tab character.




For example, suppose that we have entries for the following students in our system:

CSCI1110 CSCI2110 CSCI2122

CSCI2110 CSCI2112 CSCI2122 CSCI2134

CSCI1110 CSCI2134 CSCI3171

CSCI2141




If we asked for a matrix for all courses above, we would get the following output




CSCI1110         0          1          0          1          1          0          1

CSCI2110         1          0          1          2          1          0          0

CSCI2112         0          1          0          1          1          0          0

CSCI2122         1          2          1          0          1          0          0

CSCI2134         1          1          1          1          0          0          1

CSCI2141         0          0          0          0          0          0          0

CSCI3171         1          0          0          0          1          0          0





