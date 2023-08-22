# RetailAnalitycs

SQL проект реализованный на postgresql. В нем содержаться скрипты, необходимые для создания базы данных со знаниями о клиентах розничных сетей, а также для создания персональных предложений для клиентов.

## Database Creation

Чтобы создать базу данных и таблицы, описанные во входных данных, выполните следующие действия:

1. Укажите в part1.sql путь до необходимых tsv файлов которые лежат в datasets
2. Запустите скрипт `part1.sql` из репозитория.
3. Этот сценарий создаст необходимые таблицы, а также включает процедуры импорта и экспорта данных для каждой таблицы из/в файлы CSV и TSV.


## Forming Personal Offers for Average Check Growth

Для формирования персональных предложений, направленных на рост среднего чека, выполните следующие действия:

1. Запустите скрипт `part4.sql` из репозитория.
2. Данный скрипт содержит функцию, определяющую предложения на основе метода расчета среднего чека, первой и последней дат периода, количества транзакций, коэффициента увеличения среднего чека, максимального показателя оттока, максимальной доли транзакций со скидкой и допустимой доля маржи.
3. Функция выводит идентификатор клиента, среднее целевое значение чека, группу предложений и максимальную глубину скидки для каждого предложения.

## Forming Personal Offers for Increasing Frequency of Visits

Для формирования персональных предложений, направленных на увеличение частоты посещений, выполните следующие действия:

1. Запустите скрипт `part5.sql` из репозитория.
2. Этот скрипт содержит функцию, которая определяет предложения на основе первой и последней дат периода, добавленного количества транзакций, максимального индекса оттока, максимальной доли транзакций со скидкой и допустимой доли маржи.
3. Функция выводит идентификатор клиента, дату начала периода, дату окончания периода, целевое количество транзакций, группу предложений и максимальную глубину скидки для каждого предложения.

## Forming Personal Offers for Cross-Selling

Для формирования персональных предложений, направленных на кросс-продажи, выполните следующие действия:

1. Запустите скрипт `part6.sql` из репозитория.
2. Этот скрипт содержит функцию, которая определяет предложения на основе количества групп, максимального индекса оттока, максимального индекса стабильности потребления, максимальной доли SKU и допустимой доли маржи.
3. Функция выводит идентификатор клиента, предложения SKU и максимальный размер скидки для каждого предложения.

---

## Structure tables

### Personal information Table

|       **Field**       | **System field name**  |                                   **Format / possible values**                                    | **Description** |
| :-------------------: | :--------------------: | :-----------------------------------------------------------------------------------------------: | :-------------: |
|      Customer ID      |      Customer_ID       |                                                ---                                                |       ---       |
|         Name          |     Customer_Name      | Cyrillic, the first letter is capitalized, the rest are upper case, dashes and spaces are allowed |       ---       |
|        Surname        |    Customer_Surname    | Cyrillic, the first letter is capitalized, the rest are upper case, dashes and spaces are allowed |       ---       |
|    Customer E-mail    | Customer_Primary_Email |                                           E-mail format                                           |       ---       |
| Customer phone number | Customer_Primary_Phone |                                     +7 and 10 Arabic numerals                                     |       ---       |

---

### Cards Table

|  **Field**  | **System field name** | **Format / possible values** |          **Description**           |
| :---------: | :-------------------: | :--------------------------: | :--------------------------------: |
|   Card ID   |   Customer_Card_ID    |             ---              |                ---                 |
| Customer ID |      Customer_ID      |             ---              | One customer can own several cards |

---

### Transactions Table

|    **Field**     | **System field name** | **Format / possible values** |                          **Description**                           |
| :--------------: | :-------------------: | :--------------------------: | :----------------------------------------------------------------: |
|  Transaction ID  |    Transaction_ID     |             ---              |                            Unique value                            |
|     Card ID      |   Customer_Card_ID    |             ---              |                                ---                                 |
| Transaction sum  |   Transaction_Summ    |        Arabic numeral        | Transaction sum in rubles(full purchase price excluding discounts) |
| Transaction date | Transaction_DateTime  |     dd.mm.yyyy hh:mm:ss      |            Date and time when the transaction was made             |
|      Store       | Transaction_Store_ID  |           Store ID           |              The store where the transaction was made              |

---

### Checks Table

