# Microservices training Go

The goal of this project is to create 3 microservices, one for clients, one for invoices and one for products in Golang. Each microservice will be in it's own folder with the corresponding name.

## Clients microservice

This endpoint will manage clients and will interact with other microservices. It will expose the endpoint in port **5000**
The endpoints to create for this service are:

### Get all clients
**GET**
**/clients**
*Response:*
´´´
[
	{
		"Id": 1,
		"Name": "Google"
	},
	...
]
´´´

### Get client by id
**GET**
**/clients/{id}**
*Response:*
´´´
{
	"Id": 1,
	"Name": "Google",
	"HasDebt": false
}
´´´
*Special considerations*
This endpoint will use the invoice endpoint (GET /invoices/client/{id}) to check if the client has debt and will include that information in the response

### Create client
**POST**
**/clients**
*Request data*
´´´
{
	"Name": "Google"
}
´´´
*Response:*
´´´
{
	"Id": 1,
	"Name": "Google",
	"HasDebt": false
}
´´´
*Special considerations*
The id of the client will be autoincremental determined by the endpoint or storage

### Update client
**PUT**
**/clients/{id}**
*Request data*
´´´
{
	"Name": "Google"
}
´´´
*Response:*
´´´
{
	"Id": 1,
	"Name": "Google",
	"HasDebt": false
}
´´´

### Delete client
**DELETE**
**/clients/{id}**

## Invoices microservice

This endpoint will manage invoices and will interact with other microservices. It will expose the endpoint in port **5001**
The endpoints to create for this service are:

### Get all invoices
**GET**
**/invoices**
*Response:*
´´´
[
	{
		"Id",
		"ClientId",
		"ClientName",
		"InvoiceDate",
		"Products": [
			"ProductId",
			"ProductName",
			"Quantity",
			"Cost"
		]
	},
	...
]
´´´
*Special considerations*
The client name (GET /clients/{id}) and the product name (GET /products/{id}) will come from the corresponding endpoints.

### Get invoice by id
**GET**
**/invoices/{id}**
*Response:*
´´´
{
	"Id",
	"ClientId",
	"ClientName",
	"InvoiceDate",
	"Products": [
		"ProductId",
		"ProductName",
		"Quantity",
		"Cost"
	]
}
´´´
*Special considerations*
The client name (GET /clients/{id}) and the product name (GET /products/{id}) will come from the corresponding endpoints.

### Get client's debt
**GET**
**/invoices/client/{id}**
*Response:*
´´´
{
	"HasDebt": true 
}
´´´

### Get product usage
**GET**
**/invoices/product/{id}**
*Response:*
´´´
{
	"IsUsed": true 
}
´´´

### Create invoice
**POST**
**/invoices**
*Request data*
´´´
{
	"ClientId",
	"InvoiceDate",
	"Products": [
		"ProductId",
		"Quantity",
		"Cost"
	]
}
´´´
*Response:*
´´´
{
	"Id",
	"ClientId",
	"ClientName",
	"InvoiceDate",
	"Products": [
		"ProductId",
		"ProductName",
		"Quantity",
		"Cost"
	]
}
´´´
*Special considerations*
The id of the invoice will be autoincremental determined by the endpoint or storage

## Products microservice

This endpoint will manage products and will interact with other microservices. It will expose the endpoint in port **5002**
The endpoints to create for this service are:

### Get all products
**GET**
**/products**
*Response:*
´´´
[
	{
		"Id": 1,
		"Name": "Computer"
	},
	...
]
´´´

### Get product by id
**GET**
**/products/{id}**
*Response:*
´´´
{
	"Id": 1,
	"Name": "Computer"
}
´´´

### Create product
**POST**
**/products**
*Request data*
´´´
{
	"Name": "Computer"
}
´´´
*Response:*
´´´
{
	"Id": 1,
	"Name": "Computer"
}
´´´
*Special considerations*
The id of the product will be autoincremental determined by the endpoint or storage

### Update product
**PUT**
**/products/{id}**
*Request data*
´´´
{
	"Name": "Computer"
}
´´´
*Response:*
´´´
{
	"Id": 1,
	"Name": "Computer"
}
´´´

### Delete product
**DELETE**
**/products/{id}**
*Special considerations*
This endpoint will use the invoice endpoint (GET /invoices/product/{id}) to check if the product has been used in any invoice and can't be deleted if it has been used
		
## Tips and special considerations
- Use GORM [example usage](https://github.com/mludeiro/go-micro)
- Use in memory database [example usage](https://github.com/mludeiro/go-micro)

## Next steps
- Unit tests
- Docker usage