# Data Dictionary


## WEBSOCKET error code comparison table
<a id="WSERR"></a>

| Error code | Instructions |
|-----|------------|
| 200 | Success |
| 400 | Invalid Request |
| 401 | Not logged in |
| 500 | System error, please try again later |


## User authentication error code comparison table

<a id="ERR2"></a>

| Error code | Instructions |
|-------|------------------------------------------- |
| 40001 | ACCESS_KEY cannot be empty |
| 40002 | ACCESS_SIGN cannot be empty |
| 40003 | ACCESS_TIMESTAMP cannot be empty |
| 40005 | Invalid ACCESS_TIMESTAMP |
| 40006 | Invalid ACCESS_KEY |
| 40008 | Request timestamp expired |
| 40010 | API verification failed |
| 40012 | Invalid API User |
| 40013 | User banned |
| 40014 | User frozen |
| 40015 | Invalid IP request |
| 40016 | API_KEY has expired |

## Business error code comparison table

`amount limit value` is the upper and lower limit of the transaction amount/quantity configured by the product. In order to protect the market stability, BGE has limited the transaction limit according to the real-time market conditions, and the specific amount will fluctuate in real time with the market transaction situation.
<a id="ERR1"></a>

| Error code | Instructions |
|------|------------------------------------|
| 999 | The query data is empty |
| 1000 | No login |
| 1001 | Parameter error |
| 1006 | Currency information does not exist |
| 1008 | K line does not exist |
| 1009 | Quote does not exist |
| 1050 | Pair does not exist |
| 1051 | Notch depth does not exist |
| 1053 | Order has been blocked |
| 1054 | Less than the minimum order quantity |
| 1055 | Amount is less than balance |
| 1056 | Greater than the maximum order quantity |
| 1057 | Market order amount is invalid |
| 1058 | Failed to place an order |
| 1059 | Order does not exist |
| 1060 | Cancellation failed |
| 1061 | Order canceled |
| 1062 | Order completed |
| 1063 | Cancellation failed |
| 1099 | Currency does not exist |
| 1029 | Transferred amount is greater than transferable amount |
| 1049 | The user requests the interface too frequently |
| 1100 | Insufficient balance available |
| 1064 | KYC verification failed |
| 1065 | You have been frozen and cannot place an order |
| 1066 | Transaction password invalid |
| 1067 | Price, Price ≥ ${amount limit value} |
| 1068 | Price, Price ≤ ${amount limit value} |
| 1069 | Transaction amount precision is incorrect |
| 1070 | Quantity precision is incorrect |
| 1071 | Incorrect price precision |
| 1072 | The order quantity cannot be less than or equal to 0 |
| 1073 | The order price cannot be less than or equal to 0 |
| 1074 | Insufficient account balance |
| 1075 | Quantity, Amount ≥ ${amount limit value} |
| 1076 | Quantity, Amount ≤ ${amount limit value} |
| 1077 | Amount, Total ≥ ${amount limit value} |
| 1078 | Quantity, Amount ≥ ${amount limit value} |
| 1079 | Amount, Total ≤ ${amount limit value} |
| 1080 | Quantity, Amount ≤ ${amount limit value} |
| 1081 | The market order buy order has reached the highest transaction limit price, and the order has been automatically canceled |
| 1082 | The sell order of the market order has reached the minimum transaction limit price, and the order has been automatically canceled |
| 1083 | Failed to obtain user data |
| 1084 | No current order |
| 1085 | Exceeded the maximum number of orders |
| 1087 | No data found |
| 1088 | No way to verify user identity |
| 1089 | One of order_id or product cannot be empty |
| 1090 | client_oid already exists |
| 1091 | client_oid does not meet the rules |
| 1092 | The available balance of the current trading account is insufficient, whether to automatically transfer the ${0} in the fund account to the trading account, and initiate an order |
| 1093 | Insufficient account balance |
| 1094 | Transfer fail |
| 1095 | The quantity is too large |
| 1096 | Account does not exist |
| 1097 | System timeout                             |
| 1098 | The system is busy                             |
| 1101 | Trading pair and account type mismatch                            |








