# SQL-Data-of-Motor-Vehicle-Thefts-in-New-Zealand-
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








