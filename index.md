# Mont-Blanc Web Service
DESCRIPTION OF THE WEB SERVICE FOR ORDERING TRAVEL INSURANCE 

© TRAVEL SUPPORT SYSTEMS 2023

## Content

- [Mont-Blanc Web Service](#mont-blanc-web-service)
  - [Content](#content)
  - [Introduction and usage](#introduction-and-usage)
    - [Insurance products](#insurance-products)
    - [Basic Features of the Interface](#basic-features-of-the-interface)
    - [Using the Service](#using-the-service)
    - [Security](#security)
    - [Endpoint](#endpoint)
    - [Online tester](#online-tester)
  - [Insurance calculation with UNQCalculator](#insurance-calculation-with-unqcalculator)
    - [Request format](#request-format)
    - [Response format](#response-format)
  - [Calculation with UNQInsuranceOrder](#calculation-with-unqinsuranceorder)
    - [Request format](#request-format-1)
    - [Reponse format](#reponse-format)
  - [Create order with UNQInsuranceOrder](#create-order-with-unqinsuranceorder)
    - [Request format](#request-format-2)
    - [Reposnse format](#reposnse-format)
  - [Cancel an order UNQDeleteOrder](#cancel-an-order-unqdeleteorder)
    - [Request format](#request-format-3)
    - [Response format](#response-format-1)
  - [Activate an order UNQPaymentConfirm](#activate-an-order-unqpaymentconfirm)
    - [Request format](#request-format-4)
    - [Response fomat:](#response-fomat)
  - [Insurance order information UNQOrderInfo](#insurance-order-information-unqorderinfo)
    - [Request format](#request-format-5)
    - [Response format](#response-format-2)
  - [Contacts](#contacts)
  - [Annex 1 Risk zones](#annex-1-risk-zones)
  - [Annex 2 Error codes](#annex-2-error-codes)


## Introduction and usage

WS Mont-Blanc is a platform designed to facilitate insurance processes. It offers the following features:

- Requesting product information and pricing without any obligations.
- Directly inserting draft insurance contracts.
- Confirming payment, especially when collecting payments through partner channels.
- Canceling a paid order if it hasn't been processed yet (up until 24:00 on the day of payment).
- Accessing information about draft insurance contracts.
- Downloading and printing draft contracts and other documents in PDF format. This access also includes assistance cards after payment.
  
Important Note: All records stored within the system are considered as draft contracts. The transition of a draft contract to a full insurance contract occurs only upon the transfer of the insurance premium payment. This transition is initiated either by launching the ```UNQPaymentConfirm``` process or by the client making the insurance premium payment to the insurance company's account. The export of these finalized contracts takes place daily at 24:00.

Please make sure to adapt this corrected version according to your specific context if needed.

### Insurance products

Currently, it is possible to execute orders of UNIQA products:

**Table 1 UNIQA insurance products**

|**Product** |**Short**|
| :- | :- |
|**Annual travel insurance Basic**|RBAN|
|**Annual travel insurance Plus**|RPLN|
|**Annual travel insurance Comfort**|RKON|
|**Annual travel insurance Extra**|REXN|
|**Annual travel insurance for professional drivers Basic**|RBAV|
|**Annual travel insurance for professional drivers Plus**|RPLV|
|**Travel insurance Comfort**|KO|
|**Travel insurance Extra**|EX|
|**Travel Insurance Basic**|BA|
|**Travel Insurance Plus**|PL|
|**Trip cancellation insurance**|ST|
|**Travel insurance in Slovakia**|CPSK|
|**Children's camps in Slovakia**|DTSK|
|**Travel Insurance Student**|STUD|
|**Ticket cancellation insurance**|STV|

The set of insurance products available to clients on the Service may vary according to the contractual relationship with the insurance company.


### Basic Features of the Interface

- Ability to order insurance for more than one person with the same insurance conditions (same type of insurance, period, and place of residence).
- Family discount of 20% applies to the total amount for all persons. Annual products have custom family rates - refer to the Product Manual of Annual Products for details.
- Age-based products restriction:
  - Short-term *BASIC* and *COMFORT* available for individuals up to 70 years old at the time of arranging insurance.
  - Annual BASIC, PLUS, and COMFORT available for individuals up to 70 years old at the time of arranging insurance.
  - Annual BASIC and PLUS driver plans available for individuals up to 65 years old at the start of insurance.
  - Children's camps in Slovakia available for those under 18 years old at the time of arranging insurance.
- Interface supports COMFORT, EXTRA, and CANCELLATION up to a maximum of 15,000 EUR/person service. Attempting a higher amount triggers an error, contact CP Manager: <tomas.mokos@uniqa.sk>.
- Extended assistance services insurance available for up to five passengers, valid for selected European countries (see Annex 1) within the insurance period, up to 30 days.
- Cancellation insurance for COMFORT and EXTRA products is automatically handled by a separate policy if service price exceeds the included threshold.
- 15% discount on total insurance price for groups of 15 or more people.
- Submitting an order query (UNQInsuranceOrder) without OrderNo returns premium price without recording in the database. Such queries only need calculation data.
- Queries with OrderNo (for ordrers creation) must fill all required data. Mark with (M) - mandatory.
- Parners can use their own order numbering (OrderNo) with an assigned prefix. Reusing an order number overwrites the old order with the new one if unpaid.
- Using "..." in OrderNo field assigns a unique order identifier.
- Partners who not collecting premiums can use OrderNo with prefix as variable symbol for identifying insurance premium payments from clients.
- Use test UserKey for testing, a MontBlanc UserKey change is needed for production.


### Using the Service

1. **Calculation:** You have two options to determine products and their prices:
  - `UNQCalculator`: This will provide possible insurance types and their prices based on basic selections.
  - `UNQInsuranceOrder` without an OrderNo: Premiums will be calculated and returned for a specific product.

2. **Create orders:** To save an order as a draft insurance policy while calculating:
  - `UNQInsuranceOrder` with an OrderNo: This action stores the order in Mont-Blanc.

3. **Order Delete:** You can purge an order using `UNQDeleteOrder`, provided the order hasn't been activated by `UNQPaymentConfirm`.

4. **Activating Your Order:**
  - If payment is made through a payment gateway (e.g., by transferring to the insurance company's account), the order is automatically activated.
  - For collection partners, each order needs activation via `UNQPaymentConfirm`. Typically, partners use this when all ordered services are paid by the client.

5. **Draft Insurance Contract Information:**
  - Use the `UNQOrderInfo` query to obtain real-time details about the draft insurance policy's status.


### Security
To access WS Mont-Blanc, you need to activate the XML service in office Mont-Blanc in the User Profile office mont-blanc and add the required data (as a manager) to generate the **UserKey** passcode that is available to users who have XMLdirect enabled. In addition, it is necessary to get access from a specific IP address by sending [an administration request](https://www.mont-blanc.sk/modules.php?name=Poziadavky).
 
IP address setting from which you will call XMLdirect. IPv4 is expected in e.g. 127.0.0.1 format, or ip/CIDR address range with network mask, e.g. 127.0.0.0/24.

**Notes:**
External IP address is show even in the footer of the [tester](https://www.mont-blanc.sk/webservices/tester.php) page. 

### Endpoint
[**https://www.mont-blanc.sk/webservices/xmldirect.php**](https://www.mont-blanc.sk/webservices/xmldirect.php)

**Notes:**
- Query this URL using the POST method in the variable name="xml"
- The script is for EUR currency only.

### Online tester

Requests and Responses can be tested  online at this address
[**https://www.mont-blanc.sk/webservices/tester.php**](https://www.mont-blanc.sk/webservices/tester.php)

## Insurance calculation with UNQCalculator

The calculator is a tool that simplifies the selection of insurance products according to the input parameters. 

### Request format

|Tag Name|Description|
| :- | :- |
|UNQCalculator|Main Tag|
|UserKey|Assigned code for user authorization|
|Ticketing|1=ticket insurance, 0=travel insurance|
|Area|W=world, E=Europe, SK=Slovakia |
|DateFrom|Start Date Format: D.M.YYYY|
|DateTo|End date, for annual (Annual=1), makes no sense|
|Annual|Annual travel insurance - 1=annual, 0=short-term|
|Aupair|Study residence insurance abroad 1=Au-pair/student, 0=unelected|
|Bussiness|Business trip insurance 1=manual work, 0=unselected|
|MedCosts|Insurance of medical expenses abroad 1=medical costs, 0=unelected|
|Cancellation|Insurance cancellation fee - 1 =selected, 0 =unelected|
|LegailAid|Insurance containing legal protection 1=selected 0=unelected|
|Luggage|Luggage insurance - 1 =selected, 0=unelected|
|Damage|Foreign liability insurance - 1=selected, 0=unelected|
|Injury|Accident insurance - 1=selected, 0=unelected|
|CarAssistance|Extended assistance services AUTO 1=selected, 0=unelected|
|TourPrice|Price in EUR for all persons|
|Children|Number of children (<18 years), integer|
|Adults|Number of adults, integer|
|Seniors|Number of seniors (70+) |

### Response format

|Tag name:|Description:|
| :- | :- |
|UNQCalcResponse|Main Tag|
|Products||
|- Product||
|- InsuranceType|Abbreviation of the type of insurance |
|- Supplement|Skratka pripoistenia|
|- SumPrice|Price in EUR|
|- Info|Insurance name for display|

**Notes:** 

- The abbreviation for the insurance type is applicable to **InsuranceType** in the `UNQInsuranceOrder` command through which the order can follow.
- Request example and response see [online tester](#online-tester).


## Calculation with UNQInsuranceOrder

`UNQInsuranceOrder` returns only a response without saving if OrderNo is not filled in.  

### Request format

|Tag Name|Length and type|Description|
| :--- | :--- | :--- |
|UNQInsuranceOrder||Main Tag|
|UserKey|32, String|Code assigned to you to indicate the insurance intermediary|
|DateFrom|D.M.YYYY, Date|Date of validity from|
|DateTo|D.M.YYYY, Date|Date of validity until|
|InsuranceType|4, String|Abbreviation of the insurance type according to Table 1|
|Currency|3, String|Unit of currency (dafault value: EUR)|
|Area|2, String|W = World<br>E = Europe,<br>SK = Slovakia<br> If it is filled **CountryCode** so  **Area** is optional|
|TotalTourPrice|Double<br>(Max.2 decimal places for the EUR currency. Integer for CZK)|The total price of the tour, ticket, or other services together, for all persons in the units according to the currency used|
|CountryCode|3, String|Country code according to the Annex, for Storn there is an empty  3-digit iso alpha code, see http://cs.wikipedia.org/wiki/ISO\_3166-1|
|Passangers||Poistené osoby. Elementy **Passenger** sa môžu opakovať.|
|- Passenger||Poradové číslo (0 to max 61)Insured persons. **Passenger** elements  can be repeated.|
|-- ID|number2, Integer|Serial number (0 to max 61)|
|-- BirthDate|dd.mm.YYYYD.M.YYYY, Date|Client's date of birth|
|-- TourPrice|Double<p>max.2 decimal places for EUR. Integer for CZK.|Price of  tour, airfare, or other services in units in currency.  Unless the total amount of **TotalTourPrice** is given, the total amount is the sum of the TourPrice of individual persons.|
|-- TicketPrice|Double|Ticket price for STV product. If the price is only for the first person, the service assumes that it is a group/family ticket.|

### Reponse format

|Tag name:|Description:|
| :- | :- |
|UNQResponse|Main Tag|
|- OrderNo|Order number from the request|
|- SumPrice|Cost ofThe amount of premiums|
|- Currency|Currency of premiums|
|- ErrorInfo|Return code|
|- ErrDescription|Error description|

## Create order with UNQInsuranceOrder

`UNQInsuranceOrder` with OrderNo filled  in, inserts or overwrites an unpaid order.  

### Request format

|Tag Name|Length and type|Description|
| :- | :- | :- | 
|UNQInsuranceOrder||Main Tag|
|OrderNo|10, String|The order number you specify, when it is empty, will only be recalculated - without registration, if repeated, the old order is deleted (if unpaid). If you wish the system would generate an automatic order number for you, enter a special symbol - three dots (...) in the field. |
|UserKey|32, String|Assigned code for user authorization|
|DateFrom|D.M.YYYY, Date|Date of validity from|
|DateTo|D.M.YYYY, Date|Date of validity until|
|InsuranceType|4, String|Abbreviation of the insurance type according to Table 1|
|Currency|3, String|Unit of currency used (EUR)|
|TotalTourPrice|Double<br>(Max.2 decimal places for currency EUR. Integer for CZK)|Total price of a trip, airfare, or other services total, for all persons in units according to the currency used.|
|CountryCode|3, String|Country code according to the Annex, for Storn there is an empty <br>3-digit iso alpha code, see http://cs.wikipedia.org/wiki/ISO\_3166-1|
|Area|2, String|W = World,<br>E = Europe,<br>SK = Slovakia.<br>If **CountryCode** is filled in,  the **Area** is  optional</p><p>In the case of driver insurance **Area** is  always **E**.|
|Arrangement|100, String|Special arrangements for the contract (optional field)|
|InternalAcqNum|20, String|The employee ID, if it is listed in the profile as **Int.zisk.číslo**, can be distinguished in production report. (optional field)|
|IndividualCode|20, String|Custom code - tour resolution, invoice number, etc. (optional field)|
|InsurerFirstName|10, String|Name of the policyholder/person acting (if not filled in 1. person is a policyholder)|
|InsurerLastName|20, String|Surname of the policyholder/person acting (if not filled in 1.person is a policyholder)|
|InsurerBirthDate|D.M.YYYY, Date|Date of birth of the policyholder (if not completed 1.person is a policyholder)|
|InsurerCompany|50, String|Name of legal entity/ company (optional field)|
|InsurerId|10, String|Company ID of a legal entity/company |
|InsurerIdentityCardType|3 String|OP=ID card, PAS=passport, RES=residence permit for residents (optional field)|
|InsurerIdentityCardNo|30 String|Document number (optional)|
|Town|30, String|Municipality – residence of the customer|
|StreetNo|30, String|Street and Number|
|Zip code|5, String|Postcode|
|Phone|30, String|Customer's phone number|
|Email|40, String|Email policyholder|
|Nationality|3, String|ISO state of the policyholder's country of residence (Optional field)|
|Passengers|||
|- Passenger||Insured persons. **Passenger** elements  can be repeated 1 to 60 times. For driver insurance, only one person can be entered.|
|-- ID|2, Integer|Serial Number |
|-- FirstName|10, String|Client Name|
|-- LastName|20, String|Client's last name|
|-- BirthDate|D.M.YYYY, Date|Client's date of birth|
|-- TourPrice|Float|<p>The price of a trip, airfare, or other services in units by currency.  Unless the total amount of TotalTourPrice is given, the total amount is the sum of the TourPrice of individual personsin units according to the currency used.<br>Must be the sum of individual prices - for all persons to state this amount |
|-- sp|Double|Ticket price for STV product. If the price is only for the first person, the service assumes that it is a group/family ticket.|

**Notes:** Possible policyholder combinations:

||**InsurerFirstName**|**InsurerLastName**|**InsurerBirthDate**|**InsurerCompany**|**InsurerId**|
| :- | :- | :- | :- | :- | :- |
|The policyholder is the first person from **Passengers**|**-**|**-**|**-**|**-**|**-**|
|The policyholder is another natural person|**+**|**+**|**+**|**-**|**-**|
|The policyholder is a natural person – entrepreneur|**+**|**+**|**+**|**-**|**+**|
|The policyholder is a company-legal entity|**+**<br>(person acting)|**+**<br>(person acting)|**-**|**+**|**+**|

### Reposnse format

|Tag name:|Description:|
| :- | :- | 
|UNQResponse|Main Tag|
|OrderNo|Order number from the request|
|SumPrice|Price in EUR|
|Contracts||
|- Contract|Insurance contracts|
|-- ContractNo|Contract No.|
|-- InsuranceType|Type of insurance|
|-- ContractSum|Total premiums for the draft insurance contract|
|-- LegalAidAmount|Amount of insurance protection included in the total amount per application|
|-- AgreementUrl|Link to the policy documents page|
|-- PassengerID|Serial numbers of persons in the contract|
|-- Documens|List of documents and annexes to the draft insurance contract|
|--- Document|A document with a **key** parameter indicates the print order, and the **code** is truncated in the document label|
|---- Name|Document Name|
|---- Url|Absolute link for downloading a document in pdf|
|---- PaymentURL|Contains a link with an encrypted parameter allowing the customer to be redirected to the payment gateway |
|ErrorInfo|Return code|
|ErrDescription|Error description|

**Notes:**

- Premiums are given only by the amount per order, not for persons. 
- Request example and response see  [Online tester](#online-tester)

## Cancel an order UNQDeleteOrder

Cancellation of the order is possible only if the order has not been paid.

### Request format

|Tag Name|Length and type|Description|
| :- | :- | :- |
|UNQDeleteOrder||Main Tag|
|OrderNo|9, String|Order number|
|UserKey|32, String|Assigned code for user authorization|

### Response format

|Tag name:|Description:|
| :- | :- |
|UNQResponse|Main Tag|
|ErrorInfo|Return code|
|ErrDescription|Error description|

**Notes:**

- In case of error, returns a code different from 0 (ErrorInfo > 0)


## Activate an order UNQPaymentConfirm

Activation of the order is possible only if the order has not been paid. This order only makes sense if you use your own premium collection.    

### Request format

|Tag Name|Length and type|Description|
| :- | :- | :- |
|UNQPaymentConfirm||Main Tag|
|OrderNo|9, String|Order number|
|UserKey|64, String|Assigned code for user authorization|
|Mailing|1, Char|1=send an informational e-mail to the client about the conclusion of the insurance contract<br>0=do not send e-mail. (The User solves on his own account)|
||||
### Response fomat:

**Notes:**

- In case of error, returns a code different from 0 (ErrorInfo > 0)
- If activated successfully, the `UNQPaymentConfirm` response is  the same as the `UNQOrderInfo` call response
 
## Insurance order information UNQOrderInfo

Use this command to obtain information about the status of the order.

### Request format

|**Tag name**|**Length and type**|**Description**|
| :- | :- | :- |
|UNQOrderInfo||Main Tag|
|OrderNo|9, String|Order number|
|UserKey|8, String|Assigned code for user authorization|

### Response format

|**Tag name**|**Length and type**|**Description**|
| :- | :- | :- |
|UNQResponse||Main Tag|
|OrderNo|9, String|Order number|
|SumPrice|Float|Insurance price|
|TourPrice|Float|Total trip price, tickets in units by CURRENCY<br>It is the sum of the prices of individuals |
|Currency|3, String|Unit of currency used |
|Contracts||
|- Contract||||
|-- ContractNo|Integer||
|-- InsuranceType|4, String|Abbreviation of the insurance type according to Table 1|
|-- Supplement|4, String|Abbreviation of additional insurance if it is negotiated|
|-- CarBrand|100 String|Vehicle brand, if there is a car supplementation|
|-- CarType|100, String|Type of the vehicle, if the car's supplementary insurance is negotiated|
|-- CarNumberPlate|7, String|EČV vehicle if the car is supposed to be adjusted|
|-- ContractSum|Float|Amount of insurance premiums per contract|
|-- NumPersons|Integer|Number of persons|
|-- DateFrom|D.M.YYYY, Date|Date of validity from|
|-- DateTo|D.M.YYYY, Date|Date of validity until|
|-- Area|1, Char|Possible values:<br>W – World<br>E – Europe<br>SK – Slovakia|
|-- Paid|1, Integer|Paid=0 unpaid, Paid=1 activated order paid|
|-- DatePaid|DD.MD.YYYY HH:MM:SS, Datetime|Date and time of activation |
|-- Inserted|DD.MD.YYYY HH:MM:SS, Datetime|Date and time of the order inserted|
|-- Phone|30, String|Customer's phone number|
|-- Email|40, String|Client email of the client of the customer|
|-- Passengers|||
|--- Passenger||Insured persons. |
|---- Id|2, Integer|Serial Number |
|---- Name|10, String|Client Name|
|---- Surame|20, String|Client's last name|
|---- BirthDate|D.M.YYYY, Date|Client's date of birth|
|---- AgreementUrl||Link for withdrawal of the insurance contract|
|---- Documens||List of documents and annexes to the draft insurance contract|
|----- Document|Integer|Document with **id** indicates print order, and **code** is truncated document label|
|----- Name|255, String|Document Name|
|----- Url|255, String|Absolute link for downloading a document in pdf|

**Note**: 

- Tag Contract can be repeated according to the number of contract proposals in the order.

## Contacts

**Administrácia Mont-Blanc administration:** 

Tel. EU: +420777212132

Tel. SK: +421948125132

Email: 		<admin@mont-blanc.sk>	

**Technical implementation:**

Karel Větrovský, Ph.D.	

Email: 		<vetrovsky@gmail.com>

## Annex 1 Risk zones

States treated under insurance conditions as risk zone 'E':

Albania, Andorra, Belgium, Belarus, Bosnia and Herzegovina, Bulgaria, Czech Republic, Montenegro, Denmark, Estonia, Finland, France, Greece, Netherlands, Croatia, Ireland, Iceland, Libya, Liechtenstein, Lithuania, Latvia, Luxembourg, Macedonia, Hungary, Malta, Moldova, Monaco, Germany, Norway, Poland, Portugal (including the Azores and Madeira), Austria, Romania, Russia (European Part), San Marino, Slovenia, Serbia  And the Canary Islands), Switzerland, Sweden, Italy, Ukraine, Vatican City, United Kingdom.

States where extended AUTO assistance applies:

Albania, Andorra, Belgium, Belarus, Bosnia and Herzegovina, Bulgaria, Montenegro, Denmark, Estonia, Finland, France, Croatia, Italy, Ireland, Iceland, Cyprus, 

Liechtenstein, Lithuania, Latvia, Luxembourg, Hungary, Macedonia, Malta, Monaco, 

Germany, Norway, Poland, Portugal, Austria, Romania, Greece, San Marino, Slovenia, United Kingdom of Great Britain and Northern Ireland, Republic of Serbia, Spain, Sweden, Switzerland, Turkey, Ukraine, Vatican City, Moldova, Morocco, Tunisko.

All others belong to the risk zone "W" with the exception of the Slovak Republic, which does not belong to any of the listed risk zones and has its own risk zone code: SK.

## Annex 2 Error codes

|Error No.|Error text|Slovak importance|
| :- | :- | :- |
|1|Unknown product|Unknown product IDENTIFIER|
|2|Failed loading XML|Failed to process XML|
|3|Period exceeded|Insurance scope exceeded|
|4|Wrong riz.zone definition|Incorrect risk zone|
|5|DateTo is invalid|Departure date is out of range|
|6|DateFrom is invalid|odchodu Return date is out of range|
|7|Rate does not exist|Sadzba does not exist|
|8|SellPrice is out of bounds|Gross price is out of range|
|9|DB error|DB I think|
|10|This operation is not supported|This operation is not supported|
|11|Invalid userkey|Invalid userkey|
|12|Access expired|Access expired|
|13|Wrong date from|Erroneous date from|
|14|Wrong date to|Erroneous date by|
|15|Area is not defined|Partition not defined|
|16|Wrong currency|We accept only EUR|
|17|Missing data for person #|Missing data forperson No.|
|18|Missing OrderNo or UserKey|Missing OrderNo or UserKey|
|19|PIN could not write/update records|The operator cannot write / update records|
|20|User disabled|User is disabled|
|21|OrderNo already activated|OrderNo already activated, cannot be erased or reactivate|
|22|0 record was updated.  Already activated?|No status change, records already marked activated|
|23|Order not found|Order does not exist|
|24|OrderNo is already used|OrderNo is already in use|
|25|Records not exist or already deleted|Records do not exist or are deleted|
|26|Duplicity found for person #|Duplicate person embedding|
|27|No products for such criteria|A  suitable product is not available for the specified criteria|
|28|Over limit 7.000 EUR/person. Please contact hotline. |Překročené limit na osoba. Contact the administration.|
|29|Invalid age for the product|Exceeded age for this product|
|30|Missing insurer data|Complete all necessary information about the policyholder|
|31|Insurer has to be over 18|The age of the policyholder (or 1st person) must be at least 18 years|
|32|Number of insured persons exceeded|Number of persons exceeded|
|33|Supplement not applicable|It is not possible to use for this type of insurance, group, or for this risk zone.|


