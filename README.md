# SQL Data System of Motor Vehicle Thefts and Analysis
Created an SQL Database of New Zealand Motor Vehicle Theft data with over 4,500 records. Joined theft data with location data that included state, population, and density data to determine thefts by population and density. Answered pertinent questions relating to theft and population data, and finalized with hotspot maps.


## Create Database and Tables

```sql
create database motor_thefts
use motor_thefts;
```

I have two datasets I will be using. Location data and Stolen cars data.

```sql
create table locations (
location_id Int primary key, 
region varchar (255) not NULL, 
country varchar (255) not null,
population Int not null, 
density decimal (10,2) not null 
);

create table stolen (
vehicle_id INT not null, 
vehicle_type varchar (255) not null,
make_id int,
model_year INT not null, 
vehicle_desc varchar (255) not null,
color varchar (255) not null,
date_stolen date not null,
location_id INT,
foreign key (make_id) references carmake(make_id),
foreign key (location_id) references locations (location_id)
);

alter table Locations
modify density decimal(12,4);
```
I altered the precision and scale of the decimal data type for density to accomodate for the large numbers in my dataset before I insert it into the table.

### Formatting Excel Data for SQL
I used a formula in the next available empty column in both my location data spreadsheet and stolen cars data spreadsheet according to the data type. 
<img width="1277" height="593" alt="image" src="https://github.com/user-attachments/assets/72dfe22d-8372-4f8a-9c63-335052a9b07b" />
<img width="1279" height="590" alt="image" src="https://github.com/user-attachments/assets/d48ed9b2-2dc2-4c72-b501-f19c989369ba" />


### Inserting Location Values and Stolen Cars Values
```sql
insert into locations VALUES
(101,'Northland','New Zealand', 201500, 16.11),
(102,'Auckland','New Zealand',	1695200, 343.09),
(103,'Waikato','New Zealand',513800,21.5),
(104,'Bay of Plenty','New Zealand',347700,28.8),
(105,'Gisborne','New Zealand',52100,6.21),
(106,'Hawkes Bay','New Zealand',182700,12.92),
(107,'Taranaki','New Zealand',127300,	17.55),
(108,'Manawatū-Whanganui','New Zealand',258200,11.62),
(109,'Wellington','New Zealand',543500,67.52),
(110,'Tasman','New Zealand',	58700,6.1),
(111,'Nelson','New Zealand',54500,129.15),
(112,'Marlborough','New Zealand',51900,4.94),
(113,'West Coast','New Zealand',32700,1.41),
(114,'Canterbury','New Zealand',655000,14.72),
(115,'Otago','New Zealand', 246000, 7.89),
(116,'Southland','New Zealand',102400,3.28);

insert into stolen values 
(1, 'Trailer', 623, 2021, 'BST2021D', 'Silver', '2021-11-05', 102),
(2, 'Boat Trailer', 623, 2021, 'OUTBACK BOATS FT470', 'Silver', '2021-12-13', 105),
(3, 'Boat Trailer', 623, 2021, 'ASD JETSKI', 'Silver', '2022-02-13', 102),
(4, 'Trailer', 623, 2021, 'MSC 7X4', 'Silver', '2021-11-13', 106),
(5, 'Trailer', 623, 2018, 'D-MAX 8X5', 'Silver', '2022-01-10', 102),
(6, 'Roadbike', 636, 2005, 'YZF-R6T', 'Black', '2021-12-31', 102),
(7, 'Trailer', 623, 2021, 'CAAR TRANSPORTER', 'Silver', '2021-11-12', 114),
(8, 'Boat Trailer', 623, 2001, 'BOAT', 'Silver', '2022-02-22', 109),
(9, 'Trailer', 514, 2021, '7X4-6" 1000KG', 'Silver', '2022-02-25', 115),
(10, 'Trailer', 514, 2020, '8X4 TANDEM', 'Silver', '2022-01-03', 114)
.....);
```
I am only putting in 10 out of the 4527 stolen car data on my github for sake of space. But I did input the entire data into my database. 


## Analysis Q&A

**1. Which dates had the most vehicles stolen and least vehicles stolen? Limit 5**
```sql
select date, 
count(*) as frequency
from stolen 
group by date
order by frequency desc
limit 5;
```
<img width="199" height="102" alt="image" src="https://github.com/user-attachments/assets/53e7b761-ea0d-4362-bb8f-2b9d4372a29d" />
April 4th, 2022 had the most cars stolen in a day with 81. 

