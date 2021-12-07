# training-go

3 microservicios:
	Clientes
	Facturas
	Productos
	
Servicio de clientes (port 5000):
	API basica de clientes (CRUD) (no se puede eliminar si esta usado en alguna factura) -> consumiendo microservicio de factura
		En el get del cliente incluir si tiene deuda o no (factura impaga) -> consumiendo microservicio de facturas
	GET 	/clients
	GET 	/clients/{id} -> consulta al endpoint de facturas si tiene deuda (GET /invoices/client/{id})
	POST	/clients
			{
				"Id",
				"Name"
			}
	PUT 	/clients/{id}
			{
				"Name"
			}
	DELETE 	/clients/{id}
		
Servicio de facturas (port 5001):
	API basica de facturas, solo GET y POST
		En el get de factura incluir el nombre del cliente y de los productos -> consumiendo microservicio de clientes y de productos
	GET 	/invoices
	GET 	/invoices/{id} -> consulta al endpoint de clientes para obtener el nombre (GET /clients/{id}) y al endpoint de producto para obtener el nombre (GET /products/{id})
	GET 	/invoices/client/{id}
			Response: { "HasDebt": true }
	GET 	/invoices/product/{id}
			Response: { "IsUsed": true }
	POST 	/invoices
			{
				"ClientId",
				"InvoiceDate",
				"Products": [
					"ProductId",
					"Quantity",
					"Cost"
				]
			}

Servicio de productos (port 5002):
	API basica de productos (CRUD) (no se puede eliminar si esta usado en alguna factura) -> consumiendo microservicio de factura
		En el get de producto incluir cantidad vendida en el ultimo mes -> consumiendo microservicio de facturas
	GET 	/products
	GET 	/products/{id}
	POST	/products
			{
				"Id",
				"Name"
			}
	PUT 	/products/{id}
			{
				"Name"
			}
	DELETE 	/products/{id} -> consulta al endpoint de facturas si se uso alguna vez, si se uso no se puede eliminar (GET /invoices/product/{id})

Aclaraciones:
	No hacer unit tests todavia
	Usar GORM (ejemplo: https://github.com/mludeiro/go-micro)
	Usar base de datos en memoria (ejemplo: https://github.com/mludeiro/go-micro)