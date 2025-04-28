# crimemanagementsystem
create database CrimeManagementSystem;
use CrimeManagementSystem;

CREATE TABLE Crime (
    CrimeID INT PRIMARY KEY,
    IncidentType VARCHAR(255),
    IncidentDate DATE,
    Location VARCHAR(255),
    Description TEXT,
    Status VARCHAR(20)
);
CREATE TABLE Victim (
    VictimID INT PRIMARY KEY,
    CrimeID INT,
    Name VARCHAR(255),
    ContactInfo VARCHAR(255),
    Injuries VARCHAR(255),
    FOREIGN KEY (CrimeID) REFERENCES Crime(CrimeID)
);
CREATE TABLE Suspect (
    SuspectID INT PRIMARY KEY,
    CrimeID INT,
    Name VARCHAR(255),
    Description TEXT,
    CriminalHistory TEXT,
    FOREIGN KEY (CrimeID) REFERENCES Crime(CrimeID)
);
INSERT INTO Crime (CrimeID, IncidentType, IncidentDate, Location, Description, Status)
VALUES 
    (1, 'Robbery', '2023-09-15', '123 Main St, Cityville', 'Armed robbery at a convenience store', 'Open'),
    (2, 'Homicide', '2023-09-20', '456 Elm St, Townsville', 'Investigation into a murder case', 'Under Investigation'),
    (3, 'Theft', '2023-09-10', '789 Oak St, Villagetown', 'Shoplifting incident at a mall', 'Closed');
INSERT INTO Victim (VictimID, CrimeID, Name, ContactInfo, Injuries)
VALUES 
    (1, 1, 'John Doe', 'johndoe@example.com', 'Minor injuries'),
    (2, 2, 'Jane Smith', 'janesmith@example.com', 'Deceased'),
    (3, 3, 'Alice Johnson', 'alicejohnson@example.com', 'None');
INSERT INTO Suspect (SuspectID, CrimeID, Name, Description, CriminalHistory) 
VALUES 
    (1, 1, 'Robber 1', 'Armed and masked robber', 'Previous robbery convictions'), 
    (2, 2, 'Unknown', 'Investigation ongoing', NULL), 
    (3, 3, 'Suspect 1', 'Shoplifting suspect', 'Prior shoplifting arrests');
select * from Crime;
select * from Victim;
select * from Suspect;


select * from Crime where Status ='open';

select count(incidenttype) from crime;

select distinct incidenttype
from crime;

select * from crime
where incidentdate between '2023-09-01' and '2023-09-10';


-- i dont have age in vitim  and suspect so i add age
alter table Victim
add age int;
alter table Suspect 
add age int;

update Victim
set age = 28
where victimid = 1;

update Victim
set age = 30
where victimid = 2;

update Victim
set age = 22
where victimid = 3;

update Suspect
set age = 35
where suspectid = 1;

update Suspect
set age = 45
where suspectid = 2;

update Suspect
set age = 25
where suspectid = 3;

select name, age 
from victim
union 
select name, age 
from suspect
order by age desc;


select avg(age) 
from (
    select age from victim
    union 
    select age from suspect
) as persons;

select incidenttype, count(incidenttype) 
from crime 
where status = 'Open' 
group by incidenttype;


select name
from victim
where name like '%Doe%'
union
select name
from suspect
where name like '%Doe%';


select name
from victim v
join crime c on v.crimeid = c.crimeid
where c.status in ('Open', 'Closed')
union all
select name 
from suspect s
join crime c on s.crimeid = c.crimeid
where c.status in ('Open', 'Closed');

select distinct c.incidenttype
from crime c
join victim v on c.crimeid = v.crimeid
where v.age in (30, 35)
union
select distinct c.incidenttype
from crime c
join suspect s on c.crimeid = s.crimeid
where s.age in (30, 35);

select v.name
from victim v
join crime c on v.crimeid = c.crimeid
where c.incidenttype = 'Robbery'
union
select s.name 
from suspect s
join crime c on s.crimeid = c.crimeid
where c.incidenttype = 'Robbery';

select incidenttype
from crime
where status = 'Open'
group by incidenttype
having count(*) > 1;

select c.crimeid, c.incidenttype, c.status, s.name as suspectname, v.name as victimname
from crime c
join suspect s on c.crimeid = s.crimeid
join victim v on s.name = v.name
where c.crimeid != v.crimeid;

select c.crimeid, c.incidenttype,
       v.name,v.contactinfo, v.injuries, 
       s.name,s.description,s.criminalhistory
from crime c
left join victim v on c.crimeid = v.crimeid
left join suspect s on c.crimeid = s.crimeid;

select c.crimeid, c.incidenttype, c.status, 
s.name, s.age, v.name, v.age
from crime c
join suspect s on c.crimeid = s.crimeid
join victim v on c.crimeid = v.crimeid
where s.age > v.age;

select s.suspectid, s.name
from suspect s
join crime c on s.crimeid = c.crimeid
group by s.suspectid, s.name
having count(c.crimeid) > 1;

select c.crimeid, c.incidenttype, c.status
from crime c
left join suspect s on c.crimeid = s.crimeid
where s.suspectid is null;

select distinct c1.crimeid
from crime c1
join crime c2 on c1.crimeid = c2.crimeid
where c1.incidenttype = 'homicide'
  and c2.incidenttype = 'robbery';

   -- here i dont know how to add no suspect for null
select c.crimeid, c.incidenttype, c.status, s.name
from crime c
left join suspect s on c.crimeid = s.crimeid;

select s.suspectid, s.name 
from suspect s
join crime c on s.crimeid = c.crimeid
where c.incidenttype in ('Robbery', 'Assault');


