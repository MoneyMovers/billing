# API Method PAY
This method is used for top up user account in remote service system.

## Request Example
```bash
curl "https://billing.mm.ge/pay.php?\
&agent=agent_name\
service=78\
&user=test_account\
&amount=1.00\
&id=14\
&service_sub_id=0\
&service_second_sub_id=0\
&service_third_sub_id=0\
&date=2012-05-21+19%3A26%3A08\
&hash=fcc7196557825e7b612cdfcf5fb0d4444cf114a84c1a4d55d9c339c65307eb7e"
```
## Request Param Description
| Param Name | Param type | Param Description  |
|-|-|-|
| agent | char | agent unique name in MoneyMovers Billing System |
| service | uint | remote service unique identifier in MoneyMovers Billing system |
| user | char | user account in remote service system | 
| *amount | float | amount fot top up user account in remote service system |
| id | char [a-zA-z0-9] | request unique identifier on agent system side | 
| **service_*_id | char | additional params |
| date | datetime [yyyy-mm-dd hh:ii:ss] | datetime when is request generated |
| hash | char | see description |

*_amount is sended in agent currency, lets say if agent try to top up remote service which is in EUR currency but agent currency is USD
 in this case agent send 1.00 USD and MoneyMovers Billing System will exchange USD to EUR and send EUR to remote service_
 
**_some of remote services can require additional params for request (account curency, user personal number , etc.) this params is used for sending additional info to remote service_

## Generating **hash** param
In this method **hash** param is generating with concatinating **agent,service,amount,id,user,** param values, at the end agent must
add **secret string** which is provided by MoneyMovers Billing System and is unique per agent.
### Example php code
```php
<?php 
...
$agent = "agent_name";
$service = 78;
$amount = 1.00;
$id = 14;
$user = "test_account";

$secretString = "secret#123#terces";
$concatinated = $agent.$service.$amount.$id.$user.$secretString;
$hash = hash('sha256',$concatinated);

```
## Response example
```xml
<result>
	<error>
		<errorcode>0</errorcode>
		<errorru>Удачно</errorru>
		<errorge>წარმატებით</errorge>
		<erroren>successfully</erroren>
	</error>
	<amount>
		<usd>1</usd>
	</amount>
	<user>test_acount</user>
	<service>skype</service>
	<operationid>16549</operationid>
	<data>
		<RATE>0.81</RATE>
		<GENERATED_AMOUNT>0.81</GENERATED_AMOUNT>
		<CURRENCY>EUR</CURRENCY>
		<avance>2811.99</avance>
		<date>2018-02-21 22:08:25</date>
	</data>
</result>
```
## XML params description

| param name | param type | param description |
|-|-|-|
|result/error/errorcode | int | error code of response, see error codes |
|result/error/error[en/ge/ru] | char | error description |
|result/amount/{agent currency}| float | amount in agent currency this amount will be cutted from agent avance when agent will make pay request |
|result/user | char | user account in remote service system |
|result/service | char | remote service name |
|result/data/RATE | float | rate by which was calculated remote service amount from agent amount
|result/data/GENERATED_AMOUNT | float | amount which will be topped up user account in remote service system |
|result/data/CURRENCY | char | currency code of remote service system |
|result/data/avance | float | agent avance |
|result/data/date | datetime [yyyy-mm-dd hh:ii:ss] | datetime of transaction |

## Notes



