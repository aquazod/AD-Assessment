# WooCommerce → Zoho CRM Integration

## Objective

Automatically synchronize WooCommerce orders
into Zoho CRM Contacts and Deals.

## Architecture

WooCommerce -> REST API -> Deluge Script -> Zoho CRM API -> Contacts + Deals 

## Features

- Fetch latest WooCommerce orders
- Create/update Contact
- Create Deal
- Prevent duplicate Contacts
- Error logging
- Modular code structure


# General Thoughts
- Only completed orders are synchronized to CRM to avoid creating Deals for orders that have not been successfully fulfilled.

- Instead of fetching all the orders from WooCommerce website, I wanted to fetch only the unprocessed orders on Zoho CRM, so I don't fetch orders that I already processed with previous fetching.
I had to decide between two approaches:
1- Saving last_processed_order_id and only process orders with id > last_processed_order_id
the tradeoff would be that an order with id <= last_processed_order_id would be updated and require me to update the Contact data at Zoho CRM, but it would be ignored.
I didn't decide for this approach
2- Saving last_sync_timestamp variable which holds the time of the last sync between Zoho and WooCommerce REST API, and add "?after=last_sync_timestamp" parameter to the REST API parameters.
This ensures getting all the orders after the last_sync_timestamp, but same issue, orderes which are modified/updated, won't be fetched.
I didn't decide for this approach.
3- Saving last_sync_timestamp variable but instead add the paramter "?modified_after=last_sync_timestamp" to REST API paramters.
This ensures getting only the newly created + updated orders after the last_sync_timestamp, ensuring a few orders retreival instead of bulk response.
I decided for this approach to follow the required integration flow in the assessment

the "modified_after" parameter is from the official WooCommerce Documentation https://developer.woocommerce.com/docs/apis/rest-api/v3/orders/

What I would do in production systems?
I would use WooCommerce WebHooks as the initiator of the integration instead of Zoho API, this ensures the most data accuracy/availability, and also fewer API calls.


## My Scope:

- ZohoCRM.modules.leads.ALL, ZohoCRM.modules.contacts.ALL, ZohoCRM.modules.deals.ALL, ZohoCRM.settings.variables.ALL


I choose the client type to be "Self Client"

## Challenges I faced in this assessment: 
- Zoho doesn't recognize the localhost website on my machine to send a request to WooCommerce REST API
Solution: used API gateway ngrok to get a public URL, and used the URL for integrations with Zoho
- I created all the functions as standalone, all standalone functions have to return string, not other data types
Solution: converted other datatypes toString() in the return statements for functions, converted them to other data types in the functions calls