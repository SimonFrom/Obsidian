En gruppe til mange scannere

SCANNER_GROUPS(<u>group_id</u>, group_number, group_name, organization_id, IMEI)

Primary: group_id
Foreign: IMEI


USE airplate_dk_db_development; 
CREATE TABLE SCANNER_GROUPS 
( 
	group_id INT NOT NULL AUTO_INCREMENT, 
	group_name VARCHAR NOT NULL,
	group_number INT NOT NULL,
	organization_id INT NOT NULL, 
	IMEI VARCHAR NOT NULL, 
	PRIMARY KEY (group_id), 
	CONSTRAINT fk_scanner_groups_imei 
	FOREIGN KEY (IMEI) REFERENCES Scanners(IMEI) 
);


CREATE TABLE SCANNER_GROUPS 
( 
	group_id INT NOT NULL AUTO_INCREMENT, 
	group_name VARCHAR(255) NOT NULL, 
	group_number INT NOT NULL, 
	organization_id INT NOT NULL, 
	IMEI VARCHAR(20) NOT NULL, 
	PRIMARY KEY (group_id), 
	CONSTRAINT fk_scanner_groups_imei 
	FOREIGN KEY (IMEI) 
	REFERENCES Scanners(IMEI) 
);

