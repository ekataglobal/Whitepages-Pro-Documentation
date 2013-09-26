# WhitePages PRO API Documentation

*Documentation and Implementation Guide for WhitePages PRO API Version
1.1*

Copyright 2013, WhitePages, Inc.

Last updated: April 14, 2013

## Introduction and Overview

WhitePages, Inc PRO API gives businesses the ability to easily integrate
and leverage its industry leading People and Business search. The API is
accessed via a RESTful interface using simple HTTP GET requests. The
WhitePages.com API will provide XML responses by default. Optionally,
the response format can be specified as JSON if desired. Other formats
may be supported in the future. This protocol is simple to use and
accessible from any common web programming language on any machine with
Internet access.

-   **Find Person:** search by a person's name and location to find the
    person’s complete address and phone number.

-   **Reverse Phone:** search by phone number to find the related name
    and address.

-   **Reverse Address**: search by street address to find the related
    name and phone number.

-   **Find Business**: search by keywords and location to find a
    business’ complete address and phone number.

## Access and Security

### API Keys

WhitePages controls access to the API and data via an API Key. The API
keys are the primary data authentication method for your account and are
how usage is recorded and reported. Your sales representative will
provide you one or more keys for your production environment according
to the need of your business.

You may also obtain an API key for our free developer API at
http://developer.whitepages.com/. These developer keys are intended to
be used for testing purposes only and will not be valid for your
production configuration due to intentional capacity limitations. The
developer key also doesn’t have access to our premium data sources.

### Security

Please take appropriate measures to protect access to your API keys as
they give access to your paid account and if leaked could cause
additional fees to be attributed to your account. The API is provided
via HTTP and HTTPS. Some application developers may not be able to use
HTTPS so HTTP is provided as an option, however we recommend using HTTPS
where possible to prevent third parties from acquiring your API key
while in use.

## Rates and Quotas

The API is metered and measured in two ways that matter to the customer,
Rates and Quotas.

### Rates

Your access to API is metered by queries per second per API Key (QPS).
This rate is set at the initial contract time and can be adjusted by
your sales representative any time. This is primarily to control the
overall queries per second coming into the system. For example, if you
have a QPS of 16 on a particular API Key, you can access all search
types at a rate of 16 per second. If you have more than one API Key,
each key has an independent QPS associated to it.

### Quotas

We measure billing primarily by matches per month per API key (Quota).
If you have a Quota of 25,000, you can retrieve 25,000 matches
(successful queries with data) at your contracted amount. If you exceed
25,000, you will be charged only for matches beyond that at your overage
rate. For more information on this, please contact your WhitePages PRO
representative.

### Billable Queries

Generally a query is considered billable only if WhitePages is able to
provide additional data points like name, address or phone number. If
your search parameters returned no results or WhitePages was not able to
return a name, address or number, the query is called a "no match" and
is not billed. These rules vary by search type however. The specific
rules for what makes a query billable are found within the documentation
for each search type below.

## API Documentation

### Request Format

Each search request consists of the following parts:

1.  The base URL, **https://proapi.whitepages.com** or **http://proapi.whitepages.com**
1.  The search type, either **find\_person**, **reverse\_phone**,
    **reverse\_address** or **find\_business**

1.  The API version, currently 1.1

1.  The search parameters, which is a semicolon separated list of
    *parameter*=*value* entries

1.  The API key parameter with your API key value.

For example, to search for the person "Mike Smith" in 98101, the request
would look like:

	http://proapi.whitepages.com/find_person/1.1/?firstname=mike;lastname=smith;zip=98101;api_key=KEYVAL

Note that KEYVAL is your API key value provided by WhitePages.

## PRO API Responses

### Results Response

If the search produces results, the search returns a response containing
one or more records. The data structure and field definitions are found
in section Data Dictionary below.

Results Responses may or may not be billable, according to rules
described under Billable Queries above. The results may contain full
name, address and phone number data, or only part of these depending on
what data are associated with the input parameters in the WhitePages
contact database. For example, a reverse phone search may return a name
and address or just a name; a reverse address search may return name and
phone or just a name, etc.

