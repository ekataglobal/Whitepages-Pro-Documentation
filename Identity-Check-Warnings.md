# Identity Check Warnings Samples

## Phone Checks Response ##

| Error/Warning                                                 | Description                                                  | Example Input Parameter                                                |
| -------------                                                 | -----------                                                  | -----------------------                                                |
| InternationalPhoneDisabled                                    | API key is not configured for international phone validation |                                                                        |
| Invalid Input                                                 | Input phone number is invalid or not a phone number          | `primary.phone=123`                                                    |
| Invalid county_hint value. Only Alpha-2 and Alpha-3 supported | Country hint is not valid alpha2 or alpha3 value             | `primary.phone=2061115101`<br>`primary.phone.country_hint=United+States` |

## Address Checks Response ##

| Error/Warning                                                  | Description                                                                                                       | Example Input Parameter                                                                                                                                                 |
| -------------                                                  | -----------                                                                                                       | -----------------------                                                                                                                                                 |
| InternationalAddressDisabled                                   | API key is not configured for international address validation                                                    |                                                                                                                                                                         |
| Invalid Input                                                  | Input address is invalid                                                                                          | `primary.address.street_line_1=-`                                                                                                                                       |
| Invalid Country Code                                           | Country code is not alpha 2 or alpha 3 value                                                                      | `primary.address.country_code=United+States`                                                                                                                            |
| Missing unit/apt/suite number                                  | Input address is a multi-unit address and unit number is missing in the input address                             | `primary.name=Whitepages`<br>`primary.address.street_line_1=1301+5th+Ave`<br>`primary.address.city=Seattle`<br>`primary.address.state_code=WA`                            |
| Invalid unit/apt/suite number                                  | Input address is a multi-unit address and unit number is invalid in the input address                             | `primary.name=Whitepages`<br>`primary.address.street_line_1=1301+5th+Ave+5th+Floor`<br>`primary.address.city=Seattle`<br>`primary.address.state_code=WA`                |
| Input postal code was corrected. Potential impact to AVS code. | Input postal code didn’t match street line 1, city, and state. To validate the address postal code was corrected. | `primary.address.street_line_1=1301+5th+Ave+1600`<br>`primary.address.city=Seattle`<br>`primary.address.state_code=WA`<br>`primary.address.postal_code=10001`           |
| Resident not found at address                                  | Person or business could not be found at partial input address                                                    | `primary.name=Test+Na`<br>`primary.address.city=Seattle`<br>`primary.address.state_code=WA`<br>`primary.address.country_code=US`<br>`primary.address.postal_code=98102` |
|                                                                |                                                                                                                   |                                                                                                                                                                         |

## Email Address Checks ##

| Error/Warning                             | Example Input Parameter                                                                    |
| -------------                             | -----------------------                                                                    |
| The mailbox is invalid or does not exist  | `email_address=whitepages.test.email@gmail.com`                                            |
| General syntax error                      | `email_address=@whitepages.com`                                                            |
| Invalid domain syntax                     | `email_address=whitepages.test.email@gmail.`                                               |
| Invalid username syntax                   | `email_address=whitepages.test.email1111111111111111111111111111111111111111111@gmail.com` |
| Invalid top-level-domain (TLD) in address | `email_address=whitepages.test.email@gmail.com1`                                           |
| Domain does not exist                     | `email_address=whitepages.test.email@gmail`                                                |

## IP Address Checks ##

| Error/Warning                  | Example Input Parameter |
| -------------                  | ----------------------- |
| Invalid Input                  | `ip_address=invalid-IP` |
| IP address is in private range | `ip_address=127.0.0.1`  |
