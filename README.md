# google-sheets-woocommerce-seo-updater-1gilb

## Overview

This workflow is designed to streamline the SEO update process for WooCommerce products. It triggers on new or updated rows in a specified Google Sheet, extracts product URLs, meta titles, and descriptions. It then attempts to find the corresponding product in WooCommerce using its slug or search term. If found, it updates the product's meta data (supporting both Rank Math and Yoast SEO fields) and records the status back into the Google Sheet.

## Features

- Triggers automatically from Google Sheets for new/updated data.
- Extracts product slug or search term from various URL formats.
- Finds WooCommerce products using their slug or a search query.
- Updates WooCommerce product meta titles and descriptions.
- Supports both Rank Math and Yoast SEO meta data fields.
- Reports update status (Found, Updated, Failed) back to Google Sheets.
- Includes batching for HTTP requests to manage API rate limits.
- Handles cases where products are not found or updates fail.

## Services Used

- Google Sheets
- WooCommerce REST API
- n8n

## Trigger

The workflow is triggered by the 'Google Sheets Trigger' node, which polls a specified Google Sheet for changes or new rows at a defined interval (e.g., every minute).

## Prerequisites

- An active n8n instance.
- A Google Sheets document with 'Urls', 'Meta Title', 'Meta Description', and 'Status' columns.
- A WooCommerce store with REST API enabled and products matching the URLs/slugs.
- Rank Math or Yoast SEO plugin installed on the WooCommerce store for meta data updates.

## Credentials

- Google Sheets Trigger (OAuth2 API) for reading sheet data.
- Google Sheets (OAuth2 API) for writing back status updates.
- WooCommerce API (Consumer Key and Secret) for accessing and updating product data.

## Configuration

1. Configure 'Google Sheets Trigger' node with your Google Sheets OAuth2 API credential and select the target document and sheet.
2. Configure 'Update Google Sheet' node with your Google Sheets OAuth2 API credential and select the same document and sheet.
3. Configure 'Find Product by Slug' and 'Update Meta' nodes with your WooCommerce API credential (Consumer Key and Secret).
4. Verify the WooCommerce store URL in 'Find Product by Slug' and 'Update Meta' nodes is correct (e.g., 'https://yourstore.com/wp-json/wc/v3/products').
5. Ensure the column names for 'Urls', 'Meta Title', and 'Meta Description' in the 'Prepare Data' node's JavaScript match your Google Sheet headers exactly.
6. If using a different SEO plugin or meta keys, adjust the 'meta_data' array in the 'Update Meta' node's JSON body accordingly.

## Usage

1. Populate the designated Google Sheet with product URLs, desired meta titles, and meta descriptions. The 'Status' column will be updated by the workflow.
2. Activate the n8n workflow.
3. The workflow will automatically trigger based on the 'Google Sheets Trigger' schedule (e.g., every minute).
4. Monitor the 'Status' column in your Google Sheet to see the outcome for each row (e.g., 'Found', 'Updated', 'Failed').
5. Manually execute the workflow once to process all existing rows or to test the setup.

## Troubleshooting

- If 'No URL found in row' error occurs, ensure the 'Urls' column in your Google Sheet is correctly named and populated.
- If 'Product not found' in the status, verify the URL in the Google Sheet corresponds to an existing WooCommerce product and that the WooCommerce API has read permissions.
- If 'Product not found or update failed' is returned, check WooCommerce API credentials, ensure the base URL is correct, and verify the API has write permissions to products and meta data.
- For rate limit issues with WooCommerce, adjust the 'batchSize' and 'batchInterval' parameters in the 'Find Product by Slug' and 'Update Meta' nodes' 'Options > Batching' settings.
- If meta data is not updating correctly, confirm the 'meta_data' keys in the 'Update Meta' node (e.g., 'rank_math_title', '_yoast_wpseo_title') match those used by your specific SEO plugin.

## Security Notes

- WooCommerce API credentials (Consumer Key and Secret) grant significant access to your store. Ensure they are stored securely in n8n and only have the necessary permissions (read products, write product meta data).
- Google Sheets credentials provide access to your specified spreadsheet. Ensure sensitive data is not stored in publicly accessible sheets.
- Regularly review workflow executions and logs for any unauthorized access attempts or errors that might indicate a security vulnerability.
- Be mindful of the data being processed and ensure compliance with any relevant data privacy regulations.
