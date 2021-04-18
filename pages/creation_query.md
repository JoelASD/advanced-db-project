```sql
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

DROP SCHEMA IF EXISTS `online-store` ;
CREATE SCHEMA IF NOT EXISTS `online-store` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci ;
USE `online-store` ;

-- -----------------------------------------------------
-- Table `online-store`.`Contract`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Contract` (
  `ContractID` INT NOT NULL AUTO_INCREMENT,
  `Salary` DECIMAL(10,2) NOT NULL,
  `Position` VARCHAR(45) NOT NULL,
  `ContractStartDate` DATETIME NOT NULL,
  `ContractEndDate` DATETIME NULL,
  `ContractInfo` VARCHAR(2000) NULL,
  PRIMARY KEY (`ContractID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`UserAccount`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`UserAccount` (
  `UserAccountID` INT NOT NULL AUTO_INCREMENT,
  `FirstName` VARCHAR(45) NOT NULL,
  `LastName` VARCHAR(45) NOT NULL,
  `Email` VARCHAR(100) NOT NULL,
  `Password` VARCHAR(255) NOT NULL,
  `UserInfo` VARCHAR(1000) NULL,
  `ContractID` INT NULL,
  PRIMARY KEY (`UserAccountID`),
  INDEX `fk_UserAccount_Contract1_idx` (`ContractID` ASC),
  CONSTRAINT `fk_UserAccount_Contract1`
    FOREIGN KEY (`ContractID`)
    REFERENCES `online-store`.`Contract` (`ContractID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`PhoneNumber`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`PhoneNumber` (
  `PhoneNumberID` INT NOT NULL AUTO_INCREMENT,
  `PhoneNumber` VARCHAR(45) NOT NULL,
  `UserAccountID` INT NOT NULL,
  PRIMARY KEY (`PhoneNumberID`),
  INDEX `fk_PhoneNumber_UserAccount1_idx` (`UserAccountID` ASC),
  CONSTRAINT `fk_PhoneNumber_UserAccount1`
    FOREIGN KEY (`UserAccountID`)
    REFERENCES `online-store`.`UserAccount` (`UserAccountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Country`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Country` (
  `CountryID` INT NOT NULL AUTO_INCREMENT,
  `Country` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`CountryID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`City`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`City` (
  `CityID` INT NOT NULL AUTO_INCREMENT,
  `City` VARCHAR(45) NOT NULL,
  `CountryID` INT NOT NULL,
  PRIMARY KEY (`CityID`),
  INDEX `fk_City_Country1_idx` (`CountryID` ASC),
  CONSTRAINT `fk_City_Country1`
    FOREIGN KEY (`CountryID`)
    REFERENCES `online-store`.`Country` (`CountryID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`ZipCode`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`ZipCode` (
  `ZipCodeID` INT NOT NULL AUTO_INCREMENT,
  `ZipCode` VARCHAR(45) NOT NULL,
  `CityID` INT NOT NULL,
  PRIMARY KEY (`ZipCodeID`),
  INDEX `fk_ZipCode_City1_idx` (`CityID` ASC),
  CONSTRAINT `fk_ZipCode_City1`
    FOREIGN KEY (`CityID`)
    REFERENCES `online-store`.`City` (`CityID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Address`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Address` (
  `AddressID` INT NOT NULL AUTO_INCREMENT,
  `Street` VARCHAR(100) NOT NULL,
  `Apartment` VARCHAR(10) NOT NULL,
  `UserAccountID` INT NOT NULL,
  `ZipCodeID` INT NOT NULL,
  PRIMARY KEY (`AddressID`),
  INDEX `fk_Address_UserAccount1_idx` (`UserAccountID` ASC),
  INDEX `fk_Address_ZipCode1_idx` (`ZipCodeID` ASC),
  CONSTRAINT `fk_Address_UserAccount1`
    FOREIGN KEY (`UserAccountID`)
    REFERENCES `online-store`.`UserAccount` (`UserAccountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Address_ZipCode1`
    FOREIGN KEY (`ZipCodeID`)
    REFERENCES `online-store`.`ZipCode` (`ZipCodeID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`ProductCategory`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`ProductCategory` (
  `ProductCategoryID` INT NOT NULL AUTO_INCREMENT,
  `ProductCategory` VARCHAR(100) NOT NULL,
  `ProductCategoryDescription` VARCHAR(1000) NULL,
  `ProductCategoryTax` INT NULL,
  `ParentProductCategoryID` INT NULL,
  PRIMARY KEY (`ProductCategoryID`),
  INDEX `fk_ProductCategory_ProductCategory1_idx` (`ParentProductCategoryID` ASC),
  CONSTRAINT `fk_ProductCategory_ProductCategory1`
    FOREIGN KEY (`ParentProductCategoryID`)
    REFERENCES `online-store`.`ProductCategory` (`ProductCategoryID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Brand`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Brand` (
  `BrandID` INT NOT NULL AUTO_INCREMENT,
  `BrandName` VARCHAR(80) NOT NULL,
  `BrandURL` VARCHAR(512) NULL,
  `BrandInfo` VARCHAR(1000) NULL,
  PRIMARY KEY (`BrandID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Product`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Product` (
  `ProductID` INT NOT NULL AUTO_INCREMENT,
  `ProductTitle` VARCHAR(255) NOT NULL,
  `Barcode` VARCHAR(255) NOT NULL,
  `Price` DECIMAL(10) NOT NULL,
  `Description` VARCHAR(1000) NULL,
  `ProductInfo` VARCHAR(1000) NULL,
  `ProductCategoryID` INT NOT NULL,
  `ProductURL` VARCHAR(2000) NULL,
  `BrandID` INT NOT NULL,
  PRIMARY KEY (`ProductID`),
  INDEX `fk_Product_ProductCategory1_idx` (`ProductCategoryID` ASC),
  INDEX `fk_Product_Brand1_idx` (`BrandID` ASC),
  CONSTRAINT `fk_Product_ProductCategory1`
    FOREIGN KEY (`ProductCategoryID`)
    REFERENCES `online-store`.`ProductCategory` (`ProductCategoryID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Product_Brand1`
    FOREIGN KEY (`BrandID`)
    REFERENCES `online-store`.`Brand` (`BrandID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Basket`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Basket` (
  `BasketID` INT NOT NULL AUTO_INCREMENT,
  `BasketInfo` VARCHAR(1000) NULL,
  `UserAccountID` INT NOT NULL,
  PRIMARY KEY (`BasketID`),
  INDEX `fk_Basket_UserAccount1_idx` (`UserAccountID` ASC),
  CONSTRAINT `fk_Basket_UserAccount1`
    FOREIGN KEY (`UserAccountID`)
    REFERENCES `online-store`.`UserAccount` (`UserAccountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`PaymentMethod`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`PaymentMethod` (
  `PaymentMethodID` INT NOT NULL,
  `PaymentMethod` VARCHAR(100) NULL,
  PRIMARY KEY (`PaymentMethodID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Discount`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Discount` (
  `DiscountID` INT NOT NULL AUTO_INCREMENT,
  `DiscountCode` VARCHAR(100) NULL,
  `DiscountCodeInfo` VARCHAR(1000) NULL,
  `DiscountCodePer` INT NOT NULL,
  PRIMARY KEY (`DiscountID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Shipment`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Shipment` (
  `ShipmentID` INT NOT NULL AUTO_INCREMENT,
  `ShipmentMethod` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`ShipmentID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`ProductOrder`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`ProductOrder` (
  `OrderID` INT NOT NULL,
  `OrderStatus` VARCHAR(45) NOT NULL,
  `TotalPrice` DECIMAL(15,0) NOT NULL,
  `BasketID` INT NOT NULL,
  `PaymentMethodID` INT NOT NULL,
  `DiscountCodeID` INT NULL,
  `OrderInfo` VARCHAR(1000) NULL,
  `ShipmentID` INT NOT NULL,
  `EmployeeID` INT NOT NULL,
  PRIMARY KEY (`OrderID`),
  INDEX `fk_Order_Basket1_idx` (`BasketID` ASC),
  INDEX `fk_Order_PaymentMethod1_idx` (`PaymentMethodID` ASC),
  INDEX `fk_Order_DiscountCode1_idx` (`DiscountCodeID` ASC),
  INDEX `fk_Order_Shipment1_idx` (`ShipmentID` ASC),
  INDEX `fk_Order_UserAccount1_idx` (`EmployeeID` ASC),
  CONSTRAINT `fk_Order_Basket1`
    FOREIGN KEY (`BasketID`)
    REFERENCES `online-store`.`Basket` (`BasketID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Order_PaymentMethod1`
    FOREIGN KEY (`PaymentMethodID`)
    REFERENCES `online-store`.`PaymentMethod` (`PaymentMethodID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Order_DiscountCode1`
    FOREIGN KEY (`DiscountCodeID`)
    REFERENCES `online-store`.`Discount` (`DiscountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Order_Shipment1`
    FOREIGN KEY (`ShipmentID`)
    REFERENCES `online-store`.`Shipment` (`ShipmentID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Order_UserAccount1`
    FOREIGN KEY (`EmployeeID`)
    REFERENCES `online-store`.`UserAccount` (`UserAccountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`GiftCard`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`GiftCard` (
  `GiftCardID` INT NOT NULL AUTO_INCREMENT,
  `GiftCardCode` VARCHAR(255) NOT NULL,
  `ExpirationDate` DATETIME NULL,
  `ProductCategoryID` INT NOT NULL,
  PRIMARY KEY (`GiftCardID`),
  INDEX `fk_GiftrCard_ProductCategory1_idx` (`ProductCategoryID` ASC),
  CONSTRAINT `fk_GiftrCard_ProductCategory1`
    FOREIGN KEY (`ProductCategoryID`)
    REFERENCES `online-store`.`ProductCategory` (`ProductCategoryID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Review`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Review` (
  `ReviewID` INT NOT NULL AUTO_INCREMENT,
  `Rating` TINYINT NOT NULL,
  `Review` VARCHAR(1000) NULL,
  `UserAccountID` INT NOT NULL,
  `ProductID` INT NOT NULL,
  PRIMARY KEY (`ReviewID`),
  INDEX `fk_Review_UserAccount1_idx` (`UserAccountID` ASC),
  INDEX `fk_Review_Product1_idx` (`ProductID` ASC),
  CONSTRAINT `fk_Review_UserAccount1`
    FOREIGN KEY (`UserAccountID`)
    REFERENCES `online-store`.`UserAccount` (`UserAccountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Review_Product1`
    FOREIGN KEY (`ProductID`)
    REFERENCES `online-store`.`Product` (`ProductID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`timestamps`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`timestamps` (
  `create_time` TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP,
  `update_time` TIMESTAMP NULL,
  `UserAccountID` INT NULL,
  `ProductID` INT NULL,
  `BasketID` INT NULL,
  `OrderID` INT NULL,
  `GiftrCardID` INT NULL,
  `ReviewID` INT NULL,
  INDEX `fk_timestamps_UserAccount_idx` (`UserAccountID` ASC),
  INDEX `fk_timestamps_Product1_idx` (`ProductID` ASC),
  INDEX `fk_timestamps_Basket1_idx` (`BasketID` ASC),
  INDEX `fk_timestamps_Order1_idx` (`OrderID` ASC),
  INDEX `fk_timestamps_GiftrCard1_idx` (`GiftrCardID` ASC),
  INDEX `fk_timestamps_Review1_idx` (`ReviewID` ASC),
  CONSTRAINT `fk_timestamps_UserAccount`
    FOREIGN KEY (`UserAccountID`)
    REFERENCES `online-store`.`UserAccount` (`UserAccountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_timestamps_Product1`
    FOREIGN KEY (`ProductID`)
    REFERENCES `online-store`.`Product` (`ProductID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_timestamps_Basket1`
    FOREIGN KEY (`BasketID`)
    REFERENCES `online-store`.`Basket` (`BasketID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_timestamps_Order1`
    FOREIGN KEY (`OrderID`)
    REFERENCES `online-store`.`ProductOrder` (`OrderID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_timestamps_GiftrCard1`
    FOREIGN KEY (`GiftrCardID`)
    REFERENCES `online-store`.`GiftCard` (`GiftCardID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_timestamps_Review1`
    FOREIGN KEY (`ReviewID`)
    REFERENCES `online-store`.`Review` (`ReviewID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION);


-- -----------------------------------------------------
-- Table `online-store`.`Basket_has_Product`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Basket_has_Product` (
  `BasketID` INT NOT NULL,
  `ProductID` INT NOT NULL,
  `Quantity` INT NOT NULL,
  PRIMARY KEY (`BasketID`, `ProductID`),
  INDEX `fk_Basket_has_Product_Product1_idx` (`ProductID` ASC),
  INDEX `fk_Basket_has_Product_Basket1_idx` (`BasketID` ASC),
  CONSTRAINT `fk_Basket_has_Product_Basket1`
    FOREIGN KEY (`BasketID`)
    REFERENCES `online-store`.`Basket` (`BasketID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Basket_has_Product_Product1`
    FOREIGN KEY (`ProductID`)
    REFERENCES `online-store`.`Product` (`ProductID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Product_has_Discount`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Product_has_Discount` (
  `ProductID` INT NOT NULL,
  `DiscountID` INT NOT NULL,
  PRIMARY KEY (`ProductID`, `DiscountID`),
  INDEX `fk_Product_has_Discount_Discount1_idx` (`DiscountID` ASC),
  INDEX `fk_Product_has_Discount_Product1_idx` (`ProductID` ASC),
  CONSTRAINT `fk_Product_has_Discount_Product1`
    FOREIGN KEY (`ProductID`)
    REFERENCES `online-store`.`Product` (`ProductID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Product_has_Discount_Discount1`
    FOREIGN KEY (`DiscountID`)
    REFERENCES `online-store`.`Discount` (`DiscountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Order_has_GiftCard`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Order_has_GiftCard` (
  `OrderID` INT NOT NULL,
  `GiftCardID` INT NOT NULL,
  PRIMARY KEY (`OrderID`, `GiftCardID`),
  INDEX `fk_Order_has_GiftrCard_GiftrCard1_idx` (`GiftCardID` ASC),
  INDEX `fk_Order_has_GiftrCard_Order1_idx` (`OrderID` ASC),
  CONSTRAINT `fk_Order_has_GiftrCard_Order1`
    FOREIGN KEY (`OrderID`)
    REFERENCES `online-store`.`ProductOrder` (`OrderID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Order_has_GiftrCard_GiftrCard1`
    FOREIGN KEY (`GiftCardID`)
    REFERENCES `online-store`.`GiftCard` (`GiftCardID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Comment`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Comment` (
  `CommentID` INT NOT NULL AUTO_INCREMENT,
  `Comment` VARCHAR(1000) NOT NULL,
  `UserAccountID` INT NOT NULL,
  `ProductID` INT NOT NULL,
  PRIMARY KEY (`CommentID`),
  INDEX `fk_Comment_UserAccount1_idx` (`UserAccountID` ASC),
  INDEX `fk_Comment_Product1_idx` (`ProductID` ASC),
  CONSTRAINT `fk_Comment_UserAccount1`
    FOREIGN KEY (`UserAccountID`)
    REFERENCES `online-store`.`UserAccount` (`UserAccountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Comment_Product1`
    FOREIGN KEY (`ProductID`)
    REFERENCES `online-store`.`Product` (`ProductID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Comment_has_Comment`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Comment_has_Comment` (
  `ParentCommentID` INT NOT NULL,
  `ChildCommentID` INT NOT NULL,
  PRIMARY KEY (`ParentCommentID`, `ChildCommentID`),
  INDEX `fk_Comment_has_Comment_Comment2_idx` (`ChildCommentID` ASC),
  INDEX `fk_Comment_has_Comment_Comment1_idx` (`ParentCommentID` ASC),
  CONSTRAINT `fk_Comment_has_Comment_Comment1`
    FOREIGN KEY (`ParentCommentID`)
    REFERENCES `online-store`.`Comment` (`CommentID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Comment_has_Comment_Comment2`
    FOREIGN KEY (`ChildCommentID`)
    REFERENCES `online-store`.`Comment` (`CommentID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Bundle`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Bundle` (
  `BundleID` INT NOT NULL AUTO_INCREMENT,
  `BundleName` VARCHAR(100) NOT NULL,
  `BundleInfo` VARCHAR(1000) NULL,
  `BundleDiscount` INT NOT NULL,
  PRIMARY KEY (`BundleID`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Bundle_has_Product`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Bundle_has_Product` (
  `BundleID` INT NOT NULL,
  `ProductID` INT NOT NULL,
  PRIMARY KEY (`BundleID`, `ProductID`),
  INDEX `fk_Bundle_has_Product_Product1_idx` (`ProductID` ASC),
  INDEX `fk_Bundle_has_Product_Bundle1_idx` (`BundleID` ASC),
  CONSTRAINT `fk_Bundle_has_Product_Bundle1`
    FOREIGN KEY (`BundleID`)
    REFERENCES `online-store`.`Bundle` (`BundleID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Bundle_has_Product_Product1`
    FOREIGN KEY (`ProductID`)
    REFERENCES `online-store`.`Product` (`ProductID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Media`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Media` (
  `MediaID` INT NOT NULL AUTO_INCREMENT,
  `MediaURL` VARCHAR(512) NOT NULL,
  `MediaType` VARCHAR(45) NOT NULL,
  `ProductID` INT NOT NULL,
  PRIMARY KEY (`MediaID`),
  INDEX `fk_Media_Product1_idx` (`ProductID` ASC),
  CONSTRAINT `fk_Media_Product1`
    FOREIGN KEY (`ProductID`)
    REFERENCES `online-store`.`Product` (`ProductID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`ProductAlert`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`ProductAlert` (
  `ProductID` INT NOT NULL,
  `UserAccountID` INT NOT NULL,
  `ProductAlertInfo` VARCHAR(1000) NULL,
  PRIMARY KEY (`ProductID`, `UserAccountID`),
  INDEX `fk_Product_has_UserAccount_UserAccount1_idx` (`UserAccountID` ASC),
  INDEX `fk_Product_has_UserAccount_Product1_idx` (`ProductID` ASC),
  CONSTRAINT `fk_Product_has_UserAccount_Product1`
    FOREIGN KEY (`ProductID`)
    REFERENCES `online-store`.`Product` (`ProductID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Product_has_UserAccount_UserAccount1`
    FOREIGN KEY (`UserAccountID`)
    REFERENCES `online-store`.`UserAccount` (`UserAccountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `online-store`.`Session`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `online-store`.`Session` (
  `SessionID` INT NOT NULL AUTO_INCREMENT,
  `Device` VARCHAR(255) NOT NULL,
  `Started` DATETIME NOT NULL,
  `Expires` DATETIME NOT NULL,
  `UserAccount_UserAccountID` INT NOT NULL,
  PRIMARY KEY (`SessionID`),
  INDEX `fk_Session_UserAccount1_idx` (`UserAccount_UserAccountID` ASC),
  CONSTRAINT `fk_Session_UserAccount1`
    FOREIGN KEY (`UserAccount_UserAccountID`)
    REFERENCES `online-store`.`UserAccount` (`UserAccountID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;

SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
```

# Triggers

```sql
-- -----------------------------------------------------
-- Triggers
-- -----------------------------------------------------

DELIMITER $$
CREATE TRIGGER review_trig
BEFORE INSERT ON Review
FOR EACH ROW
BEGIN
	IF NEW.Rating > 5 || NEW.Rating < 1 || (NEW.UserAccountID IN (SELECT UserAccountID FROM Review) and NEW.ProductID IN (SELECT ProductID FROM Review))
	THEN
	CALL `'Rating only accepts 1-5! One rating per product per user!'`;
	END IF;
END$$
DELIMITER ;

---------------
-- UserAccount
---------------

DELIMITER $$
CREATE TRIGGER create_UserAccount_trig
AFTER INSERT ON UserAccount
FOR EACH ROW
BEGIN
	INSERT INTO timestamps(create_time, update_time, UserAccountID)
	VALUES (now(), now(), NEW.UserAccountID);
END$$
DELIMITER ;


DELIMITER $$
CREATE TRIGGER update_UserAccount_trig
AFTER UPDATE ON UserAccount
FOR EACH ROW
BEGIN
	UPDATE timestamps
	SET update_time = now()
	WHERE UserAccountID = NEW.UserAccountID;
END$$
DELIMITER ;

-----------
-- Product
-----------
DELIMITER $$
CREATE TRIGGER create_Product_trig
AFTER INSERT ON Product
FOR EACH ROW
BEGIN
	INSERT INTO timestamps(create_time, update_time, ProductID)
	VALUES (now(), now(), NEW.ProductID);
END$$
DELIMITER ;


DELIMITER $$
CREATE TRIGGER update_Product_trig
AFTER UPDATE ON Product
FOR EACH ROW
BEGIN
	UPDATE timestamps
	SET update_time = now()
	WHERE ProductID = NEW.ProductID;
END$$
DELIMITER ;


-----------
-- Basket
-----------
DELIMITER $$
CREATE TRIGGER create_Basket_trig
AFTER INSERT ON Basket
FOR EACH ROW
BEGIN
	INSERT INTO timestamps(create_time, update_time, BasketID)
	VALUES (now(), now(), NEW.BasketID);
END$$
DELIMITER ;


DELIMITER $$
CREATE TRIGGER update_Basket_trig
AFTER UPDATE ON Basket
FOR EACH ROW
BEGIN
	UPDATE timestamps
	SET update_time = now()
	WHERE BasketID = NEW.BasketID;
END$$
DELIMITER ;


-----------
-- Order
-----------

DELIMITER $$
CREATE TRIGGER create_Order_trig
AFTER INSERT ON ProductOrder
FOR EACH ROW
BEGIN
	INSERT INTO timestamps(create_time, update_time, OrderID)
	VALUES (now(), now(), NEW.OrderID);
END$$
DELIMITER ;


DELIMITER $$
CREATE TRIGGER update_Order_trig
AFTER UPDATE ON ProductOrder
FOR EACH ROW
BEGIN
	UPDATE timestamps
	SET update_time = now()
	WHERE OrderID = NEW.OrderID;
END$$
DELIMITER ;


-----------
-- GitfCard
-----------

DELIMITER $$
CREATE TRIGGER create_GiftCard_trig
AFTER INSERT ON GiftCard
FOR EACH ROW
BEGIN
	INSERT INTO timestamps(create_time, update_time, GiftCardID)
	VALUES (now(), now(), NEW.GiftCardID);
END$$
DELIMITER ;


DELIMITER $$
CREATE TRIGGER update_GiftCard_trig
AFTER UPDATE ON GiftCard
FOR EACH ROW
BEGIN
	UPDATE timestamps
	SET update_time = now()
	WHERE UserAccountID = NEW.GiftCardID;
END$$
DELIMITER ;


-----------
-- Review
-----------

DELIMITER $$
CREATE TRIGGER create_Review_trig
AFTER INSERT ON Review
FOR EACH ROW
BEGIN
	INSERT INTO timestamps(create_time, update_time, ReviewID)
	VALUES (now(), now(), NEW.ReviewID);
END$$
DELIMITER ;


DELIMITER $$
CREATE TRIGGER update_Review_trig
AFTER UPDATE ON Review
FOR EACH ROW
BEGIN
	UPDATE timestamps
	SET update_time = now()
	WHERE UserAccountID = NEW.ReviewID;
END$$
DELIMITER ;

```