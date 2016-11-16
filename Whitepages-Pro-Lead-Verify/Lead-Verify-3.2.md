# Lead Verify API

Use Whitepages Pro dynamic identity data to confidently verify lead authenticity. In one simple query verify lead data like name, phone, email, ip, and address info to validate the lead is a real person and can be contacted. Lead Verify confirms the name-to-phone, name-to-address, and name-to-email match from the lead.

## Request
Lead Verify can do up to 5 searches as part of a single query. You can provide as few or as many inputs in the query. We will only populate the components in the API return that correspond to the inputs provided.
**Request format**

```
https://proapi.whitepages.com/3.2/lead_verify.json?&name=Drama+Number&phone=6464806649&email_address=medjalloh1@yahoo.com&address_city=Ashland&address.postal_code=59004&address.state=MT&address.street_line_1=302+Gorham+Ave&ip_address=108.194.128.165&api_key=KEYVAL
```

**Request Parameters**

All parameters are case sensitive.

| Parameters     | Description | Examples |
| ------- | ---- | ---- |
| api_key | API key. REQUIRED | |
| name | The complete name of a person or business | Jan Smith |
| phone | Contains a raw unparsed or a formatted phone number | 2069735184 or 12069735184 or 206-601-3561 |
| email_address | Lead Verify supports all standard email addresses | example@gmail.com |
| ip_address | IPv4 IP Addresses. | 192.0.2.1 |
| address.street_line_1 | Primary address with number and street name to be searched | 2808 Nero Blvd |
| address.street_line_2 | Apartment or other additional address information to be searched | Apt 265 Box 34Rs |
| address.city | City | Seattle |
| address.state_code | 2 character state code | WA |
| address.country_code | alpha-2 county code | CA or US |


## Response

A Lead Verify response consists of various checks based on the inputs entered. All possible components are below:

**Phone Checks**

The checks indicate whether the phone matches the input name provided on the lead, and whether the phone numbers is valid or not, is currently in service at time of inquiry.

| Field     | Type | Description |
| ------- | ---- | ---- |
| warnings | Array | A warning message that returns "Missing Input" or null |
| is_valid | Boolean | A boolean value indicating if the phone is a valid phone number and has been assigned to a carrier. Possible values are true, false or null. |
| phone_contact_score | Integer | A 1-4 score on whether this is a valid phone number and matches to the input name provided for the lead. Score of 1 indicates a valid, connected number with the subscriber name matching to the input name provided with the lead. 4 indicates very high confidence that the lead cannot be reached via this phone number |
| is_connected | Boolean | Indicates whether the phone is connected or not in service. Possible values are True, False, or null. |
| phone_to_name | String | Verification result whether the subscriber name for the phone matches the input name. Possible values: Match, No Match, No Name Found, null |
| subscriber_name | String | Verification result whether the subscriber name for the phone matches the input name. Possible values: Match, No Match, No Name Found, null |

A sample response:

```json
"phone_checks": {
"warnings": [ ],
"is_valid":true,
"phone_contact_score": 2,
"is_connected": null,
"phone_to_name": "Match",
"subscriber_name": "Drama Number"
}
```

**Email Address Checks**

The check indicates whether the email is valid or malformed, active or inactive, and verifies if the email registered name matches the input name provided.


| Field     | Type | Description |
| ------- | ---- | ---- |
| warnings | Array | A warning message that returns the reason why email is invalid. For example: "Domain cannot receive email" |
| is_valid | Boolean | Indicates whether the email address is valid and email deliverable. |
| diagnostics | Array | Diagnostic message for the is_valid flag. This is the reason why we call the email valid. Possible values: Domain does not support validation (accepts all mailboxes), Syntax OK and domain valid according to the domain database, Syntax OK, domain exists, and mailbox does not reject mail |
| email_contact_score | Integer | A 1-4 score on whether this is a valid email address and matches to the input name provided for the lead. Score of 1 indicates a valid email with the registered name matching to the input name provided with the lead. 4 indicates very high confidence that the lead cannot be reached via this email |
| is_disposable | Boolean | Indicates whether the email domain is disposable. Possible values are True, False, or null. |
| email_to_name | String | Verification result whether the registered name for the email matches the input name. Possible values: Match, No match, No name found, null |
| registered_name | String | Name associated with input email address |


A sample response:

```json
"email_address_checks": {
"warnings": [ ],
"is_valid": true,
"diagnostics": [
"Syntax OK, domain exists, and mailbox does not reject mail"
],
"email_contact_score": 3,
"is_disposable": false,
"email_to_name": "No match",
"registered_name": "MOHAMED JALLOH"
}
```

**Address Checks**

The check indicates whether the address is real and active and verifies if the resident matches the input name provided.

| Field     | Type | Description |
| ------- | ---- | ---- |
| warnings | Array | A warning message that returns "Invalid Input" or null. |
| is_valid | Boolean | A boolean value indicating if the address is a valid existing address. Possible values are true, false, and null. |
| address_contact_score | Integer | A 1-4 score on whether this is a valid address and matches to the input name provided for the lead. Score of 1 indicates a valid address with the resident name matching to the input name provided with the lead. 4 indicates very high confidence that the lead cannot be reached via this address.  |
| is_active | Boolean | Indicates if the address is currently receiving mail. Possible values are true, false, or null. |
| address_to_name | String | Verification result whether the resident name for the address matches the input name. Possible values: Match, No Match, No Name Found, null |
| resident_name | String | Resident name of the input address. |


A sample response:

```json
"address_checks": {
"warnings": [ ],
"is_valid": true,
"address_contact_score": 1,
"is_active": true,
"address_to_name": "Match",
"resident_name": "Drama Number"
}
```

**IP Checks**

The check indicates whether the IP is proxy and it’s geolocation 

| Field     | Type | Description |
| ------- | ---- | ---- |
| warnings | Array | A warning message that returns "Invalid Input" or null. |
| is_proxy | Boolean | Indicates whether the IP address is a known proxy. Possible values are true, false, or null. |
| distance_from_address | Integer | Distance between the IP address and physical address in miles. The longer the distance between the IP address and a lead’s physical address, the stronger the risk indicator. |
| geolocation | String | Location of the IP address. Includes postal_code, city_name, country_name, continent_code, and country_code when available. |

A sample response:

```json
"ip_address_checks": {
"warnings": [ ],
"is_proxy": null
"distance_from_address": "1117",
"geolocation": {
"postal_code": "29205",
"city_name": "Columbia",
"country_name": "United States",
"continent_code": "NA",
"country_code": "US"
},
}
```

