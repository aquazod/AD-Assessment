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