```sql
select date, 
count(*) as frequency
from stolen
group by date
order by frequency asc
limit 5;
```
<img width="197" height="101" alt="image" src="https://github.com/user-attachments/assets/a82f5513-489b-4901-a4f2-b6443048c307" />
April 6th, 2022 had the least cars stolen in a day with just 7.


**2. Which types of vehicles are most and least often stolen? Limit 3.**
```SQL
select type,
Count(*) as often 
from stolen 
group by type
order by often desc
limit 3;
```
<img width="176" height="70" alt="image" src="https://github.com/user-attachments/assets/3c1f2074-e987-4af0-8975-db2d19b13cc2" />

The top 3 stolen cars are the Stationwagon, Saloon, and Hatchback.

```sql
select type, 
count(*) as lessoften
from stolen 
group by type 
order by lessoften asc
limit 3;
```
<img width="226" height="77" alt="image" src="https://github.com/user-attachments/assets/690117c3-a3df-4a43-8985-e0d2b8606f7f" />

The least 3 stolen cars are the Articulated Truck, Special Purpose Vehicles, and Trail Bikes.

**3. Does the type of vehicle stolen vary by region?**
- This question requires an inner join to combine the location and stolen cars table.
```sql
select l.region, s.type,
count(*) AS total_stolen
FROM stolen as s
INNER JOIN locations l
ON s.location_id = l.location_id
GROUP BY l.region, s.type
order by region asc;
```
<img width="345" height="132" alt="image" src="https://github.com/user-attachments/assets/eaeb7ad5-6457-4c4b-98ae-9c7cda4c7ab8" />
I ordered the data by region in alphabetical order so it would be easier to see which types of cars were most stolen in each region of New Zealand. 

These were the most stolen types by region: 

| Region              | Vehicle Type   | Total |
|---------------------|----------------|-------|
| Auckland            | Saloon         | 327   |
| Bay of Plenty       | Utility        | 100   |
| Canterbury          | Stationwagon   | 165   |
| Gisborne            | Saloon         | 41    |
| Hawkes Bay          | Trailer        | 23    |
| Manawatū-Whanganui  | Stationwagon   | 29    |
| Nelson              | Trailer        | 19    |
| Northland           | Stationwagon   | 69    |
| Otago               | Stationwagon   | 31    |
| Southland           | Stationwagon   | 9     |
| Taranaki            | Stationwagon   | 23    |
| Waikato             | Saloon         | 80    |
| Wellington          | Stationwagon   | 92    |


**4. What type of vehicle is stolen in the most dense region?**
```sql
select s.type, l.region, l.density,
count(*) as total_stolen
from stolen s
inner join locations l
on s.location_id = l.location_id
group by s. type, l.region, l.density
order by total_stolen desc
limit 1;
```
<img width="367" height="46" alt="image" src="https://github.com/user-attachments/assets/28c1c589-d476-40b0-a7da-e5dec98bbd28" />
This query pulled the most dense region, Auckland, with a total of 327 Saloon type cars stolen.

**5. Which region has the most and least cars stolen?**
```sql
select l.region,
count(*) as total_stolen
from stolen as s
inner join locations l
on s.location_id=l.location_id
group by l.region
order by total_stolen desc
limit 1;
```
<img width="215" height="47" alt="image" src="https://github.com/user-attachments/assets/d9c92728-e51b-4583-afb2-7fae84c65c3a" />
Auckland has the most cars stolen with a total of 1,626. 

```sql
select l.region,
count(*) as total_stolen
from stolen as s
inner join locations l
on s.location_id = l.location_id
group by l.region
order by total_stolen asc 
limit 1;
```
<img width="217" height="43" alt="image" src="https://github.com/user-attachments/assets/2abf6abc-fa96-4b9e-8995-a50c4af62b25" />

Southland has the least cars stolen with 26.

## Hot Spot Maps: Google Data Studio
Mapping of stolen cars total by region:
<img width="1066" height="707" alt="image" src="https://github.com/user-attachments/assets/3ce7ce95-1772-440d-bbec-94bde2991ec2" />

Mapping of region by population data:
<img width="1112" height="721" alt="image" src="https://github.com/user-attachments/assets/4303f80a-e4d5-44f4-8876-252efa73fdc5" />