The number of results returned is limited based on search results and on
the type of query which was requested. All search types by default will
return up to 20 results. If more results are needed, the API key can be
configured to return additional results, up to 60 for find\_business
searches and up to 300 for all other search types.

## No Results Response

If the search does not produce any results, the search returns a
response that specifies no results were found. The default XML format
response is described below. See the section on optional JSON output for
an illustration of how this might differ if optional JSON output format
was requested.

For example, given the following search query for someone with the
non-existent last name of "zzzzzz":

	http://proapi.whitepages.com/find_person/1.1/?lastname=zzzzzz;api_key=API_KEY

*The results response (the elements are described below) should look
like the following:*

	<wp:wp>
	<wp:meta>
	<wp:recordrange wp:lastrecord="0" wp:firstrecord="0" wp:totalavailable="0"/\>
	<wp:apiversion>1.1</wp:apiversion>
	<wp:searchlinks>
	<wp:link wp:linktext="Link to this api call" wp:type="self">URL_TO_THIS_API_CALL</wp:link>
	</wp:searchlinks>
	</wp:meta>
	<wp:result wp:type="success" wp:message="The search did not find results" wp:code="No Data Found">
	</wp:wp>

Note that *URL\_TO\_THIS\_API\_CALL* is the same URL as the original
query

## Picklist Response


If the search criteria are too ambiguous to produce a clear result, the
default XML format response is described below. See the section on
optional JSON output for an illustration of how this might differ if
optional JSON output format was requested.

For example, given the following search query for someone named "smith"
in the city "portland":

	http://proapi.whitepages.com/find_person/1.1/?lastname=smith;city=portland;api_key=API_KEY

The results response (the elements are described below) seeks
clarification since Portland is found in several states. The actual
response should look like the following:

	<wp:wp>
	<wp:listings>
	<wp:listing>
	<wp:address>***
	<wp:city>PORTLAND</wp:city>
	<wp:state>OR</wp:state>
	<wp:country>US</wp:country>
	</wp:address>
	<wp:listingmeta>
	<wp:type>unknown</wp:type>
	</wp:listingmeta>
	</wp:listing>
	…
	<wp:listing>
	<wp:address>
	<wp:city>PORTLAND</wp:city>
	<wp:state>ME</wp:state>
	<wp:country>US</wp:country>
	<wp:address>
	<wp:listingmeta>
	<wp:type>unknown</wp:type>
	</wp:listingmeta>
	</wp:listing>
	</wp:listings>
	<wp:meta>
	<wp:recordrange wp:firstrecord="0" wp:lastrecord="0" wp:totalavailable="17"/>
	<wp:apiversion>1.1</wp:apiversion>
	<wp:searchlinks>
	<wp:self wp:linktext="Link to this api call">
	<wp:url> URL_TO_THIS_API_CALL
	</wp:url>
	</wp:self>
	</wp:searchlinks>
	</wp:meta>
	<wp:result wp:type="error" wp:code="Refine Input" wp:message="Ambiguous geographic location" wp:billable="true">
	<wp:errormessages>
	<wp:message>Refine Input: multiple cities found</wp:message>
	</wp:errormessages>
	</wp:wp>

Note that in the above, URL\_TO\_THIS\_API\_CALL is the same URL as the original query

## Error Response

If the search request contains errors, the search returns a response
that specifies where the error occurred. The default XML format response
is described below. See the section on optional JSON output for an
illustration of how this might differ if optional JSON output format was
requested.

For example, given the following search query, which does not include
the required street parameter:

	http://proapi.whitepages.com/reverse\_address/1.1/?house=12345;city=seattle;state=wa;api_key=API_KEY

