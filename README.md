# LGPD
# Advanced Topics
The project aims to develop a solution to problems related to the LGPD.

### Data Deletion
Today, for a company to delete the data of one of its customers there is a somewhat complicated process, because there are companies that have numerous backups of the data in various databases. Deleting customer data is simplified, because in order to delete this data from some companies' databases, this process needs to be done on dozens or hundreds of backups.
Encryption helps to privatize this data.

### Partitioning
Partitioning works as follows: we started from the “sales” table, where it contained both the data of the sale and the customer who made the sale. Once we partitioned this table, we separated the data of the sale and the customer, making it possible to delete the customer's data without having to delete the sale.
In order to do this, we first had to retrieve the data from the “sales” table, after which we separated the data. With the data that was separated, we inserted it into 2 other databases.

### Data portability
Data portability is a right included in the General Data Protection Act (LGPD) and the General Data Protection Regulation (GDPR) for individuals to obtain and reuse their personal data for their own purposes in different services. Therefore, according to art. 18 of the LGPD, the holder of personal data has the right, upon express request, to obtain from the controller the portability of the data to another service or product provider, observing commercial and industrial secrets, guaranteeing in many cases a reduction in exchange costs and preventing consumer harassment. Examples of cases in which data portability may occur include: consumers changing telephone/internet companies, companies going bankrupt, companies being taken over, etc. 
In the case of this project, in order to ensure the integrity and security of the customer's data, the SSL protocol was used to transfer it to the new database that controls and carries the data in question. Using the customer's CPF, all their data will be relocated to the new database, and the atomicity of the transaction is guaranteed, since there is control from the beginning to the end of the data traffic and all the blocks are executed in full (in the event of an error, all the operations that make up the transaction will be discarded).

<a name="estrutura"></a>
# Project structure 
## Use case diagram:
![Casos de uso](/caso_de_uso.PNG)


##  Database model:
**CryptoKey**
```json
{
  "_id": {
    "$oid": "62993c873c933fa7e6706a6f"
  },
  "id": {
    "$numberLong": "1509293973392"
  },
  "chave": "K5OLfuM/DmQ5bQphAxlGvtWqB8HqxX7m",
  "cpf_client": "3574789"
}
```

**Client(Portabilized table)**
```json
{
  "_id": {
    "$oid": "62993da53c933fa7e6707442"
  },
  "Nome": "Deanna Vaughn",
  "Telefone": "1-947-658-1423",
  "Email": "non.massa@aol.net",
  "CPF": "7866913"
}
```

**Vendas(Table before partitioning)**
```json
{
  "_id": {
    "$oid": "62993c873c933fa7e6706a69"
  },
  "produto_venda": "enim. Etiam gravida molestie arcu. Sed eu nibh vulputate mauris",
  "valor_venda": 99576,
  "qtd_venda": 14907,
  "idCli": "bab20b48541b40b698acd801cb387a71"
}
```


**Cliente(Table after partitioning)**
```json
{
  "_id": {
    "$oid": "62993ccc3c933fa7e6707239"
  },
  "id": "bab20b48541b40b698acd801cb387a71",
  "nome_cli": "En+NXk0VcSekegz/dPh0CRSeNXws2Hhfs1aBKxQ9AZM=",
  "telefone_cli": "oGAcP86TGsiAjme5Od7h43DI4J8WhYK4CdDo92lA2vg=",
  "email_cli": "4QF1Tu97bmASRyNyBENlbfiPczI3nhFEfo3TQO3vamU=",
  "cpf_cli": "bMKtl9cg8CxIdPt3yjSumWFwzplAOSDDcC+5JyDT3sU=",
  "id_chave": {
    "$numberLong": "1509293239536"
  }
}
```

**VendaSimples(Table after partitioning)**
```json
{
  "_id": {
    "$oid": "62993c873c933fa7e6706a69"
  },
  "produto_venda": "enim. Etiam gravida molestie arcu. Sed eu nibh vulputate mauris",
  "valor_venda": 99576,
  "qtd_venda": 14907,
  "idCli": "bab20b48541b40b698acd801cb387a71"
}
```

## API Documentation
<details >
<summary>
<b>🟦GET</b>  /find_user/[CPF usuário]/ 
</summary>

Search job by id.
<p>Response 200:</p>

``` json
{
    "Nome": "Alden Harper",
    "Telefone": "1-998-995-1116",
    "Email": "in@outlook.com",
    "CPF": "6735596"
}
```
</details>

<details>
<summary>
<b>🟩POST</b> /insert_user
</summary>
Insert a job
<p>Exmaple of parameters:</p>

``` json
{"produto_venda":"elementum at,",
"valor_venda":49510,
"qtd_venda":31484,
"nome_cli":"Deanna Vaughn",
"telefone_cli":"1-947-658-1423",
"cpf_cli":666222,
"email_cli":"non.massa@aol.net"}
```
</details>

<details>
<summary>
<b>🟥DELETE</b> delete_user/6735596[CPF do usuário]
</summary>
Delete a job based on paramter, case it's found.
<p>Response 200:</p>

``` json
{
   "message": "Key deleted"
}
```
</details>

<details>
<summary>
<b>🟩POST</b> /old_sales
</summary>
<p>Response 200:</p>
</details>

<details>
<summary>
<b>🟩POST</b> /split_sale
</summary>
Partitioned Table
<p>Response 200:</p>
``` json
{
   "message": "CV inserted sucessfully"
}
```
</details>

<details>
<summary>
<b>🟩POST</b> client_data_portability/[CPF do usuário]
</summary>
User portability
<p>Response 200:</p>
</details>


<a name="tecnology"></a>
## Technologies used:
 * Python 
 * Django
 * Symmetric cryptography aes128 bytes
 * MongoDb