|                    **Field**                     | **System field name** | **Format / possible values** |                                                **Description**                                                |
| :----------------------------------------------: | :-------------------: | :--------------------------: | :-----------------------------------------------------------------------------------------------------------: |
|                  Transaction ID                  |    Transaction_ID     |             ---              |                           Transaction ID is specified for all products in the check                           |
|               Product in the check               |        SKU_ID         |             ---              |                                                      ---                                                      |
|          Number of pieces or kilograms           |      SKU_Amount       |        Arabic numeral        |                                     The quantity of the purchased product                                     |
| Total amount for which the product was purchased |       SKU_Summ        |        Arabic numeral        | The purchase amount of the actual volume of this product in rubles (full price without discounts and bonuses) |
|          The paid price of the product           |     SKU_Summ_Paid     |        Arabic numeral        |                      The amount actually paid for the product not including the discount                      |
|                 Discount granted                 |     SKU_Discount      |        Arabic numeral        |                          The size of the discount granted for the product in rubles                           |

---

### Product grid Table

|       **Field**        | **System field name** |         **Format / possible values**          |                                                                                                        **Description**                                                                                                        |
| :--------------------: | :-------------------: | :-------------------------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|       Product ID       |        SKU_ID         |                      ---                      |                                                                                                              ---                                                                                                              |
|      Product name      |       SKU_Name        | Cyrillic, Arabic numerals, special characters |                                                                                                              ---                                                                                                              |
|       SKU group        |       Group_ID        |                      ---                      | The ID of the group of related products to which the product belongs (for example, same type of yogurt of the same manufacturer and volume, but different flavors). One identifier is specified for all products in the group |
| Product purchase price |  SKU_Purchase_Price   |                Arabic numeral                 |                                                                                       The purchase price of the product for this store                                                                                        |
|  Product retail price  |   SKU_Retail_Price    |                Arabic numeral                 |                                                                               The sale price of the product excluding discounts for this store                                                                                |

---

### Stores Table

|       **Field**        | **System field name** | **Format / possible values** |                         **Description**                          |
| :--------------------: | :-------------------: | :--------------------------: | :--------------------------------------------------------------: |
|         Store          | Transaction_Store_ID  |             ---              |                               ---                                |
|       Product ID       |        SKU_ID         |             ---              |                               ---                                |
| Product purchase price |  SKU_Purchase_Price   |        Arabic numeral        |           Purchasing price of products for this store            |
|  Product retail price  |   SKU_Retail_Price    |        Arabic numeral        | The sale price of the product excluding discounts for this store |

---

### SKU group Table

| **Field**  | **System field name** |         **Format / possible values**          | **Description** |
| :--------: | :-------------------: | :-------------------------------------------: | :-------------: |
| SKU group  |       Group_ID        |                      ---                      |       ---       |
| Group name |      Group_Name       | Cyrillic, Arabic numerals, special characters |       ---       |

---

### Date of analysis formation Table

|    **Field**     | **System field name** | **Format / possible values** | **Description** |
| :--------------: | :-------------------: | :--------------------------: | :-------------: |
| Date of analysis |  Analysis_Formation   |     dd.mm.yyyy hh:mm:ss      |       ---       |

---

## Output data

### Customers View

|                   **Field**                   |     **System field name**      | **Format / possible values** |                                   **Description**                                    |
| :-------------------------------------------: | :----------------------------: | :--------------------------: | :----------------------------------------------------------------------------------: |
|                  Customer ID                  |          Customer_ID           |             ---              |                                     Unique value                                     |
|          Value of the average check           |     Customer_Average_Check     |   Arabic numeral, decimal    |             Value of the average check in rubles for the analyzed period             |
|             Average check segment             | Customer_Average_Check_Segment |      High; Middle; Low       |                                 Segment description                                  |
|          Transaction frequency value          |       Customer_Frequency       |   Arabic numeral, decimal    | Value of customer visit frequency in the average number of days between transactions |
|         Transaction frequency segment         |   Customer_Frequency_Segment   | Often; Occasionally; Rarely  |                                 Segment description                                  |
| Number of days since the previous transaction |    Customer_Inactive_Period    |   Arabic numeral, decimal    |              Number of days passed since the previous transaction date               |
|                  Churn rate                   |      Customer_Churn_Rate       |   Arabic numeral, decimal    |                           Value of the customer churn rate                           |
|              Churn rate segment               |     Customer_Churn_Segment     |      High; Middle; Low       |                                 Segment description                                  |
|                Segment number                 |        Customer_Segment        |        Arabic numeral        |               The number of the segment to which the customer belongs                |
|                 Main store ID                 |     Customer_Primary_Store     |             ---              |                                         ---                                          |