The results response (the elements are described below) should look like
the following:

	<wp:wp>
	<wp:listings/>
	<wp:meta>
	<wp:recordrange wp:firstrecord="0" wp:lastrecord="0" wp:totalavailable="0"/>
	<wp:apiversion>1.1\</wp:apiversion>
	<wp:searchlinks>
	<wp:self wp:linktext="Link to this api call">
	<wp:url> URL_TO_THIS_API_CALL
	</wp:url>
	</wp:self>
	</wp:searchlinks>
	</wp:meta>
	<wp:result wp:type="error" wp:code="Missing Input" wp:message="One or more input parameters are invalid" wp:billable="false"/>
	<wp:errormessages>
	<wp:message>Missing Input: street\</wp:message>
	</wp:errormessages>
	</wp:wp>

Note that in the above, URL\_TO\_THIS\_API\_CALL is the same URL as the original query

### Error Messages

Error messages are intended to be self explanatory. Here are some of the
common error messages.

**Invalid or Inactive API key:*****KEYVAL*** - indicates that an invalid
KEYVAL has been passed to WPAPI. Since an API\_KEY takes about 10
minutes to activate, this can occur on first use of the WPAPI if less
than 10 minutes has elapsed between registering for the API\_KEY and
first attempt at using it. Other common causes include a typo or
transposition when entering the KEYVAL.

**API key is required to use this API** - indicates that the api\_key
parameter was entirely missing from the WPAPI call made. To correct this
error add  

	;api_key=KEYVAL

where *KEYVAL* represents the API registration key you received when
registering to the use the WPAPI.

**Invalid Input Parameter:paramname** - indicates that an input
parameter was not recognized as a valid name of a parameter. The
offending name will be identified as **paramname** in the message.

**Missing Input:lastname** - indicates that a required input parameter
was not supplied. In this example the missing parameter was lastname.

#### Error Payloads
Error payloads are provided in JSON or XML as requested in the API call.

##### JSON Error Payload Example

	result: {
		type: "error",
		code: "Missing Input",
		message: "One or more input parameters are invalid",
		billable: "false"
	}
	
##### XML Error Payload Example

	<wp:result wp:type="error" wp:code="Missing Input" wp:message="One or more input parameters are invalid" wp:billable="false"/>
	<wp:errormessages>
		<wp:message>Missing Input: areacode</wp:message>
	</wp:errormessages>

##### Error response codes

The following error response codes are delivered within the above error payloads:


| HTTP Response | Type | Code | Message | Description |
| ------------- | ---- | ---- | ------- | -------- |
| 200 | 	 |  |  | The API responded successfully |
| 400 | error | Error | The method is invalid | The method you attempted to invoke does not exist |
| 400 | error | Error | API Key required | An API key is required to be passed to invoke this API method |
| 403 | error | Error | Quota exceed | You have exceeded your API call quota, please contact your account manager |
| 400 | error | Error | Invalid API Version | The API version requested is invalid | 
| 400 | error | Error | Invalid Output Type | The selected output format is not valid | 
| 403 | error | Error | Inactive or Invalid API Key | The API key is inactive or invalid | 
| 403 | error | Error | Invalid API URI | A method and version are required to use this API | 
| 403 | error | Error | Invalid Input Parameters | The parameters passed to the method were not recognized | 
| 500 | error | Error | Data Provider Error | Error from the data provider. Please try again in a few seconds | 
| 500 | error | Error | Generic WPAPI Error | A generic error has occurred. Please try again in a few seconds |

## Optional Feature - Receiving Mail

The WhitePages PRO API now offers the ability to inform you on whether
an address is currently receiving mail or not, via two new data points
in the search response: ‘***receiving\_mail***’ and
‘***not\_receiving\_mail\_reason***’.

### How to Use It

The new ‘***receiving\_mail***’ feature is turned off by default, but
can be enabled by your WhitePages Account Executive at your request.
When it is enabled, you will receive the new ‘***receiving\_mail***’ and
‘***not\_receiving\_mail\_reason***’ data points for all searches that
return address information:

-   ***find\_person***
-   ***reverse\_phone***
-   ***reverse\_address***
-   ***find\_business***

### New Data Points

Whether an address is currently receiving mail is indicated by the
following two new data points which are found as attributes of the
wp:address element in the XML response:

