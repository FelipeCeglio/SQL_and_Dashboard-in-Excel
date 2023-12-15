KPIs:
	1 -> Análise Comercial do Ano de 2013:
		a. Total de VENDAS realizadas pela internet x CATEGORIA
		b. Total de VENDAS realizadas pela internet x MÊS x PEDIDO
		c. Total de VENDAS realizadas pela internet x CUSTO x PAÍS
		d. Total de VENDAS realizadas pela internet x SEXO

TABELAS DE REFERÊNCIAS:

	Vendas realizadas pela internet: FactInternetSales
	Categoria: DimProductCategory ** em cadeia com DimProduct
	Mês: FactInternetSales
	Pedido: FactInternetSales
	Custo: FactInternetSales
	Pais: DimSalesTerritory
	Sexo: DimCustomer

 

COLUNAS:
	SalesOrderNumber				Tabela: FactInternetSales
	OrderDate						Tabela: FactInternetSales
	EnglishProductCategoryName		Tabela: DimProductCategory
	FirstName + LastName			Tabela: DimCustomer
	Gender							Tabela: DimCustomer
	SalesTerritoryCountry			Tabela: DimSalesTerritory
	OrderQuantity					Tabela: FactInternetSales
	TotalProductCost				Tabela: FactInternetSales
	SalesAmount						Tabela: FactInternetSales


COMANDO SQL:
  
	CREATE OR ALTER VIEW VENDAS_INTERNET AS
	  SELECT
	  	fis.SalesOrderNumber AS 'Order No',
	  	fis.OrderDate AS 'Order Date',
	  	dpc.EnglishProductCategoryName AS 'Product Category',
	  	dc.FirstName + ' ' + dc.LastName AS 'Customer Name',
	  	dc.Gender AS 'Gender',
	  	dst.SalesTerritoryCountry AS 'Country',
	  	fis.OrderQuantity AS 'Order Qtd',
	  	fis.TotalProductCost AS 'Order Cost',
	  	fis.SalesAmount AS 'Order Revenue'
	  FROM
	  	FactInternetSales AS fis
	  INNER JOIN DimProduct AS dp ON fis.ProductKey = dp.ProductKey
	  	INNER JOIN DimProductSubcategory AS dps ON dp.ProductSubcategoryKey = dps.ProductSubcategoryKey
	  		INNER JOIN DimProductCategory AS dpc ON dps.ProductCategoryKey = dpc.ProductCategoryKey
	  INNER JOIN  DimCustomer AS dc ON fis.CustomerKey = dc.CustomerKey
	  INNER JOIN DimSalesTerritory AS dst ON fis.SalesTerritoryKey = dst.SalesTerritoryKey
	  WHERE YEAR(ORDERDATE) = 2013
