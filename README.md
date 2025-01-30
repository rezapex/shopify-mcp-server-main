# Shopify MCP Server

[![smithery badge](https://smithery.ai/badge/@rezapex/shopify-mcp-server-main)](https://smithery.ai/server/@rezapex/shopify-mcp-server-main)
MCP Server for Shopify API, enabling interaction with store data through GraphQL API. This server provides tools for managing products, customers, orders, and more.

<a href="https://glama.ai/mcp/servers/bemvhpy885"><img width="380" height="200" src="https://glama.ai/mcp/servers/bemvhpy885/badge" alt="Shopify Server MCP server" /></a>

## Features

* **Product Management**: Search and retrieve product information
* **Customer Management**: Load customer data and manage customer tags
* **Order Management**: Advanced order querying and filtering
* **GraphQL Integration**: Direct integration with Shopify's GraphQL Admin API
* **Comprehensive Error Handling**: Clear error messages for API and authentication issues

## Tools

1. `get-products`
   * Get all products or search by title
   * Inputs:
     * `searchTitle` (optional string): Filter products by title
     * `limit` (number): Maximum number of products to return
   * Returns: Formatted product details including title, description, handle, and variants

2. `get-products-by-collection`
   * Get products from a specific collection
   * Inputs:
     * `collectionId` (string): ID of the collection to get products from
     * `limit` (optional number, default: 10): Maximum number of products to return
   * Returns: Formatted product details from the specified collection

3. `get-products-by-ids`
   * Get products by their IDs
   * Inputs:
     * `productIds` (array of strings): Array of product IDs to retrieve
   * Returns: Formatted product details for the specified products

4. `get-variants-by-ids`
   * Get product variants by their IDs
   * Inputs:
     * `variantIds` (array of strings): Array of variant IDs to retrieve
   * Returns: Detailed variant information including product details

5. `get-customers`
   * Get shopify customers with pagination support
   * Inputs:
     * `limit` (optional number): Maximum number of customers to return
     * `next` (optional string): Next page cursor
   * Returns: Customer data in JSON format

6. `tag-customer`
   * Add tags to a customer
   * Inputs:
     * `customerId` (string): Customer ID to tag
     * `tags` (array of strings): Tags to add to the customer
   * Returns: Success or failure message

7. `get-orders`
   * Get orders with advanced filtering and sorting
   * Inputs:
     * `first` (optional number): Limit of orders to return
     * `after` (optional string): Next page cursor
     * `query` (optional string): Filter orders using query syntax
     * `sortKey` (optional enum): Field to sort by ('PROCESSED_AT', 'TOTAL_PRICE', 'ID', 'CREATED_AT', 'UPDATED_AT', 'ORDER_NUMBER')
     * `reverse` (optional boolean): Reverse sort order
   * Returns: Formatted order details

8. `get-order`
   * Get a single order by ID
   * Inputs:
     * `orderId` (string): ID of the order to retrieve
   * Returns: Detailed order information

9. `create-discount`
   * Create a basic discount code
   * Inputs:
     * `title` (string): Title of the discount
     * `code` (string): Discount code that customers will enter
     * `valueType` (enum): Type of discount ('percentage' or 'fixed_amount')
     * `value` (number): Discount value (percentage as decimal or fixed amount)
     * `startsAt` (string): Start date in ISO format
     * `endsAt` (optional string): Optional end date in ISO format
     * `appliesOncePerCustomer` (boolean): Whether discount can be used only once per customer
   * Returns: Created discount details

10. `create-draft-order`
    * Create a draft order
    * Inputs:
      * `lineItems` (array): Array of items with variantId and quantity
      * `email` (string): Customer email
      * `shippingAddress` (object): Shipping address details
      * `note` (optional string): Optional note for the order
    * Returns: Created draft order details

11. `complete-draft-order`
    * Complete a draft order
    * Inputs:
      * `draftOrderId` (string): ID of the draft order to complete
      * `variantId` (string): ID of the variant in the draft order
    * Returns: Completed order details

12. `get-collections`
    * Get all collections
    * Inputs:
      * `limit` (optional number, default: 10): Maximum number of collections to return
      * `name` (optional string): Filter collections by name
    * Returns: Collection details

13. `get-shop`
    * Get shop details
    * Inputs: None
    * Returns: Basic shop information

14. `get-shop-details`
    * Get extended shop details including shipping countries
    * Inputs: None
    * Returns: Extended shop information including shipping countries

15. `manage-webhook`
    * Subscribe, find, or unsubscribe webhooks
    * Inputs:
      * `action` (enum): Action to perform ('subscribe', 'find', 'unsubscribe')
      * `callbackUrl` (string): Webhook callback URL
      * `topic` (enum): Webhook topic to subscribe to
      * `webhookId` (optional string): Webhook ID (required for unsubscribe)
    * Returns: Webhook details or success message

## Setup

### Shopify Access Token

To use this MCP server, you'll need to create a custom app in your Shopify store:

1. From your Shopify admin, go to **Settings** > **Apps and sales channels**
2. Click **Develop apps** (you may need to enable developer preview first)
3. Click **Create an app**
4. Set a name for your app (e.g., "Shopify MCP Server")
5. Click **Configure Admin API scopes**
6. Select the following scopes:
   * `read_products`, `write_products`
   * `read_customers`, `write_customers`
   * `read_orders`, `write_orders`
7. Click **Save**
8. Click **Install app**
9. Click **Install** to give the app access to your store data
10. After installation, you'll see your **Admin API access token**
11. Copy this token - you'll need it for configuration

Note: Store your access token securely. It provides access to your store data and should never be shared or committed to version control.
More details on how to create a Shopify app can be found [here](https://help.shopify.com/en/manual/apps/app-types/custom-apps).

### Usage with Claude Desktop

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "shopify": {
      "command": "npx",
      "args": ["-y", "shopify-mcp-server"],
      "env": {
        "SHOPIFY_ACCESS_TOKEN": "<YOUR_ACCESS_TOKEN>",
        "MYSHOPIFY_DOMAIN": "<YOUR_SHOP>.myshopify.com"
      }
    }
  }
}
```

## Development

1. Clone the repository
2. Install dependencies:
```bash
npm install
```
3. Create a `.env` file:
```
SHOPIFY_ACCESS_TOKEN=your_access_token
MYSHOPIFY_DOMAIN=your-store.myshopify.com
```
4. Build the project:
```bash
npm run build
```
5. Run tests:
```bash
npm test
```

## Dependencies

- @modelcontextprotocol/sdk - MCP protocol implementation
- graphql-request - GraphQL client for Shopify API
- zod - Runtime type validation

## Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) first.

## License

MIT

## Community

- [MCP GitHub Discussions](https://github.com/modelcontextprotocol/servers/discussions)
- [Report Issues](https://github.com/your-username/shopify-mcp-server/issues)

---

Built with ❤️ using the [Model Context Protocol](https://modelcontextprotocol.io) 