* ***receiving\_mail***: ***true/false***. If WhitePages data indicate that the property is not receiving mail, then a ***false*** value will be returned. Otherwise the value will be ***true***.
* ***not\_receiving\_mail\_reason***: If the property is not receiving mail (if ***receiving\_mail=false***), then where possible this will provide a reason why mail is not being received. The possible values will be updated over time as WhitePages incorporate new data, and currently include:

| reason code | description |
| ----------- | ----------- |
| vacant | this location is vacant |
| new\_construction | this locaiton is new construction and mail delivery has not started | 
| rural | rural address |
| partial_address | the address returned is incomplete, contains zip+4 and street information | 
| other | no specific reason was provided | 
| unknown | no data available | 


### Impact on Match Rate and Billable Queries

Under the default WhitePages PRO API configuration, a search on an address that returns no name is considered a ‘no match’ and is not a billable query. However if the new ‘***receiving\_mail***’ feature is enabled, then the WhitePages Pro API can still provide the ‘***receiving\_mail***’ flag even if there is no name available, and as a result what would otherwise have been a ‘no match’ will now be considered a match and a billable query. 

For this reason, the new ‘***receiving\_mail***’ feature will usually cause a significant increase in match rate and number of billable queries. As such it is an optional feature that is turned off by default, but can be enabled if desired by your WhitePages Account Executive.

## Search Types

The WhitePages PRO API supports the following four search types:

* Find Person
* Reverse Phone
* Reverse Address
* Find Business

Find Person
===========

Finds contact information for a person, based on name and location.

	http://proapi.whitepages.com/find_person/1.1/?parameter_list;api_key=KEYVAL

### Syntax

where *parameter\_list* signifies one or more *parameter*=*value*
entries separated by semicolons and *KEYVAL* is the API key value you
received when you registered.

### Parameters

Parameters can appear in any order. Parameter names are case-sensitive;
parameter values case-insensitive. For example "lastname=smith" matches
"Smith", but "Lastname=smith" is not a valid parameter assignment, since
the parameter name is (all lower case) lastname and not (capitalized)
Lastname.

| Parameter | Required? | Notes | Example |
| --------- | :-------: | ----- | ------- |
| firstname | no | | firstname=robert |
| lastname | yes | Either lastname or name is requierd | lastname=johnson |
| name | yes | Full name, an alternative to lastname and firstname | name=robert%20johnson |
| house | no | The house parameter is the house number | house=400 |
| street | no | The street parameter is the street name, including any directional prefix or suffix | street=maple%20st |
| city | no | | city=seattle |
| state | no | Usps two-character state | state=wa |
| zip | no | Will accept 5 digit ZIP or 9 digit ZIP+4 | zip=98101 |
| areacode | no | | areacode=206 |
| metro | no | Whether to expand the search to the metro area. The default value is 0 (false) which means searches are not expanded to metro area by default. | metro=1 |

### Remarks

Only one of the parameters lastname or name is required. However, we
recommend providing as much information as you have as that will give
better results and faster response.

Although the API supports separate input parameters for house and
street, both values can be appended together in the street input
parameter. For example, searching house=2468 and street=main+st will
return the same result as just submitting street=2468+main+st.

The firstname and lastname parameters support both an exact name match
as well as a "begins with" match using asterisk ("\*") as a wild-card
character. To search for "Bernie" or "Bert" set the firstname parameter
to "Ber\*". No other parameters support this character and it can only
be used to match the end of a first name or the end of a last name.

Searches for common nicknames, such as searching for "Doug" or "Douglas"
as a first name, are automatically done without requiring the wildcard
in the firstname field. If multiple matches are found the listings will
be ordered so that more exact matches to the search parameters will be
presented first with nicknames or other wildcard results lower in the
results set.

The metro parameter is a Boolean value where **1** signifies true or
**0** signifies false. It specifies whether to expand the search to
include the metropolitan area around the specified city parameter value.

### Limitations

A find\_person search will return at most 20 listings using the WPAPI.
Additional listings may be available by following one of the URLs
returned as links with a successful search.

