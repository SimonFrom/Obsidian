En gruppe til mange scannere

SCANNER_GROUPS(<u>group_id</u>, group_number, group_name, organization_id, IMEI)

Primary: group_id
Foreign: IMEI

SCANNER_GROUP_NAMES(<u>id</u>, organization_id, name)
Primary: id


## SQL Scripts:
CREATE TABLE SCANNER_GROUPS (
  group_id          INT           NOT NULL AUTO_INCREMENT,
  group_name        VARCHAR(255)  NOT NULL,
  organization_id   INT           NOT NULL,
  IMEI              VARCHAR(20)   NOT NULL,
  PRIMARY KEY (group_id),
  UNIQUE KEY uq_scanner_org_imei (organization_id, IMEI),
  KEY fk_scanner_groups_imei (IMEI),
  CONSTRAINT fk_scanner_groups_imei FOREIGN KEY (IMEI) 
  REFERENCES Scanners (IMEI)
);

CREATE TABLE SCANNER_GROUP_NAMES (
  id                INT           NOT NULL AUTO_INCREMENT,
  organization_id   INT           NOT NULL,
  name              VARCHAR(255)  NOT NULL,
  PRIMARY KEY (id),
  UNIQUE KEY uniq_org_group_name (organization_id, name)
);

