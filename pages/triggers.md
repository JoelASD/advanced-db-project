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