### Billable Queries

A find\_person search is considered billable if either an address and/or
phone number are returned. If WhitePages doesn’t return any address or
number, the search is not billed.

The exception to this is that for customers who are only interested in
phone numbers and not addresses, accounts can be configured so that a
find\_person search is only considered billable if a phone number is
returned. If you are interested in this feature, please contact your
WhitePages PRO account representative.

### Example

The following example specifies a search for Mike Smith on Main Street
in Seattle, Washington.

	http://proapi.whitepages.com/find_person/1.1/?firstname=Mike;lastname=Smith;street=Main;city=seattle;state=wa;api_key=KEYVAL

## Reverse Phone Search

Finds a name and contact information, based on a telephone number.

	http://proapi.whitepages.com/reverse_phone/1.1/?parameter_list;api_key=KEYVAL

### Syntax

where *parameter\_list* signifies one or more *parameter*=*value*
entries separated by semicolons and *API\_KEY* is the API key you
received when you registered.

### Parameters

Parameters can appear in any order. Parameter names are case-sensitive;
parameter values case-insensitive. For example "state=wa" is equivalent
to "state=WA", but "State=WA" is not a valid parameter assignment.

| Parameter | Required? | Notes | Example |
| --------- | :-------: | ----- | ------- |
| phone | yes | Can be 7 or 10 digits | phone=2069730000 |
| state | yes, if phone is 7 digits | USPS two character abbreviations only | state=WA |

### Remarks

The phone parameter is required and the state parameter is also required
if the phone parameter value is 7 digits.

### Limitations

A reverse\_phone search will return at most 20 listings using the
WhietPages.com API. Additional listings may be available by following
one of the URLs returned as links with a successful search.

### Billable Queries

Reverse\_phone searches are billable only if a first and last name or
business name are returned. If no names are provided, then the search is
not billable.

### Example

The following examples specify a reverse phone search for the
Seattle-area telephone number (206) 973-0000:

	http://proapi.whitepages.com/reverse_phone/1.1/?phone=2069730000;api_key=KEYVAL

or

	http://proapi.whitepages.com/reverse\phone/1.1/?phone=9730000;state=WA;api_key=KEYVAL

## Reverse Address Search

Finds information based on an address.

	http://proapi.whitepages.com/reverse_address/1.1/?parameter_list;api_key=KEYVAL

### Syntax

where *parameter\_list* signifies one or more *parameter*=*value*
entries separated by semicolons and *API\_KEY* is the API key you
received when you registered.

### Parameters

Parameters can appear in any order. Parameter names are case-sensitive;
parameter values case-insensitive. For example "state=wa" is equivalent
to "state=WA", but "State=WA" is not a valid parameter assignment.

| Parameter | Required? | Notes | Example |
| --------- | :-------: | ----- | ------- |
| house | no |  | house=400 |
| apt | no | | apt=2a | 
| street | yes | The street parameter is the street name, including any directional prefix or suffix | street="maple%20st" | 
| city | no | | city=seattle |
| state | see remarks | Usps two-character state | state=wa |
| zip | see remarks | Will accept 5 digit ZIP ir 9 digit ZIP+4 | zip=98101 | 
| areacode | see remarks | | areacode=206 |

### Remarks

Only the street parameter and one of the state, zip, or areacode
parameters is required. However, we recommend providing as much
information as you have as that will give better results and faster
response.

Although the API supports separate input parameters for house, street
and apt, all three values can be appended together in the street input
parameter. For example, searching house=2468, street=main+st, and
apt=101 will return the same result as just submitting
street=2468+main+st+apt+101.

### Limitations

A reverse\_address search will return at most 20 listings using the
WPAPI. Additional listings may be available by following one of the URLs
returned as links with a successful search.

### Billable Queries

A reverse\_address search is considered billable if either first and
last name, business name, and/or phone number are returned. If
WhitePages doesn’t return any names or number for a given address is not
billed.

### Example