---

### Purchase history View

|     **Field**     | **System field name** | **Format / possible values** |                                                                                                        **Description**                                                                                                        |
| :---------------: | :-------------------: | :--------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|    Customer ID    |      Customer_ID      |             ---              |                                                                                                              ---                                                                                                              |
|  Transaction ID   |    Transaction_ID     |             ---              |                                                                                                              ---                                                                                                              |
| Transaction date  | Transaction_DateTime  | dd.mm.yyyyy hh:mm:ss.0000000 |                                                                                            The date when the transaction was made                                                                                             |
|     SKU group     |       Group_ID        |             ---              | The ID of the group of related products to which the product belongs (for example, same type of yogurt of the same manufacturer and volume, but different flavors). One identifier is specified for all products in the group |
|    Prime cost     |      Group_Cost       |   Arabic numeral, decimal    |                                                                                                              ---                                                                                                              |
| Base retail price |      Group_Summ       |   Arabic numeral, decimal    |                                                                                                              ---                                                                                                              |
| Actual cost paid  |    Group_Summ_Paid    |   Arabic numeral, decimal    |                                                                                                              ---                                                                                                              |

---

### Periods View

|               **Field**               |   **System field name**   | **Format / possible values** |                                                                                                        **Description**                                                                                                        |
| :-----------------------------------: | :-----------------------: | :--------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|              Customer ID              |        Customer_ID        |             ---              |                                                                                                              ---                                                                                                              |
|               SKU group               |         Group_ID          |             ---              | The ID of the group of related products to which the product belongs (for example, same type of yogurt of the same manufacturer and volume, but different flavors). One identifier is specified for all products in the group |
|  Date of first purchase of the group  | First_Group_Purchase_Date | yyyy-mm-dd hh:mm:ss.0000000  |                                                                                                              ---                                                                                                              |
|  Date of last purchase of the group   | Last_Group_Purchase_Date  | yyyy-mm-dd hh:mm:ss.0000000  |                                                                                                              ---                                                                                                              |
| Number of transactions with the group |      Group_Purchase       |   Arabic numeral, decimal    |                                                                                                              ---                                                                                                              |
|     Intensity of group purchases      |      Group_Frequency      |   Arabic numeral, decimal    |                                                                                                              ---                                                                                                              |
|        Minimum group discount         |    Group_Min_Discount     |   Arabic numeral, decimal    |                                                                                                              ---                                                                                                              |

---

### Groups View

|               **Field**               | **System field name**  | **Format / possible values** |                                                              **Description**                                                               |
| :-----------------------------------: | :--------------------: | :--------------------------: | :----------------------------------------------------------------------------------------------------------------------------------------: |
|              Customer ID              |      Customer_ID       |             ---              |                                                                    ---                                                                     |
|               Group ID                |        Group_ID        |             ---              |                                                                    ---                                                                     |
|            Affinity index             |  Group_Affinity_Index  |   Arabic numeral, decimal    |                                                   Customer affinity index for this group                                                   |
|              Churn index              |    Group_Churn_Rate    |   Arabic numeral, decimal    |                                                 Customer churn index for a specific group                                                  |
|            Stability index            | Group_Stability_Index  |   Arabic numeral, decimal    |                               Indicator demonstrating the stability of the customer consumption of the group                               |
|      Actual margin for the group      |      Group_Margin      |   Arabic numeral, decimal    |                                   Indicator of the actual margin for the group for a particular customer                                   |
| Share of transactions with a discount |  Group_Discount_Share  |   Arabic numeral, decimal    | Share of purchasing transactions of the group by a customer, within which the discount was applied (excluding the loyalty program bonuses) |
|     Minimum size of the discount      | Group_Minimum_Discount |   Arabic numeral, decimal    |                                            Minimum size of the group discount for the customer                                             |
|           Average discount            | Group_Average_Discount |   Arabic numeral, decimal    |                                            Average size of the group discount for the customer                                             |

---
