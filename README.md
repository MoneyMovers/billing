# MoneyMovers Billing API Doc
This documentation describe protocol for cummunicating with MoneyMovers Billing

## Common characteristics 
Request - Response
* All data sended to Billing API must be sended via HTTP GET method
* Billing return aswer as XML string (with UTF-8 encoding)
* All requests must containt **agent** param, which is unique agent name in MoneyMovers Billing system
* Before going to production agent can make tests on testing server

## Security
* All methods require **secret** param 
* secret param is generated with concatinating other request params and adding **secret key** at the end.
* contatinated string must be hashed with [SHA256](https://en.wikipedia.org/wiki/SHA-2) alrogithm.

## API Methods
1. [info method](info.md)
2. [pay method](pay.md)
   - [status codes](info_pay_status.md)
3. [status method](status.md)
   - [status codes](status_status.md)
   
## Miscellaneous   
1. [Error processing](error_processing.md)