The following example specifies a search for the address 2468 Main St,
Seattle, Washington.

	http://proapi.whitepages.com/reverse_address/1.1/?house=2468;street=main%20st;city=seattle;state=wa;api_key=KEYVAL

## Find Business

Finds contact information for a business, based on location and either
name or category.

	http://proapi.whitepages.com/find_business/1.1/?parameter_list;api_key=KEYVAL

### Syntax

where *parameter\_list* signifies one or more *parameter*=*value*
entries separated by semicolons and *KEYVAL* is the API key value you
received when you registered.

### Parameters

Parameters can appear in any order. Parameter names are case-sensitive;
parameter values case-insensitive. For example "keywords=mazda" matches
"Mazda", but "Keywords=mazda" is not a valid parameter assignment, since
the parameter name is (all lower case) keywords and not (capitalized)
Keywords.

| Parameter | Required? | Example |
| --------- | :-------: | ------- |
| keywords | yes | keywords=a1%20pizza | 
| location | yes | location=seattle | 

### Notes

Both input parameters are required. Keywords can be the business name,
category, or other identifying attribute. The location must be more
specific than nationwide, i.e. the city, state, or zip of the business.
In order to make sure you get the correct relevant result, the location
parameter should be as specific as possible. For example, searching
location=Portland should be avoided since Portland occurs in multiple
states. Instead, for common city names it is recommended that you submit
either a zip code or both city and state together (i.e. location=97223
or location=portland%20or).

### Limitations

A find\_business search will return at most 20 listings using the WPAPI.
Additional listings may be available by following one of the URLs
returned as links with a successful search.

### Billable Queries

A find\_business search is considered billable if either business name
or address are returned. If WhitePages doesn’t return any name or
address then the search is not billed.

### Example

The following example specifies a search for Pizza businesses in
Seattle.

	http://proapi.whitepages.com/find_business/1.1/?keywords=pizza;location=seattle%20wa;api_key=KEYVAL

## Data Dictionary

The following data points are returned in a Results Response by
WhitePages PRO API.

-   Listings

    -   Listing

        -   Business

            -   Businessname

        -   Displayname

        -   People

            -   Person

                -   Rank: "primary", "secondary"

                -   Firstname

                -   Lastname

                -   Middlename: usually an initial

                -   Suffix: e.g. "junior", "III"

                -   Age\_range: e.g. "25-29", "60-64"

        -   Phonenumbers

            -   Phone

                -   Rank: "primary", "secondary"

                -   Do\_not\_call: opt-in Boolean value, returned only
                    by reverse phone searches

                -   Type: : returned only by reverse phone searches,
                    "landline", "mobile", "non\_fixed\_voip", "unknown"

                -   Fullphone

                -   Areacode

                -   Exchange

                -   Linenumber

                -   Carrier: returned only by reverse phone searches

        -   Address

            -   Deliverable

            -   Receiving\_mail: opt-in, Boolean value

            -   Not\_receiving\_mail\_reason: opt-in, e.g. "vacant",
                "new\_construction", "other"

            -   Type: opt-in, "single-unit", "multi-unit", "po\_box",
                "commercial\_mail\_drop"

            -   Fullstreet

            -   House

            -   Street

            -   Streettype

            -   City

            -   State

            -   Zip

            -   Zip4

            -   Country

        -   Geodata

            -   Latitude

            -   Longitude

            -   Geoprecision: integer representing preciseness of
                geo-encoding: 1=rooftop, 2=street, 3=zip, 4=city,
                5=state, 6=country

        -   Listingmeta

            -   Lastvalidated

            -   Type: "home", "work", "business"

-   Meta

    -   Recordrange

        -   Firstrecord

        -   Lastrecord

        -   Totalavailable

    -   Apiversion

    -   Searchlinks

        -   Self

            -   url: the url of this API call

-   Result

    -   Type: "success", "error"

    -   Code: e.g. "Found Data", "Missing Input", etc.

    -   Message: blank for Results Response, otherwise meaningful error
        message

    -   Billable: Boolean value
  

WhitePages PRO API Implementation Guide

Copyright 2013, WhitePages, Inc.
