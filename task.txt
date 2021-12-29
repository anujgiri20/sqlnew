-- 1. CCreate tables for the below list given
-- (users
-- codekata
-- attendance
-- topics
-- tasks
-- company_drives
-- mentors
-- students_activated_courses
-- courses)


CREATE DATABASE students;
use students;

-- courses
CREATE TABLE courses(courseid INTEGER AUTO_INCREMENT PRIMARY KEY,coursename VARCHAR(100));

-- mentors
CREATE TABLE mentors(mentorid INTEGER AUTO_INCREMENT PRIMARY KEY,mentorname VARCHAR(100),mentoremail VARCHAR(100));

-- Users
CREATE TABLE users(userid INTEGER AUTO_INCREMENT PRIMARY KEY, username VARCHAR(100),useremail VARCHAR(100)
,mentorid INTEGER, 
FOREIGN KEY (mentorid) REFERENCES mentors(mentorid)
);

-- codekata
CREATE TABLE codekata(codekataid INTEGER AUTO_INCREMENT PRIMARY KEY,userid INTEGER,number_of_problems INTEGER,
 string_problems INTEGER,
 FOREIGN KEY (userid) REFERENCES users(userid)
 );
 
-- topics
CREATE TABLE topics(topicsid INTEGER AUTO_INCREMENT PRIMARY KEY,courseid INTEGER, topic VARCHAR(200),
FOREIGN KEY (courseid) REFERENCES courses(courseid)
);

-- tasks
CREATE TABLE tasks(tasksid INTEGER AUTO_INCREMENT PRIMARY KEY,courseid INTEGER,task VARCHAR(200),
FOREIGN KEY (courseid) REFERENCES courses(courseid)
);

-- Attendance
CREATE TABLE attendance(attendanceid INTEGER AUTO_INCREMENT PRIMARY KEY, userid INTEGER,courseid INTEGER ,topicsid INTEGER, attended BOOLEAN,
FOREIGN KEY (userid) REFERENCES users(userid),
FOREIGN KEY (topicsid) REFERENCES topics(topicsid),
FOREIGN KEY (courseid) REFERENCES courses(courseid)
);

-- Company drives
CREATE TABLE company_drives(drivesid INTEGER AUTO_INCREMENT PRIMARY KEY,userid INTEGER,company VARCHAR(100),
FOREIGN KEY (userid) REFERENCES users(userid)
);

-- students_activated_courses
CREATE TABLE students_activated_courses(id INTEGER AUTO_INCREMENT PRIMARY KEY,userid INTEGER,courseid INTEGER,
FOREIGN KEY (userid) REFERENCES users(userid),
FOREIGN KEY (courseid) REFERENCES courses(courseid)
);

-- 2. insert at least 5 rows of values in each table


use students;

-- courses
INSERT INTO courses(coursename) VALUES("HTML"),("CSS"),("Bootstrap"),("JavaScript"),("ReactJS");

-- mentors
INSERT INTO mentors(mentorname,mentoremail) VALUES ("Anuj","Anuj@gmail.com"),
                                                   ("ganesh","ganesh@gmail.com"),
                                                   ("aniket","aniket@gmail.com"),
                                                   ("sagar","sagar@gmail.com"),
                                                    ("akshay","akshay@gmail.com"),


-- users

INSERT INTO users(mentorname,mentoremail) VALUES ("Anuj","Anuj@gmail.com"),
                                                   ("ganesh","ganesh@gmail.com"),
                                                   ("aniket","aniket@gmail.com"),
                                                   ("sagar","sagar@gmail.com"),
                                                    ("akshay","akshay@gmail.com"),


-- codekata
INSERT INTO codekata(number_of_problems,string_problems,userid) VALUES(20,10,1),
                                                                       (30,15,2),
                                                                       (25,10,3),
                                                                       (50,30,4),
                                                                       (80,60,5);


-- topics
INSERT INTO topics(topic,courseid) VALUES("HTML Basics",1),
                                         ("CSS Basics",2),
                                         ("Bootstrap - Containers",3),
                                         ("JavaScript Basics",4),
                                         ("React Components",5);


-- tasks
INSERT INTO tasks(task,courseid) VALUES ("HTML Task",1),
                                         ("CSS Task",2),
                                         ("Bootstrap Task",3),
                                         ("JavaScript Task",4),
                                         ("React Task",5),;


-- attendence
INSERT INTO attendance(userid,courseid,topicsid,attended) VALUES(2,5,5,true),
                                                                (5,1,1,true)
                                                                (1,3,3,false)
                                                                (3,4,4,ture)
                                                                (4,2,2,true);


-- company drives
INSERT INTO company_drives(userid,company) VALUES (1,"Microsys"),
                                                    (3,"Amazon")
                                                    (5,"HCL")
                                                    (2,"TCS");
                                                    (4,"Wipro")


-- students activated courses
INSERT INTO students_activated_courses(userid,courseid) VALUES(1,1),(2,1),(3,5),(4,2),(5,3);






-- 3. get number problems solved in codekata by combining the users
use students;

SELECT users.userid, users.username,users.useremail, codekata.number_of_problems 
FROM users
INNER JOIN codekata ON users.userid = codekata.userid;

-- 4.display the no of company drives attended by a user
use students;

SELECT userid ,COUNT(userid) FROM company_drives GROUP BY userid;

-- 5. combine and display students_activated_courses and courses for a specific user groping them based on the course
use students;

SELECT students_activated_courses.userid,students_activated_courses.courseid,
COUNT(students_activated_courses.courseid) ,courses.coursename
FROM students_activated_courses
INNER JOIN courses 
ON students_activated_courses.courseid=courses.courseid
WHERE students_activated_courses.userid=1
GROUP BY courses.courseid;

-- 6 .list all the mentors
use students;

SELECT mentorname FROM mentors;

-- 7. list the number of students that are assigned for a mentor
USE students;

SELECT mentors.mentorid,mentors.mentorname,COUNT(users.mentorid) 
FROM mentors,users
WHERE mentors.mentorid=users.mentorid 
GROUP BY mentors.mentorid;
