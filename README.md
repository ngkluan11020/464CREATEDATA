-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0; SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0; SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- Schema mydb

-- Schema grocerystoredb

DROP SCHEMA IF EXISTS grocerystoredb ;

-- Schema grocerystoredb

CREATE SCHEMA IF NOT EXISTS grocerystoredb DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci ; USE grocerystoredb ;

-- Table grocerystoredb.marketingcampaign

DROP TABLE IF EXISTS grocerystoredb.marketingcampaign ;

CREATE TABLE IF NOT EXISTS grocerystoredb.marketingcampaign ( Campaign_ID INT NOT NULL AUTO_INCREMENT, Campaign_Name VARCHAR(255) NOT NULL, Start_Date DATE NOT NULL, End_Date DATE NOT NULL, Budget DECIMAL(10,2) NOT NULL, PRIMARY KEY (Campaign_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.category

DROP TABLE IF EXISTS grocerystoredb.category ;

CREATE TABLE IF NOT EXISTS grocerystoredb.category ( Category_ID INT NOT NULL AUTO_INCREMENT, Category_Name VARCHAR(255) NOT NULL, PRIMARY KEY (Category_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.product

DROP TABLE IF EXISTS grocerystoredb.product ;

CREATE TABLE IF NOT EXISTS grocerystoredb.product ( Product_ID INT NOT NULL AUTO_INCREMENT, Product_Name VARCHAR(255) NOT NULL, Category_ID INT NULL DEFAULT NULL, Price DECIMAL(10,2) NOT NULL, PRIMARY KEY (Product_ID), INDEX Category_ID (Category_ID ASC) VISIBLE, CONSTRAINT product_ibfk_1 FOREIGN KEY (Category_ID) REFERENCES grocerystoredb.category (Category_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.campaignproduct

DROP TABLE IF EXISTS grocerystoredb.campaignproduct ;

CREATE TABLE IF NOT EXISTS grocerystoredb.campaignproduct ( CampaignProduct_ID INT NOT NULL AUTO_INCREMENT, Campaign_ID INT NULL DEFAULT NULL, Product_ID INT NULL DEFAULT NULL, Discount DECIMAL(10,2) NOT NULL, PRIMARY KEY (CampaignProduct_ID), INDEX Campaign_ID (Campaign_ID ASC) VISIBLE, INDEX Product_ID (Product_ID ASC) VISIBLE, CONSTRAINT campaignproduct_ibfk_1 FOREIGN KEY (Campaign_ID) REFERENCES grocerystoredb.marketingcampaign (Campaign_ID), CONSTRAINT campaignproduct_ibfk_2 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.employee

DROP TABLE IF EXISTS grocerystoredb.employee ;

CREATE TABLE IF NOT EXISTS grocerystoredb.employee ( Employee_ID INT NOT NULL AUTO_INCREMENT, Employee_Name VARCHAR(255) NOT NULL, Position VARCHAR(255) NOT NULL, Salary DECIMAL(10,2) NOT NULL, PRIMARY KEY (Employee_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.cashregister

DROP TABLE IF EXISTS grocerystoredb.cashregister ;

CREATE TABLE IF NOT EXISTS grocerystoredb.cashregister ( Register_ID INT NOT NULL AUTO_INCREMENT, Employee_ID INT NULL DEFAULT NULL, Shift_Start DATE NOT NULL, Shift_End DATE NOT NULL, Total_Cash DECIMAL(10,2) NOT NULL, PRIMARY KEY (Register_ID), INDEX Employee_ID (Employee_ID ASC) VISIBLE, CONSTRAINT cashregister_ibfk_1 FOREIGN KEY (Employee_ID) REFERENCES grocerystoredb.employee (Employee_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.customer

DROP TABLE IF EXISTS grocerystoredb.customer ;

CREATE TABLE IF NOT EXISTS grocerystoredb.customer ( Customer_ID INT NOT NULL AUTO_INCREMENT, Customer_Name VARCHAR(255) NOT NULL, Email VARCHAR(255) NOT NULL, PRIMARY KEY (Customer_ID), UNIQUE INDEX Email (Email ASC) VISIBLE) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table `grocerystoredb`.`clubcard`
DROP TABLE IF EXISTS `grocerystoredb`.`clubcard`;

CREATE TABLE IF NOT EXISTS `grocerystoredb`.`clubcard` (
  `Card_ID` INT NOT NULL,
  `Points_Balance` INT NOT NULL,
  `Customer_ID` INT NULL DEFAULT NULL,
  `Last_Updated` TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`Card_ID`),
  UNIQUE INDEX `Customer_ID` (`Customer_ID` ASC) VISIBLE, -- Ensure Customer_ID is unique
  CONSTRAINT `clubcard_ibfk_1`
    FOREIGN KEY (`Customer_ID`)
    REFERENCES `grocerystoredb`.`customer` (`Customer_ID`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8mb4
COLLATE = utf8mb4_0900_ai_ci;
-- Table grocerystoredb.customeraddress

DROP TABLE IF EXISTS grocerystoredb.customeraddress ;

CREATE TABLE IF NOT EXISTS grocerystoredb.customeraddress ( Address_ID INT NOT NULL AUTO_INCREMENT, Customer_ID INT NULL DEFAULT NULL, Address_Line_1 VARCHAR(255) NOT NULL, Address_Line_2 VARCHAR(255) NULL DEFAULT NULL, City VARCHAR(255) NOT NULL, State VARCHAR(255) NULL DEFAULT NULL, ZipCode VARCHAR(20) NULL DEFAULT NULL, Country VARCHAR(255) NULL DEFAULT NULL, PRIMARY KEY (Address_ID), INDEX Customer_ID (Customer_ID ASC) VISIBLE, CONSTRAINT customeraddress_ibfk_1 FOREIGN KEY (Customer_ID) REFERENCES grocerystoredb.customer (Customer_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.customerfeedback

DROP TABLE IF EXISTS grocerystoredb.customerfeedback ;

CREATE TABLE IF NOT EXISTS grocerystoredb.customerfeedback ( Feedback_ID INT NOT NULL AUTO_INCREMENT, Customer_ID INT NULL DEFAULT NULL, Feedback_Date DATE NOT NULL, Rating INT NOT NULL, Comments TEXT NULL DEFAULT NULL, PRIMARY KEY (Feedback_ID), INDEX Customer_ID (Customer_ID ASC) VISIBLE, CONSTRAINT customerfeedback_ibfk_1 FOREIGN KEY (Customer_ID) REFERENCES grocerystoredb.customer (Customer_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.inventory

DROP TABLE IF EXISTS grocerystoredb.inventory ;

CREATE TABLE IF NOT EXISTS grocerystoredb.inventory ( Inventory_ID INT NOT NULL AUTO_INCREMENT, Product_ID INT NULL DEFAULT NULL, Stock_Quantity INT NOT NULL, Last_Updated TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP, PRIMARY KEY (Inventory_ID), INDEX Product_ID (Product_ID ASC) VISIBLE, CONSTRAINT inventory_ibfk_1 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.inventoryadjustment

DROP TABLE IF EXISTS grocerystoredb.inventoryadjustment ;

CREATE TABLE IF NOT EXISTS grocerystoredb.inventoryadjustment ( Adjustment_ID INT NOT NULL AUTO_INCREMENT, Product_ID INT NULL DEFAULT NULL, Adjustment_Date DATE NOT NULL, Adjusted_Quantity INT NOT NULL, Reason TEXT NULL DEFAULT NULL, PRIMARY KEY (Adjustment_ID), INDEX Product_ID (Product_ID ASC) VISIBLE, CONSTRAINT inventoryadjustment_ibfk_1 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.inventorycheck

DROP TABLE IF EXISTS grocerystoredb.inventorycheck ;

CREATE TABLE IF NOT EXISTS grocerystoredb.inventorycheck ( InventoryCheck_ID INT NOT NULL AUTO_INCREMENT, Product_ID INT NULL DEFAULT NULL, Check_Date DATE NOT NULL, Stock_Count INT NOT NULL, PRIMARY KEY (InventoryCheck_ID), INDEX Product_ID (Product_ID ASC) VISIBLE, CONSTRAINT inventorycheck_ibfk_1 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.order

DROP TABLE IF EXISTS grocerystoredb.order ;

CREATE TABLE IF NOT EXISTS grocerystoredb.order ( Order_ID INT NOT NULL AUTO_INCREMENT, Customer_ID INT NULL DEFAULT NULL, Order_Date DATE NOT NULL, Discount_Amount DECIMAL(10,2) NULL DEFAULT '0.00', Total_Amount DECIMAL(10,2) NOT NULL, Order_Status ENUM('Pending', 'Complete') NOT NULL DEFAULT 'Pending', PRIMARY KEY (Order_ID), INDEX Customer_ID (Customer_ID ASC) VISIBLE, CONSTRAINT order_ibfk_1 FOREIGN KEY (Customer_ID) REFERENCES grocerystoredb.customer (Customer_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.orderitem

DROP TABLE IF EXISTS grocerystoredb.orderitem ;

CREATE TABLE IF NOT EXISTS grocerystoredb.orderitem ( OrderItem_ID INT NOT NULL AUTO_INCREMENT, Order_ID INT NULL DEFAULT NULL, Product_ID INT NULL DEFAULT NULL, Quantity INT NOT NULL, Price DECIMAL(10,2) NOT NULL, PRIMARY KEY (OrderItem_ID), INDEX Order_ID (Order_ID ASC) VISIBLE, INDEX Product_ID (Product_ID ASC) VISIBLE, CONSTRAINT orderitem_ibfk_1 FOREIGN KEY (Order_ID) REFERENCES grocerystoredb.order (Order_ID), CONSTRAINT orderitem_ibfk_2 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.orderstatushistory

DROP TABLE IF EXISTS grocerystoredb.orderstatushistory ;

CREATE TABLE IF NOT EXISTS grocerystoredb.orderstatushistory ( History_ID INT NOT NULL AUTO_INCREMENT, Order_ID INT NULL DEFAULT NULL, Status_ID INT NULL DEFAULT NULL, Change_Date DATE NOT NULL, PRIMARY KEY (History_ID), INDEX Order_ID (Order_ID ASC) VISIBLE, INDEX Status_ID (Status_ID ASC) VISIBLE, CONSTRAINT orderstatushistory_ibfk_1 FOREIGN KEY (Order_ID) REFERENCES grocerystoredb.order (Order_ID), CONSTRAINT orderstatushistory_ibfk_2 FOREIGN KEY (Status_ID) REFERENCES grocerystoredb.orderstatus (Status_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.payment

DROP TABLE IF EXISTS grocerystoredb.payment ;

CREATE TABLE IF NOT EXISTS grocerystoredb.payment ( Payment_ID INT NOT NULL AUTO_INCREMENT, Order_ID INT NULL DEFAULT NULL, Payment_Date DATE NOT NULL, Amount DECIMAL(10,2) NOT NULL, PRIMARY KEY (Payment_ID), INDEX Order_ID (Order_ID ASC) VISIBLE, CONSTRAINT payment_ibfk_1 FOREIGN KEY (Order_ID) REFERENCES grocerystoredb.order (Order_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.paymentmethod

DROP TABLE IF EXISTS grocerystoredb.paymentmethod ;

CREATE TABLE IF NOT EXISTS grocerystoredb.paymentmethod ( PaymentMethod_ID INT NOT NULL AUTO_INCREMENT, Customer_ID INT NULL DEFAULT NULL, Payment_Type VARCHAR(50) NOT NULL, Account_Number VARCHAR(255) NULL DEFAULT NULL, Expiry_Date DATE NULL DEFAULT NULL, PRIMARY KEY (PaymentMethod_ID), INDEX Customer_ID (Customer_ID ASC) VISIBLE, CONSTRAINT paymentmethod_ibfk_1 FOREIGN KEY (Customer_ID) REFERENCES grocerystoredb.customer (Customer_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.pointstransaction

DROP TABLE IF EXISTS grocerystoredb.pointstransaction ;

CREATE TABLE IF NOT EXISTS grocerystoredb.pointstransaction ( Transaction_ID INT NOT NULL AUTO_INCREMENT, Card_ID INT NULL DEFAULT NULL, Transaction_Type VARCHAR(50) NULL DEFAULT NULL, Points_Changed INT NULL DEFAULT NULL, Transaction_Date TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP, PRIMARY KEY (Transaction_ID), INDEX Card_ID (Card_ID ASC) VISIBLE, CONSTRAINT pointstransaction_ibfk_1 FOREIGN KEY (Card_ID) REFERENCES grocerystoredb.clubcard (Card_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.priceadjustment

DROP TABLE IF EXISTS grocerystoredb.priceadjustment ;

CREATE TABLE IF NOT EXISTS grocerystoredb.priceadjustment ( Adjustment_ID INT NOT NULL AUTO_INCREMENT, Product_ID INT NULL DEFAULT NULL, Adjustment_Date DATE NOT NULL, New_Price DECIMAL(10,2) NOT NULL, Reason TEXT NULL DEFAULT NULL, PRIMARY KEY (Adjustment_ID), INDEX Product_ID (Product_ID ASC) VISIBLE, CONSTRAINT priceadjustment_ibfk_1 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.productavailability

DROP TABLE IF EXISTS grocerystoredb.productavailability ;

CREATE TABLE IF NOT EXISTS grocerystoredb.productavailability ( Availability_ID INT NOT NULL AUTO_INCREMENT, Product_ID INT NULL DEFAULT NULL, Available_Stock INT NOT NULL, Update_Date DATE NOT NULL, PRIMARY KEY (Availability_ID), INDEX Product_ID (Product_ID ASC) VISIBLE, CONSTRAINT productavailability_ibfk_1 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.productcategorytable

DROP TABLE IF EXISTS grocerystoredb.productcategorytable ;

CREATE TABLE IF NOT EXISTS grocerystoredb.productcategorytable ( ProductCategory_ID INT NOT NULL AUTO_INCREMENT, Product_ID INT NULL DEFAULT NULL, Category_ID INT NULL DEFAULT NULL, PRIMARY KEY (ProductCategory_ID), INDEX Product_ID (Product_ID ASC) VISIBLE, INDEX Category_ID (Category_ID ASC) VISIBLE, CONSTRAINT productcategorytable_ibfk_1 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID), CONSTRAINT productcategorytable_ibfk_2 FOREIGN KEY (Category_ID) REFERENCES grocerystoredb.category (Category_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.promotion

DROP TABLE IF EXISTS grocerystoredb.promotion ;

CREATE TABLE IF NOT EXISTS grocerystoredb.promotion ( Promotion_ID INT NOT NULL AUTO_INCREMENT, Promotion_Name VARCHAR(255) NOT NULL, Discount_Percentage DECIMAL(5,2) NOT NULL, Start_Date DATE NOT NULL, End_Date DATE NOT NULL, PRIMARY KEY (Promotion_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.productpromotion

DROP TABLE IF EXISTS grocerystoredb.productpromotion ;

CREATE TABLE IF NOT EXISTS grocerystoredb.productpromotion ( ProductPromotion_ID INT NOT NULL AUTO_INCREMENT, Product_ID INT NULL DEFAULT NULL, Promotion_ID INT NULL DEFAULT NULL, PRIMARY KEY (ProductPromotion_ID), INDEX Product_ID (Product_ID ASC) VISIBLE, INDEX Promotion_ID (Promotion_ID ASC) VISIBLE, CONSTRAINT productpromotion_ibfk_1 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID), CONSTRAINT productpromotion_ibfk_2 FOREIGN KEY (Promotion_ID) REFERENCES grocerystoredb.promotion (Promotion_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.productreview

DROP TABLE IF EXISTS grocerystoredb.productreview ;

CREATE TABLE IF NOT EXISTS grocerystoredb.productreview ( Review_ID INT NOT NULL AUTO_INCREMENT, Product_ID INT NULL DEFAULT NULL, Customer_ID INT NULL DEFAULT NULL, Rating INT NOT NULL, Review_Date DATE NOT NULL, Comments TEXT NULL DEFAULT NULL, PRIMARY KEY (Review_ID), INDEX Product_ID (Product_ID ASC) VISIBLE, INDEX Customer_ID (Customer_ID ASC) VISIBLE, CONSTRAINT productreview_ibfk_1 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID), CONSTRAINT productreview_ibfk_2 FOREIGN KEY (Customer_ID) REFERENCES grocerystoredb.customer (Customer_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.supplier

DROP TABLE IF EXISTS grocerystoredb.supplier ;

CREATE TABLE IF NOT EXISTS grocerystoredb.supplier ( Supplier_ID INT NOT NULL AUTO_INCREMENT, Supplier_Name VARCHAR(255) NOT NULL, Contact_Info VARCHAR(255) NULL DEFAULT NULL, PRIMARY KEY (Supplier_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.productsuppliercontact

DROP TABLE IF EXISTS grocerystoredb.productsuppliercontact ;

CREATE TABLE IF NOT EXISTS grocerystoredb.productsuppliercontact ( Contact_ID INT NOT NULL AUTO_INCREMENT, Product_ID INT NULL DEFAULT NULL, Supplier_ID INT NULL DEFAULT NULL, Contact_Date DATE NOT NULL, Notes TEXT NULL DEFAULT NULL, PRIMARY KEY (Contact_ID), INDEX Product_ID (Product_ID ASC) VISIBLE, INDEX Supplier_ID (Supplier_ID ASC) VISIBLE, CONSTRAINT productsuppliercontact_ibfk_1 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID), CONSTRAINT productsuppliercontact_ibfk_2 FOREIGN KEY (Supplier_ID) REFERENCES grocerystoredb.supplier (Supplier_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.producttag

DROP TABLE IF EXISTS grocerystoredb.producttag ;

CREATE TABLE IF NOT EXISTS grocerystoredb.producttag ( Tag_ID INT NOT NULL AUTO_INCREMENT, Product_ID INT NULL DEFAULT NULL, Tag_Name VARCHAR(255) NOT NULL, PRIMARY KEY (Tag_ID), INDEX Product_ID (Product_ID ASC) VISIBLE, CONSTRAINT producttag_ibfk_1 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.refund

DROP TABLE IF EXISTS grocerystoredb.refund ;

CREATE TABLE IF NOT EXISTS grocerystoredb.refund ( Refund_ID INT NOT NULL AUTO_INCREMENT, Order_ID INT NULL DEFAULT NULL, Refund_Date DATE NOT NULL, Refund_Amount DECIMAL(10,2) NOT NULL, Refund_Method VARCHAR(50) NULL DEFAULT NULL, PRIMARY KEY (Refund_ID), INDEX Order_ID (Order_ID ASC) VISIBLE, CONSTRAINT refund_ibfk_1 FOREIGN KEY (Order_ID) REFERENCES grocerystoredb.order (Order_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.return

DROP TABLE IF EXISTS grocerystoredb.return ;

CREATE TABLE IF NOT EXISTS grocerystoredb.return ( Return_ID INT NOT NULL, OrderItem_ID INT NOT NULL, Return_Date DATE NOT NULL, Quantity_Returned INT NOT NULL, Reason TEXT NULL DEFAULT NULL, PRIMARY KEY (Return_ID), INDEX OrderItem_ID (OrderItem_ID ASC) VISIBLE, CONSTRAINT return_ibfk_1 FOREIGN KEY (OrderItem_ID) REFERENCES grocerystoredb.orderitem (OrderItem_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.salestransaction

DROP TABLE IF EXISTS grocerystoredb.salestransaction ;

CREATE TABLE IF NOT EXISTS grocerystoredb.salestransaction ( Transaction_ID INT NOT NULL AUTO_INCREMENT, Employee_ID INT NULL DEFAULT NULL, Customer_ID INT NULL DEFAULT NULL, Transaction_Date DATE NOT NULL, Amount DECIMAL(10,2) NOT NULL, PRIMARY KEY (Transaction_ID), INDEX Employee_ID (Employee_ID ASC) VISIBLE, INDEX Customer_ID (Customer_ID ASC) VISIBLE, CONSTRAINT salestransaction_ibfk_1 FOREIGN KEY (Employee_ID) REFERENCES grocerystoredb.employee (Employee_ID), CONSTRAINT salestransaction_ibfk_2 FOREIGN KEY (Customer_ID) REFERENCES grocerystoredb.customer (Customer_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.salesreport

DROP TABLE IF EXISTS grocerystoredb.salesreport ;

CREATE TABLE IF NOT EXISTS grocerystoredb.salesreport ( Report_ID INT NOT NULL AUTO_INCREMENT, Employee_ID INT NULL DEFAULT NULL, Transaction_ID INT NULL DEFAULT NULL, Report_Date DATE NOT NULL, Total_Sales DECIMAL(10,2) NOT NULL, PRIMARY KEY (Report_ID), INDEX Employee_ID (Employee_ID ASC) VISIBLE, INDEX Transaction_ID (Transaction_ID ASC) VISIBLE, CONSTRAINT salesreport_ibfk_1 FOREIGN KEY (Employee_ID) REFERENCES grocerystoredb.employee (Employee_ID), CONSTRAINT salesreport_ibfk_2 FOREIGN KEY (Transaction_ID) REFERENCES grocerystoredb.salestransaction (Transaction_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.shipment

DROP TABLE IF EXISTS grocerystoredb.shipment ;

CREATE TABLE IF NOT EXISTS grocerystoredb.shipment ( Shipment_ID INT NOT NULL AUTO_INCREMENT, Order_ID INT NULL DEFAULT NULL, Address_ID INT NULL DEFAULT NULL, Shipment_Date DATE NOT NULL, Estimated_Arrival DATE NULL DEFAULT NULL, Shipping_Cost DECIMAL(10,2) NOT NULL, PRIMARY KEY (Shipment_ID), INDEX Order_ID (Order_ID ASC) VISIBLE, INDEX Address_ID (Address_ID ASC) VISIBLE, CONSTRAINT shipment_ibfk_1 FOREIGN KEY (Order_ID) REFERENCES grocerystoredb.order (Order_ID), CONSTRAINT shipment_ibfk_2 FOREIGN KEY (Address_ID) REFERENCES grocerystoredb.customeraddress (Address_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.staffrole

DROP TABLE IF EXISTS grocerystoredb.staffrole ;

CREATE TABLE IF NOT EXISTS grocerystoredb.staffrole ( StaffRole_ID INT NOT NULL AUTO_INCREMENT, Employee_ID INT NULL DEFAULT NULL, Role VARCHAR(255) NOT NULL, PRIMARY KEY (StaffRole_ID), INDEX Employee_ID (Employee_ID ASC) VISIBLE, CONSTRAINT staffrole_ibfk_1 FOREIGN KEY (Employee_ID) REFERENCES grocerystoredb.employee (Employee_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.supplierproduct

DROP TABLE IF EXISTS grocerystoredb.supplierproduct ;

CREATE TABLE IF NOT EXISTS grocerystoredb.supplierproduct ( SupplierProduct_ID INT NOT NULL AUTO_INCREMENT, Supplier_ID INT NULL DEFAULT NULL, Product_ID INT NULL DEFAULT NULL, Price DECIMAL(10,2) NOT NULL, PRIMARY KEY (SupplierProduct_ID), INDEX Supplier_ID (Supplier_ID ASC) VISIBLE, INDEX Product_ID (Product_ID ASC) VISIBLE, CONSTRAINT supplierproduct_ibfk_1 FOREIGN KEY (Supplier_ID) REFERENCES grocerystoredb.supplier (Supplier_ID), CONSTRAINT supplierproduct_ibfk_2 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.supplierinvoice

DROP TABLE IF EXISTS grocerystoredb.supplierinvoice ;

CREATE TABLE IF NOT EXISTS grocerystoredb.supplierinvoice ( Invoice_ID INT NOT NULL AUTO_INCREMENT, Supplier_ID INT NULL DEFAULT NULL, Invoice_Date DATE NOT NULL, Amount DECIMAL(10,2) NOT NULL, Payment_Status VARCHAR(50) NULL DEFAULT NULL, PRIMARY KEY (Invoice_ID), INDEX Supplier_ID (Supplier_ID ASC) VISIBLE, CONSTRAINT supplierinvoice_ibfk_1 FOREIGN KEY (Supplier_ID) REFERENCES grocerystoredb.supplier (Supplier_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.stockreplenishment

DROP TABLE IF EXISTS grocerystoredb.stockreplenishment ;

CREATE TABLE IF NOT EXISTS grocerystoredb.stockreplenishment ( Replenishment_ID INT NOT NULL AUTO_INCREMENT, SupplierProduct_ID INT NULL DEFAULT NULL, Replenishment_Date DATE NOT NULL, Quantity INT NOT NULL, Invoice_ID INT NOT NULL, PRIMARY KEY (Replenishment_ID), INDEX SupplierProduct_ID (SupplierProduct_ID ASC) VISIBLE, INDEX fk_stockreplenishment_supplierinvoice1_idx (Invoice_ID ASC) VISIBLE, CONSTRAINT stockreplenishment_ibfk_1 FOREIGN KEY (SupplierProduct_ID) REFERENCES grocerystoredb.supplierproduct (SupplierProduct_ID), CONSTRAINT fk_stockreplenishment_supplierinvoice1 FOREIGN KEY (Invoice_ID) REFERENCES grocerystoredb.supplierinvoice (Invoice_ID) ON DELETE NO ACTION ON UPDATE NO ACTION) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.supplierpayment

DROP TABLE IF EXISTS grocerystoredb.supplierpayment ;

CREATE TABLE IF NOT EXISTS grocerystoredb.supplierpayment ( Payment_ID INT NOT NULL AUTO_INCREMENT, Invoice_ID INT NULL DEFAULT NULL, Payment_Date DATE NOT NULL, Amount DECIMAL(10,2) NOT NULL, Payment_Method VARCHAR(50) NULL DEFAULT NULL, PRIMARY KEY (Payment_ID), INDEX Invoice_ID (Invoice_ID ASC) VISIBLE, CONSTRAINT supplierpayment_ibfk_1 FOREIGN KEY (Invoice_ID) REFERENCES grocerystoredb.supplierinvoice (Invoice_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- Table grocerystoredb.supplierstock

DROP TABLE IF EXISTS grocerystoredb.supplierstock ;

CREATE TABLE IF NOT EXISTS grocerystoredb.supplierstock ( SupplierStock_ID INT NOT NULL AUTO_INCREMENT, Supplier_ID INT NULL DEFAULT NULL, Product_ID INT NULL DEFAULT NULL, Quantity INT NOT NULL, PRIMARY KEY (SupplierStock_ID), INDEX Supplier_ID (Supplier_ID ASC) VISIBLE, INDEX Product_ID (Product_ID ASC) VISIBLE, CONSTRAINT supplierstock_ibfk_1 FOREIGN KEY (Supplier_ID) REFERENCES grocerystoredb.supplier (Supplier_ID), CONSTRAINT supplierstock_ibfk_2 FOREIGN KEY (Product_ID) REFERENCES grocerystoredb.product (Product_ID)) ENGINE = InnoDB DEFAULT CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

SET SQL_MODE=@OLD_SQL_MODE; SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS; SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
