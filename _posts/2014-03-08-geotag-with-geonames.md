---
layout: post
title: "Using Geonames Data"

---

This [nice forum post on setting up geonames](http://forum.geonames.org/gforum/posts/list/732.page "Setting up Geonames") is all you really need.

Below are some parts I found useful:

<pre class="prettyprint sql">

	CREATE TABLE geoname ( 
	geonameid int PRIMARY KEY, 
	name varchar(200), 
	asciiname varchar(200), 
	alternatenames varchar(4000), 
	latitude decimal(10,7), 
	longitude decimal(10,7), 
	fclass char(1), 
	fcode varchar(10), 
	country varchar(2), 
	cc2 varchar(60), 
	admin1 varchar(20), 
	admin2 varchar(80), 
	admin3 varchar(20), 
	admin4 varchar(20), 
	population int, 
	elevation int, 
	gtopo30 int, 
	timezone varchar(40), 
	moddate date 
	) CHARACTER SET utf8; 	

	CREATE TABLE countryinfo ( 
	iso_alpha2 char(2), 
	iso_alpha3 char(3), 
	iso_numeric integer, 
	fips_code character varying(3), 
	name character varying(200), 
	capital character varying(200), 
	areainsqkm double precision, 
	population integer, 
	continent char(2), 
	tld char(3), 
	currency char(3), 
	currencyName char(20), 
	Phone char(10), 
	postalCodeFormat char(20), 
	postalCodeRegex char(20), 
	geonameId int 
	languages character varying(200), 
	neighbours char(20), 
	equivalentFipsCode char(10) 
	) CHARACTER SET utf8; 


	CREATE TABLE admin1CodesAscii ( 
	code CHAR(6), 
	name TEXT, 
	nameAscii TEXT, 
	geonameid int 
	) CHARACTER SET utf8; 

	LOAD DATA LOCAL INFILE '/data/allCountries.txt' INTO TABLE geoname (geonameid,name,asciiname,alternatenames,latitude,longitude,fclass,fcode,country,cc2, admin1,admin2,admin3,admin4,population,elevation,gtopo30,timezone,moddate); 

	LOAD DATA LOCAL INFILE '/data/admin1CodesAscii.txt' INTO TABLE admin1CodesAscii (code, name, nameAscii, geonameid);
	
	LOAD DATA LOCAL INFILE '/data/countryInfo-n.txt' INTO TABLE countryInfo IGNORE 1 LINES (iso_alpha2,iso_alpha3,iso_numeric,fips_code,name,capital,areaInSqKm,population,continent,languages,currency,geonameId); 

</pre>

