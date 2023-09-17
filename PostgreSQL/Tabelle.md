
```sql
CREATE TABLE Customer (
    Id SERIAL PRIMARY KEY,
    FirstName VARCHAR(125) NOT NULL,
    LastName VARCHAR(125) NOT NULL,
    CustomerNumber VARCHAR(20) NOT NULL,
    BirthDay DATE NOT NULL,
    Nationality VARCHAR(50) NOT NULL
);

CREATE TABLE Additional_Contract_Fee_Template (
    Id SERIAL PRIMARY KEY,
    AdditionalContractFeeName VARCHAR(60) NOT NULL,
    AdditionalContractFeeDescription VARCHAR NOT NULL,
    IsPayedOnce BOOLEAN NOT NULL DEFAULT TRUE,
    Fee DECIMAL NOT NULL
);

CREATE TABLE Contract_Template (
    Id SERIAL PRIMARY KEY,
    ContractName VARCHAR(60) NOT NULL,
    Description VARCHAR NOT NULL,
    RecurringFee DECIMAL NOT NULL,
    IsPayedMonthly BOOLEAN DEFAULT TRUE
);

CREATE TABLE Customer_Address (
    Id SERIAL PRIMARY KEY,
    CustomerId INT NOT NULL,
    Street VARCHAR(100) NOT NULL,
    HouseNumber VARCHAR(10) NOT NULL,
    ZipCode VARCHAR(10) NOT NULL,
    City VARCHAR(50) NOT NULL,
    CountryCode VARCHAR(10) NOT NULL,
    IsPrimaryAddress BOOLEAN DEFAULT FALSE,
    CONSTRAINT FK_CUSTOMER_ADDRESS_CUSTOMER FOREIGN KEY(CustomerId) REFERENCES Customer(Id)
);

CREATE TABLE Customer_Contract (
    Id SERIAL PRIMARY KEY,
    CustomerId INT NOT NULL,
    ContractTemplateId INT NOT NULL,
    IsSigned BOOLEAN NOT NULL DEFAULT FALSE,
    CONSTRAINT FK_CUSTOMER_CONTRACT_CUSTOMER FOREIGN KEY(CustomerId) REFERENCES Customer(Id),
    CONSTRAINT FK_CUSTOMER_CONTRACT_TEMPLATE FOREIGN KEY(ContractTemplateId) REFERENCES Contract_Template(Id)
);

CREATE TABLE Additional_Contract_Fee (
    Id SERIAL PRIMARY KEY,
    CustomerContractId INT NOT NULL,
    AdditionalContractFeeTemplateId INT NOT NULL,
    CONSTRAINT FK_CONTRACT_ADDITIONAL_FEE_TEMPLATE FOREIGN KEY(CustomerContractId) REFERENCES Customer_Contract(Id),
    CONSTRAINT FK_CONTRACT_ADDITIONAL_FEE_TEMPLATE_FEE FOREIGN KEY(AdditionalContractFeeTemplateId) REFERENCES Additional_Contract_Fee_Template(Id)
);


```

![[Pasted image 20230916124011.png]]